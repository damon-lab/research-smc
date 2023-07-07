# Triggerflow: Trigger-based Orchestration of Serverless Workflow

FGCS [Future Generation Computer Systems](https://dl.acm.org/journal/fgcs) 2021

**研究背景**

当前无服务器应用主要专注与短时间运行的工作流和同步大规模并行作业，并且带来相当大的开销。它们都不支持可扩展拦截和自定义工作量优化的开放系统。为此文章提出了Triggerflow：一个可扩展的基于触发器的业务流程架构，建立在Knative event和Kubernetes技术之上的无服务器工作流。



**研究内容**

由于其简单性、计费模型和固有的弹性，无服务器功能即服务(FaaS)正在成为云计算中非常流行的编程模型。FaaS编程模型被认为是基于事件的，因为函数被激活(触发)以响应特定的Cloud Events（如Amazon S3这样的分解对象存储中的状态更改）。FaaS模型也被证明非常适合PyWren， ExCamera执行令人尴尬的并行计算任务。但是PyWren和ExCamera都需要它们自己的临时外部编排服务来同步函数的并行执行。Lambda创建者Tim Wagner最近概述了[24]云提供商必须为应用程序提供新的无服务器构建块。他预测了一些新的服务，如细粒度、低延迟的编排、执行数据流，以及大规模定制代码和数据的能力，以支持基于无服务器功能的新兴数据密集型应用程序。

现实情况是，现有的无服务器编排系统不是为长时间运行的数据分析任务而设计的。

- 短期运行的高交互性工作流
- 同步大规模并行作业

文章提出了Triggerflow，一个用于组合基于事件的服务的新型构建块。随着越来越多的应用程序被转移到云端，这种服务将能够以一种**反应式和可扩展**的方式控制这些**应用程序的生命周期**。该系统的灵活性也可用于透明地优化对事件反应的任务的执行。

- 提出了一个Rich Trigger framework，它遵循Event-Condition-Action（ECA，事件-条件-动作）架构，在所有层面上都是可扩展的（事件源和可编程的条件和行动）。该架构确保复合事件检测和事件路由机制是由基于反应式事件的中间件来调解的。

- 展示了 Triggerflow 的可扩展性和通用性，在其之上创建状态机工作流调度程序、DAG 引擎、命令式工作流即代码（使用事件源）调度程序，以及与 PyWren 等外部调度程序的集成。
- 提出了模型在标准 CNCF 云技术（如 Kubernetes、Knative Eventing 和 CloudEvents）上的通用实现，验证了系统可以支持大量事件处理工作负载、按需自动扩展并透明地优化科学工作流程。



**研究方法**

作者开发了两种不同的Triggerflow实现

- 基于Knative，遵循基于推送的机制将事件从事件源传递给适当的工作人员

- 使用 Kubernetes 事件驱动的自动缩放（KEDA），worker跟随一种基于拉取的机制，用于直接从事件源检索事件

作者在 IBM Cloud 基础架构之上创建了原型，利用其目录中的服务来部署我们架构的不同组件：

- 前端 RESTful API，用户可以在其中连接以与 Triggerflow 交互
- 一个数据库，负责存储工作流信息，例如触发器、上下文等
- 控制器，负责在Kubernetes 中创建工作流工作者
- Workflow workers (TF-Worker hereafter)，负责通过检查触发器的条件和应用操作来处理事件

每个工作流都有自己的 TF-Worker，系统的可扩展性是在工作流级别而不是 TF-Worker 级别提供的。在验证（第 6 节）中，我们演示了每个 TF-Worker 如何提供足够的事件摄取率来每秒处理大量事件。

在 Triggerflow 系统中，事件按逻辑分组在工作流中。工作流抽象很有用，例如，区分和隔离来自多个工作流的事件，允许在（相关）事件之间共享公共上下文。



**在KEDA上的部署**

事件驱动的应用的难点在于**处理可靠性和可扩展性**。事件系统可能在事件创建后立即接收（"pushed"），也可能在事件准备好后再处理（"pull"或 "poll"），对于这两种情况，都需要处理**容量限制和错误处理**。

![image-20230621111019007](C:\Users\苏铄淼\AppData\Roaming\Typora\typora-user-images\image-20230621111019007.png)

KEDA 提供基于pull的可配置事件队列监控和 Kubernetes 容器的反应式可扩展实例化。 KEDA 还提供可配置的自动缩放机制，以放大或缩小到零。

在这种情况下，Triggerflow Controller 集成了 KEDA 以监视事件源并启动适当的 TFWorker，并在必要时将它们缩放为零。不同类型的工作流可能需要不同的配置参数。

这里的优势在于，与 Knative Eventing 不同，TF-Workers 使用代理的本机协议直接连接到消息代理（Kafka、Redis Streams），**这允许在单个 pod 中每秒处理更多事件**。

图 2 为 KEDA 实施的高级透视图。在此部署中，Triggerflow 的工作方式如下：

通过客户端，用户必须首先向 Triggerflow 注册表创建一个空工作流，并引用该工作流将使用的事件源。然后，用户可以开始向其中添加触发器 (1)。所有信息都保存在数据库中（例如 Redis）(2)。然后，在创建工作流后，前端 API 立即与 Triggerflow 控制器通信 (3)，在 Kubernetes 中部署为单个无状态 pod 容器（服务），以在 KEDA 中创建可自动扩展的 TF-Worker (4)。从这一刻起，KEDA 负责扩大和缩小 TF-Workers (5)。在 KEDA 中，如上所述，TF-Worker 负责直接与事件源 (6) 通信以拉取传入事件。最后，TF-Workers 定期与数据库交互 (7) 以保持可用触发器的本地缓存更新，并存储上下文（检查点）以实现容错目的。



**研究结论**

