# Budget-Aware Online Control of Edge Federated Learning on Streaming Data With Stochastic Inputs

在边缘网络中连续执行联合学习，同时训练数据动态地、不可预测地流向设备，面临着关键的挑战，包括全局模型收敛、长期资源预算以及不确定的随机网络和执行环境。

我们制定了一个整数程序来捕捉所有这些挑战，使设备上的流学习和设备与边缘服务器之间的联合学习的累积总延迟最小。然后，我们对问题进行解耦，设计了一种在线学习算法，通过凸凹重构和矫正梯度白炽化步骤来控制本地模型更新的数量，并设计了一种匪徒学习算法，通过纳入预算信息来选择边缘服务器进行全局模型聚合，以达到利用-探索的平衡。我们严格地证明了关于优化目标的亚线性遗憾和关于最大设备负载的亚线性约束违反，同时保证了全局模型训练的收敛性。用真实世界的训练数据和输入痕迹进行的广泛评估证实了我们的方法在经验上优于多种最先进的算法。

联邦学习通过迭代更新本地设备上的本地模型来训练机器学习模型，并将来自多个设备的本地模型汇总到远程服务器上组成全球模型。这种模式将训练数据保存在每个本地设备中，不上传到服务器上[1]，从而保护用户的隐私[2]。然而，联合学习往往需要事先准备好所有的数据样本，然后开始训练过程，在训练时间段内会不断产生沉重的工作负荷（即高CPU负荷），造成额外的等待时间，即使有复用功能也会影响设备的使用。

- 联邦学习通过在本地进行模型训练，不需要将训练数据上传到服务器以保护用户隐私，但其训练过程间会产生高额的工作负荷（即CPU负荷），这可能会造成额外的等待时间，即负载拥堵。

避免本地设备出现这种 "负载拥堵 "的一种方法是将训练负载分散在一段时间内，也就是说，在训练数据逐渐动态地到达每个本地设备时进行联合学习。这样一来，联合学习只是与其他应用一起运行，因此不会对这些应用的正常使用造成损害。不幸的是，这种在流媒体数据上进行联合学习的设想是不可行的，特别是在边缘网络中，我们需要控制每个单一设备上的流媒体学习和动态网络环境中的边缘服务器选择，如图1所示。事实上，我们面临着如下的多重挑战。

- 可以通过以下思想解决负载拥堵问题：将训练负载分散在一段时间内，使训练负载动态到达本地设备进行联邦学习，将数据考虑为流媒体的形式，但存在需要在边缘网络中控制每个设备上的流媒体学习和动态网络环境中的边缘服务器选择的困难

首先，在联合学习的框架下，从流式数据中学习，而不是从静态数据中学习，使全局模型的训练变得复杂。随机梯度下降（SGD）的方法可以用来动态处理流媒体数据，但它往往很难收敛，因为SGD的每一次迭代都不能减少总损失。因此，希望有一种更好的方法来对流媒体数据进行设备上的训练，这种方法应该消耗更少的计算，避免持续的高负载，更重要的是，还希望明确地保证全局模型的收敛。

- 在联邦学习中考虑从流式数据学习而不是从静态数据学习使全局模型的训练变得复杂，需要保证保证全局模型的收敛
- 需要明智的决策俩考虑租用和使用边缘资源进行联邦学习的长期预算
- 边缘网络和执行环境可能具有内在的不确定性和随机性[5]，增加了以在线方式控制联合学习的难度。全局聚合的服务器，其传输模型的网络延迟和执行聚合的执行延迟都会随着时间的推移而变化。

建立模型：

在长期范围内对控制流数据的联邦学习的目标场景进行建模，模型考虑了整个时间范围内设备上流学习中本地模型更新的计算和通信以及联合学习中边缘服务器上的全局聚合所产生的整体延迟。模型具有边缘资源限制、全局模型收敛以及不可预测的数据到达、动态资源价格以及异质和随机的边缘性能下的长期预算。