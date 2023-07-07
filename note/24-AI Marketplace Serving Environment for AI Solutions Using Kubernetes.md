# AI Marketplace Serving Environment for AI Solutions Using Kubernetes 

## 研究背景

人工智能任务在实践中的应用仍然落后，其中一个主要原因是行业专家对可用的人工智能应用领域缺乏了解。人工智能市场通过为行业专家和人工智能开发者之间的合作提供平台来解决这个问题。该平台的一个基本功能是提供一个服务环境，允许人工智能开发人员向行业专家展示他们的解决方案。

解决方案以统一的方式打包，所有平台成员都可以通过服务环境访问这些解决方案。在本文中，我们介绍了该环境的概念设计，以及使用Amazon Web Services的实现，并通过两个示例用例说明其应用程序。



## 研究内容

Kubernetes事件驱动的自动扩展(KEDA)(Microsoft and Red Hat, 2019)扩展了触发pod自动扩展的条件。

默认情况下，Kubernetes Horizontal Pod Autoscaler (HPA)只能**根据内存或CPU消耗**来扩展Pod。因此，如果没有KEDA这样的扩展，**基于流量的扩展是不可能的**。此外，HPA不能缩小到zero-pods。因此，即使可能不需要，pod的至少一个副本也必须始终保持活动状态。KEDA提供了从零到零的扩展，也提供了从零到最大成本效率的扩展。

traffic 流量

基于流量的扩缩容是通过使Ingress Controller将与请求相关的事件存储到Prometheus来实现的。随后，每个对托管AI解决方案的子域名的请求都会带有时间戳一起存储。通过这些指标，KEDA可以查询并决定扩缩容的相关策略。例如，过去30分钟内没有接收到任何请求的每个AI pod都会被移除。由于每个节点只托管相关的AI pod，因此所有的pod都会被终止，随后集群自动扩缩容器(Cluster Autoscaler)也会将节点移除，以优化成本。另一方面，KEDA检测到有关已终止pod的请求，会相应地重新调度它们。



