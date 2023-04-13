# Ekya: Continuous Learning of Video Analytics Models on Edge Compute Servers

[paper](https://www.usenix.org/conference/nsdi22/presentation/bhardwaj), [persentation](https://www.youtube.com/watch?v=EM0nb-ewJPw)

论文主要解决问题：通过周期性再训练处理边缘端执行视频分析的数据漂移问题，同时解决了在边缘服务器上联合推理和再训练任务的挑战。

- 数据漂移（Data dfift）：边缘端所用的机器学习模型通常是轻量级的DNN模型（一般通过模型压缩获得），该模型在训练时所采用的训练样本通常是清晰且具有代表性的，而实际的视频分析任务摄像头所采集的数据可能由于照明、物体密度等影响使得样本分布特征与原始样本不一致，即数据漂移。
- 连续模型再训练（Continuous model retraining）：周期性对DNN进行再训练，两次再训练之间的时间间隔称为”再训练窗口“。
- 再训练与推理准确性权衡：在再训练期间，需要向推理工作中拿走部分GPU资源继而降低推理准确性，以此提升训练准确性。也可理解为实时推理精度与由于数据漂移导致的精度下降之间的权衡。
- Ekya设计了micro-profiler，通过几个epochs观察再训练串口的精确度，同是根据其资源消耗以估计再训练的资源需求。



问题陈述：为再训练考虑如下决策

- 在再训练窗口期间决定哪个边缘模型进行再训练
- 配置边缘服务器GPU以权衡再训练和推理任务
- 选择再训练和推理任务的配置
- 保证在做出每次决策后推理精度不会低于最小值，保证服务质量
- 优化目标为：最大化平均推理精度



决策组件：

资源调度器

- 优先考虑再训练性能变化最大模型，决定不再训练不能提高优化目标的模型
- 只考虑GPU的粗略分数以简化空间复杂性
- 在再训练期间不更改模型的配置

性能估计器：微剖析器 micro-profiler

- 只需训练数据钟少量的epoch就可以观察其准确性
- 仅限于一小部分“有前途的”再训练配置

As explained in ?2, the golden model cannot keep up with inference on the live videos and we use it to label only a small subset of the videos for retraining.



due to reasons of network cost and video privacy, it is preferred to run **both inference and retraining on the edge compute device itself** without relying on the cloud. In fact, with bandwidths typical in edge deployments, cloud-based solutions are slower and result in lower accuracies (§6.5).

## 连续学习

- 边缘DNN在随时间逐渐观察新样本的过程中连续学习
- Ekya使用改进的iCaRL学习算法（一种增量学习，可以系统增量学习新的类别）来搭载新的类别，以适应类别的变化
- 再训练的标记是从”黄金模型“中获得的，由于黄金模型所需要算力资源很大，其无法跟上实时视频的推理，故仅采用对它来标记再培训时间窗口的小部分视频
- 算法本质上是用高成本的teacher模型来监督学习低成本的student模型
- Ekya每次再训练采用再训练窗口累计的样本

## GPU调度

见论文Fig4

- 统一调度：再训练会从推理工作中窃取GPU资源，使得推理准确度下降，再训练结束后，由于留给窗口的时间太少，无法从再训练阶段获益（即较高的推理精度只维持很小一段时间）

- 精度优化调度：根据推理精度、再训练的资源需求以及再训练时间多维权衡，如调度器在窗口1中优先考虑B的再训练，因为再训练后其推理精度提高了35%（相比之下，视频A的推理精度为5%）。

这篇论文的连续学习是很有意思的一个部分，如何将训练和推理联合处理，如何建模是值得思考的一个问题。