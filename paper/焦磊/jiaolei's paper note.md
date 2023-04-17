**AI in 5G: The Case of Online Distributed Transfer Learning over Edge Networks(infocom 2022)**

------

MODELS AND PROBLEM FORMULATION

**A. System Models**

Edge Computing Networks:

Offline and Online Classifiers:

Data Samples: 

Distributed Transfer Learning: 

Control Decisions:

Cost of Transfer Learning:

Mistakes of Transfer Learning:

**B. Problem Formulation, Challenges, and Goal**

Problem Formulation: 

Challenges:

Goal:

------

scratch 划伤 

from scratch 从零开始

**leverage** 利用

polynomial 多项式的

exploit 利用

We design polynomial-time online algorithms by exploiting the real-time trade-off between preserving previous decisions and applying new decisions, based on primal-dual one-shot solutions for each single time slot.

While ==orchestrating model placement, data dispatching, and inference aggregation==, our approach produces new models ==via== combining the existing offline models and the online models being trained using weights adaptively updated based on inference upon data samples that dynamically arrive.

achieves a constant competitive ratio for the total cost. 实现了恒定比

non-trivial 非平凡的、重要的

extract 提取

potential-potentially 潜在的、潜在地

reside vi 居住、位于

dispatch vt 分派、派遣

We also consider that this edge environment provides a virtual machine (VM) or a container to host and run each offline or online classifier, as elaborated below.

The cloud maintains ==a set K of pre-trained "offline" classifiers== that are to be downloaded to the edges to serve users with ultra-low latency and used to train a single "online" classifier being updated continuously to accommodate any concept drift.

==Without loss of generality==, we assume qt m ∈ {−1, 1}, ∀m, ∀t. We ==emphasize== that qt m is only observable right after we ==conduct the inference（进行推理）== for m using our offline and online classifiers. We also note that any data sample m may arrive at one edge but be dispatched to a different edge to do the inference. We use dt mi to represent the delay between the edge where the data sample m arrives and the edge i at t.

At the time slot t, as ==the data sample m arrives at the system==, we design distributed transfer learning that works as follows, also shown in Fig. 2:

We use $x^t_{ki} ∈ \{1, 0\}$ to denote ==whether or not== the offline classifier k is downloaded from the cloud and hosted on the edge i at the time slot t.

**Cost of Transfer Learning:** ==The cost of distributed transfer learning at any individual time slot t consists of multiple components:==

==the performance overhead(性能开销)== incurred by running distributed transfer learning across edges, including the delay of dispatching data samples

Constraints (1c) and (1d) ==ensure== that the online classifier can ==only be hosted== by a single edge, and every offline classifier can only be hosted by a single edge.

While f k(pt m) can be observed as the offline classifiers are given and the data samples arrive, ==we need to determine how we should train or update the online classifier f t m(·) at $t$== in our algorithms in addition to accommodating the nonlinearity.

==The NP-hardness demands computationally efficient algorithms. It is not easy to achieve so in the offline setting, and it will be harder to do it in an online setting==

pursue 从事、继续、追赶、寻求

==pursuing the dynamic trade-off(寻求动态权衡)== between switching to a new decision and continuing to stay at the previous decision

accommodate vt容纳、顺应、向某人提供住所

conduct online training 进行在线训练

We have **rigorously** proved that 严格证明



**Preemptive Scheduling for Distributed Machine Learning Jobs in Edge-Cloud Networks(jsac22)**

------

SYSTEM MODEL

**A. System Overview**

Edge-Cloud System.

ML Jobs.

Decision Variables.

**B. Problem Formulation**

------

preemptive learning 抢占式学习

specifically 具体而言

As shown in Fig. 1, we consider an edge-cloud system managed by a resource operator, who owns a number of heterogeneous edge servers and a remote cloud. ==These servers are denoted as a set [S].==

be regarded as 被视为

An binary variable $v_{jsw} (ojsp)$ indicates whether a given worker $w (PS p)$ on server s can serve job j or not ('0': no; '1': yes).

A set of J ML jobs arrives at organizations online with large input data ==over a large time span [T ] = {1, 2, ..., T }==. 

==An binary variable $v_{jsw} (ojsp)$== indicates whether a given workerw (PS p) on server s can serve job j or not ('0': no; '1': yes).

Let nj be the processing capacity of job j, i.e., the number of mini-batches that can be trained by a qualified worker in one time slot.

As a result, the communication time between workers and PSs is computed as follows.

We use $\frac{qj}{bj}$ to denote the time of sending gradients or receiving updated parameters, where qj is the size of gradients /parameters and bj denotes the reserved bandwidth.

==Upon arrival of an ML job j==, the following decisions are made: yjsw (t) ∈ {0, 1} (zjsp(t) ∈ {0, 1}), which indicates whether worker w (PS p) of server s is scheduling job j in time t or not.

Important notations are listed in Table I.

一些同义词汇：imply、guarantee、limit、ensure、assure



**Online Edge Computing Demand Response via Deadline-Aware V2G Discharging Auctions**

------

SYSTEM MODEL AND PROBLEM FORMULATION

**A. System Modeling**

Edge System:

EV Discharging:

Auction with Deadlines:

Control Decisions:

Cost of EVs:

Cost of Edges:

**B. Problem Formulation and Algorithmic Challenges**

Social Cost Minimization:

Algorithmic Challenges:

Algorithmic Goal:

------

EVs: electrical vehicles 电动汽车

V2G: vehicle-to-grid 车辆到电网

Electrical Vehicles (EVs) and Vehicle-to-Grid (V2G)

auction 拍卖

bid 出价、投标

mechanism 机制

incentivize 激励

tricky 棘手的

ideal 理想的、完美的

agile-agility 敏捷、灵活

confront 遭遇、面对

confront difficult challenges 面临困难的挑战

intrinsic-intrinsically 从本质上讲

joint 联合的、共同的

make joint control decisions 做出联合控制决策

irrevocable 不可改变地

on the fly 及时

- 边缘系统作为拍卖者（auctioneer、buyer），电动汽车作为出价者（bidder）