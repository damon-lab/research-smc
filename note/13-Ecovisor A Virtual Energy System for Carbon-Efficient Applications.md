# Ecovisor: A Virtual Energy System for Carbon-Efficient Applications

云平台的快速增长正在引起人们对其碳排放的极大关注。为了减少碳排放，未来的云平台将需要增加对可再生能源的依赖，**如太阳能和风能，这些能源的排放为零，但非常不可靠**。

不幸的是，今天的能源系统在硬件上有效地**掩盖了这种不可靠**，这使得应用程序**无法优化其碳效率**，或每公斤碳排放的工作。

为了解决这个问题，我们设计了一个 "Ecovisor"，它将**能源系统虚拟化**，并将**软件定义的控制暴露给应用程序**。生态管理程序使每个应用程序能够根据自己的具体要求，在软件中处理清洁能源的不可靠。我们实现了一个小规模的Ecovisor原型，它**虚拟了一个物理能源系统**，以实现基于**软件**的应用层面：i）对**可变电网碳强度和本地可再生能源发电的可见性**；ii）**对服务器电力使用和电池充放电的控制**。我们通过展示多个应用程序如何以不同的方式同时行使他们的虚拟能源系统来评估ecovisor方法，以便与一般的全系统政策相比，根据他们的具体要求更好地优化碳效率。

随着云平台的发展，为了缓解其能源消耗和成本的大幅增长，**云平台积极优化其能源效率，例如，通过降低其能源使用效率**（PUE）--数据中心总功率与服务器功率的比率--接近1的最佳值[11, 18]。

减少云平台的碳排放将要求他们**使用更清洁的 "低碳 "能源为其云和边缘数据中心供电**。

**清洁能源的一个显著特点是其不可靠**：它是**间歇性的**，不是在任何一个地方都能无限量地使用。值得注意的是，**清洁能源的不可靠在我们目前的能源系统中表现为两种不同的方式：i）可再生能源发电的不可靠；ii）电网电力的碳密度的不稳定。**

也就是说，能源系统在传统上隐含了一个可靠性的抽象概念--按需求提供可靠的电力，达到某种最大值的抽象概念--通过其电气插座接口暴露给电气设备，包括服务器。当然，在许多情况下，**现在的能源系统不仅包括与电网的连接，还包括日益丰富的本地能源系统，其中可能包括大量的能源储存，如电池[56]，以及共同定位的可再生能源，如风能和太阳能[44]**。由于能源系统将其日益增长的复杂性隐藏在可靠性的抽象背后，它们为应用程序提供了对其能源供应特性的控制或可见性，即其消费、发电或碳排放。因此，应用程序不能通过调节其电力使用来优化碳效率，以应对电网电力的碳强度和可再生能源的可用性的变化。

我们的假设是，与一般的系统政策相比，**暴露出软件定义的可见性和对虚拟化能源系统的控制**，使应用程序能够根据其具体特点和要求更好地优化碳效率。在评估我们的假设时，本文有以下贡献：

**虚拟化的能源系统**。我们介绍了我们的ecovisor设计，它将**物理能源系统虚拟化，以实现对服务器功耗和电池充电/放电的软件控制，以及对可变电网碳强度和可再生能源发电的可视性**。特别是，我们的ecovisor**为应用程序提供了一个软件API，使它们能够控制它们的电力使用**，以应对电网电力的碳强度和可再生能源的可用性的不可控变化。

**碳效率优化**。我们提出了多个案例研究，显示一系列不同的应用程序**如何使用ecovisor API来优化其碳效率**。我们的案例研究强调了两个重要的概念，包括：i）不同的应用以不同的方式使用他们的虚拟能源系统来优化碳效率，以及ii）与一般的一刀切的系统政策相比，特定应用的政策可以更好地优化碳效率。虽然优化能源效率在计算机领域得到了很好的研究，但对优化碳效率的研究却很少，而碳效率既是根本上的不同，也是对解决气候变化真正重要的唯一指标。

实施和评估。我们在一个微型服务器集群上实现了一个小规模的ecovisor原型，**它向应用程序暴露了一个虚拟电网连接、太阳能阵列和电池**。我们通过同时执行上述案例研究的应用来评估我们原型的灵活性，并表明在一个共享的基础设施上优化其碳效率需要特定的应用政策。例如，一个交互式的网络服务可以使用碳预算来维持一个严格的延迟SLO，因为碳强度的变化，而一个平行的批处理工作可能会调整其平行度。我们将ecovisor原型作为一个开源工具发布，研究人员和从业人员可以在开发碳优化时使用：github.com/carbonfirst/ecovisor

由于修改云平台的操作以适应碳强度的变化，例如在碳强度高时节流工作负载，是具有挑战性的，所以云供应商主要集中在**利用碳抵消来透明地减少其净碳排放**。这种抵消是一种会计机制，可以通过购买另一时间和地点产生的零碳可再生能源来抵消碳密集型能源的直接使用[43, 58]。碳抵消是有吸引力的，因为它们不需要复杂的操作变化来减少净碳排放。许多著名的技术公司已经消除了他们的净碳排放[1, 31, 54, 65]，他们经常提到用 "100%可再生能源 "运行。"不幸的是，**碳抵消并不能减少直接的碳排放，而且随着碳排放的减少，效果越来越差，因为剩下的碳越来越少，可以抵消**。相比之下，消除绝对碳排放最终将要求云平台改变其运作方式，通过将其计算负荷与可获得低碳能源的时间和地点更好地结合起来，减少其直接碳排放。



图2提供了我们ecovisor的总体设计概况，它**使用容器或虚拟机（VM）作为资源分配和能源管理的基本单位**。我们选择了一个容器/虚拟机实例级的API，部分原因是它与现有的实例级云计算API相一致，并可以轻松地扩展。正如我们所讨论的，实例级API也可以支持更高级别的集群或云端级API，为特定类型的应用（如地理分布的应用）提供简化的抽象概念。

ecovisor集成并扩展了现有的协调平台，**该平台已经提供了基本的容器（或虚拟机）管理和监控功能，包括创建和销毁容器（或虚拟机），以及为它们分配资源**。请注意，**容器协调平台**（COPs），如Kubernetes和Mesos，以及类似的虚拟机协调平台，通常不提供复杂的细粒度能源监控和管理功能。如第4节所述，我们的实现特别建立在LXD[50]的基础上，它是一个简单的COP，通过REST API暴露基本的容器管理功能，类似于Kubernetes和Mesos。我们选择为我们的原型扩展一个COP，因为这些平台已经成为统一管理大型服务器集群资源的事实上的操作系统。

然而，虽然我们将讨论的重点放在COP上，但我们的设计也适用于协调虚拟机的类似平台。

COPs为分布式应用提供了自己的虚拟集群的抽象，该集群由多个容器组成，每个容器都有指定的资源分配。这些虚拟集群是有弹性的，**比如容器的数量和每个容器分配的资源可以根据应用需求和资源的可用性随时间增长或缩小**。特别是，**应用程序可以随着需求的变化水平地扩展其分配的容器数量，或者垂直地扩展分配给每个容器的资源。**

COP包括一个调度策略，决定如何在约束条件下向应用程序分配资源。有许多可能的资源调度政策，针对不同的目标进行优化，如公平性，如主导资源公平性[32]，或收入，如云现货市场[2，5]。这些政策可能要求调度员从分布式应用中回收（或撤销）资源。

因此，运行在COP上的分布式应用已经被设计为对资源撤销有弹性。正如我们所讨论的，这种弹性对于设计碳效率的应用也是有用的，因为低碳能源的不可靠可能会导致电力短缺，这也表现为资源撤销。



面临的挑战：

由于修改云平台的操作以适应碳强度的变化是具有挑战性的，例如在碳强度高时节流工作负载，所以云供应商主要集中在利用碳抵消来减少其净碳排放。

减少直接碳排放具有挑战性，主要是因为它引入了一个新的制约因素，要求**用户自愿在性能/可用性、成本和碳排放之间做出艰难的权衡**。

但更普遍的是，云用户，即公司，在减少碳排放（以增加成本和降低性能/可用性为代价）方面有广泛不同的目标、策略和容忍度，而云平台并不了解这些。因此，**云平台并不能很好地代表他们的用户在系统层面上管理碳排放，这就促使我们的ecovisor将能源和碳管理暴露给应用程序**。

Ecovisor对碳的应用级控制的动机类似于云的自动扩展：所有的云平台都支持弹性自动扩展，使应用程序能够水平或垂直地扩展其资源，以响应其工作负载强度的变化[3, 14]。这些自动扩展策略是针对具体应用的，原因与上述类似，**即不同的应用需求和用户在成本和性能/可用性之间的权衡**。我们的ecovisor的API（在第3节中讨论）可以实现类似的 "自动扩展"，但要对电网电力的碳密度和当地可再生能源的可用性的变化做出反应。使用ecovisor实现这种 "碳扩展 "的一个简单的进化路径是增强现有的云计算自动扩展API。例如，现有的API，如Amazon CloudWatch[37]和Azure Monitor[4]，已经揭示了平台资源使用的可见性，可以很容易地扩展到包括电力和碳信息。

在这种情况下，**云平台将 "委托 "碳扩展给应用程序，就像他们目前委托自动扩展资源一样**。

虽然ecovisor方法可以适用于现有的云平台，特别是那些托管在有大量同地可再生能源[44]和能源储存[59]的数据中心的云平台，但**目前还没有减少碳的经济激励措施**。这是一个社会问题，而不是一个技术问题。**最终，为了阻止气候变化，政府政策可能有必要为监测和减少碳排放创造强有力的激励措施，可以是直接的，如通过碳上限，也可以是间接的，如通过碳定价或其他激励**。

尽管如此，云平台已经开始公开其碳排放的可见性[10]，这是**因为其客户越来越希望测量和报告碳排放数据**。这种客户需求和政府政策的结合，可能会激励未来的云平台采用类似ecovisor的机制来测量和控制碳排放。



Ecovisor架构设计：

假设数据中心的物理能源系统连接到三个不同的电源：电网、本地电池（或其他形式的能源存储）和本地可再生能源发电，如太阳能或风能，ecovisor需要对服务器电源和物理能源系统进行**软件定义的监测和控制**，即电源的供应、需求和碳排放。

- 监测电力。Ecovisor必须能够监测每个能源的发电量和消耗量。能源系统组件通常通过程序化的API暴露出电力监测
- 监测碳。Ecovisor必须能够实时监测电网电力的碳强度，如electricityMap
- 控制电力。Ecovisor必须能够根据电网电力的碳密度和可再生能源的可用性的变化来控制电力的使用，包括调节服务器的电力消耗和电池的充电/放电，即通过软件来限制电池的最大放电功率，并调节何时和多少从电网和可再生能源为电池充电。

Ecovisor使用**容器或虚拟机（VM）**作为资源分配和能源管理的基本单位。它集成并扩展了现有的协调平台，该平台已经提供了基本的容器（或虚拟机）管理和监控功能，包括创建和销毁容器（或虚拟机），以及为它们分配资源。

Container Orchestration Platforms (COPs) 容器协调平台：如Kubernetes和Mesos，但是通过LXD（一个简单的COP），可以实现上述功能。

