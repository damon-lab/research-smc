# Online Learning

在线学习是机器学习中的一个系列，学习者尝试通过从一个序列的数据实例中逐个时隙的学习以解决一些预测性（或任何类型的决策）任务。

在线学习的目标，基于对以前预测/学习任务的学习任务的正确答案的了解，最大限度地提高在线学习者所作的预测/决策序列的准确性。

与批处理学习算法相比，在线学习是一种针对按顺序到达的数据的机器学习方法，学习者的目标是在每一步学习和更新未来数据的最佳预测器。

在线学习主要分为三大类

- 完整反馈的在线监督学习
- 有限反馈的在线学习
- 没有反馈的在线无监督学习



在线学习任务的目标是最小化在线学习者的预测对事后最佳固定模型的regret，定义如下
$$
R_T =\sum^T_{t=1} l_t(\mathbf{w}_t)-\min_\mathbf{w}\sum^T_{t=1}l_t(\mathbf{w})
$$
凸优化理论

- 一阶方法
- 二阶方法
- 正则化
  - Follow-the-Regularized-Leader (FTRL) 跟随正则化领导
  - Online Mirror Descent (OMD) 在线镜像梯度
  - Exponential Gradient (EG) 指数梯度
  - Adaptive (Sub)-Gradient Methods 自适应（次）梯度方法



在线凸优化

- 带有长期约束条件的在线凸优化
- 在线乘法交替方向法（Online Alternating Direction Method of Multipliers, ADMM），适合分布式优化应用
- RESCALED-EXP algorithm



博弈论

- K-Player Normal-Form Games
  - 纳什均衡：在纳什均衡中，每个玩家都能得到自己的最优策略，没有改变策略的动机

- Repeated Two-player Zero-Sum Games







# Introduction to Online Convex Optimization



## Expert Adivce

- 在每个时间间隙中进行选择A或B的决策
- 决策者有N个提供建议的专家
- 每次做出决策后，每个决策都会有一个损失反馈（定义正确选择为0，错误选择为1）

有以下基本情况：

- 若进行随机决策，则期望损失为 $T/2$



### weighted majority (WM) algorithm 加权多数算法

**![image-20230424205829278](C:\Users\苏铄淼\AppData\Roaming\Typora\typora-user-images\image-20230424205829278.png)**

由于该问题与算法与二元变量相关，不适用于我们用的边缘联合推理与训练模型

通过权值更新来学习专家的正确性



### Randomized weighted majority(RWM) 随机加权多数

与 weighted majority (WM) algorithm 算法类似

![image-20230424210552738](C:\Users\苏铄淼\AppData\Roaming\Typora\typora-user-images\image-20230424210552738.png)



### Hedge 

采用指数更新权值

![image-20230424211651068](C:\Users\苏铄淼\AppData\Roaming\Typora\typora-user-images\image-20230424211651068.png)



## 凸优化的基本概念

### 对凸集的投射

![image-20230424215609926](C:\Users\苏铄淼\AppData\Roaming\Typora\typora-user-images\image-20230424215609926.png)



## KKT条件

![image-20230424213023661](C:\Users\苏铄淼\AppData\Roaming\Typora\typora-user-images\image-20230424213023661.png)

### Gradient Descent 梯度下降

基于我们前面对模型的分析，将问题考虑成每个时隙的问题进行求解是不可行的

![image-20230424213215457](C:\Users\苏铄淼\AppData\Roaming\Typora\typora-user-images\image-20230424213215457.png)

#### The Polyak stepsize

![image-20230424214130150](C:\Users\苏铄淼\AppData\Roaming\Typora\typora-user-images\image-20230424214130150.png)

使得梯度系数是可变的而不是固定的



### Constrained Gradient/Subgradient Descent 带约束的梯度/次梯度下降



#### Basic gradient descent--linear convergence 基本梯度下降，线性收敛

![image-20230424214820072](C:\Users\苏铄淼\AppData\Roaming\Typora\typora-user-images\image-20230424214820072.png)



### Reductions to Non-smooth and Non-strongly Convex Functions 简化到非光滑和非强凸函数

#### Reduction to smooth, non strongly convex functions 简化为平滑的非强凸函数

![image-20230424221409567](C:\Users\苏铄淼\AppData\Roaming\Typora\typora-user-images\image-20230424221409567.png)

#### Reduction to strongly convex, non-smooth functions 减少到强凸的非光滑函数

![image-20230424221707488](C:\Users\苏铄淼\AppData\Roaming\Typora\typora-user-images\image-20230424221707488.png)



### Support Vector Machine Training

![image-20230425221525011](C:\Users\苏铄淼\AppData\Roaming\Typora\typora-user-images\image-20230425221525011.png)

![image-20230425222202420](C:\Users\苏铄淼\AppData\Roaming\Typora\typora-user-images\image-20230425222202420.png)



## First-Order Algorithms for Online Convex Optimization



### Online Gradient Descent

![image-20230425223131388](C:\Users\苏铄淼\AppData\Roaming\Typora\typora-user-images\image-20230425223131388.png)



### Stochastic Gradient Descent 随机梯度下降

![image-20230426112010183](C:\Users\苏铄淼\AppData\Roaming\Typora\typora-user-images\image-20230426112010183.png)





## Second-Order Methods



### Universal Portfolio Selection 通用投资组合选择



#### Mainstream portfolio theory 主流投资组合理论

主流金融理论将股票价格作为一个随机过程，称为几何布朗运动（GBM）

当前时刻的价格对数，表示为$l_{t+1}$，由 $t$ 时刻的价格对数与高斯随机变量的总和给出。

![image-20230426172950734](C:\Users\苏铄淼\AppData\Roaming\Typora\typora-user-images\image-20230426172950734.png)

![image-20230426173747503](C:\Users\苏铄淼\AppData\Roaming\Typora\typora-user-images\image-20230426173747503.png)

![image-20230426173730418](C:\Users\苏铄淼\AppData\Roaming\Typora\typora-user-images\image-20230426173730418.png)

![image-20230426173900905](C:\Users\苏铄淼\AppData\Roaming\Typora\typora-user-images\image-20230426173900905.png)

### The Online Newton Step Algorithm 在线牛顿下降

![image-20230426212041615](C:\Users\苏铄淼\AppData\Roaming\Typora\typora-user-images\image-20230426212041615.png)



## Regularization

Regularized Follow The Leader (RFTL)

以最小化regret为目标，**增加决策的稳定性，从而得到更低的regret**。下面将要提出的方法就是**正则化**，添加一个正则化函数，让决策者每一轮的最小化目标不仅仅是前面所有轮损失函数和，而是在此基础上增加了正则化项。

