# Online Resource Optimization for Elastic Stream Processing with Regret Guarantee

2022 ICPP

基于遗憾保证的弹性流处理在线资源优化

背景：

- 流处理系统：Apache Storm 和 Flink 是流式计算框架的代表，主要与处理大数据相关，也是分布式实时计算框架，详见数据科学与工程课件。
- 流处理系统中，流处理应用被认为是操作者的有向无环图(DAG)。
- 在大数据处理中，DAG计算常常指的是将计算任务在内部分解成为若干个子任务，将这些子任务之间的逻辑关系或顺序构建成DAG(有向无环图)结构。
- 数据流程序一般在由数据流图表示，数据流图描述了数据如何在操作之间流动。在数据流图中，节点被称为operator，代表计算；边代表数据依赖。
- Kubernetes，简称 k8s(k，8 个字符，s——明白了？)或者 “kube”，是一个开源的 Linux 容器自动化运维平台，它消除了容器化应用程序在部署、伸缩时涉及到的许多手动操作。换句话说，你可以将多台主机组合成集群来运行 Linux 容器，而 Kubernetes 可以帮助你简单高效地管理那些集群。构成这些集群的主机还可以跨越公有云、私有云以及混合云。

解决问题：作者提出了Dragster，一种用于弹性流处理的基于在线优化的动态资源分配方案。以解决在急剧(drastically-drasgster)改变提供的负载下的动态资源分配方案。通过将在线优化框架与置信上界技术相结合，Dragster可以保证预期吞吐量后悔时间的次线性增长——采用了贝叶斯优化，并且通过实验验证实现了Dragster来提高Kubernetes上Flink应用程序的吞吐量。



当前存在的问题：

- 分布式流处理应用程序忽略了云计算的弹性，仍然存在资源利用率低的问题。大量的分布式流处理系统(如Spark和Flink)的迁移时幼稚的。

如何解决：

- 设计一种基于在线优化的动态资源分配方案，该方案可以**跟踪最优资源分配**，**以最大化应用吞吐量**，同时提高弹性流处理的**资源利用率**
  - 流处理应用程序中的运算符组成有向无环图(DAG)，其中每个operator的性能严重依赖于彼此。如果不了解DAG的拓扑结构，动态调整所有运营商的资源分配以提高吞吐量或延迟并不是一个好的选择
  - operator的性能以不可预测的方式受到配置资源数量的影响。通过增加额外的执行者来增加资源，可能只会带来边际的绩效收益。资源配置和operator性能之间的关系是不一般的(例如，非线性和多模态)，并且**难以使用简单的良好形式的函数**来表征
  - 长时间运行的流处理应用程序不可避免地面临所提供**负载的逐渐漂移/意外变化**，这可能会威胁其稳定性。流处理系统必须通过适当增加或减少资源分配来应对外部冲击，以确保始终保持稳定。

- 提出了一种基于**在线优化**的弹性流处理动态资源分配策略Dragster，Dragster将流处理应用程序建模为DAG，并采用**两级在线优化框架**以**最大化吞吐量**
  - 首先，Dragster采用**在线优化算法**，根据其服务能力识别**瓶颈operator**
  - 其次，Dragster采用**贝叶斯优化**调整瓶颈operator的**资源配置**
  - 考虑每个operator包含缓冲区的情况，并制定长期约束来控制缓冲区大小

![image-20230308215041698](C:\Users\苏铄淼\Desktop\论文笔记\10-1.png)

模型建立：

基于DAG建模流处理应用程序

以Yahoo流媒体基准的数据流图为例，处理节点称为组件(component)，流处理系统包含三个组件：

- 源(source)：源从外部消息队列读取数据并向下游发送无限数量的元组(一种数据结构)
- 节点(operator)：节点可以消耗/使用来自其他组件的元组以执行一些处理，并发送新的流以进一步处理
- Sink：Sink 是 Flink 处理完 Source 后数据的输出，主要负责实时计算结果的输出和持久化。sink接收元组并将结果写入HDFS或Redis。

使用吞吐量作为评估度量，即一段时间内发出的元组数。在不失一般性的情况下，我们假设只有一个sink。sink的吞吐量就是应用程序吞吐量。如果应用程序中有多个sink，我们可以添加一个虚拟sink来解决此问题。

考虑1,...,N的N个sources和N+1到N+M的M个operators，每个operator的前身形成前身集，即Pi＝{p^j_i | p^j_i是operator i}的前身，而后继形成后继集，即Si＝{S^i_j | S^i_j是operator i的后继}.使用e^i_j来测量从算子i到算子j发出的吞吐量，设→e_j＝{e^i_j}_i表示由operator j接收的吞吐量向量。



两级在线优化问题：

如何识别瓶颈operator，调整服务容量yt跟踪最佳yt*，将调整服务容量所需的operator定义为瓶颈operator

如何调整瓶颈operator的资源配置，采用贝叶斯框架优化，每个operator都遵循独立的贝叶斯优化过程，operator ide服务容量yi是以从高斯过程采样的xi配置下得到的

![image-20230309150928765](C:\Users\苏铄淼\Desktop\论文笔记\10-2.png)

并且在配置xi后，**服务容量yi也无法被直接看到**

在给定配置下，运营商的吞吐量随着其CPU利用率和服务容量的上限而线性增加，以此估计yi(t)

![image-20230309151148447](C:\Users\苏铄淼\Desktop\论文笔记\10-3.png)

**观察到的容量ci(t)是受高斯噪声干扰的yi(t)的噪声样本**，即ci(t)=yi(t)+εi(t)，其中εi(t)～N(0，σ2)。基于历史观测，我们可以导出高斯过程的后验分布，以估计尚未观测到的配置的服务容量的均值和方差。之后，我们选择当前的时隙配置来跟踪目标容量yi(t)，并将其部署在流处理系统中。

以xi为参数的高斯过程采样得到服务容量yi是无法看到的，因此采用cit对yi进行估计，但是每个时刻的吞吐量是可以看到的，即ft(yt)



算法设计

![image-20230309160626660](C:\Users\苏铄淼\Desktop\论文笔记\10-7.png)

- Dragster通过在线鞍点算法确定当前时隙的**目标容量 target capacity yt**，拉格朗日对偶，这里的拉格朗日函数为什么是减号，通过这里确定瓶颈operator i

![image-20230309155309396](C:\Users\苏铄淼\Desktop\论文笔记\10-4.png)

- Dragster处理贝叶斯优化问题，通过**扩展高斯过程UCB算法**调整瓶颈operator的配置xi(t)，以实现目标服务能力yi(t)。文章对每个运营商的服务容量yi进行建模，**该服务容量yi是从高斯过程中采样的**。GP(μ(x), k(x, x ′))由其均值函数μ(x)=E[y(x)]和协方差(或核)函数k(x, x ′)=E[(y(x)-μ(x))(y(x ′)-μ(x ′)]指定。GP的一个主要优点是存在后验分布均值和协方差的解析公式。对于点At={x1，x2，··，xt}处的噪声样本yt={y1，y2，···，yt}，y上的后验仍然是GP分布，平均值ut(x)和方差σ2t(x)由下式给出：

  ![image-20230309160118841](C:\Users\苏铄淼\Desktop\论文笔记\10-5.png)

  定义x = {xi }i ，μt (x) = {μi t (xi )}i 和 σt (x) ={σ i t (xi )}i 。我们采用扩展的高斯过程UCB算法来调整当前的时隙配置，使其成为以下优化问题的最优

  ![image-20230309160410302](C:\Users\苏铄淼\Desktop\论文笔记\10-6.png)

  UCB权重βt提供了一个利用（即μt（x））和探索（即σt（x））之间的权衡。

![image-20230309160646467](C:\Users\苏铄淼\Desktop\论文笔记\10-8.png)

总结一下文章的思路，文章基于可观察到的初始输入x1、吞吐量hij函数以及应用程序的吞吐量进行在线优化问题的求解以及基于UCB算法得到每个时刻的配置xt。文章有意思的点在于服务容量yt(xt)是不可直接观察以及不可直接设置的，只有通过xt来调整yt使其接近我们的目标服务能力yt*，

- 文章首先通过拉格朗日对偶求得目标服务容量yt
- 其次通过高斯分布的后验知识，以噪声采样得到的yt（即cit）以及决策xt计算GP分布的均值函数与协方差函数
- 由上述计算得到的均值函数与协方差函数下，使用文章提出的改进的GP UCB算法计算得到下一决策xt
