# RECL: Responsive Resource-Efficient Continuous Learning for Video Analytics

2023 NSDI

当前的连续学习的方法存在以下问题：

- 依赖周期性的再训练并且延迟高、计算成本大
- 依赖于选择历史模型并且由于没有利用持续再训练学习的潜力造成精确度损失

RECl是一个新的视频分析框架，他集成了模型复用和在线模型再训练，使其能够在给定任何视频帧样本的情况下快速调整专家模型。

- 在边缘设备间共享一个'model zoo'，包括先前针对所有边缘设备训练的历史专家模型，并且能够跨视频会话重用历史模型
- 使用快速程序从该共享'model zoo'在线选择高度准确的专家模型
- 动态优化‘model zoo’模型再训练、模型选择和及时更新之间的GPU分配



连续学习和部署专门的DNN有两个基本限制：

- 花费较多的计算资源
- 再训练专门的DNN需要花费时间，并且再此期间会有准确度的急剧下降



文章提出的方法继续采用历史专门DNN的复用：

- 视频流通常表现时空相关性，当前的视频片段可能于历史视频片段有一些相似之处，可被历史专门DNN复用



利用模型复用对视频分析的技术挑战：

- 快速且准确地找到适用当前视频片段地专用DNN，很难比较高维和非结构化数据（视频片段）的相似性，并且与所有历史视频片段进行比较是不可行的
- 保持模型复用的成本远低于模型再训练的成本



同一地理区域附近的视频场景是相似的，在该场景下的其他设备也会看到相似的场景，可以潜在得利用这些历史专家模型来最小化再训练的必要性

Network-Aware Optimization of Distributed Learning for Fog Computing