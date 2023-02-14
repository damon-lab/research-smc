# Accuracy-Guaranteed Collaborative DNN Inference in Industrial IoT via Deep Reinforcement Learning

研究IoT网络中的协作DNN推理问题，并将问题表述为约束马尔可夫决策过程（CMDP）

- 联合考虑采样自适应、推理任务卸载和边缘计算资源分配，最小化平均服务延迟的同时保证不同推理服务的长期精度要求
- 利用Lyapunov优化技术将CMDP转换为MDP（无约束），构造精度缺失队列以表征长期精度约束的满足状态
- 提出一种基于深度强化学习（RL）的MDP算法
- 在MDP算法中嵌入了优化子程序以加快训练过程，直接获得最佳的边缘计算资源分配

