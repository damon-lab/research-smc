# CoolEdge: Hotspot-Relievable Warm Water Cooling for Energy-Efficient Edge Datacenters

ASPLOS 2022

论文通过提出CoolEdge--一种可调节热点的温水冷却系统以解决温水冷却中边缘数据中心的服务器、硬件存在的热不平衡问题，以提高边缘数据中心的冷却效率和节约成本。具体地，通过对水循环的精心设计，

- CoolEdge可以**动态地调整每个异构硬件组建的水温和流速**，以消除**硬件级热点(hardware-level hotspots)**。
- 通过重新设计cold plates，可以快速分散**芯片级热点(chip-level hotspots)**，无需人工干预。
- 从理论上量化了温水冷却实现的功率节省，并提出了一个定制设计的冷却解决方案，以定期决定适当的水温和流量。

Gartner predicts around **75% of enterprise-generated data will be created and processed at the edge by 2025, though the value is only 10% in 2018** [30], bringing explosive growth in the number of edge datacenters.

Although the power rating of an edge datacenter i**s only 10’s to 100’s of kW** that is three orders of magnitude smaller than a cloud datacenter, such a growing number of edge datacenters will inevitably bring heavy energy burden. **By 2028, the energy demand of edge datacenters will reach the same order of magnitude as that of the global datacenters in 2020** [42, 51].

腾讯云开放第一个边缘数据中心，以提供视频处理、云游戏、智能医疗等实时服务。为了适应多样化应用，如深度学习推理，严重依赖加速器，如图形处理单元（GPU），张量处理单元，现场可编程门阵列，智能网络接口卡等，边缘服务器需要接各种各样的异构硬件，使其称为高功率配置。

- 自由冷却技术条件苛刻，不适用与边缘场景
- 随着功率密度的急剧增长，空气冷却将不再适合边缘数据中心，水冷是一种有前途的技术

研究发现，提高水温可以减少40%的冷却成本，与电价之间的trade off，而且对水温的降低要求不高使得冷水机的尺寸和冷却能力都可以较小。

对于多层面的严重热点(hotspots)问题，粗粒度的温水冷却对于边缘数据中心来说是低效的

- 不平衡的硬件利用率以及异构硬件的不同热规格导致了服务器和硬件之间的热不平衡。为了冷却一部分硬件组件，水温需要同步冷却到一个温度是低效的。可以通过分布式的冷水机来解决但是这是高成本的、也有技术难度。
- 热不平衡也存在与硬件组成内部，CPU核心内部温度和CPU内部温差都很大。局部热点不处理会导致性能下降和寿命减短，还会因为过度冷却其他非热点单位而增加冷却成本——分散多级热点，就能在保证安全的同时进一步提高冷却效率。

本文的贡献：提出CoolEdge，一个细粒度的温水冷却系统，用于缓解高密度和异构边缘数据中心的多级热点并节省冷却能源

- 提出热点可控的温水冷却系统
  - 精心设计的水循环下进行精细的冷却控制，可以减小硬件级别的热点
  - 通过新开的蒸气室的冷板，可以分散芯片级热点，无需人工干预
- 第一个从理论上量化通过温水冷却实现的节能，在量化的基础上，提出了一个定制设计的冷却解决方案，使用面向散热（HDO）或面向冷却器功率（CPO）的策略，为异质硬件提供冷却设置适应性，同时提高冷却效率。
- 建立了硬件原型验证CoolEdge的实用性并进行数据中心的模拟

边缘数据中心的三个要求分析他们的低效率：接近终端用户、高密度、异质性

现有的冷却技术：

- 自由冷却与边缘数据中心靠近终端用户的需求相悖
  - 自由冷却是将服务器部署在寒冷干燥的地方，冰岛是最具成本效益的数据中心目的地之一。微软将数据中心建在海底。
  - 边缘数据中心应该建立在靠近终端用户的地方

- 空气冷却与服务器高密度的矛盾
  - 边缘数据中心日益增长需求与城市土地短缺的矛盾，增加了冷却需求
  - 空气冷却难以满足严格冷却需求，先进的高密度冷却在效率、可扩展性或边缘数据中心的适应性方面表现不佳

- 水冷与热点不均衡的矛盾
  - 水冷可以支持更高功率密度的热容量，但是粗粒度的水冷冷却效率低

边缘数据中心的热点问题：

与云数据中心相比，由于高密度和异质性的要求以及边缘端硬件利用率偏低和环境条件多变，其热点问题相较严重。边缘数据中心存在两种热不平衡：硬件层面和芯片层面

- 硬件层面的热点问题
  - 异构硬件的热平衡更加显著（CPU、GPU、DRAM）