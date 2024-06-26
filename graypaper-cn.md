<h1 align="center">JAM</h1>

<h5 align="center">JOIN-ACCUMULATE MACHINE: A SEMI-COHERENT SCALABLE TRUSTLESS VM</h5>
<h5 align="center">聚合-累计 机器: 一个半相干的可以扩展的无需信任的虚拟机 </h5>
<h5 align="center"> 译者: haordggg </h5>
<h5 align="center"> 时间: 2024-5-15 </h5>
<h6 align="center">( JOIN: 表示多个实体，可能是不同种类的，不同用途的实体一起来使用。)</h6>
<h6 align="center">( ACCUMULATE： 表示这些实体的使用该机器的结果，是依赖于前置的，历史的，类似于状态机，当前结果是依赖于前一个结果。)</h6>
<h6 align="center">( semi-coherent: semi一半，coherent一致，相关，相干。也许是不一定需要强一致性，类似于一个区块，只有唯一的历史结果，只有一个世界树，这个也许可以包含多个历史结果，类似平行宇宙。 )</h6>
<h6 align="center"> ( 为什么是半一致性呢，或者半相干了。我们可以用以太坊，或者现在的polkadot中继链作为例子，在他们中，我们全网的节点，会对一个数据，形成一个相同的共识，这样会导致全网，比如1000个节点的计算量等于一台计算机，因为全网在做同一件事情，这样是无法扩展的，并且并行的。jam 是不会保持这样子的，Jam的目标是实现现有eth/dot链计算的300倍，jam 选择放弃了全网节点同时做一件事，而是让全网节点分为不同角色运行不同的事情，分工合作，通过经济博弈保护安全，从而实现高扩展性 ) </h6>
**Abstract.** We present a comprehensive and formal definition of Jam, a protocol combining elements of both Polkadot and Ethereum. In a single coherent model, Jam provides a global singleton permissionless object environment—much like the smart-contract environment pioneered by Ethereum—paired with secure sideband computation parallelized over a scalable node network, a proposition pioneered by Polkadot.
<h6>摘要。我们提出了全面，并且正式的JAM定义，即一种同时结合了ethereum和polkadot两种的元素的协议。JAM既提供了类似于以太坊首创的全局单例无许可对象智能合约环境，又结合了 Polkadot 开创的、可安全地在可扩展节点网络上并行处理的侧带计算功能。</h6>
<h6>( sideband:边带。 一般而言，术语“边带”用于指代用于传输附加信息的次要或辅助信道。这可以是在无线电通信、计算或其他领域的背景下。在这里指的可能是polkadot中继-平行链模型，中继通过hrmp或者xcmp 处理平行链的信息 )</h6>
Jam introduces a decentralized hybrid system offering smart-contract functionality structured around a secure and scalable in-core/on-chain dualism. While the smart-contract functionality implies some similarities with Ethereum’s paradigm, the overall model of the service offered is driven largely by underlying architecture of Polkadot.
<h6>JAM 提出了一种去中心化混合系统，拥有围绕在安全和可扩展性 "核心内"/"在链上" 的二元性的智能合约功能。虽然智能合约功能与以太坊的范例有一些相似之处，但所提供服务的整体模型主要由 Polkadot 的底层架构驱动。</h6>
<h6>( in-core/on-chain dualism，二元性，有点像一个硬币的两面，或者量子的概念。即可能是在核心里，也可能是在链上，关键取决于你怎么看待这件事情。可以即是在核心里，也可以在链上。 )</h6>
Jam is permissionless in nature, allowing anyone to deploy code as a service on it for a fee commensurate with the resources this code utilizes and to induce execution of this code through the procurement and allocation of core-time, a metric of resilient and ubiquitous computation, somewhat similar to the purchasing of gas in Ethereum. We already envision a Polkadot-compatible CoreChains service.
<h6>JAM 在本质上是无需许可的，允许任何人去部署代码提供服务，费用和代码所使用的资源相当，并且去引发代码执行通过获取和分配核心时间，有点类似以太坊中购买gas。我们已经设想了一种兼容polkadot的核心链服务</h6>


<h3 align="center"> 1. Introduction </h3>

**1.1. Nomenclature.** In this paper, we introduce a decentralized, crypto-economic protocol to which the Polkadot Network could conceivably transition itself in a major revision. Following this eventuality (which must not be taken for granted since Polkadot is a decentralized network) this protocol might also become known as Polkadot or some derivation thereof. However, at this stage this is not the case, therefore our proposed protocol will for the present be known as Jam.

<h6><strong>1.1. 命名</strong> 在这篇论文中，我们提出了一个去中心化，加密经济协议，对于polkadot 网络在设想中，可以通过重大修订过渡到该协议。如果该事件发生（由于波卡是一个去中心化网络，因此这并非板上钉钉），该协议也可能被称为 Polkadot 或类似名称。然而，目前情况并非如此，因此我们提出的协议暂时将被命名为 Jam。</h6>

An early, unrefined, version of this protocol was first proposed in Polkadot Fellowship rfc31, known as CoreJam. CoreJam takes its name after the collect/refine/join/accumulate model of computation at the heart of its service proposition. While the CoreJam rfc suggested an incomplete, scope-limited alteration to the Polkadot protocol, Jam refers to a complete and coherent overall blockchain protocol.
<h6>该协议的早期、未经完善的版本最初在 Polkadot Fellowship rfc31 中提出，名为 CoreJam。CoreJam 这个名字来源于其服务核心——收集/精炼/加入/累积计算模型。虽然 CoreJam RFC 提出了对波卡协议不完整、范围有限的修改，但 Jam 指的是一个完整且连贯的整体区块链协议。</h6>
<h6>( collect/refine/join/accumulate model,这个模型听起来很抽象，但是我们也许可以类比成我们熟悉的区块链，collect->从rpc，fullnode收集交易，refine->对收集的交易进行处理，将不合理的交易给丢弃掉，合理的交易被保留到内存池，join -> 区块生成过程，从内存池选择交易，开始构建区块， accumulate -> 生产一个个区块。 )</h6>

**1.2. Driving Factors.** Within the realm of blockchain and the wider Web3, we are driven by the need first and foremost to deliver resilience. A proper Web3 digital system should honor a declared service profile—and ideally meet even perceived expectations—regardless of the desires, wealth or power of any economic actors including individuals, organizations and, indeed, other Web3 systems. Inevitably this is aspirational, and we must be pragmatic over how perfectly this may really be delivered. Nonetheless, a Web3 system should aim to provide such radically strong guarantees that, for practical purposes, the system may be described as unstoppable.
<h6><strong>1.2. 驱动因素。 </strong>在区块链和更广泛的Web3领域中，我们最优先的驱动因素是提供韧性(弹性)。在理想的 Web3 数字系统中，服务质量承诺应当得到尊重，并尽力满足用户即使是潜在的期望，这与任何经济参与者的意愿、财富或权力无关，这些参与者包括个人、组织，当然也包括其他 Web3 系统。不可避免地，这是一个理想化的目标，我们必须务实地看待它能被完美实现的程度。尽管如此，Web3 系统仍应致力于提供如此强有力的保障，以至于在实际操作中，该系统可以被描述为不可阻挡的(不会停机的)。 </h6>

While Bitcoin is, perhaps, the first example of such a system within the economic domain, it was not general purpose in terms of the nature of the service it offered. A rules-based service is only as useful as the generality of the rules which may be conceived and placed within it. Bitcoin’s rules allowed for an initial use-case, namely a fixedissuance token, ownership of which is well-approximated and autonomously enforced through knowledge of a secret, as well as some further elaborations on this theme.

<h6>虽然比特币可能是经济领域内这种系统最早的例子之一，但它所提供的服务性质使其并非通用型。基于规则的服务的效用取决于放置在其内部的规则的普遍性。比特币的规则允许一个初始用例，即固定发行量的代币，其所有权可以通过秘密的知识以及该主题的一些进一步阐述来很好地近似和自主地执行。 </h6>

Later, Ethereum would provide a categorically more general-purpose rule set, one which was practically Turing complete.[^1] In the context of Web3 where we are aiming to deliver a massively multiuser application platform, generality is crucial, and thus we take this as a given.
<h6>后来，以太坊提供了一套更通用、几乎是图灵完备的规则集。[^1] 在我们旨在提供一个庞大的多人用户应用程序平台的 Web3 背景下，通用性至关重要，因此我们将其视为理所当然。</h6>

Beyond resilience and generality, things get more interesting, and we must look a little deeper to understand what our driving factors are. For the present purposes,we identify three additional goals:
<h6>超越弹性和普遍性之后，事情变得更具吸引力，我们需要更深入地挖掘我们的驱动因素是什么。为了当前的目的，我们确定了三个额外的目标：</h6>
1. Resilience: highly resistant from being stopped,corrupted and censored.
2. Generality: able to perform Turing-complete computation.
3. Performance: able to perform computation quickly and at low cost.
4. Coherency: the causal relationship possible between different elements of state and how thus how well individual applications may be composed.
5. Accessibility: negligible barriers to innovation;easy, fast, cheap and permissionless.
<h6>1.韧性: 对阻止、破坏和审查具有高度抵抗力。</h6>
<h6>2.通用性: 能够执行图灵完备计算。</h6>
<h6>3.性能: 能快速且低成本地执行计算。</h6>
<h6>4.连贯性: 指状态的不同元素之间可能存在的因果关系，以及由此单个应用程序可以被良好组合的程度。</h6>
<h6>易访问性: 创新障碍微乎其微；易于、快速、廉价且无需许可。</h6>
As a declared Web3 technology, we make an implicit assumption of the first two items. Interestingly, items 3 and 4 are antagonistic according to an information theoretic principle which we are sure must already exist in some form but are nonetheless unaware of a name for it. For argument’s sake we shall name it *size-synchrony antagonism*.
<h6>Web3 作为一种已声明的 Web3 技术，我们隐含地假定了韧性和通用性这两个特性。有趣的是，根据某个我们确信应该以某种形式存在但尚未得知名称的信息论原理，性能和可访问性这两个特性之间存在着对抗性。为了方便讨论，我们将这种对抗性称为规模同步对抗</h6>

**1.3. Scaling under Size-Synchrony Antagonism.** Size-synchrony antagonism is a simple principle implying that as the state-space of information systems grow, then the system necessarily becomes less synchronous. The argument goes:
1. The more state a system utilizes for its dataprocessing, the greater the amount of space this state must occupy.
2. The more space used, then the greater the mean and variance of distances between statecomponents.
3. As the mean and variance increase, then interactions become slower and subsystems must manage the possibility that distances between interdependent components of state could be materially different, requiring asynchrony.

<h6>**规模同步对抗下的扩展。** 规模同步对抗是一个简单的原理，它意味着随着信息系统的状态空间增长，系统必然变得不那么同步。 论证如下：</h6>
<h6>1. 信息系统处理数据所利用的状态越多，该状态所占用的空间就越大</h6>
<h6>2. 占用空间越大，状态组件之间的距离的平均值和方差就越大</h6>
<h6>3. 随着平均值和方差的增加，交互会变得更加缓慢，子系统必须设法管理状态的相互依存组件之间距离存在实质性差异的可能性，这需要异步性</h6>

This assumes perfect coherency of the system’s state.Setting the question of overall security aside for a moment,we can avoid this rule by applying the divide and conquer maxim and fragmenting the state of a system, sacrificing its coherency. We might for example create two independent smaller-state systems rather than one large-state system. This pattern applies a step-curve to the principle;intra-system processing has low size and high synchrony,inter-system processing has high size but low synchrony.It is the principle behind meta-networks such as Polkadot,Cosmos and the predominant vision of a scaled Ethereum(all to be discussed in depth shortly).
<h6>以上分析假设系统状态完美一致。暂且不论整体安全问题，我们可以通过分而治之的策略来打破这一限制，即牺牲系统状态的整体一致性，将状态拆分成更小的片段。例如，我们可以创建两个独立的小状态系统，而不是一个大的状态系统。这种方式相当于为该原则引入了一个阶跃曲线：系统内部处理具有较小的状态规模和较高的同步性，系统间处理则具有较大的状态规模和较低的同步性。这就是 Polkadot、Cosmos 等跨链网络以及扩容后的以太坊（我们稍后将详细讨论）背后的核心思想。</h6>
The present work explores a middle-ground in the antagonism, avoiding the persistent fragmentation of statespace of the system as with existing approaches. We do this by introducing a new model of computation which pipelines a highly scalable element to a highly synchronous element. Asynchrony is not avoided, but we do open the possibility for a greater degree of granularity over how it is traded against size. In particular fragmentation can be made ephemeral rather than persistent, drawing upon a coherent state and fragmenting it only for as long as it takes to execute any given piece of processing on it.
<h6>本文探索了一种在对抗性之间寻求平衡的方法，避免了现有方案中系统状态空间的持续碎片化。我们通过引入一种新的计算模型来实现这一点，该模型将一个高度可扩展的元素与一个高度同步的元素进行流水线处理。这种方法并没有完全避免异步性，但它允许我们在权衡规模和同步性方面拥有更大的粒度控制。具体而言，碎片化可以变为临时性的而非永久性的，它可以利用一个连贯的状态，并且只在执行特定处理任务所需的时间内进行碎片化。</h6>

Unlike with snark-based L2-blockchain techniques for scaling, this model draws upon crypto-economic mechanisms and inherits their low-cost and high-performance profiles and averts a bias toward centralization.
<h6>与基于 zk-SNARK 的 L2 区块链扩容技术不同，这个模型利用密码经济机制，从而继承了它们低成本和高性能的优势，并且避免了中心化倾向。</h6>

**1.4. Document Structure.** We begin with a brief overview of present scaling approaches in blockchain technology in section 2. In section 3 we define and clarify the notation from which we will draw for our formalisms.
<h6>1.4 文档结构。 第二部分我们将概述当前区块链技术中的扩展方法。 第三部分我们将定义和阐明我们将用于形式化描述的符号。</h6>
We follow with a broad overview of the protocol in section 4 outlining the major areas including the Polka Virtual Machine (pvm), the consensus protocols Safrole and Grandpa, the common clock and build the foundations of the formalism.
<h6>在第四部分，我们将对协议进行概述，重点介绍主要部分，包括 Polka 虚拟机 (pvm)、共识协议 Safrole 和 Grandpa、通用时钟，并在此基础上构建形式化描述方法。</h6>
We then continue with the full protocol definition split into two parts: firstly the correct on-chain state-transition formula helpful for all nodes wishing to validator the chain state, and secondly, in sections 13 and 15 the honest strategy for the off-chain actions of any actors who wield a validator key.
<h6>然后我们将继续进行完整的协议定义，分为两部分：首先是正确的链上状态转换公式，这对于所有希望验证链状态的节点都非常有用。其次，在第 13 和 15 节中，我们将定义持有验证者密钥的任何参与者的链下行为的诚实策略。</h6>

The main body ends with a discussion over the performance characteristics of the protocol in section 17 and finally conclude in section 18.
<h6>本文主体部分以第 17 节对协议性能特征的讨论结束，最后在第 18 节总结全文。</h6>

The appendix contains various additional material important for the protocol definition including the pvm in appendices A & B, serialization and Merklization in appendices C & D and cryptography in appendices F, G & H. We finish with an index of terms which includes the values of all simple constant terms used in the work in appendix I, and close with the bibliography.
<h6>附录包含了几个对协议定义很重要的附加材料，包括：A & B 附录：Polka 虚拟机 (pvm)，C & D 附录：序列化和 Merkle 化，F、G & H 附录：密码学。最后，我们提供了术语索引，其中包含了本文所有使用的简单常量术语的值，并以参考文献列表结束全文</h6>

<h3 align="center">2. Previous Work and Present Trends</h3>
<h3 align="center">2. 前人的工作和当前的趋势 </h6>

In the years since the initial publication of the Ethereum *YP* , the field of blockchain development has grown immensely. Other than scalability, development has been done around underlying consensus algorithms, smart-contract languages and machines and overall state environments. While interesting, these latter subjects are mostly out scope of the present work since they generally do not impact underlying scalability.
<h6>自以太坊黄皮书 (Ethereum Yellow Paper) 初版发布以来，区块链开发领域已经取得了巨大发展。除了可扩展性之外，开发还围绕底层共识算法、智能合约语言和机器以及整体状态环境进行。尽管这些后续主题很有趣，但它们通常不会影响底层可扩展性，因此大部分超出了本文的讨论范围。</h6>

**2.1. Polkadot.** In order to deliver its service, Jam coopts much of the same game-theoretic and cryptographic machinery as Polkadot known as Elves and described by Stewart 2018. However, major differences exist in the actual service offered with Jam, providing an abstraction much closer to the actual computation model generated by the validator nodes its economy incentivizes
<h6>Polkadot。Polkadot 和 Jam 都使用了相同的博弈论和密码学工具（称为精灵，由 Stewart 在 2018 年描述）。然而，两者提供的实际服务存在重大差异。Jam 提供的抽象层更接近其经济激励机制所产生的验证器节点实际执行的计算模型。</h6>

It was a major point of the original Polkadot proposal, a scalable heterogeneous multichain, to deliver highperformance through partition and distribution of the workload over multiple host machines. In doing so it took an explicit position that composability would be lowered. Polkadot’s constituent components, parachains are, practically speaking,highly isolated in their nature. Though a message passing system (xcmp) exists it is asynchronous,coarse-grained and practically limited by its reliance on a high-level slowly evolving interaction language xcm.
<h6>原始的 Polkadot 协议旨在通过将工作负载划分和分布到多个主机上来提供可扩展的异构多链架构，从而实现高性能。然而，这种设计也带来了一个权衡，即降低了可组合性。Polkadot 的组成部分 - 平行链 (parachains) - 实际上彼此之间是高度隔离的。虽然存在一个消息传递系统 (xcmp)，但它却是异步的、粒度较粗的，并且实际上受到依赖于高级、缓慢演进的交互语言 xcm 的限制。</h6>

As such, the composability offered by Polkadot between its constituent chains is lower than that of Ethereum-like smart-contract systems offering a single and universal object environment and allowing for the kind of agile and innovative integration which underpins their success. Polkadot, as it stands, is a collection of independent ecosystems with only limited opportunity for collaboration, very similar in ergonomics to bridged blockchains though with a categorically different security profile. A technical proposal known as spree would utilize Polkadot’s unique shared-security and improve composability, though blockchains would still remain isolated.
<h6>因此，Polkdot 的平行链之间提供的可组合性低于以太坊等智能合约系统。后者提供单一的通用对象环境，并允许灵活创新的集成，正是这种集成支持了它们的成功。Polkdot 目前更像是一系列独立的生态系统，合作机会有限，在用户体验方面与采用桥梁连接的区块链非常相似，尽管它们的安全特性完全不同。一个名为 Spree 的技术提案旨在利用 Polkadot 独特的共享安全机制来提高可组合性，但区块链之间仍然是隔离的。</h6>

Implementing and launching a blockchain is hard, timeconsuming and costly. By its original design, Polkadot limits the clients able to utilize its service to those who are both able to do this and raise a sufficient deposit to win an auction for a long-term slot, one of around 50 at the present time. While not permissioned per se, accessibility is categorically and substantially lower than for smart-contract systems similar to Ethereum.
<h6>实现和启动一个区块链非常困难、耗时且成本高昂。根据最初的设计，Polkdot 的服务仅限于那些既能够做到这一点，又能够筹集足够资金赢得长期插槽拍卖（目前大约有 50 个插槽）的客户端。尽管 Polkadot 本身并不是许可制的，但其可访问性在本质上和实质上都比以太坊等智能合约系统要低得多。</h6>
Enabling as many innovators to participate and interact, both with each other and each other’s user-base, appears to be an important component of success for a Web3 application platform. Accessibility is therefore crucial.
<h6>对 Web3 应用平台而言，让尽可能多的创新者参与和互动（既得相互之间互动，也得和彼此的用户群互动）似乎是成功的重要因素。因此，可访问性至关重要。</h6>

**2.2. Ethereum.** The Ethereum protocol was formally defined in this paper’s spiritual predecessor, the Yellow Paper, by Wood 2014. This was derived in large part from the initial concept paper by Buterin 2013. In the decade since the YP was published, the de facto Ethereum protocol and public network instance have gone through a number of evolutions, primarily structured around introducing flexibility via the transaction format and the instruction set and “precompiles” (niche, sophisticated bonus instructions) of its scripting core, the Ethereum virtual machine(evm).
<h6>以太坊协议的演变。以太坊协议在形式上由伍德 (Wood) 在 2014 年发布的“黄皮书”进行定义，该黄皮书也可以被非正式地称为以太坊白皮书。白皮书很大程度上源自布特林 (Buterin) 在 2013 年撰写 的初始概念性论文。自那以后的十年间，以太坊协议及其公共网络实例经历了一系列演变，这些演变主要集中在通过引入灵活性来进行优化，具体方面包括交易格式、指令集以及以太坊虚拟机 (EVM) 这个脚本核心所提供的“预编译”（小众的复杂指令扩展）。</h6>

Almost one million crypto-economic actors take part in the validation for Ethereum.[^2] Block extension is done through a randomized leader-rotation method where the physical address of the leader is public in advance of their block production.[^3] Ethereum uses Casper-FFG introduced by Buterin and Griffith 2019 to determine finality, which with the large validator base finalizes the chain extension around every 13 minutes.
<h6>以太坊的验证过程由近百万名密码经济参与者共同完成。[^2] 区块扩展通过一种随机的领导者轮换机制进行，领导者的物理地址会在他们生产区块之前公开。[^3] 以太坊使用 Buterin 和 Griffith 在 2019 年提出的 Casper-FFG 算法来确定最终状态，借助庞大的验证者基础，Casper-FFG 大约每 13 分钟完成一次链扩展的最终确定。</h6>

Ethereum’s direct computational performance remains broadly similar to that with which it launched in 2015, with a notable exception that an additional service now allows 1mb of commitment data to be hosted per block (all nodes to store it for a limited period). The data cannot be directly utilized by the main state-transition function, but special functions provide proof that the data(or some subsection thereof) is available. According to Ethereum Foundation 2024b, the present design direction is to improve on this over the coming years by splitting responsibility for its storage amongst the validator base in a protocol known as *Dank-sharding.*
<h6>以太坊的直接计算性能与 2015 年推出时大致相当，但有一个值得注意的例外：现在允许每个区块额外存储 1MB 的承诺数据（所有节点在有限时间内存储）。这些数据不能直接被主状态转换函数使用，但是特殊的功能可以证明数据（或其某个子部分）是可用的。根据以太坊基金会 2024 年的报告 (Ethereum Foundation 2024b)，未来几年计划通过一种名为「Dank-sharding」的协议，在验证者群体之间分摊存储责任来改善这一点。</h6>

According to Ethereum Foundation 2024a, the scaling strategy of Ethereum would be to couple this data availability with a private market of roll-ups, sideband computation facilities of various design, with zk-snark-based roll-ups being a stated preference. Each vendor’s roll-up design, execution and operation comes with its own implications.
<h6>根据以太坊基金会 2024 年的报告 (Ethereum Foundation 2024a)，以太坊的扩展策略是将这种数据可用性与一个私有汇总 (sī yǒu huì zong) 市场结合起来。汇总是指各种设计形式的旁路计算设施，其中基于零知识证明 (líng zhī zhī shí zhèng míng) 的汇总被明确列为优先选项。每个供应商的汇总设计、执行和运营都会带来其自身的影响。</h6>

One might reasonably assume that a diversified marketbased approach for scaling via multivendor roll-ups will allow well-designed solutions to thrive. However, there are potential issues facing the strategy. A research report by Sharma 2023 on the level of decentralization in the various roll-ups found a broad pattern of centralization, but notes that work is underway to attempt to mitigate this. It remains to be seen how decentralized they can yet be made.
<h6>我们可以合理地假设，通过多家供应商提供的汇总来实现扩展的市场化多元化方法将允许设计良好的解决方案蓬勃发展。然而，这种策略也面临着潜在的问题。Sharma 在 2023 年关于各种汇总的去中心化程度的研究报告中发现了一个普遍的中心化倾向，但他指出正在进行缓解这种趋势的工作。汇总最终能去中心化到什么程度还有待观察。</h6>

Heterogeneous communication properties (such as datagram latency and semantic range), security properties(such as the costs for reversion, corruption, stalling and censorship) and economic properties (the cost of accepting and processing some incoming message or transaction) may differ, potentially quite dramatically, between major areas of some grand patchwork of roll-ups by various competing vendors. While the overall Ethereum network may eventually provide some or even most of the underlying machinery needed to do the sideband computation it is far from clear that there would be a “grand consolidation” of the various properties should such a thing happen. We have not found any good discussion of the negative ramifications of such a fragmented approach.[^4]
<h6>各种汇总解决方案在通信属性（例如数据报延迟和语义范围）、安全属性（例如恢复、损坏、停滞和审查的成本）以及经济属性（接受和处理传入消息或交易的成本）方面可能存在异质性，并且在来自不同竞争供应商的大型汇总混合体的主要区域之间，这些属性可能会存在巨大差异。虽然整体以太坊网络最终可能提供一些甚至大部分用于执行旁路计算的基础设施，但即使发生这种情况，各种属性是否会实现“大规模整合” 仍不清楚。我们没有找到任何关于这种碎片化方法负面影响的良好讨论。</h6>

*2.2.1. Snark Roll-ups.* While the protocol’s foundation makes no great presuppositions on the nature of roll-ups, Ethereum’s strategy for sideband computation does centre around snark-based rollups and as such the protocol is being evolved into a design that makes sense for this. Snarks are the product of an area of exotic cryptography which allow proofs to be constructed to demonstrate to a neutral observer that the purported result of performing some predefined computation is correct. The complexity of the verification of these proofs tends to be sub-linear in their size of computation to be proven and will not give away any of the internals of said computation,nor any dependent witness data on which it may rely.
<h6>零知识证明汇总 (Snark Roll-ups)虽然协议本身并没有对汇总的性质做出太多预设，但以太坊的旁路计算策略确实以基于零知识证明的汇总为核心，因此协议正朝着有利于这种方式的方向发展。零知识证明是一种密码学技术，允许构建证明，向中立观察者证明预定义计算的结果是正确的。这些证明的验证复杂度往往与需要证明的计算大小成次线性关系，并且不会泄露任何计算的内部细节，也不会泄露计算所依赖的任何相关见证数据。</h6>

Zk-snarks come with constraints. There is a trade-off between the proof’s size, verification complexity and the computational complexity of generating it. Non-trivial computation, and especially the sort of general-purpose computation laden with binary manipulation which makes smart-contracts so appealing, is hard to fit into the model of snarks.
<h6>零知识证明并非万能，也存在一些限制。证明的尺寸、验证复杂度和生成证明的计算复杂度之间需要权衡取舍。复杂的计算，尤其是充满二进制操作的通用计算，正是让智能合约如此吸引人的原因，这类计算很难塞进零知识证明的模型里。</h6>
To give a practical example, risc-zero (as assessed by Bögli 2024) is a leading project and provides a platform for producing snarks of computation done by a risc-v virtual machine, an open-source and succinct risc machine architecture well-supported by tooling. A recent benchmarking report by PolkaVM Project 2024 showed that compared to risc-zero’s own benchmark, proof generation alone takes over 61,000 times as long as simply recompiling and executing even when executing on 32 times as many cores, using 20,000 times as much ram and an additional state-of-the-art gpu. According to hardware rental agents https://cloud-gpus.com/, the cost multiplier of proving using risc-zero is 66,000,000x of the cost[^5] to execute using our risc-v recompiler.
<h6>举个实际例子，risc-zero（根据 Bögli 2024 年的评估）是一个领先的项目，它提供了一个平台，用于生成由 RISC-V 虚拟机执行的计算的零知识证明。RISC-V 是一种开源的精简指令集架构，拥有完善的工具支持。PolkaVM 项目 2024 年的一份最新基准测试报告显示，与 risc-zero 自己做的基准测试相比，仅生成证明的时间就比简单地重新编译和执行要长 61,000 多倍，即使是在使用 32 倍的内核、20,000 倍的内存和额外的最新 GPU 的情况下。根据硬件租赁代理商 https://www.runpod.io/ 的数据，使用 risc-zero 进行证明的成本是使用我们基于 RISC-V 的重新编译器执行成本的 66,000,000 倍。[^5]</h6>

Many cryptographic primitives become too expensive to be practical to use and specialized algorithms and structures must be substituted. Often times they are otherwise suboptimal. In expectation of the use of snarks (such as plonk as proposed by Gabizon, Williamson, and Ciobotaru 2019), the prevailing design of the Ethereum project’s Dank-sharding availability system uses a form of erasure coding centered around polynomial commitments over a large prime field in order to allow snarks to get acceptably performant access to subsections of data. Compared to alternatives, such as a binary field and Merklization in the present work, it leads to a load on the validator nodes orders of magnitude higher in terms of cpu usage.
<h6>许多密码学原语因为成本太高而变得不切实际，需要用专门的算法和结构来替代，但这些替代方案往往不是最优的。以太坊项目正在设计的 Dank-sharding 数据可用性系统预计会用到零知识证明 (例如 Gabizon、Williamson 和 Ciobotaru 在 2019 年提出的 plonk 方案)，为了让零知识证明能够高效地访问数据子集，该系统采用了基于大素数域上的多项式承诺的纠删码形式。与本文提出的使用二元域和 Merkle 树等替代方案相比，这种方法会极大地增加验证器节点的 CPU 使用量。</h6>

In addition to their basic cost, snarks present no great escape from decentralization and the need for redundancy,leading to further cost multiples. While the need for some
benefits of staked decentralization is averted through their verifiable nature, the need to incentivize multiple parties to do much the same work is a requirement to ensure that
a single party not form a monopoly (or several not form a cartel). Proving an incorrect state-transition should be impossible, however service integrity may be compromised in other ways; a temporary suspension of proof-generation, even if only for minutes, could amount to major economic ramifications for real-time financial applications.
<h6>除了基本成本之外，零知识证明也无法完全避免去中心化和冗余的需求，这将进一步增加成本。虽然可验证的特性避免了对完全去中心化权益证明的一些需求，但为了确保单个参与方不会形成垄断（或多个参与方不会形成卡特尔），仍然需要激励多个参与方去做大致相同的工作。证明一个错误的状态转换应该是做不到的，但是服务的完整性可能会受到其他方面的损害；即使只是几分钟的暂停生成证明，对于实时金融应用来说也可能产生巨大的经济影响。</h6>

Real-world examples exist of the pit of centralization giving rise to monopolies. One would be the aforementioned snark-based exchange framework; while notionally serving decentralized exchanges, it is in fact centralized with Starkware itself wielding a monopoly over enacting trades through the generation and submission of proofs, leading to a single point of failure—should Starkware’s service become compromised, then the liveness of the system would suffer.
<h6>确实存在因中心化导致垄断的现实案例。例如我们之前提到的基于零知识证明的交易所框架，虽然名义上服务于去中心化交易所，但实际上它是中心化的，Starkware 公司本身通过生成和提交证明来垄断交易的执行，这就会产生单点故障——如果 Starkware 的服务受到损害，那么系统的运行就会受到影响。</h6>

It has yet to be demonstrated that snark-based strategies for eliminating the trust from computation will ever be able to compete on a cost-basis with a multi-party crypto-economic platform. All as-yet proposed snarkbased solutions are heavily reliant on crypto-economic systems to frame them and work around their issues. Data availability and sequencing are two areas well understood as requiring a crypto-economic solution.
<h6>尚未证明基于零知识证明的无信任计算策略在成本方面能够与多方密码经济平台竞争。所有目前提出的基于零知识证明的解决方案都严重依赖于密码经济系统来构建框架并解决其问题。数据可用性和排序都是公认需要密码经济解决方案的两个领域。</h6>

We would note that snark technology is improving and the cryptographers and engineers behind them do expect improvements in the coming years. In a recent article by Thaler 2023 we see some credible speculation that with some recent advancements in cryptographic techniques, slowdowns for proof generation could be as little as 50,000x from regular native execution and much of this could be parallelized. This is substantially better than the present situation, but still several orders of magnitude greater than would be required to compete on a cost-basis with established crypto-economic techniques such as elves.
<h6>我们注意到，零知识证明技术正在进步，该领域的密码学家和工程师预计未来几年会有所改善。Thaler 在 2023 年的一篇文章中提出了可信的推测，随着一些最新的密码学技术进步，生成证明的速度可能会比原生执行慢 50,000 倍左右，并且其中大部分可以并行处理。这比目前的情况要好得多，但仍然比与成熟的密码经济技术（例如精灵）在成本方面竞争所需的速度高出几个数量级。</h6>

**2.3. Fragmented Meta-Networks.** Directions for general-purpose computation scalability taken by other projects broadly centre around one of two approaches; either what might be termed a fragmentation approachor alternatively a centralization approach. We argue that neither approach offers a compelling solution.
<h6>2.3 分片-元网络 (Fragmented Meta-Networks)。在通用计算可扩展性方面，其他项目采取的两种主要方法是：分片方法 (fragmentation approach) 和中心化方法 (centralization approach)。我们认为这两种方法都无法提供令人满意的解决方案。</h6>

The fragmentation approach is heralded by projects such as Cosmos (proposed by Kwon and Buchman 2019) and Avalanche (by Tanana 2019). It involves a system fragmented by networks of a homogenous consensus mechanic, yet staffed by separately motivated sets of validators. This is in contrast to Polkadot’s single validator set and Ethereum’s declared strategy of heterogeneous rollups secured partially by the same validator set operating under a coherent incentive framework. The homogeneity of said fragmentation approach allows for reasonably consistent messaging mechanics, helping to present a fairly unified interface to the multitude of connected networks.
<h6>分片方法由一些项目率先提出，例如 Cosmos（由 Kwon 和 Buchman 在 2019 年提出）和 Avalanche（由 Tanana 在 2019 年提出）。它涉及一个由具有相同共识机制的网络分片而成的系统，但由独立的验证者集合来维护。这与 Polkadot 的单一验证者集和以太坊宣布的由部分由相同验证者集（在一致的激励框架下运行）保护的异构汇总策略形成对比。这种分片方法的同质性允许相对一致的消息传递机制，帮助为连接的多个网络呈现一个相当统一的接口。</h6>

However, the apparent consistency is superficial. The networks are trustless only by assuming correct operation of their validators, who operate under a crypto-economic security framework ultimately conjured and enforced by economic incentives and punishments. To do twice as much work with the same levels of security and no special coordination between validator sets, then such systems essentially prescribe forming a new network with the same overall levels of incentivization.
<h6>然而，这种表面的统一性是肤浅的。这些网络的“无需信任”仅仅是假设其验证器能够正确运行，而验证器则是在最终由经济激励和惩罚机制设计并实施的密码经济安全框架下工作的。为了在没有验证器集合之间特殊协调的情况下，用相同的安全级别做两倍的工作，那么这样的系统实际上是在规定形成一个具有相同整体激励水平的新网络。</h6>

Several problems arise. Firstly, there is a similar downside as with Polkadot’s isolated parachains and Ethereum’s isolated roll-up chains: a lack of coherency due to a persistently sharded state preventing synchronous composability.
<h6>几个问题接踵而至。首先，它与 Polkadot 的隔离平行链和以太坊的隔离汇总链存在类似的缺点：由于持续分片的状态阻碍了同步可组合性，导致缺乏连贯性。</h6>
More problematically, the scaling-by-fragmentation approach, proposed specifically by Cosmos, provides no homogenous security—and therefore trustlessness—guarantees. Validator sets between networks must be assumed to be independently selected and incentivized with no relationship, causal or probabilistic, between the Byzantine actions of a party on one network and potential for appropriate repercussions on another. Essentially, this means that should validators conspire to corrupt or revert the state of one network, the effects may be felt across other networks of the ecosystem.
<h6>更糟糕的是，Cosmos 提出的“分片扩展”方法无法保证均质的安全性和无信任性。该方法假设不同网络的验证者集合是独立选拔和激励的，它们之间在拜占庭行为 (bāi zhàn tíng xíng wéi) 方面没有任何因果关系或概率上的关联。这意味着，如果验证者串谋破坏或回滚一个网络的状态，其影响可能会波及整个生态系统中的其他网络。</h6>

That this is an issue is broadly accepted, and projects propose for it to be addressed in one of two ways. Firstly, to fix the expected cost-of-attack (and thus level of security) across networks by drawing from the same validator set. The massively redundant way of doing this, as proposed by Cosmos Project 2023 under the name replicated security, would be to require each validator to validate on all networks and for the same incentives and punishments. This is economically inefficient in the cost of security provision as each network would need to independently provide the same level of incentives and punishment-requirements as the most secure with which it wanted to interoperate. This is to ensure the economic proposition remain unchanged for validators and the security proposition remained equivalent for all networks. At the present time, replicated security is not a readily
available permissionless service. We might speculate that these punishing economics have something to do with it.
<h6>这个问题被广泛认可，项目们提出了两种解决方法。第一种是通过使用相同的验证者集来修复预期攻击成本（从而提高安全级别） across networks（在多个网络之间）。Cosmos 项目在 2023 年提出的“复制安全” (replicated security) 是实现这一点的一种非常冗余的方式。该方法要求每个验证者在所有网络上进行验证，并获得相同的激励和惩罚。但这在经济上效率低下，因为每个网络都需要独立提供与它想要互操作的最安全网络相同的激励和惩罚要求，以确保验证者的经济利益不变，所有网络的安全水平保持一致。目前，“复制安全”并不是一个 readily available permissionless service（ readily available 可以理解为 readily offered 或 widely available，permissionless service 可以理解为无需许可的服务）。我们可以推测，这些惩罚性的经济因素与此有关。</h6>

The more efficient approach, proposed by the OmniLedger team, Kokoris-Kogias et al. 2017, would be to make the validators non-redundant, partitioning them between different networks and periodically, securely and randomly repartitioning them. A reduction in the cost to attack over having them all validate on a single network is implied since there is a chance of having a single network accidentally have a compromising number of malicious validators even with less than this proportion overall. This aside it presents an effective means of scaling under a basis of weak-coherency.
<h6>另一种更有效的方法由 OmniLedger 团队 (Kokoris-Kogias et al. 2017) 提出，即让验证者不再冗余，而是将它们分配到不同的网络中，并定期以安全和随机的方式重新分配。这样做可以降低攻击成本（与所有验证者都在单个网络上验证相比），因为即使整体比例低于恶意验证者的阈值，也存在单个网络偶然拥有足够数量恶意验证者的可能性。除此之外，这种方法在弱一致性的基础上提供了一种有效的扩展手段。</h6>

Alternatively, as in Elves by Stewart 2018, we may utilize non-redundant partitioning, combine this with a proposal-and-auditing game which validators play to weed out and punish invalid computations, and then require that the finality of one network be contingent on all causally-entangled networks. This is the most secure and economically efficient solution of the three, since there is a mechanism for being highly confident that invalid transitions will be recognized and corrected before their effect is finalized across the ecosystem of networks. However, it requires substantially more sophisticated logic and their causal-entanglement implies some upper limit on the number of networks which may be added.
<h6> 另一种替代方案，如 Stewart 在 2018 年提出的 Elves，可以结合非冗余分区和提议-审计博弈 这两种方法。提议-审计博弈可以让验证者识别并惩罚无效计算。然后，要求一个网络的最终状态取决于所有与其存在因果关联的网络。这是三种方案中最安全、最经济高效的一种，因为它提供了一种机制，可以高度确信无效的转移会在其影响整个网络生态系统之前被识别和纠正。然而，这种方法需要更加复杂的逻辑，并且其因果关联性意味着可添加的网络数量存在上限。</h6>

**2.4. High-Performance Fully Synchronous Networks.** Another trend in the recent years of blockchain development has been to make “tactical” optimizations over data throughput by limiting the validator set size or diversity, focusing on software optimizations, requiring a higher degree of coherency between validators, onerous requirements on the hardware which validators must have, or limiting data availability.
<h6>2.4 高性能完全同步网络近年来。区块链发展的另一个趋势是通过一些“战术性”优化来提高数据吞吐量，这些优化手段包括：限制验证者集的规模或多样性、侧重软件优化、要求验证者之间具有更高的连贯性、对验证者硬件提出苛刻的要求，以及限制数据可用性</h6>
The Solana blockchain is underpinned by technology introduced by Yakovenko 2018 and boasts theoretical figures of over 700,000 transactions per second, though according to Ng 2024 the network is only seen processing a small fraction of this. The underlying throughput is still substantially more than most blockchain networks and is owed to various engineering optimizations in favor of maximizing synchronous performance. The result is a highlycoherent smart-contract environment with an api not unlike that of *YP* Ethereum (albeit using a different underlying VM), but with a near-instant time to inclusion and finality which is taken to be immediate upon inclusion.
<h6>Solona 区块链底层使用的是 Yakovenko 在 2018 年提出的技术，理论上每秒可以处理超过 70 万笔交易，尽管根据 Ng 在 2024 年的报告，网络目前仅被观察到处理一小部分的交易量。尽管如此，其底层的吞吐量仍然比大多数区块链网络高得多，这得益于各种工程优化，旨在最大限度地提高同步性能。结果是创建了一个高度连贯的智能合约环境，其 API 与 YP 以太坊的 API 类似（尽管使用了不同的底层虚拟机），但是包含和最终确定时间几乎即时，包含即意味着最终确定。</h6>
Two issues arise with such an approach: firstly, defining the protocol as the outcome of a heavily optimized codebase creates structural centralization and can undermine resilience. Jha 2024 writes “since January 2022, 11 significant outages gave rise to 15 days in which major or partial outages were experienced”. This is an outlier within the major blockchains as the vast majority of major chains have no downtime. There are various causes to this downtime, but they are generally due to bugs found in various subsystems.
<h6>这种方法存在两个问题：首先，将协议定义为高度优化的代码库的产物会产生结构性中心化，并可能削弱系统的弹性。Jha 在 2024 年的文章中写道：“自 2022 年 1 月以来，发生了 11 次重大的停机事件，导致系统经历了总计 15 天的全部或部分停运”。这在主要区块链中是罕见的，因为绝大多数主要链路都没有停机时间。导致停机的原因多种多样，但通常是由于在各种子系统中发现的漏洞。</h6>
Ethereum, at least until recently, provided the most contrasting alternative with its well-reviewed specification, clear research over its crypto-economic foundations and multiple clean-room implementations. It is perhaps no surprise that the network very notably continued largely unabated when a flaw in its most deployed implementation was found and maliciously exploited, as described by Hertig 2016.
<h6>作为对比，以太坊（至少到最近为止）提供了截然不同的替代方案，它拥有经过审查良好的规范、清晰的密码经济基础研究以及多种 clean-room 实现。当其最广泛部署的实现中发现并被恶意利用的漏洞时 (Hertig 2016 年有所描述)，该网络能够继续运行几乎不受影响，这或许不足为奇。</h6>
The second issue is concerning ultimate scalability of the protocol when it provides no means of distributing workload beyond the hardware of a single machine.
<h6>第二个问题在于该协议的最终可扩展性令人担忧，因为它没有办法将工作负载分配到单个机器的硬件之外。</h6>
In major usage, both historical transaction data and state would grow impractically. Solana illustrates how much of a problem this can be. Unlike classical blockchains, the Solana protocol offers no solution for the archival and subsequent review of historical data, crucial if the present state is to be proven correct from first principle by a third party. There is little information on how Solana manages this in the literature, but according to Solana Foundation 2023, nodes simply place the data onto a centralized database hosted by Google.[^6]
<h6>在大量使用的情况下，历史交易数据和状态都会增长到难以实际处理的地步。Solana 区块链就很好地说明了这个问题的严重性。与传统区块链不同，Solana 协议没有提供存档和随后审查历史数据的解决方案，而这些数据对于第三方通过第一性原理证明当前状态的正确性至关重要。文献中很少提及 Solana 如何管理这一点，但根据 Solana 基金会 2023 年的说法，节点只是将数据放置在由 Google 托管的中心化数据库中</h6>

Solana validators are encouraged to install large amounts of ram to help hold its large state in memory (512 gb is the current recommendation according to Solana Labs 2024). Without a divide-and-conquer approach, Solana shows that the level of hardware which validators can reasonably be expected to provide dictates the upper limit on the performance of a totally synchronous, coherent execution model. Hardware requirements represent barriers to entry for the validator set and cannot grow without sacrificing decentralization and, ultimately, transparency.
<h6>Solana 的验证者被建议安装大量内存来帮助其在内存中保存庞大的状态 (Solana Labs 2024 年的当前推荐是 512 GB)。Solana 证明了，如果没有分而治之的方法，验证者可以合理提供的硬件水平就决定了完全同步、连贯执行模型的性能上限。硬件要求对验证者集而言是进入门槛，并且硬件需求的增长会以牺牲去中心化和最终的透明度为代价。</h6>

<h3 align="center">3. Notational Conventions</h3>
<h6 align="center">3. 符号约定 </h6>

Much as in the Ethereum Yellow Paper, a number of notational conventions are used throughout the present work. We define them here for clarity. The Ethereum Yellow Paper itself may be referred to henceforth as the *YP*.

<h6> 就像以太坊黄皮书一样，在该工作中使用了大量约定的符号。为了清楚起见，我们在这里定义他们。以太坊黄皮书本身在以后用 YP 缩写。</h6>

**3.1. Typography.** We use a number of different typefaces to denote different kinds of terms. Where a term is used to refer to a value only relevant within some localized section of the document, we use a lower-case roman letter e.g. $\rm{x}$ , $\rm{y}$ (typically used for an item of a set or sequence) or e.g. $\rm{i}$, $\rm{j}$ (typically used for numerical indices). Where we refer to a Boolean term or a function in a local context, we tend to use a capitalized roman alphabet letter such as $\rm{A}$, $\rm{F}$ . If particular emphasis is needed on the fact a term is sophisticated or multidimensional, then we may use a bold typeface, especially in the case of sequences and sets

<h6>3.1. 术语的字体运用。为了表示不同种类的术语，我们使用多种不同的字体。术语如果仅在文档的局部范围内引用，表示局部值，那么我们使用小写罗马字母，例如 $\rm{x}$ 、 $\rm{y}$（通常用于集合或序列中的元素）或 $\rm{i}$、 $\rm{j}$（通常用于数值索引）。当我们在局部上下文中引用布尔值或函数时，则倾向于使用大写罗马字母，例如 $\rm{A}$ 、 $\rm{F}$ 。如果特别需要强调术语的复杂性或多维性，则可以使用粗体字，尤其是在序列和集合的情况下。</h6>

For items which retain their definition throughout the present work, we use other typographic conventions. Sets are usually referred to with a blackboard typeface, e.g. $\mathbb{N}$ refers to all natural numbers including zero. Sets which may be parameterized may be subscripted or be followed by parenthesized arguments. Imported functions, used by the present work but not specifically introduced by it, are written in calligraphic typeface, e.g. $\mathcal{H}$ the Blake2 cryptographic hashing function. For other non-context dependent functions introduced in the present work, we use upper case Greek letters, e.g. $\Upsilon$ denotes the state transition function
<h6> 对于贯穿整部著作的项目，我们使用其他的排版约定。集合通常用黑板字体表示，例如 $\mathbb{N}$ 表示所有自然数，包括零。可参数化的集合可以用下标表示，也可以用带括号的参数表示。引用函数时，如果该函数在著作中使用但未特别引入，则使用书法字体，例如 $\mathcal{H}$ 表示 Blake2 加密散列函数。对于著作中引入的其他与上下文无关的函数，我们使用大写希腊字母，例如 $\Upsilon$ 表示状态转换函数。</h6> 

Values which are not fixed but nonetheless hold some consistent meaning throughout the present work are denoted with lower case Greek letters such as $\sigma$, the state identifier. These may be placed in bold typeface to denotethat they refer to an abnormally complex value.
<h6>本文中使用小写希腊字母（例如状态标识符 σ）表示未固定但贯穿始终且具有某种一致含义的值。这些字母可能加粗，以表示它们引用异常复杂的值。</h6>

**3.2. Functions and Operators.** We define the precedes relation to indicate that one term is defined in terms of another. E.g. y ≺ x indicates that y may be defined purely in terms of x:
<h6>我们定义“先于”关系来表示一个术语是通过另一个术语来定义的。例如，y ≺ x 表示 y 可以纯粹用 x 来定义。</h6>
<h6> ( x > y ,">"不是等号，是指，x在y 之前发生了或者存在，通过x 可以定义y )</h6>

(1)            $$\rm{y} < \rm{x} \Longleftrightarrow \exists\mathcal{f}\colon \rm{y} = \mathcal{f}(\rm{x})$$
<h6> ( \Longleftrightarrow 是等价的意思，全等于的意思。这个公式的意思是"x先于y"等价于 "存在一个函数f，可以完成通过x 和这个函数f，得到y"  )</h6>

The substitute-if-nothing function $\mathcal{U}$ is equivalent to the first argument which is not ∅, or ∅ if no such argument exists:
<h6> 替换空值函数，用符号 $\mathcal{U}$ 表示，可以理解为一个“如果非空则替换”函数。给定若干个参数，它返回 首个非空 的参数（不是空集 ∅）。如果所有参数都为空，则函数本身返回 空集 ∅。</h6>

(2) $$\mathcal{U}(a_0, \dots ) \equiv a_x \colon (a_x \neq \varnothing \bigvee x = n, \bigwedge\limits^{x - 1}_{i = 0} a_i = \varnothing)$$

<h6>  ( 这个公式的意思是，函数 $\mathcal{U}$ ,取一个集合里第一个不为空的值，如果集合全是空集，那么返回空集 )</h6>

Thus, e.g. $\mathcal{U}(\varnothing, 1, \varnothing, 2) = 1$ and $\mathcal{U}(\varnothing, \varnothing) = \varnothing$.
<h6> 因此，有例子 $\mathcal{U}(\varnothing, 1, \varnothing, 2) = 1$ and $\mathcal{U}(\varnothing, \varnothing) = \varnothing$ </h6>

**3.3. Sets.** We denote the cardinality of some set s, thenumber of its elements, as the usual ∣s∣. We denote setdisjointness with the relation  &midcir;  . Formally:
<h6>3.3. 集合。我们用通常的符号 |s| 表示集合 s 的 基数，即其元素的个数。我们用关系 ⫰ 表示集合的 不相交性。</h6>

$$\rm{A}\cup\rm{B} = \varnothing  \Longleftrightarrow \rm{A} ⫰ \rm{B} $$

We commonly use ∅ to indicate that some term is validly left without a specific value. Its cardinality is defined as zero. We define the operation ? such that A? ≡ A ∪ {∅} indicating the same set but with the addition of the ∅ element.
<h6>我们通常使用 ∅ 表示术语可以合法地留空而不指定具体的值。它的 基数 定义为零。我们定义运算符 ? ，使得 A? ≡ A ∪ {∅}，表示相同的集合，但添加了元素 ∅ 。</h6>

The term ∇ is utilized to indicate the unexpected failure of an operation or that a value is invalid or unexpected. (We try to avoid the use of the more conventional  here
to avoid confusion with Boolean false, which may be interpreted as some successful result in some contexts.)
<h6>该术语 ∇ 用于指示操作意外失败或值无效/超出预期的状态。（我们尽量避免使用更常用的符号「⊥」，以防止与布尔型「假」（false）混淆，因为在某些情况下，「假」可能会被解释为某种成功的结果。）</h6>

**3.4. Numbers.** $\mathbb{N}$ denotes the set of naturals including zero whereas Nn denotes the set of naturals less than n. Formally, $\mathbb{N}$ = {0, 1, . . . } and $\mathbb{N}_n$ = ${\rm{x} ∣ \rm{x} ∈ \mathbb{N}, \rm{x} < \rm{n}}$.

<h6>3.4. 数字。我们用符号 $\mathbb{N}$  表示包含零的自然数集。 $\mathbb{N}$ 的子集记为 $\mathbb{N}_n$，它表示小于正整数 $\rm{n}$ 的自然数集合。形式上，这些集合定义如下: $\mathbb{N}$ = {0, 1, . . . } and $\mathbb{N}_n$ = ${\rm{x} ∣ \rm{x} ∈ \mathbb{N}, \rm{x} < \rm{n}}$.</h6>


$\mathbb{Z}$ denotes the set of integers. We denote $\mathbb{Z}_{\rm{a} \dots \rm{b}}$ to be the set of integers within the interval [a, b). 
<h6>记号 Z 表示所有整数的集合。我们用符号 $\mathbb{Z}_{a...b}$ 表示区间 [a, b) 内的整数集合。</h6>

Formally, $\mathbb{Z}_{a \dots b} = {\rm{x} ∣ \rm{x} ∈ \mathbb{Z}, \rm{a} ≤ \rm{x} < \rm{b}}$ .  
<h6>形式上, $\mathbb{Z}_{a \dots b} = {\rm{x} ∣ \rm{x} ∈ \mathbb{Z}, \rm{a} ≤ \rm{x} < \rm{b}}$</h6>

E.g. $\mathbb{Z}_{2 \dots 5} = {2, 3, 4}$. 
<h6>例如, $\mathbb{Z}_{2 \dots 5} = {2, 3, 4}$ 。</h6>

We denote the offset/length form of this set as $\mathbb{Z}_{ \rm{a} \dots +\rm{b} }$ 
<h6>我们将此集合的偏移量/长度形式表示为 $\mathbb{Z}_{\rm{a} \dots + \rm{b}}$ </h6>

, a short form of $\mathbb{Z}_{\rm{a} \dots \rm{a}+\rm{b}}$ .
<h6>它是 $\mathbb{Z}_{\rm{a} \dots \rm{a}+\rm{b}}$ 的简写形式。</h6>

It can sometimes be useful to represent lengths of sequences and yet limit their size, especially when dealing with sequences of octets which must be stored practically. Typically, these lengths can be defined as the set  $\mathbb{N}_{2^{32}}$
<h6>
为了表示序列的长度并限制其大小，有时会非常有用，尤其是在处理需要实际存储的八位字节序列时。通常，这些长度可以定义为集合 $\mathbb{N}_{2^{32}}$ </h6>

<p>To improve clarity, we denote  $\mathbb{N}_L$ as the set of lengths of octet sequences and is equivalent to $\mathbb{N}_{2^{32}}$ </p>


<h6>为了提高清晰度，我们用  $\mathbb{N}_L$ 表示八位字节序列长度的集合，它等价于 $\mathbb{N}_{2^{32}}$ </h6>

We denote the % operator as the modulo operator, e.g. 5 % 3 = 2. Furthermore, we may occasionally express a division result as a quotient and remainder with the separator R , e.g. 5 ÷ 3 = 1 R 3.
<h6>我们将 % 符号表示为模运算符，例如 5 % 3 = 2。此外，我们有时可能会使用带有分隔符 R 的商和余数来表示除法结果，例如 5 ÷ 3 = 1 R 3。</h6>

**3.5. Dictionaries.** A dictionary is a possibly partial mapping from some domain into some co-domain in much the same manner as a regular function. Unlike functions however, with dictionaries the total set of pairings are necessarily enumerable, and we represent them in some data structure as the set of all $(key \longmapsto value)$ pairs. (In such data-defined mappings, it is common to name the values within the range a key and the values within the domain a value, hence the naming.)
<h6>3.5. 字典。 字典是一种类似于普通函数的部分映射，它从某个域映射到某个共同域。然而，与函数不同的是，字典的配对总数一定是可枚举的，我们通过数据结构将它们表示为所有(键 $\longmapsto$ 值) 对的集合。（在这种数据定义的映射中，通常将范围内的值称为键，域内的值称为值，因此得名。）</h6>

Thus, we define the formalism $\mathbb{D}⟨K \longmapsto V⟩$ to denote a dictionary which maps from the domain K to the range V. We define a dictionary as a member of the set of all dictionaries $\mathbb{D}$ and a set of pairs $\rm{p} = (\rm{k} \longmapsto \rm{v})$:
<h6>因此，我们定义形式主义 D⟨K⟼V⟩ 来表示一个字典，它将域 K 映射到范围 V。我们将字典定义为所有字典集合 D 的成员，以及一个由键值对组成的集合 p=(k⟼v)：</h6>

(3) 
```math

\mathbb{D} \subset \Big\{ \big\{(\rm{k} \longmapsto \rm{v}) \big\} \Big\}

```

A dictionary’s members must associate at most one unique value for any key k:
<h6>字典的成员（键值对）对于任何键 k 最多只能关联一个唯一的值。</h6>

(4)
$$\forall d \in \mathbb{D} : \forall(\rm{k} \longrightarrow \rm{v}) \in d : \exists!\rm{v}': (\rm{k} \longmapsto \rm{v}')$$

This assertion allows us to unambiguously define the subscript and subtraction operator for a dictionary d:
<h6>这个断言使我们可以对字典 d 的下标和减法运算符进行明确的定义。</h6>

（5)
```math
\forall d \in \mathbb{D}: d[k] \equiv \Big\{ \begin{align} \rm{v} \qquad if \quad \exists \rm{k}: (k \longmapsto v) \in d \\ \varnothing \qquad otherwise \end{align}
```
(6)
```math
\forall d \in \mathbb{D},s: a \setminus  s \equiv \{ (\rm{k} \longmapsto \rm{v}) : (\rm{k} \longmapsto \rm{v}) \in d, \rm{k} \notin \rm{s} \}
```
<h6> 我感觉是拼写错误，a 是d ,这里定义了集合的减法。也就是一个元素属于集合a\d,但是不属于集合s。就是a\s(d\s) ) </h6>

Note that when using a subscript, it is an implicit assertion that the key exists in the dictionary. Should the key not exist, the result is undefined and any block which relies on it must be considered invalid.
<h6>注意，当使用下标访问字典元素时，隐含地断言了键在字典中存在。如果键不存在，则结果是未定义的，任何依赖此结果的代码块都将被视为无效。</h6>

It is typically useful to limit the sets from which the keys and values may be drawn. Formally, we define a typed dictionary $\mathbb{D}⟨K \longrightarrow V ⟩$ as a set of pairs p of the form (k $\longmapsto$ v):
<h6>为了限制键和值的来源集合，我们通常会进行类型化。形式上，我们将类型化字典 D⟨K⟶V⟩ 定义为由形式为 (k ⟼ v) 的键值对 p 组成的集合：</h6>

(7)
```math
  \mathbb{D} \left \langle K \longrightarrow V \right \rangle \subset \mathbb{D}
```
(8)
```math
  \mathbb{D} \left \langle K \longrightarrow V \right \rangle \equiv \Big\{\{ (k \longmapsto v) | k \in K \wedge v \in V \}\Big\}
```
To denote the active domain (i.e. set of keys) of a dictionary d ∈ $\mathbb{D}$ ⟨K → V ⟩, we use $\mathcal{K}$(d) ⊂ K and for the range (i.e. set of values), $\mathcal{V}$(d) ⊂ V . Formally:
<h6>为了表示字典 d ∈ $\mathbb{D}$⟨K → V ⟩ 的活动域（即键的集合），我们使用 $\mathcal{K}$(d) ⊂ K， 表示其键集 $\mathcal{K}$(d) 是全集 K 的子集。同理，为了表示取值范围（即值的集合），我们使用 $\mathcal{V}$(d) ⊂ V， 表示其值集 $\mathcal{V}$(d) 是全集 V 的子集。</h6>

(9)
```math
  \mathcal{K}( d \in \mathbb{D}) \equiv \{k | \exists v : (k \longmapsto v) \in d \}
```
(10)
```math
  \mathcal{V}( d \in \mathbb{D}) \equiv \{v | \exists k : (k \longmapsto v) \in d \}
```
Note that since the domain of $\mathcal{V}$ is a set, should different keys with equal values appear in the dictionary, the set will only contain one such value.
<h6>注意，由于集合 $\mathcal{V}$ 的定义域是一个集合，即使字典中出现多个键对应同一个值，该集合也只会包含该值的一个副本。</h6>

**3.6. Tuples.** Tuples are groups of values where each item typically belongs to a different set. They are denoted with parentheses, e.g. the tuple t of the integers 3 and 5 is denoted t = (3, 5), and it exists in the set of integer pairs sometimes denoted $\mathbb{N}×\mathbb{N}$, but denoted in the present work as $(\mathbb{N}, \mathbb{N})$.
<h6>元组。元组是有序值集合，每个元素通常来自不同的集合。它们用括号表示。例如，包含整数 3 和 5 的元组 t 表示为 t = (3, 5)。该元组属于所有可能的整数有序对的集合。这个集合有时表示为 $\mathbb{N}×\mathbb{N}$ ，但在本文档中，我们将使用符号 $(\mathbb{N}, \mathbb{N})$ 表示。</h6>

<p>
  We have frequent need to refer to a specific item within a tuple value and as such find it convenient to declare a name for each item. E.g. we may denote a tuple with two named integer components a and b as T = $(a ∈ \mathbb{N}, b ∈ \mathbb{N})$. We would denote an item t ∈ T through subscripting its name, thus for some t =(a:3,b:5) $t_a$ = 3 and $t_b$ = 5.
</p>
<h6>我们经常需要引用元组值中的特定元素，因此为每个元素命名会比较方便。例如，我们可以用 T = (a∈ $\mathbb{N}$ ,b∈ $\mathbb{N}$ ) 表示一个包含两个命名整型元素 a 和 b 的元组。我们可以通过对名称进行下标引用来表示元组中的元素，例如对于某个元组 t = (a:3, b:5)， 则 $t_a$ =3 且 $t_b$ =5 。</h6>

**3.7. Sequences.** A sequence is a series of elements with particular ordering not dependent on their values. The set of sequences of elements all of which are drawn from some set T is denoted ⟦T⟧, and it defines a partial mapping N → T. The set of sequences containing exactly n elements each a member of the set T may be denoted $⟦T⟧_n$ and accordingly defines a complete mapping $\mathbb{N}_n$ → T. Similarly, sets of sequences of at most n elements and at least n elements may be denoted $⟦T⟧∶_n$ and $⟦T⟧_n∶$ respectively.
<h6>序列是一系列按特定顺序排列的元素，元素的取值并不影响这种顺序。由集合 T 中的元素构成的所有序列的集合记为 ⟦T⟧，它定义了一个从自然数集 $\mathbb{N}$ 到集合 T 的部分映射。对于每个正整数 n，由 n 个集合 T 的元素组成的序列的集合可以记为 $⟦T⟧_n$，它对应了一个从 n 维自然数集 $\mathbb{N}_n$ 到集合 T 的完全映射。类似地，至多包含 n 个元素的序列集合和至少包含 n 个元素的序列集合分别记为 $⟦T⟧∶_n$ and $⟦T⟧_n∶$ 。</h6>

<p>Sequences are subscriptable, thus a specific item at index i within a sequence s may be denoted s[i], or where unambiguous, $s_i$. A range may be denoted using an ellipsis for example: $[0, 1, 2, 3]_{...2}$ == [0, 1] and $[0, 1, 2, 3]_{1⋅⋅⋅+2}$ == [1, 2]. The length of such a sequence may be denoted ∣s∣.</p>
<h6>序列是可以按索引取值的，因此序列 s 中的第 i 个元素可以表示为 s[i]，或者在没有歧义的情况下简写为 si。可以使用省略号表示范围，例如： [0, 1, 2, 3]...2 == [0, 1] 表示取序列中前两个元素，即子序列 [0, 1] [0, 1, 2, 3]1⋅⋅⋅+2 == [1, 2] 表示取序列中从索引 1 开始的两个元素，即子序列 [1, 2]。序列的长度可以用 |s| 表示</h6>

We denote modulo subscription as $s[i]^\circlearrowright \equiv$ s[ i % ∣s∣ ].We denote the final element x of a sequence s = [..., x] through the function last(s) ≡ x.

<h6>我们用 $s[i]^\circlearrowright \equiv$ s[ i % ∣s∣ ] 表示序列的 模运算索引。给定序列 s，其中 i 是索引，∣s∣ 是序列的长度 (元素个数)。该表达式读作 "s 在 i 的模运算索引等于 s 在 i 除以 序列长度的余数 处的元素”。 例如，序列为 [1, 2, 3, 4] 时, $s[2]^\circlearrowright$ 等于 s[2 % 4]，即 s[2]，因为 2 除以 4 的余数为 2。我们用函数 last(s) $\equiv$ x 表示序列 s 的 最后一个元素 x。</h6>

<img width="1080" alt="image" src="https://github.com/harodggg/jam/assets/31732456/f57aa7c9-c98e-4033-8899-ac9b0a08db2b">

<h6>3.7.1 构造。我们有时希望根据其他值的增量索引来定义序列: $[x_0,x_1, . . . ]_n$  表示一个由 n 个值组成的序列,从 $x_0$ 开始一直到 $x_{n−1}$。其中 $x_0$ 是序列的第一个元素，n 是序列的长度. $x_i$ 表示序列中第 i 个元素 (i 从 0 到 n-1)。省略号 (...) 表示序列按照相同的递增 (或递减) 规则继续下去。在这种情况下，我们用 [f(i) ∣ i <− N] 表示 [f(0), f(1), ..., f(n - 1)]。因此，当元素的顺序很重要时，我们使用 <− 而不是无序符号 ∈。后者也可以简写为 [f(i <- $mathbb{N}$)]。这适用于任何具有明确顺序的集合，特别是序列，因此 [i^2 ∣ i <0 [1, 2, 3]] = [1, 4, 9]。可以组合多个序列，因此 [i ⋅ j ∣ i <- [1, 2, 3], j <− [2, 3, 4]] = [2, 6, 12]。</h6>

Sequences may be constructed from sets or other sequences whose order should be ignored through sequence ordering notation $[i_k \wr i ∈ X]$, which is defined to result in the set or sequence of its argument except that all elements i are placed in ascending order of the corresponding value $i_k$.
<h6>序列可以由集合或其他忽略顺序的序列构造而成，通过序列排序记法 $[i_k \wr i∈X]$ 来表示。该记法的作用是重新排列原始集合或序列，使得所有元素（记为 $i_k$）都按照它们对应的值升序排列。</h6>

The key component may be elided in which case it is assumed to be ordered by the elements directly; i.e. [i ∈ $\mathit{X}$] ≡ $[i \wr i ∈ X]$. $[i_k \wr \wr i ∈ X]$ does the same, but excludes any duplicate values of i. E.g. assuming s = [1, 3, 2, 3], then $[i \wr i ∈ s]$ = [1, 2, 3] and $[−i \wr i ∈ s]$ = [3, 3, 2, 1].

<h6>关键元素有时可以省略，此时假定序列由元素本身直接排序。也就是说，[i ∈ X] 等同于$[i /wr i ∈ \mathit{X}]$ 。[ik _ _i ∈ X] 做同样的事，但会排除 i 的重复值。例如，假设 s = [1, 3, 2, 3]，那么 [i $\wr \wr$ i ∈ s] = [1, 2, 3] 和 $[−i \wr \wr  i ∈ s]$ = [3, 3, 2, 1]。</h6>

Sets may be constructed from sequences with the regular set construction syntax, e.g. assuming s = [1, 2, 3, 1], then {a ∣ a ∈ s} would be equivalent to {1, 2, 3}.
<h6>可以使用常规集合构造语法从序列构造集合。例如，假设 s = [1, 2, 3, 1]，那么 {a ∣ a ∈ s} 等于 {1, 2, 3}。</h6>

Sequences of values which themselves have a defined ordering have an implied ordering akin to a regular dictionary, thus [1, 2, 3] < [1, 2, 4] and [1, 2, 3] < [1, 2, 3, 1].
<h6>由本身具有定义顺序的数值组成的序列，就像常规字典一样，也隐含着排序顺序。因此，我们可以这样进行比较：[1, 2, 3] < [1, 2, 4] 以及 [1, 2, 3] < [1, 2, 3, 1].</h6>

3.7.2. Editing. We define the sequence concatenation operator $\frown$ such that $[x_0,x_1, \dots ,y_0,y_1, \dots ] \equiv x \frown y$ . Futher, we denote element-concatenation as x<img width="52" alt="image" src="https://github.com/harodggg/jam/assets/31732456/5897dd06-684a-4edc-be25-acecf1f92612"> i ≡ x ⌢ [i]. We denote the sequence made up of the first n elements of sequence s to be ${\mathop{s}\limits^\rightarrow}^n$ ≡ $[s_0, s_1, . . . , s_{n−1}]$, and only the final elements ${\mathop{s}\limits^\leftarrow}^n$ .
<h6>编辑序列。我们定义序列连接运算符 ⌢，使得 $[x_0,x_1,\dots,y_0,y_1,\dots]$ 等于 x ⌢ y。此外，我们将元素连接表示为 x <img width="52" alt="image" src="https://github.com/harodggg/jam/assets/31732456/5fdf28e5-b6fe-4dd4-bda2-f4d2d514b94e"> i ≡ x ⌢ [i]。我们用 ${\mathop{s}\limits^\rightarrow}^n$ 表示序列 s 的前 n 个元素组成的序列，即 $[s_0, s_1, . . . , s_{n−1}]$，用 ${\mathop{s}\limits^\leftarrow}^n$  表示序列 s 的最后 n 个元素。</h6>

We denote sequence subtraction with a slight modification of the set subtraction operator; specifically, some sequence s excepting the left-most element equal to v would
be denoted s <img width="57" alt="image" src="https://github.com/harodggg/jam/assets/31732456/06f5b684-4577-41ee-91e3-ad9559ba6b64"> {v}.
<h6>我们用稍微修改集合差集运算符的方式来表示序列差分。具体来说，序列 s 去掉最左边的等于 v 的元素，可以用 s <img width="57" alt="image" src="https://github.com/harodggg/jam/assets/31732456/385bd5d3-db60-4175-b065-52e625577782">{v} 表示。</h6>

3.7.3. Boolean values. $\mathbb{B}_s$ denotes the set of Boolean strings of length s, thus $\mathbb{B}_s = [{\bot, \top}]_s$ . When dealing with Boolean values we may assume an implicit equivalence mapping to a bit whereby $\top = 1 and \bot = 0$, thus $\mathbb{B}_◻ = [\mathbb{N}_2]_◻$. We use the function bits($\mathbb{Y}$) ∈ $\mathbb{B}$ to denote the sequence of bits, ordered with the least significant first, which represent the octet sequence $/mathbb{Y}$, thus bits([5, 0]) = [1, 0, 1, 0, 0, . . . ].
<h6>
3.7.3. 布尔值。记号 $\mathbb{B}_s$ 表示长度为 s 的布尔字符串集合，因此 $\mathbb{B}_s = [{\bot, \top}]_s$ 。这表示 $\mathbb{B}_s$  中的每个字符串都由 s 个字符组成，每个字符只能是 "0" 或 "1"。
处理布尔值时，我们可以假设存在一个隐式的等价映射到比特上，其中 "1" 表示真 (true)，"0" 表示假 (false)。因此, $\mathbb{B}_◻ = [\mathbb{N}_2]_◻$，其中 $\mathbb{N}_2$ 表示包含 0 和 1 的集合，◻ 表示某种集合运算 (operator)。我们使用函数 $bits(\mathbb{Y}) ∈ \mathbb{B}$ 来表示比特序列，它以最低有效位在前 (least significant bit first) 的顺序表示八位节 (octet) 序列 $\mathbb{Y}$。例如，bits([5, 0]) = [1, 0, 1, 0, 0, ..., 0]。这里需要注意的是，省略号 "..." 表示后面还有很多个 "0"，具体数量取决于 octet 的位数 (通常为 8 位)。</h6>

<p>3.7.4. Octets and Blobs. $\mathbb{Y}$ denotes the set of octet strings ("blobs”) of arbitrary length. As might be expected, $\mathbb{Y}_x$ denotes the set of such sequences of length x. $\mathbb{Y}_\$$ denotes the subset of $\mathbb{Y}$ which are ASCII-encoded strings. Note that while an octet has an implicit and obvious bijective relationship with natural numbers less than 256, and we may implicitly coerce between octet form and integer form, we do not treat them as exactly equivalent entities. In particular for the purpose of serialization an octet is always serialized in the sequence containing only itself, whereas an integer may be serialized as a sequence of potentially several octets, depending on its magnitude.</p>

<h6>3.7.4 八位节和数据块 (Blobs). $\mathbb{Y}$ 表示任意长度的八位节字符串集合，又称为数据块。八位节是计算机数据单位，由 8 个比特组成，可以表示从 0 到 255 之间的数值或单个字符. $\mathbb{Y}_x$ 表示长度为 x 的八位节序列集合。换句话说, $\mathbb{Y}_x$ 中的每个元素都是由 x 个八位节组成的有序序列. $\mathbb{Y}_\$$ 表示 $\mathbb{Y}$ 的子集，包含所有使用 ASCII 码编码的字符串。ASCII 码是一种字符编码标准，用于表示英文字母、数字和其他常用符号。隐式双射关系 (implicit bijective relationship)文中提到八位节和小于 256 的自然数之间存在着一种隐式的双射关系。这意味着我们可以将一个八位节的值转换为小于 256 的自然数，也可以将一个自然数转换为一个八位节。但是需要注意，这种转换是隐式的，并不总是显式地执行。强制转换,文中提到了在八位节形式和整数形式之间可能存在隐式的强制转换。这表示我们可以根据上下文，将一个八位节的值视为一个整数，也可以将一个整数视为一个八位节序列。序列化 (serialization),将数据结构或对象转换为可存储或传输的格式的过程。关键区别,八位节和整数在序列化方面存在着关键区别。一个八位节总是被序列化为仅包含它自己的序列。而一个整数则可能会根据其大小，被序列化为包含多个八位节的序列。例如，数字 1 可以表示为单个八位节，而数字 100 则需要多个八位节来表示。</h6>

*3.7.5. Shuffling.* We define the sequence-shuffle function $\mathcal{F}$, originally introduced by Fisher and Yates 1938, with an efficient in-place algorithm described by Wikipedia 2024.This accepts a sequence and some entropy and returns a sequence of the same length with the same elements but in an order determined by the entropy. The entropy may be provided as either an indefinite sequence of integers or a hash. For a full definition see appendix E.

<h6>3.7.5. 序列洗牌。我们定义序列洗牌函数 $\mathcal{F}$ ，它最初由 Fisher 和 Yates 在 1938 年引入，并由 Wikipedia 在 2024 年描述了一种高效的原地（in-place）算法。该函数接受一个序列和一些熵 (entropy) 作为输入，并返回一个具有相同长度、相同元素但顺序由熵决定的序列。熵可以表示为无限的整数序列或哈希值。有关完整定义，请参见附录 E。</h6>

**3.8. Cryptography.**
<h6>密码学</h6>

<p>3.8.1. Hashing. $\mathcal{H}$ denotes the set of 256-bit values typically expected to be arrived at through a cryptographic function, equivalent to $\mathbb{Y}_{32}$ with 
 $\mathcal{H}^{0}$ being equal to $[0]_{32}$ . We assume a function $\mathcal{H}(m ∈ \mathbb{Y}) ∈ \mathbb{H}$ denoting the Blake2b 256-bit hash introduced by Saarinen and Aumasson 2015 and a function $\mathcal{H}_K(m ∈ \mathbb{Y}) ∈ H$ denoting the Keccak 256-bit hash as proposed by Bertoni et al. 2013 and utilized by Wood 2014.</p>

 <h6>
3.8.1 哈希（Hashing). $\mathcal{H}$ 表示一组通常由密码函数产生的 256 位值的集合，这与 $\mathbb{Y}_{32}$ 等价，其中 H0 等于 $[0]_{32}$. $[0]_{32}$ 表示由 32 个 0 组成的字节串。
$\mathcal{H}(m ∈ \mathbb{Y}) ∈ \mathbb{H}$ 表示一个函数 $\mathcal{H}$，它接受集合 $\mathbb{Y}$ 中的元素 m 为输入，并输出一个属于集合 $\mathbb{H}$ 的 256 位哈希值。Blake2b由 Saarinen 和 Aumasson 在 2015 年提出的密码散列函数，可以生成 256 位的哈希值。“散列”和“哈希”在该语境下可以互换使用. $\mathcal{H}_K(m ∈ \mathbb{Y}) ∈ H$ 表示另一个函数 $\mathcal{H}_K$ ，它也接受集合 $\mathbb{Y}$ 中的元素 m 为输入，并输出一个属于集合 $\mathbb{H}$ 的 256 位哈希值。 Keccak: 由 Bertoni 等人在 2013 年提出的密码散列函数，Keccak 也是 SHA-3 算法的基础，可以生成 256 位的哈希值。 Wood 2014: 推测 Wood 在 2014 年的某份研究或文档中使用了 Keccak 哈希函数。</h6>

We may sometimes wish to take only the first x octets of a hash, in which case we denote $\mathcal{H}_x(m) ∈ \mathbb{Y}_x$ to be the first x octets of $\mathcal{H}(m)$ . The inputs of a hash function are generally assumed to be serialized with our codec $\upvarepsilon(x) ∈ Y$ , however for the purposes of clarity or unambiguity we may also explicitly denote the serialization. Similarly, we may wish to interpret a sequence of octets as some other kind of value with the assumed decoder function $\upvarepsilon\_{−1} (x ∈ \mathbb{Y})$ . In both cases, we may subscript the transformation function with the number of octets we expect the octet sequence term to have. Thus, r = $\upvarepsilon\_4(x ∈ N)$ would assert x ∈ $\mathbb{N}\_{2^{32}}$ and r ∈ $\mathbb{Y}_4$, whereas $s = \upvarepsilon^{−1}_8 (y)$ would assert $y ∈ \mathbb{Y}\_8$ and $s ∈ \mathbb{N}\_{2^{64}}$ .

<h6>有时我们可能只想取哈希值的前 x 个八位节，记为 $\mathcal{H}_x(m) ∈ \mathbb{Y}_x$，表示提取哈希函数 $\mathcal{H}(m)$ 的前 x 个八位节的结果为一个长度为 x 的八位节序列。哈希函数的输入通常假定使用我们的编码器 $\upvarepsilon(x) ∈ \mathbb{Y}$ 进行序列化。但是为了清晰或避免歧义，我们也可以明确表示序列化过程。类似地，我们可能希望将一个八位节序列解释为某种其他类型的值，并使用解码器函数 $\upvarepsilon_{−1} (x ∈ \mathbb{Y})$ 。在这两种情况下，我们可以用期望的八位节序列的长度作为下标来标记转换函数。例如，r = $\upvarepsilon\_4(x ∈ N)$  表示 x ∈  $\mathbb{N}\_{2^{32}}$ 且 r ∈ $\mathbb{Y}_4$ ，这意味着将一个 32 位无符号整数 ( $\mathbb{N}$ ) 编码成一个 4 位节序列 $\mathbb{Y}_4$ 。另一种情况是 $s = \upvarepsilon^{−1}_8 (y)$ ，表示 y ∈ Y8 且 s ∈ N264，即解码一个 8 位节序列 ($y ∈ \mathbb{Y}_8$ ) 成一个 64 位无符号整数 ( $s ∈ \mathbb{N}_{2^{64}}$ )。</h6>

*3.8.2. Signing Schemes.* $\mathbb{E}_k⟨m⟩ ⊂ \mathbb{Y}\_{64}$ is the set of valid Ed25519 signatures, defined by Josefsson and Liusvaara 2017, made through knowledge of a secret key whose public key counterpart is k ∈ $\mathbb{Y}\_{32}$ and whose message is m. To aid readability, we denote the set of valid public keys k ∈ $\mathbb{H}\_\mathit{E}$ .
<h6>3.8.2 签名方案: $\mathbb{E}_k⟨m⟩ ⊂ \mathbb{Y}_{64}$  是由 Josefsson 和 Liusvaara 在 2017 年定义的 Ed25519 算法产生的有效签名集合。集合中的每个签名都用 64 个八位节  表示。为了便于阅读，我们用 $\mathbb{H}_\mathit{E}$ 表示有效的公钥集合。公钥 k 是一个长度为 32 个八位节的元素，它与用于生成签名的秘密密钥相对应。消息本身用符号 m 表示。</h6>

We use $\mathbb{Y}_{BLS} ⊂ \mathbb{Y}\_{144}$ to denote the set of public keys forthe bls signature scheme, described by Boneh, Lynn, and Shacham 2004, on curve bls12-381 defined by Hopwoodet al. 2020.
<h6>我们使用 $\mathbb{Y}_{BLS} ⊂ \mathbb{Y}_{144}$ 来表示 BLS 签名方案的公钥集合，该方案由 Boneh、Lynn 和 Shacham 在 2004 年描述，基于 Hopwood 等人在 2020 年定义的 bls12-381 曲线。</h6>

We denote the set of valid Bandersnatch public keys as $\mathbb{H}_B$ , defined in appendix G. $F^{m∈\mathbb{Y}}\_{k∈\mathbb{H}\_B}⟨x ∈ \mathbb{Y}⟩ ⊂ \mathbb{Y}\_{96}$ is the set of valid singly-contextualized signatures of utilizing the secret counterpart to the public key k, some context x and message m.
<h6>我们记 $\mathbb{H}_B$ 为有效 Bandersnatch 公钥的集合，具体定义参见附录 G. $F^{m∈\mathbb{Y}}_{k∈\mathbb{H}_B}⟨x ∈ \mathbb{Y}⟩ ⊂ \mathbb{Y}_{96}$ 表示利用公钥 k 的秘密密钥、某种上下文信息 x 以及消息 m 生成的单一上下文签名（singly-contextualized signatures）的有效集合。</h6>

${\mathop{F}\limits^-}^{m∈\mathbb{Y}}_{r∈\mathbb{Y}_R} ⟨x ∈ \mathbb{Y}⟩ ⊂ \mathbb{Y}\_{388}$ , meanwhile, is the set of valid Bandersnatch Ringvrf deterministic singly-contextualized proofs of knowledge of a secret within some set of secrets identified by some root in the set of valid roots $\mathbb{Y}_R ∈ \mathbb{Y}\_{196608}$. We denote $\mathcal{R}(s ∈ [\mathbb{H}_B]) ∈ \mathbb{Y}_R$ to be the root specific to the set of public key counterparts s. A root implies a specific set of Bandersnatch key pairs, knowledge of one of the secrets would imply being capable of making a unique, valid—and anonymous—proof of knowledge of a unique secret within the set.
<h6>${\mathop{F}\limits^-}^{m∈\mathbb{Y}}_{r∈\mathbb{Y}_R} ⟨x ∈ \mathbb{Y}⟩ ⊂ \mathbb{Y}_{388}$则表示利用某个根 (root) 标识的一组秘密中的秘密的知识，生成有效的 Bandersnatch RingVRF 确定性单一上下文化知识证明集合。该根属于有效根集合 $\mathbb{Y}_R ∈ \mathbb{Y}_{196608}$ .我们定义 $\mathcal{R}(s ∈ [\mathbb{H}_B]) ∈ \mathbb{Y}_R$ 表示特定于公钥集合 s 的根。一个根意味着特定的 Bandersnatch 密钥对集合，知道其中一个秘密意味着能够生成唯一的、有效的、匿名的集合内唯一秘密的知识证明。</h6>

Both the Bandersnatch signature and Ringvrf proof strictly imply that a member utilized their secret key in combination with both the context x and the message m; the difference is that the member is identified in the former and is anonymous in the latter. Furthermore, both define f output, a high entropy hash influenced by x but not by m, formally denoted $\mathcal{Y}({\mathop{\mathbb{F}}\limits^¯}^m_r ⟨x⟩) ⊂ \mathbb{H}$ and $\mathcal{Y}(\mathbb{F}^m_k ⟨x⟩) ⊂ \mathbb{H}$.
<h6>
Bandersnatch 签名和 RingVRF 证明都严格地表明成员使用了他们的秘密密钥，结合上下文信息 x 和消息 m 来生成证明。区别在于前者会识别成员身份，而后者是匿名的。这两者都定义了一个 VRf 输出，即一个受 x 影响但不受 m 影响的高熵哈希值。用形式语言表示为 $\mathcal{Y}({\mathop{\mathbb{F}}\limits^¯}^m_r ⟨x⟩) ⊂ \mathbb{H}$ 和 $\mathcal{Y}(\mathbb{F}^m_k ⟨x⟩) ⊂ \mathbb{H}$ 。</h6>

We define the function $\mathcal{S}$ as the signature function, such that $\mathcal{S}_k(m) ∈ \mathbb{F}^m_k ⟨[]⟩ ∪ \mathbb{E}_k⟨m⟩$. We assert that the ability to compute a result for this function relies on knowledge of a secret key.
<h6>我们定义签名函数为 $\mathcal{S}$ ，满足以下规则: $\mathcal{S}_k(m) ∈ \mathbb{F}^m_k ⟨[]⟩ ∪ \mathbb{E}_k⟨m⟩$。 我们断言，计算此函数结果的能力依赖于对秘密密钥的掌握。</h6>

<h5 align="center">4. Overview</h5>
As in the Yellow Paper, we begin our formalisms by recalling that a blockchain may be defined as a pairing of some initial state together with a block-level statetransition function. The latter defines the posterior state given a pairing of some prior state and a block of data applied to it. Formally, we say:
<h6> 如同eth 黄皮书所述，我们从形式化定义开始，回顾一下区块链可以定义为初始状态和块级状态转换函数的配对。后一个函数定义了给定先前状态和应用于该状态的区块数据之后的后续状态。形式上，我们说：</h6>
(11)

$$\sigma' \equiv \Upsilon (\sigma,\mathbf{B})$$

Where $\sigma$ is the prior state, $\sigma'$ is the posterior state, $\mathbf{B}$ is some valid block and $\Upsilon$ is our block-level state-transition function.
<h6>$\sigma$表示之前的状态，即应用区块之前的区块链状态. $\sigma'$ 表示之后的狀態，即应用区块之后的区块链状态. $\mathbf{B}$代表一个有效的区块，包含要添加到区块链中的数据. $\Upsilon$  表示我们的块级状态转换函数。</h6>

Broadly speaking, Jam (and indeed blockchains in general) may be defined simply by specifying $\Upsilon$ and some genesis state $\sigma^0$.[^7] We also make several additional assumptions of agreed knowledge: a universally known clock, and the practical means of sharing data with other systems operating under the same consensus rules. The latter twowere both assumptions silently made in the *YP*.
<h6>总体而言，Jam 协议（以及大体上所有的区块链）都可以通过简单指定状态转换函数 $\Upsilon$  和初始状态 $\sigma^0$ 来定义。我们还做出了一些额外的共同认知假设：一个全局统一的时间源以及与运行相同共识规则的其他系统进行数据共享的实用方法。后两个假设在比特币黄皮书 (YP) 中都被默认引用了。</h6>

**4.1. The Block.** To aid comprehension and definition of our protocol, we partition as many of our terms as possible into their functional components. We begin with the block $\mathit{B}$ which may be restated as the header $\mathit{H}$ and some input data external to the system and thus said to be extrinsic, E:
<h6>4.1 区块 为了帮助理解和定义我们的协议，我们将尽可能多地将术语划分成功能组件。首先，我们将区块 (Block) B 表示为头信息 (Header) H 和外部输入数据 (Extrinsic Data) E 的组合。外部输入数据是指系统外部的数据，因此称为外部数据。</h6>
(12)

$$\mathbf{B} \equiv (\mathbf{H}, \mathbf{E} )$$
(13)

$$\mathbf{E} \equiv (\mathbf{E}_T, \mathbf{E}_J,\mathbf{E}_P,\mathbf{E}_A,\mathbf{E}_G )$$

The header is a collection of metadata primarily concerned with cryptographic references to the blockchain ancestors and the operands and result of the present transition. As an immutable known a *priori*, it is assumed to be available throughout the functional components of block transition. The extrinsic data is split into its several portions:
<h6>区块头包含关键的元数据，例如验证区块来源的密码学引用和区块转换的输入/输出数据。由于头信息不可更改且所有组件都需要访问它，因此假设其在整个区块处理过程中可用。外部数据包含待处理的交易或消息，通常会进一步细分.</h6>

* **tickets:** Tickets, used for the mechanism which manages the selection of validators for the permissioning of block authoring. This component is denoted $\mathbf{E}_T$.
* **judgements:** Votes, by validators, on dispute(s) arising between them presently taking place. This is denoted $\mathbf{E}_J$ .
* **preimages:** Static data which is presently being requested to be available for workloads to be able to fetch on demand. This is denoted $\mathbf{E}_P$ .
* **availability:**  Assurances by each validator concerning which of the input data of workloads they have correctly received and are storing locally. This is denoted $\mathbf{E}_A$.
* **reports:** Reports of newly completed workloads whose accuracy is guaranteed by specific validators. This is denoted $\mathbf{E}_G$.

<h6>
<ul>
<li>票据: 用于管理区块创作许可验证者选择的机制。该组件记为 $\mathbf{E}_T$ 。</li>
<li>判定: 验证者针对当前发生的争论进行的投票表决。记为 $\mathbf{E}_J$ 。</li>
<li>原像: 静态数据，当前正被请求供工作负载按需获取。记为 $\mathbf{E}_P$  。</li>
<li>可用性声明: 验证者做出的保证，声明他们已经正确接收了工作负载的部分输入数据并进行本地存储 $\mathbf{E}_A$。</li>
<li>报告: 由特定验证者担保其准确性的新完成工作负载报告 $\mathbf{E}_G$ 。</li>
</ul>
</h6>

**4.2. The State.** Our state may be logically partitioned into several largely independent segments which can both help avoid visual clutter within our protocol description and provide formality over elements of computation which may be simultaneously calculated (i.e. parallelized). We therefore pronounce an equivalence between σ (some complete state) and a tuple of partitioned segments of that state:
<h6>4.2 状态。 为了使协议描述更加清晰易懂，并为可并行计算的元素提供形式化定义，我们将系统的状态划分为几个逻辑上相互独立的片段。因此，我们声明一个完整状态 σ 可以表示为其划分片段的元组。</h6>

(14）
$$\sigma \equiv ( \alpha,\beta,\gamma,\delta,\eta,\iota,\varkappa , \lambda,\rho,\tau,\varphi,\chi,\psi  )$$

In summary, δ is the portion of state dealing with services, analogous in Jam to the Yellow Paper’s (smart contract) accounts, the only state of the YP’s Ethereum. The identities of services which hold some privileged status are tracked in χ.
<h6>总结来说，δ 代表了状态中与服务相关的一部分，在 Jam 中类似于 Yellow Paper 的 (智能合约) 账户，它是 YP 以太坊的唯一状态。持有特权的服务的标识符会在 χ 中被追踪。</h6>

Validators, who are the set of economic actors uniquely privileged to help build and maintain the Jam chain, are identified within κ, archived in λ and enqueued from ι. All other state concerning the determination of these keys is held within γ. Note this is a departure from the YP proofof-work definitions which were mostly stateless, and this set was not enumerated but rather limited to those with sufficient compute power to find a partial hash-collision in the sha2-256 cryptographic hash function. An on-chain entropy pool is retained in η.
<h6>验证者 (Validators) 拥有特殊经济权益的一组参与者，他们被授予独特的权利来帮助构建和维护 Jam 区块链。κ (kappa)验证者标识符集合，用于识别验证者身份。λ (lambda)验证者存档，用来存储验证者标识符 (κ) 的历史记录。ι (iota)验证者队列，用于管理待处理的验证者任务。
γ (gamma)其他验证者状态，除了验证者标识符 (κ) 之外的所有与验证者相关的状态信息，都存储于此。例如，可能包含验证者的押金、投票权等信息。η (eta)链上熵池，Jam 区块链利用密码学中的熵池概念来保证系统的随机性，为各种操作提供不可预测的随机数来源。</h6>

Our state also tracks two aspects of each core: α, the authorization requirement which work done on that core must satisfy at the time of being reported on-chain, together with the queue which fills this, φ; and ρ, each of the cores’ currently assigned report, the availability of whose work-package must yet be assured by a super-majority of validators.
<h6>我们状态中还跟踪核心 (core) 的两个方面：α (alpha)：授权要求。这是指在链上报告时，核心执行的操作必须满足的授权要求。φ (phi) 是与此相关的队列，用于存放待满足的授权要求。ρ (rho)：每个核心当前分配的报告。验证者中的超级多数派必须保证这些报告所依赖的工作包的可用性。</h6>

Finally, details of the most recent blocks and time are tracked in β and τ respectively and ongoing disputes are tracked in ψ.
<h6>最后，最新区块的详细信息和时间分别保存在 β 和 τ 中，正在进行的争端则保存在 ψ 中。</h6>

*4.2.1. State Transition Dependency Graph.* Much as in the YP, we specify Υ as the implication of formulating all items of posterior state in terms of the prior state and block. To aid the architecting of implementations whichparallelize this computation, we minimize the depth of the dependency graph where possible. The overall dependency graph is specified here:
<h6>4.2.1 状态转换依赖图。与 YP (Yellow Paper) 类似，我们用 Υ 表示后验状态的所有元素都可以由先验状态和区块推导出来。为了帮助构建并行化计算的实现，我们尽可能地减小依赖图的深度。整体的依赖图如下所示：</h6>

(15)

$$\tau' \prec  \mathbf{H} $$

(16)

$$\beta^\dagger \prec (\mathbf{H},\beta )$$

(17)

$$\beta' \prec (\mathbf{H},\mathbf{E}_G,\beta^\dagger,C)$$

(18)

$$\gamma' \prec (\mathbf{H},\tau ,\mathbf{E}_T,\gamma,\iota' ,\eta ',\kappa') $$

(19)

$$\eta' \prec (\mathbf{H},\tau,\eta)$$

(20)

$$\kappa' \prec (\mathbf{H},\tau,\kappa,\gamma ,\psi')$$

(21)

$$\gamma ' \prec (\mathbf{H},\tau,\lambda,\kappa )$$

(22)

$$\psi' \prec (\mathbf{E}_J, \psi)$$

(23)

$$\delta ^ \dagger \prec (\mathbf{E}_P,\delta,\tau')$$

(24)

$$\rho^\dagger \prec (\mathbf{E}_J,\rho)$$

(25)

$$ \rho^\ddagger \prec (\mathbf{E}_A,\rho^\dagger) $$

(26)

$$ \rho ' \prec (\mathbf{E}_G,\rho^\ddagger,\kappa ,\tau') $$

(27)

```math
\left.\begin{matrix}
 \delta '\\
 \chi '\\
\iota ' \\
\varphi'  \\
\mathbf{C} \\
\end{matrix}\right\} \prec (\mathbf{E}_A,\rho ',\delta ^ \dagger,\chi,\iota,\varphi )
```
(28)

$$\alpha' \prec (\mathbf{E}_G,\varphi ',\alpha)$$

The only synchronous entangements are visible through the intermediate components superscripted with a dagger and defined in equations 16, 23 and 25. The latter two mark a merge and join in the dependency graph and, concretely, imply that the preimage lookup extrinsic must be folded into state before the availability extrinsic may be fully processed and accumulation of work happen.
<h6>依赖图中唯一可见的同步依赖关系是通过用匕首标记的中间组件 (equations 16, 23 and 25)。后两个方程表示了依赖图中的合并和连接操作。具体而言，这表示必须在将预查找外部函数的结果应用于状态之后，才能完全处理可用性外部函数并进行累积工作。</h6>


**4.3. Which History?** A blockchain is a sequence of blocks, each cryptographically referencing some prior block by including a hash of its header, all the way back to some first block which references the genesis header. We already presume consensus over this genesis header $\mathbf{H}^0$ and the state it represents already defined as $\sigma^0$ .
<h6>为了厘清关于历史的讨论，让我们来看看区块链的定义。区块链是一个由区块组成的序列，每个区块都包含了前一区块头部的哈希值，以此链接成一条链，一直追溯到最初的创世区块。我们已经就这个创世区块头部 (记为 
 $\mathbf{H}^0$ )以及它所代表的状态 (记为 $\sigma^0$ ) 达成了一致的共识。</h6>

By defining a deterministic function for deriving a single posterior state for any (valid) combination of prior state and block, we are able to define a unique canonical state for any given block. We generally call the block with the most ancestors the head and its state the head state.
<h6>通过定义一个确定性函数，该函数可以根据任何（有效的）先验状态和区块的组合来推导出唯一的后验状态，我们能够为给定的区块定义唯一的规范状态。我们通常将具有最多祖先的区块称为头部，并将其状态称为头状态。</h6>

It is generally possible for two blocks to be valid and yet reference the same prior block in what is known as a fork. This implies the possibility of two different heads, each
with their own state. While we know of no way to strictly preclude this possibility, for the system to be useful we must nonetheless attempt to minimize it. We therefore strive to ensure that:
<h6>通常情况下，可能会出现两个区块都符合规则 (valid) 并且引用同一个前一区块的情况，这被称为分叉。这会导致存在两个不同的头部，每个头部都拥有自己的状态。虽然我们目前没有办法完全杜绝分叉的可能性，但是为了让系统发挥作用，我们仍然需要尽力减少分叉的发生。因此，我们的目标是确保：</h6>

1. It be generally unlikely for two heads to form.
2. When two heads do form they be quickly resolved into a single head.
3. It be possible to identify a block not much older than the head which we can be extremely confident will form part of the blockchain’s history in perpetuity. When a block becomes identified as such we call it finalized and this property naturally extends to all of its ancestor blocks.

1. 通常不太可能形成两个头(区块头)。
2. 当有2个区块头出现的时候，能够很快的被处理成一个区块头
3. 在区块链的历史中，我们可以非常确信地识别出一个与顶端区块相差不大的区块，该区块将永久地成为区块链的一部分。当一个区块被识别为这样的时候，我们称之为已 finalized（最终化）。这个特性自然会延伸到它所有祖先区块上。

These goals are achieved through a combination of two consensus mechanisms: Safrole, which governs the (not-necessarily forkless) extension of the blockchain; and Grandpa, which governs the finalization of some extension into canonical history. Thus, the former delivers point 1, the latter delivers point 3 and both are important for delivering point 2. We describe these portions of the protocol in detail in sections 6 and 15 respectively.
<h6>通过结合两种共识机制来实现这些目标：Safrole 用于管理区块链的扩展（不一定没有分叉），Grandpa 用于管理将一些扩展最终确定为规范历史。因此，前者实现目标 1，后者实现目标 3，两者对于实现目标 2 都很重要。我们分别在第 6 节和第 15 节详细描述了协议的这些部分。</h6>

While Safrole limits forks to a large extent (through cryptography, economics and common-time, below), there may be times when we wish to intentionally fork since we have come to know that a particular chain extension must be reverted. In regular operation this should never happen, however we cannot discount the possibility of malicious or malfunctioning nodes. We therefore define such an extension as any which contains a block in which data is reported which any other block’s state has tagged as invalid (see section 10 on how this is done). We further require that Grandpa not finalize any extension which contains such a block. See section 15 for more information here.
<h6>虽然 Safrole 通过以下方法在很大程度上限制了分叉（密码学、经济学和共识时间，见下文），但在某些情况下，当我们得知必须回滚特定的链扩展时，我们可能希望故意进行分叉。在正常运行下这种情况应该永远不会发生，但是我们也不能排除恶意节点或故障节点的可能性。因此，我们将任何包含区块的扩展定义为数据被报告为无效的扩展，而任何其他区块的状态都将其标记为无效（有关如何做到这一点，请参见第 10 节）。 为了进一步保证这一点，我们还要求 Grandpa 不对包含此类区块的任何扩展进行最终化。有关详细信息，请参见第 15 节。</h6>

**4.4. Time.** We presume a pre-existing consensus over time specifically for block production and import. While this was not an assumption of Polkadot, pragmatic and resilient solutions exist including the ntp protocol and
network. We utilize this assumption on only one way: we require that blocks be considered temporarily invalid if their timeslot is in the future. This is specified in detail in section 6.
<h6>4.4. 时间。我们假设预先存在关于时间的一致共识，具体用于区块的生产和导入。虽然这不是波卡 (Polkadot) 的既定假设，但实用且弹性的解决方案已经存在，例如 ntp 协议和网络。我们仅以一种方式利用这一假设：要求区块的时间槽位于未来时，则将其视为暂时无效。这将在第 6 节中详细说明。</h6>

Formally, we define the time in terms of seconds passed since the beginning of the Jam Common Era, 1200 UTC on January 1, 2024. [^8] Midday CET is selected to ensure that all significant timezones are on the same date at any
exact 24-hour multiple from the beginning of the common era. Formally, this value is denoted $\tau$ .
<h6>正式地，我们以自 Jam Common Era 开始以来经过的秒数（   2024 年 1 月 1 日 1200 UTC）来定义时间。 该参考点专门选择为中午 CET（中欧时间），以保证所有主要时间自共同纪元开始以来，各时区在每个精确的 24 小时间隔内共享相同的日期。象征性地，该值由希腊字母 tau (τ) 表示。</h6>

**4.5. Best block.** Given the recognition of a number of valid blocks, it is necessary to determine which should be treated as the “best” block, by which we mean the most recent block we believe will ultimately be within of all future Jam chains. The simplest and least risky means of doing this would be to inspect the Grandpa finality mechanism which is able to provide a block for which there is a very high degree of confidence it will remain an ancestor to any future chain head.
<h6>4.5. 最佳区块. 既然存在多个有效区块，我们就需要确定哪一个应该被视为“最佳”区块，这里的“最佳”是指我们认为最终将在所有未来的 Jam 链条中都存在的最新区块。做到这一点最简单、风险最低的方法是检查 Grandpa 最终化机制，该机制能够提供一个区块，我们对该区块成为任何未来链头祖先块具有非常高的可信度。</h6>

However, in reducing the risk of the resulting block ultimately not being within the canonical chain, Grandpa will typically return a block some small period older than the most recently authored block. (Existing deployments
suggest around 1-2 blocks in the past under regular operation.) There are often circumstances when we may wish to have less latency at the risk of the returned block not ultimately forming a part of the future canonical chain. E.g. we may be in a position of being able to author a block, and we need to decide what its parent should be. Alternatively, we may care to speculate about the most recent state for the purpose of providing information to a downstream application reliant on the state of Jam.
<h6>为了降低最终结果的区块不在规范链中的风险，Grandpa 通常会返回一个比最近创建的区块稍早一些的区块。（现有部署表明正常运行下大约会滞后 1-2 个区块。） 然而在某些情况下，我们可能愿意降低延迟，即使返回的区块最终可能不会成为未来规范链的一部分。例如，我们可能能够创作一个区块，并且需要决定它的父区块应该是什么。 或者，我们可能关心为了向依赖 Jam 状态的下游应用程序提供信息而获取最新的状态。</h6>

In these cases, we define the best block as the head of the best chain, itself defined in section 15.
<h6>在这些情况下，我们将最佳区块定义为最佳链的头部，最佳链本身将在第 15 节进行定义。</h6>

**4.6. Economics.** The present work describes a cryptoeconomic system, i.e. one combining elements of both cryptography and economics and game theory to deliver a self-sovereign digital service. In order to codify and manipulate economic incentives we define a token which is native to the system, which we will simply call tokens in the present work.
<h6>4.6. 经济学. 本文描述了 密码经济系统，即一种结合密码学、经济学和博弈论的元素来提供 自主权数字服务 的系统。为了将经济激励措施 编码 并加以操控，我们定义了系统内部的原生代币，本文将简单地将其称为 代币.</h6>

A value of tokens is generally referred to as a balance, and such a value is said to be a member of the set of balances, $\mathbb{N}_B$ , which is exactly equivalent to the set of 64-bit unsigned integers:

<h6>该论文将代币的价值定义为余额。所有可能余额的集合用 $\mathbb{N}_B$ 表示，它与64 位无符号整数的集合相同。</h6>

(29) 

$$ \mathbb{N}_B \equiv \mathbb{N}\_{2^{64}} $$

Though unimportant for the present work, we presume that there be a standard named denomination for $10^9$ tokens. This is different to both Ethereum (which uses adenomination of $10^{18}$), Polkadot (which uses a denomination of $10^{10}$) and Polkadot’s experimental cousin Kusama (which uses $10^{12}$).
<h6>虽然在目前的工作中无关紧要，但我们假设存在一个标准命名的面额，表示 10⁹ 个代币。这与以太坊 (使用 10¹⁸ 面额)、波卡 (使用 10¹⁰ 面额) 和波卡的实验性兄弟网络Kusama (使用 10¹² 面额) 都不同。</h6>

The fact that balances are represented as a 64-bit integer implies that there may never be more than around $18×10^9$ tokens (each divisible into portions of $10^{−9}$ ) within Jam. We would expect that the total number of tokens ever issued will be a substantially smaller amount than this
<h6>余额表示为 64 位整数这一事实意味着 Jam 中的代币数量可能永远不会超过 $18×10^9$  左右（每个代币可分为 $10^{−9}$ 的部分）。我们预计有史以来发行的代币总数将远小于此数量。</h6>

We further presume that a number of constant prices stated in terms of tokens are known. However we leave the specific values to be determined in following work:
<h6>我们进一步假设知道一些以代币表示的固定价格。但是，我们将在后续工作中确定具体值：</h6>

* $\mathbb{B}_I$ : the additional minimum balance implied for a single item within a mapping
* $\mathbb{B}_L$ : the additional minimum balance implied for a single octet of data within a mapping
* $\mathbb{B}_S$ : the minimum balance implied for a service.


* $\mathbb{B}_I$ : 与数据结构中每个键值对相关的隐式最小余额。
* $\mathbb{B}_L$ : 映射中单个字节数据的附加隐含最低余额
* $\mathbb{B}_S$ : 服务隐含的最低余额

**4.7. The Virtual Machine and Gas.** In the present work, we presume the definition of a Polka Virtual Machine (pvm). This virtual machine is based around the risc-v instruction set architecture, specifically the rv32ecm variant, and is the basis for introducing permissionless logic into our state-transition function.
<h6>4.7 虚拟机与 Gas费用。本节工作假设定义了 Polka 虚拟机 (pvm)。此虚拟机基于 RISC-V 指令集架构 (具体为 rv32ecm 变体)，并以此为基础将无许可逻辑引入我们的状态转换函数。</h6>

The pvm is comparable to the evm defined in the Yellow Paper, but somewhat simpler: the complex instructions for cryptographic operations are missing as are those which deal with environmental interactions. Overall it is
far less opinionated since it alters a pre-existing general purpose design, risc-v, and optimizes it for our needs. This gives us excellent pre-existing tooling, since pvm remains essentially compatible with risc-v, including support from the compiler toolkit llvm and languages such as Rust and C++. Furthermore, the instruction set simplicity which risc-v and pvm share, together with the register size (32-bit), active number (13) and endianness (little) make it especially well-suited for creating efficient recompilers on to common hardware architectures.
<h6>pvm 可类比于黄皮书中定义的 evm，但更加简单：缺少复杂的密码操作指令以及处理环境交互的指令。总体而言，pvm 的设计干预更少，因为它修改了预先存在通用架构 RISC-V 并根据我们的需求进行优化。这为我们提供了非常棒的现有工具，因为 pvm 在本质上仍然兼容 RISC-V，包括来自编译器工具包 llvm 和语言（例如 Rust 和 C++）的支持。此外，RISC-V 和 pvm 共享的指令集简洁性、寄存器大小（32 位）、活动寄存器数量（13 个）和小端序特性使其非常适合在常见硬件架构上创建高效的重编译器。</h6>

The pvm is fully defined in appendix A, but for contextualization we will briefly summarize the basic invocation function Ψ which computes the resultant state of a pvm instance initialized with some registers (${⟦\mathbb{N}\_R⟧}_{13}$) and ram ($\mathbb{M}$) and has executed for up to some amount of gas ($\mathbb{N}_G$), a number of approximately time-proportional computational steps:
<h6>pvm 的完整定义见附录 A，但为了理解上下文，我们将简要概述基本调用函数 Ψ。该函数计算执行最多指定 gas 单位（表示与时间大致成比例的计算步骤数）的 pvm 实例的最终状态，该实例由一些寄存器 (${⟦\mathbb{N}_R⟧}_{13}$) 和内存 ($\mathbb{M}$) 初始化。</h6>

<img width="1280" alt="image" src="https://github.com/harodggg/jam/assets/31732456/6c80a0ec-a804-4f4f-9a3a-b56e09efc37d">

We refer to the time-proportional computational steps as gas (much like in the YP) and limit it to a 64-bit quantity. We may use either $\mathbb{N}_G$ or $\mathbb{Z}_G$ to bound it, the first as a prior argument since it is known to be positive, the latter as a result where a negative value indicates an attempt to execute beyond the gas limit. Within the context of the pvm, $\xi ∈ \mathbb{N}_G$ is typically used to denote gas.
<h6>这里沿用了 YP（黄皮书）中的概念，将与时间大致成比例的计算步骤称为 gas（燃料），并将其限制为 64 位数值。我们可以使用 $\mathbb{N}_G$ 或者 $\mathbb{Z}_G$ 来限制 gas，前者用于先验参数，因为已知它是正值；后者用于结果，负值表示尝试执行的 gas 超过限制。在 pvm 上下文中，通常用 $\xi ∈ \mathbb{N}_G$ 表示 gas。</h6>

(31)

$$\mathbb{Z}\_G \equiv  \mathbb{Z}\_{-263:363},\mathbb{N}\_G \equiv \mathbb{N}_{264}, \mathbb{N}\_R \equiv \mathbb{N}\_{232}$$

It is left as a rather important implementation detail to ensure that the amount of time taken while computing the function Ψ(. . . , ξ, . . . ) has a maximum computation time approximately proportional to the value of ξ regardless of other operands.
<h6>在确保计算函数 Ψ(. . . , ξ, . . . ) 所花费的时间大致与 ξ 的值成正比（与其他操作数无关）的情况下，实现该函数的最大执行时间被留作了一个相当重要的实现细节。</h6>

The pvm is a very simple risc register machine and as such has 13 registers, each of which is a 32-bit integer,denoted $\mathbb{N}_R$. [^9] Within the context of the pvm, ω ∈ $⟦\mathbb{N}_R⟧\_{13}$ is typically used to denote the registers.

<h6>这比 RISC-V 的 16 个寄存器要少 3 个，但是编译器输出的程序代码实际使用的寄存器数量为 13 个，因为其中两个被操作系统占用，另一个固定为零。</h6>

(32) 

$$\mathbb{M} ≡ (\mathbf{V} \in \mathbb{Y}_{2^{32}},\mathbf{A} \in ⟦{\mathbf{W}, \mathbf{R}, \varnothing}⟧_{2^{32}})$$

The pvm assumes a simple pageable ram of 32-bit addressable octets where each octet may be either immutable, mutable or inaccessible. The ram definition M includes two components: a value V and access A. If the component is unspecified while being subscripted then the value component may be assumed. Within the context of the virtual machine, µ ∈ M is typically used to denote ram.

<h6>pvm 使用了一个简单的可分页 RAM，该 RAM 由 32 位可寻址的八位字节组成，每个字节可以是不可变的、可变的或不可访问的。RAM 定义 M 包含两个组件：值 V 和访问权限 A。如果在取下标时未指定组件，则可以假定为值组件。在虚拟机的上下文中，通常用 µ ∈ M 表示 RAM。</h6>

(33)

$$V_µ ≡ {i ∣ µ_A[i] ≠ ∅} \qquad V^∗_µ ≡ {i ∣ µ_A[i] = W}$$

We define two sets of indices for the ram µ: $\mathbb{V}_µ$ is the set of indices which may be read from; and $\mathbb{V}^∗_µ$ is the set of indices which may be written to
<h6>对于 RAM µ，我们定义了两组索引集合: $\mathbb{V}_µ$ 表示可以读取的索引集合; $\mathbb{V}^∗_µ$ 表示可以写入的索引集合。</h6>

Invocation of the pvm has an exit-reason as the first item in the resultant tuple. It is either:
<h6>pvm 的调用结果是一个元组，其第一个元素是退出原因。退出原因可以是以下几种情况之一:</h6>

* Regular program termination caused by an explicit halt instruction, ∎.
* Irregular program termination caused by some exceptional circumstance, ☇.
* Exhaustion of gas, ∞.
* A page fault (attempt to access some address in ram which is not accessible), F. This includes the address at fault.
* An attempt at progressing a host-call, h̵. This allows for the progression and integration of a context-dependent state-machine beyond the regular pvm.

* 程序正常终止，由显式 halt 指令引起，记为∎ (huàn).
* 程序因异常情况而异样终止，记为 ☇ (bǎi diàn).
* 燃气耗尽，记为 ∞ (wú xiàn)。
* 对于分页内存系统，页面错误 (F) 指的是尝试访问内存中不可访问的地址。
* 在这里，h̵ 表示尝试执行“主机调用”(host-call)。这允许在常规 pvm 之外进行与上下文相关的状态机的推进和集成。


The full definition follows in appendix A.
<h6>完整定义见附录 A</h6>

**4.8. Epochs and Slots.** Unlike the YP Ethereum with its proof-of-work consensus system, Jam defines a proof-ofauthority consensus mechanism, with the authorized validators presumed to be identified by a set of public keys and decided by a staking mechanism residing within some system hosted by Jam. The staking system is out of scope for the present work; instead there is an api which may be utilized to update these keys, and we presume that whatever logic is needed for the staking system will be introduced and utilize this api as needed.
<h6>4.8 时代和槽位. 与采用工作量证明共识机制的 YP 以太坊不同，Jam 定义了一种权益证明共识机制，授权验证者的身份由一组公钥标识，并由 Jam 托管的某个系统中的权益机制决定。权益机制超出本文讨论范围；取而代之的是，存在一个用于更新这些密钥的 API，我们假设权益系统所需的任何逻辑都将被引入并根据需要使用此 API。</h6>

The Safrole mechanism subdivides time following genesis into fixed length epochs with each epoch divided into E = 600 timeslots each of uniform length P = 6 seconds, given an epoch period of E ⋅ P = 3600 seconds or one hour.
<h6>Safrole 机制将时间划分成固定长度的时代，每个时代又分为 E = 600 个长度相同的时隙，给定时代周期为 E ⋅ P = 3600 秒或 1 小时，则每个时隙的长度 P 等于 6 秒。</h6>

This six-second slot period represents the minimum time between Jam blocks, and through Safrole we aim to strictly minimize forks arising both due to contention within a slot (where two valid blocks may be produced within the same six-second period) and due to contention over multiple slots (where two valid blocks are produced in different time slots but with the same parent).
<h6>这个六秒时隙周期代表 Jam 块之间的最短时间，通过 Safrole，我们的目标是严格最小化由于时隙内争用（其中可能在同一六秒周期内产生两个有效块）和由于原因而产生的分叉。争夺多个时隙（其中两个有效块在不同时隙中生成，但具有相同的父块）。</h6>

Formally when identifying a timeslot index, we use a 32-bit integer indicating the number of six-second timeslots from the Jam Common Era. For use in this context we introduce the set $\mathbb{N}_T$ :
<h6>正式地，为了标识一个时隙索引，我们使用一个 32 位整数，表示从 Jam 共同时代 (Jam Common Era) 开始的 6 秒时隙数量。为了在这个上下文中使用，我们引入集合 $\mathbb{N}_T$ :</h6>

(34)

$$\mathbb{N}_T \equiv \mathbb{N}\_{2^{32}}$$

This implies that the lifespan of the proposed protocoltakes us to mid-August of the year 2840, which with the current course that humanity is on should be ample.
<h6>这意味着拟议协议的有效期将我们带到 2840 年 8 月中旬，按照人类当前的进程，这个期限应该是足够的。</h6>

**4.9. The Core Model and Services.** Whereas in the Ethereum Yellow Paper when defining the state machine which is held in consensus amongst all network participants, we presume that all machines maintaining the full network state and contributing to its enlargement—or, at least, hoping to—evaluate all computation. This “everybody does everything” approach might be called the onchain consensus model. It is unfortunately not scalable, since the network can only process as much logic in consensus that it could hope any individual node is capable of doing itself within any given period of time.
<h6>4.9 核心模型与服务. 以太坊黄皮书在定义所有网络参与者达成共识的 状态机时，假设所有维护完整网络状态并扩展网络状态的机器 - 或者至少希望这样做的机器 - 都将评估所有的计算。这种 “每个人做所有事” 的方法可以称为链上共识模型。遗憾的是，这种方法不可扩展，因为网络在达成共识方面所能处理的逻辑量，最多只相当于其期望任何单个节点在给定时间内能够处理的逻辑量。</h6>

*4.9.1. In-core Consensus.* In the present work, we achieve scalability of the work done through introducing a second model for such computation which we call the in-core consensus model. In this model, and under normal circumstances, only a subset of the network is responsible for actually executing any given computation and assuring the availability of any input data it relies upon to others. By doing this and assuming a certain amount of computational parallelism within the validator nodes of the network, we are able to scale the amount of computation done in consensus commensurate with the size of the network, and not with the computational power of any single machine. In the present work we expect the network to be able to do upwards of 300 times the amount of computation in-core as that which could be performed by a single machine running the virtual machine at full speed.
<h6>4.9.1 内核共识。本节工作中，我们通过引入一种用于此类计算的第二种模型（称为内核共识模型）来实现工作可扩展性。在这个模型中，在正常情况下，网络中只有少部分节点负责实际执行任何给定的计算，并确保为其他节点提供任何它所依赖的输入数据可用性。通过这样做，并且假设网络中的验证器节点具有一定的计算并行性，我们能够使共识中完成的计算量与网络规模成比例地扩展，而不是受任何单个机器的计算能力限制。在本工作中，我们期望网络能够执行的内核计算量是单台机器全速运行虚拟机所能执行的计算量的 300 倍以上。</h6>

Since in-core consensus is not evaluated or verified by all nodes on the network, we must find other ways to become adequately confident that the results of the computation are correct, and any data used in determining this is available for a practical period of time. We do this through a crypto-economic game of three stages called guaranteeing, assuring, auditing and, potentially, judging. Respectively, these attach a substantial economic cost to the invalidity of some proposed computation; then a sufficient degree of confidence that the inputs of the computation will be available for some period of time; and finally, a sufficient degree of confidence that the validity of the computation (and thus enforcement of the first guarantee) will be checked by some party who we can expect to be honest.
<h6>由于内核共识并非由网络上的所有节点进行评估和验证，因此我们需要找到其他方法来充分确信计算结果的正确性，并且用于确定这一点的任何数据在一段实用期内都是可用的。我们通过一个名为“担保、确认、审计和 (潜在的) 仲裁”的三阶段密码经济博弈来实现这一点。具体来说，这些阶段分别为：为一些提议计算的无效性附加巨额经济成本；然后，确保计算的输入数据在一段时间内可用的足够程度的信心；最后，确保计算的有效性（以及因此对第一项担保的执行）将由我们期望诚实的某个一方进行检查的足够程度的信心。</h6>

All execution done in-core must be reproducible by any node synchronized to the portion of the chain which has been finalized. Execution done in-core is therefore designed to be as stateless as possible. The requirements for doing it include only the refinement code of the service, the code of the authorizer and any preimage lookups it carried out during its execution.
<h6>在内核中执行的所有操作都必须能够被任何同步到已完成部分链条的节点所重现。因此，内核执行被设计得尽可能无状态。执行它的先决条件只包含服务的相关精炼代码、授权器代码以及它在执行过程中进行的任何预镜像查找。</h6>

When a work-report is presented on-chain, a specific block known as the lookup-anchor is identified. Correct behavior requires that this must be in the finalized chain and reasonably recent, both properties which may be proven and thus are acceptable for use within a consensus protocol.
<h6>当工作报告在链上提交时，会引用一个特定的区块，称为查找锚点。正确行为要求该区块必须位于已经完成的链条中并且足够新近。这两个属性都可以被证明，因此适合在共识协议中使用。</h6>

We describe this pipeline in detail in the relevant sections later.
<h6>我们将在后面的相关章节详细描述此流程</h6>

*4.9.2. On Services and Accounts.* In YP Ethereum, we have two kinds of accounts: contract accounts (whose actions are defined deterministically based on the account’s associated code and state) and simple accounts which act as gateways for data to arrive into the world state and are controlled by knowledge of some secret key. In Jam, all accounts are service accounts. Like Ethereum’s contract accounts, they have an associated balance, some code and state. Since they are not controlled by a secret key, they do not need a nonce.
<h6>4.9.2 服务与账户.  在 YP 以太坊 (YP Ethereum) 中，存在两种类型的账户：合约账户（其行为由账户关联的代码和状态决定性地定义）和简单账户 （用作将数据引入世界状态的网关，并受秘密密钥的知识控制）。在 Jam 中，所有账户都是服务账户 。与以太坊的合约账户类似，它们也拥有关联的余额、代码和状态。由于它们不受秘密密钥控制，因此不需要序号 (nonce)。</h6>

The question then arises: how can external data be fed into the world state of Jam? And, by extension, how does overall payment happen if not by deducting the account balances of those who sign transactions? The answer to the first lies in the fact that our service definition actually includes multiple code entry-points, one concerning refinement and the other concerning accumulation. The former acts as a sort of high-performance stateless processor, able to accept arbitrary input data and distill it into some much smaller amount of output data. The latter code is more stateful, providing access to certain on-chain functionality including the possibility of transferring balance and invoking the execution of code in other services. Being stateful this might be said to more closely correspond to the code of an Ethereum contract account.
<h6>那么问题来了：如何将外部数据引入 Jam 的世界状态？延伸来讲，如果没有通过扣减签名交易的账户余额，那么整体支付又是如何发生的？对于第一个问题的回答在于，我们的服务定义实际上包含了多个代码入口点，一个用于精炼 ，另一个用于累积。精炼部分充当一种高性能无状态处理器，能够接受任意输入数据并将其压缩成更少量的输出数据。累积部分则更具状态性，可以访问链上某些功能，例如转账以及调用其他服务中代码的执行。由于具有状态性，可以说它更接近以太坊合约账户的代码。</h6>

To understand how Jam breaks up its service code is to understand Jam’s fundamental proposition of generality and scalability. All data extrinsic to Jam is fed into the refinement code of some service. This code is not executed on-chain but rather is said to be executed incore. Thus, whereas the accumulator code is subject to the same scalability constraints as Ethereum’s contract accounts, refinement code is executed off-chain and subject to no such constraints, enabling Jam services to scale dramatically both in the size of their inputs and in the complexity of their computation.
<h6>对 Jam 服务代码的划分方式理解透彻，就相当于理解了 Jam 的通用性和可扩展性的这一根本命题。所有外部于 Jam 的数据都将被馈送到某个服务的精炼代码中。这段代码不会链上执行，而是被称作内核执行。因此，累积器代码同以太坊合约账户一样，会受到可扩展性的限制，而精炼代码则可以链下执行，不受此限制。这使得 Jam 服务在输入规模和计算复杂度上都能实现巨大的扩展。</h6>

While refinement and accumulation take place in consensus environments of a different nature, both are executed by the members of the same validator set. The Jam protocol through its rewards and penalties ensures that code executed in-core has a comparable level of cryptoeconomic security to that executed on-chain, leaving the primary difference between them one of scalability versus synchroneity.
<h6>精炼和累积虽然在本质上不同的共识环境中执行，但它们都由同一个验证者集合的成员执行。Jam 协议通过奖励和惩罚机制，确保内核执行的代码具有与链上执行的代码相当的密码经济安全性，使得它们之间的主要区别在于可扩展性与同步性。</h6>

As for managing payment, Jam introduces a new abstraction mechanism based around Polkadot’s Agile Coretime. Within the Ethereum transactive model, the mechanism of account authorization is somewhat combined with the mechanism of purchasing blockspace, both relying on a cryptographic signature to identify a single “transactor” account. In Jam, these are separated and there is no such concept of a “transactor”.
<h6>关于支付管理，Jam 围绕波卡的敏捷核心时间 (Polkadot's Agile Coretime) 引入了一种新的抽象机制。在以太坊的交易模型中，账户授权机制在某种程度上与购买区块空间的机制结合在一起，两者都依赖于密码签名来识别单个“交易发送者” (transactor) 账户。在 Jam 中，这些概念被分开，并且没有“交易发送者”的概念。</h6>

In place of Ethereum’s gas model for purchasing and measuring blockspace, Jam has the concept of coretime, which is prepurchased and assigned to an authorization agent. Coretime is analogous to gas insofar as it is the underlying resource which is being consumed when utilizing Jam. Its procurement is out of scope in the present work and is expected to be managed by a system parachain operating within a parachains service itself blessed with a number of cores for running such system services. The authorization agent allows external actors to provide input to a service without necessarily needing to identify themselves as with Ethereum’s transaction signatures. They are discussed in detail in section 8.
<h6>替代以太坊用于购买和衡量区块空间的 gas 模型，Jam 引入了核心时间 (héshín shíjiān) 的概念。核心时间预先购买并分配给授权代理。核心时间类似于 gas，它是利用 Jam 时所消耗的基础资源。其获取过程超出了本文讨论范围，预计将由平行链服务内运行的平行链系统来管理，该系统本身也拥有用于运行此类系统服务的核心数量。授权代理允许外部参与者向服务提供输入，而无需像以太坊的交易签名那样必须识别自己身份。有关授权代理的详细信息将在第 8 节中进行详细讨论。</h6>

  <h3 align="center">5. The Header</h3>

    
We must first define the header in terms of its components. The header comprises a parent hash and prior state root ($\mathbb{H}_p$ and $\mathbb{H}_r$), an extrinsic hash $\mathbb{H}_x$, a timeslot index $\mathbb{H}_t$, the epoch, winning-tickets and judgements markers $\mathbb{H}_e$, $\mathbb{H}_w$ and $\mathbb{H}_j$ , a Bandersnatch block author key $\mathbb{K}_k$ and two Bandersnatch signatures; the entropyyielding vrf signature $\mathbb{H}_v$ and a block seal $\mathbb{H}_s$. Headers may be serialized to an octet sequence with and without the latter seal component using $\varepsilon$ and $\varepsilon_U$ respectively. Formally:
<h6>为了理解区块头，我们首先需要定义其组成部分。区块头包含以下信息：父哈希( $\mathbb{H}_p$ )和前一状态根 ( $\mathbb{H}_r$ ),外部哈希 ( $\mathbb{H}_x$ ),时隙索引 ( $\mathbb{H}_t$ ),纪元 ( $\mathbb{H}_e$ ), 获奖票根 ( $\mathbb{H}_w$ ) 和判决根 ( $\mathbb{H}_j$ ) 的标记 生成该区块的班德斯纳奇块作者密钥 ( $\mathbb{K}_k$  ) 两个班德斯纳奇签名：熵生成型 VRF 签名 ( $\mathbb{H}_v$ ) 和区块密封 ( $\mathbb{H}_s$ )区块头可以序列化为包含或不包含后一个密封组件的八位字节序列，分别记为 $\varepsilon$  和 $\varepsilon_U$ 。形式上： </h6>

(35)

$$\mathbb{H} \equiv (\mathbb{H}_p,\mathbb{H}_r,\mathbb{H}_x,\mathbb{H}_t,\mathbb{H}_e,\mathbb{H}_w,\mathbb{H}_j ,\mathbb{H}_k,\mathbb{H}_v,\mathbb{H}_s)$$

Blocks considered invalid by this rule may become valid as $\tau$ advances.
<h6>根据这条规则，一些现在被认为无效的区块可能随着 τ 的增加而变得有效。</h6>

The blockchain is a sequence of blocks, each cryptographically referencing some prior block by including a hash derived from the parent’s header, all the way back to some first block which references the genesis header. We already presume consensus over this genesis header $\mathbb{H}^0$ and the state it represents defined as $\sigma^0$ .
<h6>区块链是一个由区块序列组成的结构。每个区块都通过包含其父区块头部的哈希值来引用之前的某个区块，一直追溯到引用创世区块头的第一个区块。我们已经假定就这个创世区块头 $\mathbb{H}^0$ 及其代表的状态 $\sigma^0$ 达成了共识。</h6>

Excepting the Genesis header, all block headers $\mathbf{H}$ have an associated parent header, whose hash is $\mathbf{H}_p$ . We denote the parent header $\mathbf{H}^− = \mathit{P}(\mathbf{H})$
<h6>除创世区块头之外，所有区块头 $\mathbf{H}$ 都具有一个关联的父区块头，其哈希值为 $\mathbf{H}_p$ 。我们记父区块头为 $\mathbf{H}^− = \mathit{P}(\mathbf{H})$ 。</h6>

(36)

$$\mathbf{H}_{\mathit{p}} \in \mathbb{H}, \mathbf{H}\_{\mathit{p}} \equiv \mathcal{H}(\mathit{P}(\mathbf{H}))$$

$\mathit{P}$ is thus defined as being the mapping from one block header to its parent block header. With $\mathit{P}$ , we are able to define the set of ancestor headers $mathbf{A}$ :
<h6>那么 P 可以定义为从一个区块头映射到其父区块头的函数。有了 P 函数，我们就可以定义祖先区块头集合 A 为：</h6>

(37)

$$ \mathcal{h}  \in \mathbf{A} \Leftrightarrow \mathcal{h} = \mathbf{H} \vee (\exists_i \in \mathbf{A} : \mathcal{h} = \mathit{P}(i)) $$

We only require implementations to store headers of ancestors which were authored in the previous $\mathbf{L}$ = 24 hours of any block $\mathbf{B}$ they wish to validate.
<h6>对于任何要验证的区块 B，我们仅要求实现去存储那些在该区块的前 L = 24 小时内生成的祖先区块头。</h6>

The extrinsic hash is the hash of the block’s extrinsic data. Given any block $\mathbf{B} = (\mathbf{H},\mathbf{E})$ , then formally:
<h6>外部哈希是区块的外部数据（extrinsic data）的哈希值。对于任何区块 B=(H,E) ，我们可以形式化地表示为：</h6>

(38)

$$ \mathbf{H}_x \in \mathbb{H} , \mathbf{H}_x \equiv \mathcal{H}(\varepsilon (E)) $$ 

A block may only be regarded as valid once the timeslot index $\mathbf{H}_t$ is in the past. It is always strictly greater than that of its parent. Formally
<h6>任何区块只有在它的时隙索引 $\mathbf{H}_t$ 代表的时刻已经过去之后才能被视为有效。并且该时隙索引总是严格大于其父区块的时隙索引。形式上表示为</h6>

(39)

$$ \mathbf{H}_t ∈ \mathbf{N}_T , \mathit{P}(\mathbf{H})_t < \mathbf{H}_t ∧ \mathbf{H}_t ⋅ \mathbf{P} ≤ \tau $$

The parent state root $\mathbf{H}_r$ is the root of a Merkle trie composed by the mapping of the prior state’s Merkle root, which by definition is also the parent block’s posterior state. This is a departure from both Polkadot and the Yellow Paper’s Ethereum, in both of which a block’s header contains the posterior state’s Merkle root. We do this to facilitate the pipelining of block computation and in particular of Merklization
<h6>父状态根 ( $\mathbf{H}_r$ ) 是一个 Merkle 树的根节点，该 Merkle 树由先前状态的 Merkle 根 (根据定义也是父区块的后继状态) 的映射组成。这与 Polkadot 和 Yellow Paper 提到的以太坊不同，在这两者中，区块头都包含后继状态的 Merkle 根。我们这样做是为了方便区块计算的流水线处理，尤其是 Merkle 化过程。</h6>

(40)

$$\mathbf{H}_r ∈ \mathbb{H} , \mathbf{H}_r ≡ \mathcal{M}_S(\sigma )$$

We assume the state-Merklization function $\mathcal{M}_S$ is capable of transforming our state σ into a 32-octet commitment. See appendix D for a full definition of these two functions.
<h6>我们假设状态哈希化函数 $\mathcal{M}_S$ 能将我们的状态 σ 转换为一个 32 字节的承诺值。有关这两个函数的完整定义，请参见附录 D。</h6>

All blocks have an associated public key to identify the author of the block. We identify this as an index into the current validator set $\kappa$ . We denote the Bandersnatch key of the author as $\mathbf{H}_a$ though note that this is merely an equivalence, and is not serialized as part of the header.
<h6>所有区块都关联着一个公钥，用于识别区块的作者。我们将此公钥标识为当前验证者集合 $\kappa$ 中的索引值。我们用 $\mathbf{H}_a$ 表示作者的班德斯纳奇密钥，需要注意这仅仅是一种等价关系，该密钥不会被序列化为头部的一部分。</h6>

(41)

$$\mathbf{H}_\kappa ∈ \mathbb{N}_V , \mathbf{H}_a \equiv  \kappa[\mathbf{H}_k]$$

5.1. The Epoch and Winning Tickets Markers. If not ∅, then the epoch marker specifies key and entropy relevant to the following epoch in case the ticket contest does not complete adequately (a very much unexpected eventuality). Similarly, the winning-tickets marker, if not ∅, provides the series of 600 slot sealing “tickets” for the next epoch (see the next section):
<h6>5.1 时代标记和中奖门票标记。如果不为空 (∅)，则时代标记会指定与下个时代相关的密钥和熵，以防票券竞赛无法正常完成（这种情况极少见）。类似地，中奖门票标记（如果非空）则会提供一系列用于下一时代密封区块的 600 个插槽“票券”（请参阅下一节）。</h6>

(42)

$$\mathbf{H}_e ∈ (\mathbb{H}, ⟦\mathbb{H}_B⟧_V )? , \mathbf{H}w ∈ ⟦\mathbb{C}⟧_E $$

The terms are fully defined in section 6.6.
<h6>相关术语的完整定义请参见第 6.6 节。</h6>

<h5 align="center">6. Block Production and Chain Growth</h5>
<h6>6. 区块生产和链增长</h6>

As mentioned earlier, Jam is architected around a hybrid consensus mechanism, similar in nature to that of Polkadot’s Babe/Grandpa hybrid. Jam’s block production mechanism, termed Safrole after the novel Sassafras production mechanism of which it is a simplified variant, is a stateful system rather more complex than the Nakamoto consensus described in the *YP*
<h6>如前所述，Jam 采用了一种混合共识机制，类似于 Polkadot 的 Babe/Grandpa 混合机制。Jam 的区块生产机制被称为 Safrole，以对其所简化变体的新型 Sassafras 生产机制命名，它是一种比 YP 文档中描述的 Nakamoto 共识更复杂的状态ful 系统。</h6>

The chief purpose of a block production consensus mechanism is to limit the rate at which new blocks may be authored and, ideally, preclude the possibility of “forks”: multiple blocks with equal numbers of ancestors.
<h6>区块生产共识机制的主要目的在于限制生成新区块的速率，并且理想情况下防止出现“分叉”：即拥有相同数量祖先区块的多个区块并存的局面。</h6>



To achieve this, Safrole limits the possible author of any block within any given six-second timeslot to a single key-holder from within a prespecified set of validators. Furthermore, under normal operation, the identity of the key-holder of any future timeslot will have a very high degree of anonymity. As a side effect of its operation, we can generate a high-quality pool of entropy which may be used by other parts of the protocol and is accessible to services running on it.
<h6>为了实现这一点，Safrole 在任何给定的 6 秒时间段内，将可能的新区块创作者限制为预先指定验证者集合中的单个密钥持有者。此外，在正常运行下，任何未来时间段的密钥持有者身份都将具有非常高的匿名性。作为其运行的副作用，我们可以生成高质量的熵池，该熵池可供协议的其他部分使用，并且对运行在其上的服务可用。</h6>

Because of its tightly scoped role, the core of Safrole’s state, γ, is independent of the rest of the protocol. It interacts with other portions of the protocol through ι and
$\kappa$ , the prospective and active sets of validator keys respectively; $\tau$ , the most recent block’s timeslot; and η, the entropy accumulator.
<h6>由于 Safrole 的核心状态 γ 仅负责有限的功能，因此它独立于协议的其他部分。它通过以下几个元素与协议的其他部分进行交互,ι 和 κ：分别代表候选验证者密钥集合和当前活跃的验证者密钥集合。τ：表示最新区块所属的时隙。η：表示熵累积器。</h6>

The Safrole protocol generates, once per epoch, a sequence of $\mathbf{E}$ sealing keys, one for each potential block within a whole epoch. Each block header includes its timeslot index $\mathbf{H}_t$ (the number of six-second periods since the Jam Common Era began) and a valid seal signature Hs, signed by the sealing key corresponding to the timeslot within the aforementioned sequence. Each sealing key is in fact a pseudonym for some validator which was agreed the privilege of authoring a block in the corresponding timeslot.
<h6>Safrole 协议会为每个时代生成一组密封密钥序列 $\mathbf{E}$ ，数量等于该时代内所有潜在区块的个数。每个区块头包含以下信息：时隙索引 $\mathbf{H}_t$ ：表示自 Jam 共同纪元 (Jam Common Era) 开始以来经过的 6 秒间隔数。密封签名 Hs ：由对应于上述序列中时隙的密封密钥签名，证明该区块属于正确的时间段。</h6>

In order to generate this sequence of sealing keys, and in particular to do so without making public the correspondence relation between them and the validator set, we use a novel cryptographic structure known as a Ringvrf, utilizing the Bandersnatch curve. Bandersnatch Ringvrf allows for a proof to be provided which simultaneously guarantees the author controlled a key within a set (in our case validators), and secondly provides an output, an unbiasable deterministic hash giving us a secure verifiable random function (vrf) and as a means of determining which validators are able to author in which slots.

<h6>为了生成这组密封密钥序列，并且尤其要避免公开密钥序列与验证者集合之间的对应关系，我们使用了一种称为“环签名验证函数 (RingVRF)” 的新型密码结构，该结构利用了班德斯纳奇曲线 (Bandersnatch curve)。环签名验证函数允许提供一种证明，该证明可以同时保证以下两点：作者控制着集合内的一个密钥（在本例中是验证者集合）。生成一个输出，即无偏的确定性哈希值，为我们提供了一个安全的可验证随机函数 (VRF)，并用作确定哪些验证者可以在哪些时隙内创作区块的工具。</h6>

**6.1. Timekeeping.** Here, $\tau$ defines the most recent block’s slot index, which we transition to the slot index as defined in the block’s header: 
<h6>6.1. 时间维护。符号 τ 在此处表示最新区块的槽位索引，我们将其转换为主块头文件中定义的槽位索引。</h6>
(43)

$$\tau ∈ \mathbb{N}_T , \tau^′\equiv \mathbf{H}_t$$

We track the slot index in state as $\tau$ in order that we are able to easily both identify a new epoch and determine the slot at which the prior block was authored. We denote e as the prior’s epoch index and m as the prior’s slot phase index within that epoch and $e^'$ and $m^'$ are the corresponding values for the present block:
<h6>
我们在状态中使用 τ 跟踪槽位索引，以便于轻松识别新 epoch 并确定上一个区块被创作的槽位。我们用 e 表示前一个区块的 epoch 索引，用 m 表示前一个区块在该 epoch 中的槽位相位索引，用 e和 m ‘表示当前区块的对应值：</h6>
(44)

$$ let \qquad  e \quad R \quad m = \frac{\tau}{E}, e^′ \quad R \quad m^′=\frac{\tau}{{E'}} $$

**6.2. Safrole Basic State.** We restate γ into a number of components:
<h6>6.2. Safrole 基础状态。为了更清楚地理解熵累加器如何工作，我们将符号 γ 分解成几个组件：</h6>

(45)

$$ \gamma  \equiv (\gamma_k, \gamma_z, \gamma_s, \gamma_a) $$

$\gamma_z$ is the epoch’s root, a Bandersnatch ring root composed with the one Bandersnatch key of each of the next epoch’s validators, defined in $\gamma_k$ (itself defined in the next section).
<h6>γ z代表 epoch 的根，它是由下一 epoch 所有验证者的单个 Bandersnatch 密钥（在 γ k中定义，下一节将详细介绍 γ k）构成的 Bandersnatch 环根</h6>

(46)

$$\gamma_z \in \mathbb{Y}_R $$

Finally, γa is the ticket accumulator, a series of highestscoring ticket identifiers to be used for the next epoch. $\gamma_a$ is the current epoch’s slot-sealer series, which is either a full complement of $\mathit{E}$ tickets or, in the case of a fallback mode, a series of $\mathif{E}$ Bandersnatch keys:
<h6>最后，γ a表示票据累加器，它是一系列将在下一个 epoch 中使用的最高分票据标识符。γ s表示当前 epoch 的槽位密封器序列，它可以是完整的 E 个票据，或者在回退模式下，它是一系列 $\mathif{E}$ 个 Bandersnatch 密钥：</h6>

(47)

$$ \gamma_a \in ⟦\mathbb{C}⟧_{∶E}, \gamma_s \in ⟦\mathbb{C}⟧_E ∪ ⟦\mathbb{H}_B⟧_E $$

Here, $\mathbb{C}$ is used to denote the set of tickets, a combination of a verifiably random ticket identifier y and the ticket’s entry-index r:
<h6>这里，符号 C 表示票据集合，它是由可验证的随机票据标识符 y 和票据的条目索引 r 组成的一个组合：</h6>
(48)

$$ \mathbb{C} \equiv (y \in \mathbb{H}, r \in  \mathbb{N}_N) $$

As we state in section 6.4, Safrole requires that every block header $\mathbf{H}$ contain a valid seal Hs, which is a Bandersnatch signature for a public key at the appropriate index m of the current epoch’s seal-key series, present in state as $\gamma_s$.
<h6>正如我们在 6.4 节所述，Safrole 要求每个区块头 H 都包含一个有效的密封 Hs, 它是一个针对当前 epoch 密钥序列（存储在状态中的 γ s ）中相应索引位置 m 的公钥的 Bandersnatch 签名。</h6>

**6.3. Key Rotation.** In addition to the active set of validator keys κ and staging set ι, internal to the Safrole state we retain a pending set $\gamma_k$. The active set is the set of keys identifying the nodes which are currently privileged to author blocks and carry out the validation processes, whereas the pending set $\gamma_k$, which is reset to ι at the beginning of each epoch, is the set of keys which will be active in the next epoch and which determine the Bandersnatch ring root which authorizes tickets into the sealing-key contest for the next epoch.
<h6>6.3. 密钥轮换.除了激活验证器密钥集合 κ 和暂存集合 ι 之外，Safrole 内部状态还保留了一个待处理集合 γ k。激活集合是标识当前被授权创作区块和执行验证过程的节点的密钥集合，而待处理集合 γ k会在每个 epoch 开始时重置为 ι）则包含下一个 epoch 将激活的密钥集合，它决定了授权下一 epoch 密钥竞争（用于生成密封密钥）的票据的 Bandersnatch 环根。</h6>

(49)

$$\iota  \in ⟦\mathbb{K}⟧_V, \gamma_k \in ⟦\mathbb{K}⟧_V, \kappa  \in  ⟦\mathbb{K}⟧_V, \lambda  \in ⟦\mathbb{K}⟧_V$$

We must introduce $\mathbb{K}$, the set of validator key tuples. This is a combination of cryptographic public keys for Bandersnatch and Ed25519 cryptography, and a third metadata key which is an opaque octet sequence, but utilized to specify practical identifiers for the validator, not least a hardware address
<h6>我们需要引入验证器密钥元组集合 K。它包含用于 Bandersnatch 和 Ed25519 加密的密码学公钥，以及第三个元数据密钥，该密钥是一个不透明的八位字节序列，但用于指定验证器的实用标识符，尤其是硬件地址。</h6>

The set of validator keys itself is equivalent to the set of 176-octet sequences. However, for clarity, we divide the sequence into four easily denoted components. For any
validator key $v$, the Bandersnatch key is denoted $v_b$, and is equivalent to the first 32-octets; the Ed25519 key, $v_e$, is the second 32 octets; the BLS key denoted $v_{BLS}$ is equivalent to the following 144 octets, and finally the metadata $v_m$ is the last 128 octets. Formally:
<h6>validator key 的集合本身等于 176 个八位字节序列的集合。但是，为了清晰起见，我们将序列划分成四个易于表示的组件。对于任何验证器密钥 v，Bandersnatch 密钥表示为 v b ，等于前 32 个八位字节。Ed25519 密钥表示为 v e ，是第二个 32 个八位字节。BLS 密钥表示为 v BLS ，等于接下来的 144 个八位字节。最后，元数据表示为 v m，是最后的 128 个八位字</h6>
(50)

$$ \mathbb{K} ≡ \mathbb{Y}_{336}$$

(51)

$$∀_v \in \mathbb{K} ∶ v_b \in \mathbb{H}_B ≡ v_0⋅⋅⋅+32$$

(52)

$$∀_v \in  \mathbb{K} ∶ v_e \in \mathbb{H}_E ≡ v_{32}⋅⋅⋅+32$$

(53)

$$∀_v \in \mathbb{K} ∶ v_{BLS} \in  \mathbb{Y}\_{BLS} ≡ v_{64}⋅⋅⋅+144$$

(54)

$$∀_v \in \mathbb{K} ∶ v_m ∈ \mathbb{Y}\_{208} ≡ v_{208}⋅⋅⋅+128$$

With a new epoch under regular conditions, validator keys get rotated and the epoch’s Bandersnatch key root is updated into $\gamma^'_z:
<h6>在常规情况下进入新 epoch 时，验证器密钥会进行轮换，并且 epoch 的 Bandersnatch 环根会更新为 $\gamma^'_z$：</h6>

```math
(\gamma'_k,k',\lambda , \gamma^′_z) ≡

\left\{\begin{matrix}
 (\iota , N(γ_k), N(κ), z) & if \quad ' > e ∧ \mathbf{H}_J ≠ [] \\
(γk, N(κ), N(λ), γz)  & otherwise
\end{matrix}\right.
```

(55)

```math
\begin{matrix}
 where \quad z = R([k_b ∣ k <− \gamma ^′_k])\\
 and \quad N(k) ≡ \begin{bmatrix}
\left.\begin{matrix}
 [0, 0, . . . ] & if k_e ∈ \psi ^′_p\\
 k & otherwise
\end{matrix}\right\} \mid k < - k
\end{bmatrix}
\end{matrix}
```

Note that the posterior active validator key set κ' is defined such that keys belonging to the historical judgement punish set $ψ^′_p$ are replaced with a null key containing only zeroes. The origin of this punish set is explained in section 10.
<h6>需要注意的是，后验激活验证器密钥集合 κ' 的定义方式是，将属于历史惩罚集合 ψ p的密钥替换为仅包含零的空密钥。惩罚集合的来源将在第 10 节进行解释。</h6>

**6.4. Sealing and Entropy Accumulation.** The header must contain a valid seal and valid vrf output. These are two signatures both using the current slot’s seal key; the message data of the former is the header’s serialization omitting the seal component $\mathbf{H}_s$, whereas the latter is used as a bias-resistant entropy source and thus its message must already have been fixed: we use the entropy stemming from the vrf of the seal signature. Formally:
<h6>6.4. 密封和熵积累。区块头必须包含一个有效的密封和有效的 vrf 输出。这两个都是使用当前槽位的密封密钥生成的签名：前者的消息数据是区块头的序列化，但省略了密封组件 H s 。后者用作抗偏差熵源，因此其消息必须已经固定：我们使用来自密封签名 vrf 的熵。</h6>

$$let \quad i = \gamma^′_s[\mathbf{H}_t]^\circlearrowleft :$$

(56)
```math
\gamma^′_s ∈ ⟦\mathbb{C}⟧  \Longrightarrow  \left\{\begin{matrix}
i_y = \mathcal{Y}(\mathbf{H}_s) , \\
H_s \in \mathbb{F}^{\varepsilon \frown _U(H)}_{H_a}⟨X_S \frown \eta'_3 \mp i_r⟩ , \\
T = 1
\end{matrix}\right \}
```

(57)

```math
\gamma^′_s \in ⟦\mathbb{H}_B⟧ \Longrightarrow \left\{\begin{matrix}
  i = \mathbf{H}_a ,  \\
  H_s \in \mathbb{F}^{\varepsilon _U(H)}_{H_a}⟨X_F \frown \eta'_3 ⟩ ,\\
  \mathbf{T} = 0

\end{matrix}\right.
```

(58)

$$ \mathbf{H}_v ∈ \mathbb{F}^{[]}_{H_a}⟨X_E  \frown \mathcal{Y}(H_s)⟩ $$

(59)

$$ \mathbf{X}_E = $jam_entropy $$

(60)

$$ X_F = $jam\_fallback\_seal $$

(61)

$$ X_S = $jam\_seal $$

Sealing using the ticket is of greater security, and we utilize this knowledge when determining a candidate block on which to extend the chain, detailed in section 15. We
thus note that the block was sealed under the regular security with the boolean marker $\mathbf{T}$. We define this only for the purpose of ease of later specification.
<h6>使用票据进行密封可以提供更高的安全性，我们在第 15 节详细介绍了确定用于扩展链的候选区块时如何利用这一特性。因此，我们注意到该区块使用常规安全性和布尔标记 T 进行密封。我们仅为方便后续规范而定义这一点。</h6>

In addition to the entropy accumulator $η_0$, we retain three additional historical values of the accumulator at the point of each of the three most recently ended epochs, $η_1$, $η_2$ and $η_3$. The second-oldest of these $η_2$ is utilized to help ensure future entropy is unbiased (see equation 62) and seed the fallback seal-key generation function with randomness (see equation 65). The oldest is used to regenerate this randomness when verifying the seal above.
<h6>除当前的熵累加器 η_0，我们还保留了最近结束的三个 epoch 时刻的累加器的另外三个历史值，分别记为 η_1、η_2 和 η_3。其中 η_2 用于帮助确保未来的熵没有偏差（见公式 62），并为备用密封密钥生成函数注入随机性（见公式 65）。η_1 用于在验证上述密封时重新生成此随机性。</h6>

(62)

$$ η ∈ ⟦H⟧_4$$

$η_0$ defines the state of the randomness accumulator to which the provably random output of the vrf, the signature over some unbiasable input, is combined each block. $η_1$ and $η_2$ meanwhile retain the state of this accumulator at the end of the two most recently ended epochs in order.
<h6>$η_0$ 定义了随机数累加器的状态，每个区块都会将可证明随机的 vrf 输出和对不可预测输入的签名与该状态进行组合。$η_1$ 和 $η_2$ 则分别保存了最近结束的两个 epoch 周期末尾的随机数累加器状态。</h6>

(63)

$$η'_0 ≡ \mathcal{H}(η_0 \frown \mathcal{Y}(H_v)) $$

On an epoch transition (identified as the condition e′ > e), we therefore rotate the accumulator value into the history $η_1$, $η_2$ and $η_3$:  
<h6>当 epoch 周期发生切换时（记为 e′ > e），我们将累加器值依次旋转到历史记录 $η_1$, $η_2$ and $η_3$:  中。</h6>

(64)

```math

(η^′_1, η^′_2, η^′_3) \equiv 
\left\{\begin{matrix}
 (η_0, η_1, η_2) & if \quad e^′> e\\
 (η_1, η_2, η_3) & otherwise
\end{matrix}\right.

```

**6.5. The Slot Key Sequence.** (( The posterior slot key sequence $\gamma^′_s$ is one of three expressions depending on the circumstance of the block. If the block is not the first in an epoch, then it remains unchanged from the prior $\gamma_s$. If the block signals the next epoch (by epoch index) and the previous block’s slot was within the closing period of the previous epoch, then it takes the value of the prior ticket accumulator $
gamma^′_a$. Otherwise, it takes the value of the fallback key sequence. Formally:

<h6>6.5 槽位密钥序列。后验槽位密钥序列 $\gamma^′_s$ 取决于区块的具体情形，有三种表达式可以表示。 如果区块不是 epoch 的首个区块，则它保持不变，和前一个区块的槽位密钥序列 $\gamma_s$ 相同。 如果区块通过 epoch 索引指示下一个 epoch 开始，并且前一个区块的槽位落在上一个 epoch 的 closing period 内，那么它将取前一个票据累加器 $gamma^′_a$ 的值。其他情况，它将取备用密钥序列的值。</h6>

(65)

```math
\left\{\begin{matrix}
 Z(\gamma_a) & if \quad e′= e + 1 ∧ m′ ≥ Y ∧ ∣\gamma_a∣ = E\\
\gamma^s  & if \quad e^′ = e\\
\mathbf{F}(\gamma^′_2, \aleph^′)  & otherwise 
\end{matrix}\right.
```

Here, we use Z as the inside-out sequencer function, defined as follows:
<h6>这里，我们使用 Z 函数作为内部翻转序列器函数，其定义如下：</h6>
(66)

```math
Z∶\left\{\begin{matrix}
⟦\mathbb{C}⟧_E \mapsto  ⟦\mathbb{C}⟧_E   \\
 s  \mapsto [s_0, s_{∣s∣−1}, s_1, s_{∣s∣−2}, . . . ]

\end{matrix}\right.
```
Finally, F is the fallback key sequence function which selects an epoch’s worth of validator Bandersnatch keys($⟦H_B⟧_E$) at random from the validator key set k using the entropy collected on-chain r:
<h6>最终，F 是备用密钥序列函数，它使用链上收集到的熵 r，从验证器密钥集 k 中随机选择一个 epoch 周期的验证器 Bandersnatch 密钥（记为 $⟦H_B⟧_E$ ）。</h6>
(67)

```math
F∶ \left\{\begin{matrix}
(\mathbb{H}, ⟦\mathbb{K}⟧)→ ⟦\mathbb{H}_B⟧_E \\

(r, k) \longmapsto  [k[\varepsilon ^{−1} (H_4(r \frown \varepsilon_4(i)))]^↺_b ∣ i ∈ N_E]
\end{matrix}\right.

```

**6.6. The Markers.** The epoch and winning-tickets markers are information placed in the header in order to minimize data transfer necessary to determine the validator keys associated with any given epoch. They are particularly useful to nodes which do not synchronize the entire state for any given block since they facilitate the secure tracking of changes to the validator key sets using only the chain of headers.
<h6>6.6 标志 。区块头信息中包含 epoch 标记和中奖票据标记，这些信息旨在减少确定给定 epoch 相关验证器密钥所需的数据传输。 对于那些不针对每个区块同步整个状态的节点而言，epoch 标记和中奖票据标记尤其有用，因为它们仅使用区块头链就可以安全地跟踪验证器密钥集的更改</h6>

As mentioned earlier, the header’s epoch marker $H_e$ is either empty or, if the block is the first in a new epoch, then a tuple of the epoch randomness and a sequence of Bandersnatch keys defining the Bandersnatch validator keys ($k_b$) beginning in the next epoch. Formally:
<h6>承接之前的说法，区块头的 epoch 标记 $H_e$ 可以为空，或者如果该区块是新 epoch 的第一个区块，则它是一个包含 epoch 随机数和一系列定义后续 epoch 中的 Bandersnatch 验证器密钥 ( $k_b$ ) 的 Bandersnatch 密钥序列的元组。形式化描述如下：</h6>

(68)

```math
H_e \equiv \left\{\begin{matrix}
  (η^′_1, [k_e ∣ k <- γ^′_k])  & if \quad e^′> e \\
  \varnothing & otherwise
\end{matrix}\right.
```

The winning-tickets marker $H_w$ is either empty or, ifthe block is the first after the end of the submission period for tickets and if the ticket accumulator is saturated, then the final sequence of ticket identifiers. Formally:
<h6>中奖票据标记 $H_w$ 可以为空，或者，如果该区块是票据提交周期结束后紧接的第一个区块，并且票据累加器已经饱和，那么它将包含最终的票据标识符序列。形式化描述如下：</h6>

(69)

```math
H_w \equiv \left\{\begin{matrix}
 Z(γ_a) & if \quad e^′ = e ∧ m < Y ≤ m^′ ∧ ∣γ_a∣ = E \\
  \varnothing & otherwise 
\end{matrix}\right.
```

Note that this will not be honored if the next epoch begins with a judgement in its extrinsic.
<h6>这里需要注意的是，如果下一个 epoch 的 extrinsic 中包含仲裁 结果，那么这个中奖票据标记将不会被认可 。</h6>



**6.7. The Extrinsic and Tickets.** The extrinsic $E_T$ is a sequence of proofs of valid tickets; a ticket implies an entry in our epochal “contest” to determine which validators are privileged to author a block for each timeslot in the following epoch. Tickets specify an ephemeral key and an entry index, both of which are elective, together with a proof of the ticket’s validity. The proof implies a ticket identity, a high-entropy unbiasable 32-octet sequence, which is used both as a score in the aforementioned contest and as input to the on-chain vrf.

<h6>外部数据。 $E_T$ 是一个有效的票据证明序列。票据代表了我们 epoch 周期的“竞赛”参赛凭证，该竞赛用于确定哪些验证器有权为下一个 epoch 的每个时隙创作区块。票据指定了一个临时密钥和一个条目索引（两者都是可选的），以及票据有效性的证明。该证明隐含了票据标识符，这是一个用于上述竞赛评分和链上可验证随机函数 (vrf) 输入的高熵不可预测的 32 字节序列。</h6>

Towards the end of the epoch (i.e. $\mathbf{Y}$ slots from the start) this contest is closed implying successive blocks within the same epoch must have an empty tickets extrinsic. At this point, the following epoch’s seal key sequence becomes fixed.
<h6>接近 epoch 结束时（距离 epoch 开始有 Y 个时隙），该竞赛将被关闭，这意味着同一个 epoch 内的后续区块必须具有空的票据外部数据。此时，下一个 epoch 的密封密钥序列将被固定。</h6>

We define the extrinsic as a sequence of proofs of valid tickets, each of which is a tuple of an entry index (a natural number less than N) and a proof of ticket validity. Formally:
<h6>我们将外部数据定义为有效的票据证明序列，每个证明都是一个条目索引和票据有效性证明的元组。条目索引是一个小于 N 的自然数，N 为系统定义的上限。</h6>

(70)
```math
E_T ∈ ⟦r ∈ N_N, p ∈ {\mathop{\mathbb{F}}\limits^¯}^{[]}_{γ^z} ⟨X_T \frown η^′_2  \mp r⟩⟧

```

(71)
```math
∣E_T ∣ \le  \left\{\begin{matrix}
K  & if \quad m^′ < Y\\
0  & otherwise 
\end{matrix}\right.
```

(72)
```math
X_T = $jam_ticket
```

We define n as the set of new tickets, with the ticket identity, a hash, defined as the output component of the Bandersnatch Ringvrf proof:
<h6></h6>

(73)
```math
n ≡ [ y:\mathcal{Y}(i_p), r: i_r ∣ i <− E_T ]
```
The tickets submitted via the extrinsic must already have been placed in order of their implied identity. Duplicate identities are never allowed lest a validator submit the same ticket multiple times:
<h6>我们定义集合 n 为新票据集合，其中票据标识符（记为哈希 )）被定义为 Bandersnatch 环形可验证随机函数 (Ringvrf) 证明的输出部分。</h6>

(74)

$$n = [x_y  \quad \wr \wr \quad  x ∈ n] $$

(75)

$$ {x_y ∣ x ∈ n} ⫰ {x_y ∣ x ∈ γ_a} $$

The new ticket accumulator $γ^′_a$ is constructed by merging new tickets into the previous accumulator value (or the empty sequence if it is a new epoch):
<h6>新票据累加器  $γ^′_a$的构建方法是将新票据集合 n 与之前累加器值进行合并（如果是新 epoch 则为空序列）。</h6>

(76)

```math
γ^′_a \equiv \overrightarrow{\begin{bmatrix}
x_y \wr x ∈ n \quad ∪ \left\{\begin{matrix}
\varnothing   & if \quad  e^′> e \\
  \gamma_a & otherwise 
\end{matrix}\right.
\end{bmatrix}}^E
```

The maximum size of the ticket accumulator is E. On each block, the accumulator becomes the lowest items of the sorted union of tickets from prior accumulator  ¥γ^′_a$ and the submitted tickets. It is invalid to include useless tickets in the extrinsic, so all submitted tickets must exist in their posterior ticket accumulator. Formall:

<h6>票据累加器的大小上限为 E。每个区块的累加器值都会更新为之前累加器  ¥γ^′_a$  的票据集合和提交票据集合的排序并集中的最小元素集合。外部数据中包含无用的票据是非法的，因此所有提交的票据都必须存在于它们对应的后验票据累加器中。形式化描述如下：</h6>

(77)

$$ n ⊂ γ^′_a$$

Note that it can be shown that in the case of an empty extrinsic $E_T = []$, as implied by $m^′ ≥ Y$, then $γ^′_a = γ_a$.

<h6>需要注意的是，当外部数据为空 $E_T = []$
) 且 m^′ ≥ Y$ 成立时，可以证明  $γ^′_a = γ_a$ 。 这意味着如果没有提交任何票据（即外部数据为空），并且距离 epoch 结束还有 Y 个或更少时隙，那么新的票据累加器将保持和之前的累加器值一致。</h6>

<h3 align="center">7. Recent History</h3>
<h3 align="center">7. 最近历史 </h3>

We retain in state information on the most recent H blocks. This is used to preclude the possibility of duplicate or out of date work-reports from being submitted.
<h6>我们在状态中保留了最近 H 个区块 的信息。这用于防止重复或过期的工作报告被提交。</h6>

(78)
```math
β ∈ ⟦(h ∈ H, b ∈ ⟦H?⟧, s ∈ H, p ∈ ⟦H⟧_{∶C})⟧_{∶H}
```

For each recent block, we retain its header hash, its state root, its accumulation-result mmr and the hash of each work-report made into it which is no more than the total number of cores, C = 341.
<h6>对于每个最近的块，我们保留其头哈希、状态根、累积结果 mmr 以及其中包含的每个工作报告的哈希，该哈希不超过核心总数 C = 341。</h6>

During the accumulation stage, a value with the partial transition of this state is provided which contains the update for the newly-known roots of the parent block:
<h6>在累积阶段，提供了具有该状态的部分转换的值，其中包含父块的新已知根的更新：</h6>

$$β^† ≡ β \quad except \quad  β^†\quad [0]_s = H_r $$

The final state transition is then:
<h6>最终的状态转换为：</h6>

(80)

```math
\begin{matrix}
β^′≡ \overleftarrow{β^† \pm \begin{Bmatrix}
p: [((g_w)_s)_p ∣ g <− E_G] , \\
h:  \mathcal{H}(H) , b , s: H^0
\end{Bmatrix}}^H  \\
where \quad b = \mathcal{A}(last([[]] ⌢ [x_b ∣ x <− β]), r) \\
and \quad r = \mathcal{M}_2([x \wr  \varepsilon (x) ∣ x ∈ C], \mathcal{H}_K)
\end{matrix}

```

Thus, we extend the recent history with the new block’s header hash, its accumulation-result Merkle tree root and the set of work-reports made into it. Note that the accumulation-result tree root r is derived from C (defined in section 12) using the basic binary Merklization function $\mathcal{M}_2$ (defined in appendix F) and appending it using the mmr append function $\mathcal{A}$ (defined in appendix F.2) to form a Merkle mountain range

<h6>因此，我们使用新区块的头部哈希值、其累积结果 Merkle 树根以及包含在其内的工作报告集合来扩展最近的历史记录。需要注意的是，累积结果树根 r 是通过以下步骤生成的：首先使用基本二元 Merklization 函数 $\mathcal{M}_2$ (见附录 F) 对 C (第 12 节定义) 进行计算，然后使用 MMR 附加函数 $\mathcal{A}$ (见附录 F.2) 将其附加，最终形成一个 Merkle 树。</h6>

The state-trie root is as being the zero hash, $H^0$ which while inaccurate at the end state of the block $β^′$ , it is nevertheless safe since $β^′$ is not utilized except to define the next block’s $β^†$ , which contains a corrected value for this. 
<h6>该状态树根被设置为零哈希，记为 $H^0$  。虽然在区块  $β^′$ 的最终状态下该值并不准确，但这仍然是安全的，因为 $β^′$ 仅用于定义下一个区块的 $β^†$ ，而后者包含了修正后的值。</h6>

<h3 align="center">8. Authorization</h3>

We have previously discussed the model of workpackages and services in section 4.9, however we have yet to make a substantial discussion of exactly how some coretime resource may be apportioned to some work-package and its associated service. In the YP Ethereum model, the underlying resource, gas, is procured at the point of introduction on-chain and the purchaser is always the same agent who authors the data which describes the work to be done (i.e. the transaction). Conversely, in Polkadot the underlying resource, a parachain slot, is procured with a substantial deposit for typically 24 months at a time and the procurer, generally a parachain team, will often have no direct relation to the author of the work to be done (i.e. a parachain block).
<h6>我们在 4.9 节讨论了工作包和服务的模型，但是我们尚未深入探讨如何将核心时间资源分配给工作包及其关联服务。以太坊的 YP 模型中，底层资源 Gas 会在链上引入时被获取，购买者总是创建描述待完成工作的的数据的同一个代理 (即交易). 相反，在 Polkadot 中，底层资源 - 平行链插槽 - 需要通过大额押金购买，通常一次购买 24 个月。而购买者（通常是平行链团队）往往与待完成工作的创建者（即平行链区块）没有直接关系。</h6>

On a principle of flexibility, we would wish Jam capable of supporting a range of interaction patterns both Ethereum-style and Polkadot-style. In an effort to do so, we introduce the authorization system, a means of disentangling the intention of usage for some coretime from the specification and submission of a particular workload to be executed on it. We are thus able to disassociate the purchase and assignment of coretime from the specific determination of work to be done with it, and so are able to support both Ethereum-style and Polkadot-style interaction patterns.
<h6>为了灵活性，我们希望 Jam 能够支持一系列交互模式，既支持以太坊风格的模式，也支持波卡风格的模式。为了实现这一点，我们引入了授权系统，它可以将某些核心时间的用法意图与提交要在其上执行的特定工作负载分开。因此，我们可以将核心时间的购买和分配与具体的工作确定分开，从而同时支持以太坊风格和波卡风格的交互模式。</h6>

**8.1. Authorizers and Authorizations.** The authorization system involves two key concepts: authorizers and authorizations. An authorization is simply a piece of opaque data to be included with a work-package. An authorizer meanwhile, is a piece of pre-parameterized logic which accepts as an additional parameter an authorization and, when executed within a vm of prespecified computational limits, provides a Boolean output denoting the veracity of said authorization.
<h6>8.1 授权者和授权。授权系统涉及两个关键概念：授权者和授权。授权仅仅是一份与工作包一起包含的不透明数据。而授权者则是一段预先参数化的逻辑，它接受一个授权作为额外的参数，并在预先指定计算限制的虚拟机中执行时，提供一个布尔值输出，表示授权的真实性。</h6>

Authorizations are identified as the hash of their logic (specified as the vm code) and their pre-parameterization. The process by which work-packages are determined to be authorized (or not) is not the competence of on-chain logic and happens entirely in-core and as such is discussed in section 13.2. However, on-chain logic must identify each
set of authorizers assigned to each core in order to verify that a work-package is legitimately able to utilize that resource. It is this subsystem we will now define.
<h6>授权通过其逻辑 (指定为 vm 代码) 的哈希值及其预参数化来标识。工作包是否被授权的过程不是链上逻辑的职责，而是完全发生在核心内部，因此将在第 13.2 节进行讨论。然而，链上逻辑必须识别分配给每个核心的授权者集合，以验证工作包是否能够合法地利用该资源。我们现在将定义这个子系统。</h6>

**8.2. Pool and Queue.** We define the set of authorizers allowable for a particular core c as the authorizer pool α[c]. To maintain this value, a further portion of state is
tracked for each core: the core’s current authorizer queue φ[c], from which we draw values to fill the pool. Formally:
<h6>8.2。池和队列。我们将特定核心 c 允许的授权者集合定义为授权者池 α[c]。为了维持这个值，状态的另一部分是跟踪每个核心：核心的当前授权者队列 φ[c]，我们从中提取值来填充池。正式：</h6>

(81)

$$ α ∈ ⟦⟦\mathbb{H}⟧_{∶O}⟧_C , φ ∈ ⟦⟦\mathbb{H}⟧_Q⟧_C $$

Note: The portion of state φ may be altered only through an exogenous call made from the accumulate logic of an appropriately privileged service.
<h6>注意：状态 φ 的部分只能通过从适当特权服务的累积逻辑进行的外源调用来更改。</h6>

The state transition of a block involves placing a new authorization into the pool from the queue:
<h6>块的状态转换涉及将新的授权从队列放入池中：</h6>
(82)

$$ ∀_c ∈ \mathbb{N}_C ∶ α^′[c] ≡ \overrightarrow{F(c) \pm φ′[c][H_t]^\circlearrowleft	}^O$$

(83)

```math
F(c) ≡ \left\{\begin{matrix}
  α[c]  \multimap  {g_a}  & if \quad  ∃_g ∈ E_G ∶ g_c = c\\
 α[c]  & otherwise
\end{matrix}\right.
```

Since $α^′$ is dependent on $φ^′$, practically speaking, this step must be computed after accumulation, the stage in which $φ^′$ is defined.
<h6>由于 $α^′$ 依赖于 $φ^′$ ，因此在实际操作中，这一步必须在累积阶段之后计算，累积阶段正是 $φ^′$ 被定义的阶段。</h6>


<h3 align="center">9. Service Accounts</h3>

As we already noted, a service in Jam is somewhat analogous to a smart contract in Ethereum in that it includes amongst other items, a code component, a storage component and a balance. Unlike Ethereum, the code is split over two isolated entry-points each with their own environmental conditions; one, refinement, is essentially stateless and happens in-core, and the other, accumulation, which is stateful and happens on-chain. It is the latter which we will concern ourselves with now.
<h6>正如我们之前提到的，Jam 中的服务在某种程度上类似于以太坊中的智能合约，因为它包含代码组件、存储组件和余额等其他项。与以太坊不同，代码分为两个独立的入口点，每个入口点都有自己的环境条件。其中一个，精炼，本质上是无状态的，发生在核心内部；另一个，累积，是有状态的，发生在链上。我们将重点关注后者。</h6>

Service accounts are held in state under δ, a partial mapping from a service identifier NS into a tuple of named elements which specify the attributes of the service relevant to the Jam protocol. Formally:
<h6>服务账户的状态保存在 δ 中，δ 是一个部分映射，它将服务标识符 $N_S$ 映射到一个元组，该元组由命名元素组成，这些元素指定了与 Jam 协议相关的服务属性。</h6>

(84)

$$ \mathbb{N}_S \equiv  \mathbb{N}_2^{32}$$

(85)

$$δ ∈ \mathbb{D}⟨\mathbb{N}_S \longrightarrow  A⟩ $$

The service account is defined as the tuple of storage dictionary s, preimage lookup dictionaries p and l, code hash c, and balance b as well as the two code gas limits g & m. Formally:

<h6>服务账户的形式化定义为一个元组，包含以下元素： 存储字典 s，预映射查找字典 p 和 l，代码哈希值 c，余额 b。 以及两个代码的 gas 限制 g 和 m</h6>

(86)
```math
\mathbb{A} \equiv \begin{Bmatrix}
s ∈ \mathbb{D}⟨\mathbb{H} \to  \mathbb{Y}⟩ , p ∈ \mathbb{D}⟨\mathbb{H} \to  \mathbb{Y}⟩ , \\
l ∈ \mathbb{D}⟨(\mathbb{H}, \mathbb{N}_L) \to [\mathbb{N}_T ]_{∶3}⟩ ,\\
c ∈ \mathbb{H} , b ∈ \mathbb{N}_B , g ∈ \mathbb{Z}_G , m ∈ \mathbb{Z}_G
\end{Bmatrix}
```

Thus, the balance of the service of index s would be denoted $δ[s]_b$ and the storage item of key k ∈ H for that service is written $δ[s]_s[k]$.
<h6>因此，索引 s 的服务的余额将表示为 $δ[s]_b$，并且该服务的密钥 k ∈ H 的存储项被写为 $δ[s]_s[k]$。</h6>

**9.1. Code and Gas.** The code c of a service account is represented by a hash which, if the service is to be functional, must be present within its preimage lookup (see section 9.2). We thus define the actual code c:
<h6>9.1。代码和 Gas。 服务帐户的代码 c 由哈希表示，如果服务要正常运行，则该哈希必须出现在其原像查找中（请参阅第 9.2 节）。因此，我们定义实际的代码 c：</h6>

(87)

```math
∀_a ∈ A ∶ a_c ≡ \left\{\begin{matrix}
 a_p[a_c]  & if\quad  a_c ∈ a_p \\
  \varnothing & otherwise
\end{matrix}\right.
```
There are three entry-points in the code:
<h6>代码中有三个入口点：</h6>

1. 0 refine: Refinement, executed in-core and stateless.10
2. 1 accumulate: Accumulation, executed on-chain and stateful.
3. 2 on_transfer: Transfer handler, executed onchain and stateful.

<h6>
  <ul>
    <li>
      0 细化：细化，在核心内执行且无状态。10
    </li>
    <li>
      1 累加： 累积： 在链上执行，有状态。
    </li>
    <li>
      2 on_transfer：传输处理程序，在链上执行且有状态。
    </li>
  </ul>
</h6>


Whereas the first, executing in-core, is described in more detail in section 13.2, the latter two are defined in the present section.
<h6>第 13.2 节更详细地描述了第一个（在核心内执行），后两者在本节中定义。</h6>

As stated in appendix A, execution time in the Jam virtual machine is measured deterministically in units of gas, represented as a 64-bit integer formally denoted $\mathbb{Z}_G$. There are two limits specified in the account, g, the minimum gas required in order to execute the Accumulate entry-point of the service’s code, and m, the minimum required for the On Transfer entry-point.
<h6>如附录 A 中所述，Jam 虚拟机中的执行时间以 Gas 为单位进行确定性测量，以 64 位整数表示，正式表示为 $\mathbb{Z}_G$。账户中指定了两个限制，g，执行服务代码的累积入口点所需的最低气体量，m，传输入口点所需的最低气体量。</h6>

**9.2. Preimage Lookups.** In addition to storing data in arbitrary key/value pairs available only on-chain, an account may also solicit data to be made available also incore, and thus available to the Refine logic of the service’s code. State concerning this facility is held under the service’s p and l components.
<h6>9.2。原像查找。 除了将数据存储在仅在链上可用的任意键/值对中之外，帐户还可以请求在核心中也可用的数据，从而可用于服务代码的 Refine 逻辑。有关此设施的状态保存在服务的 p 和 l 部分下。</h6>

There are several differences between preimage-lookups and storage. Firstly, preimage-lookups act as a mapping from a hash to its preimage, whereas general storage maps arbitrary keys to values. Secondly, preimage data is supplied extrinsically, whereas storage data originates as part of the service’s accumulation. Thirdly preimage data, once supplied, may not be removed freely; instead it goes through a process of being marked as unavailable, and only after a period of time may it be removed from state. This ensures that historical information on its existence is retained. The final point especially is important since preimage data is designed to be queried in-core, under the Refine logic of the service’s code, and thus it is important that the historical availability of the preimage is known.
<h6>原像查找和存储之间存在一些差异。首先，原像查找充当从散列到其原像的映射，而通用存储将任意键映射到值。其次，原像数据是外部提供的，而存储数据则源自服务积累的一部分。第三，原像数据一旦提供，就不得随意删除；相反，它会经历一个被标记为不可用的过程，并且只有在一段时间之后才能将其从状态中删除。这确保了有关其存在的历史信息得以保留。最后一点尤其重要，因为原像数据被设计为在服务代码的 Refine 逻辑下进行核心查询，因此了解原像的历史可用性非常重要。</h6>

We begin by reformulating the portion of state concerning our data-lookup system. The purpose of this system is to provide a means of storing static data on-chain such
that it may later be made available within the execution of any service code as a function accepting only the hash of the data and its length in octets.
<h6>我们首先重新制定有关数据查找系统的状态部分。该系统的目的是提供一种在链上存储静态数据的方法，例如 它稍后可以在任何服务代码的执行中作为仅接受数据散列及其八位字节长度的函数提供。</h6>

During the on-chain execution of the Accumulate function, this is trivial to achieve since there is inherently a state which all validators verifying the block necessarily have complete knowledge of, i.e. σ. However, for the incore execution of Refine, there is no such state inherently available to all validators; we thus name a historical state, the lookup anchor which must be considered recently finalized before the work result may be accumulated hence providing this guarantee.
<h6>在链上执行 Accumulate 函数时，要实现这一点并不难，因为所有验证区块的验证者都必须完全了解一个固有的状态，即 σ。然而，对于 Refine 的内核执行，所有验证者都无法获得这样一个固有的状态；因此，我们命名了一个历史状态，即查找锚，它必须被认为是最近才最终确定的，然后才能累积工作结果，从而提供这种保证。</h6>

By retaining historical information on its availability, we become confident that any validator with a recently finalized view of the chain is able to determine whether any given preimage was available at any time within the period where auditing may occur. This ensures confidence that judgements will be deterministic even without consensus
on chain state.
<h6>通过保留有关其可用性的历史信息，我们确信，任何验证者在最近最终确定了对链的看法后，都能确定任何给定的预图像在审计可能发生的期间内的任何时候是否可用。这就确保了即使在没有就链状态达成共识的情况下，判断也是确定的。
链状态的判断也是确定的。</h6>

Restated, we must be able to define some historical lookup function Λ which determines whether the preimage of some hash h was available for lookup by some service account a at some timeslot t, and if so, provide its preimage:
<h6>重述一下，我们必须能够定义某个历史查询函数Λ，它能确定某个哈希的前像在某个时间段 t 是否可供某个服务账户 a 查询，如果是，则提供其前像：</h6>

(88)

```math
A_t: \left\{\begin{matrix}
\mathbb{A}, \mathbb{N}_{H_t−C_D}...\mathbb{H}_t, \mathbb{H}) \to \mathbb{Y}? \\
\qquad (a, t, \mathcal{H}(p)) \to v ∶ v ∈ {p, \varnothing}
\end{matrix}\right.
```

This function is defined shortly below in equation 90.
<h6>该函数在下面的公式 90 中定义。</h6>
The preimage lookup for some service of index s is denoted δ[s]p is a dictionary mapping a hash to its corresponding preimage. Additionally, there is metadata associated with the lookup denoted $δ[s]_l$ which is a dictionary mapping some hash and presupposed length into historical information.
<h6>索引 s 的某项服务的前像查询表示为 δ[s]p，它是一个将哈希值映射到其相应前像的字典。此外，还有与查询相关的元数据，用 $δ[s]_l$ 表示，它是一个将某些哈希值和预设长度映射为历史信息的字典。</h6>

*9.2.1. Invariants.* The state of the lookup system naturally satisfies a number of invariants. Firstly, any preimage value must correspond to its hash, equation 89. Secondly, a preimage value being in state implies that its hash and length pair has some associated status, also in equation 89. Formally:
<h6>9.2.1。不变量。 查找系统的状态自然满足许多不变量。首先，任何原像值必须对应于其散列，方程 89。其次，处于状态的原像值意味着其散列和长度对具有某种关联的状态，也在方程 89 中。形式上：</h6>
(89)

$$ ∀_a ∈ \mathbb{A}, (h \to p) ∈ a_p ⇒ h = \mathcal{H}(p) ∧(h, ∣p∣) ∈ \mathcal{K}(a_l) $$

*9.2.2. Semantics.* The historical status component $h ∈ [N_T]_{∶3}$is a sequence of up to three time slots and the cardinality of this sequence implies one of four modes:
<h6> 9.2.2. 语义。 历史状态组件$h∈[N_T]_{∶3}$是一个由最多三个时隙组成的序列，该序列的奇偶性意味着四种模式之一：</h6>

- h = []: The preimage is requested, but has not yet been supplied
- h ∈ $⟦N_T ⟧_1$ : The preimage is available and has been from time $h_0$
- h ∈ $⟦N_T ⟧_2$: The previously available preimage is now unavailable since time $h_1$. It had been available from time $h_0$.
- h ∈ $⟦N_T ⟧_3$: The preimage is available and has been from time $h_2$. It had previously been available from time $h_0$ until time $h_1$.

  <h6>
    <ul>
      <li>h = []：已请求原像，但尚未提供</li>
      <li>h ∈ $⟦N_T ⟧_1$：前像可用，并且从 $h_0$ 开始就一直可用。</li>
      <li>h∈$⟦N_T ⟧_2$： 之前可用的前像现在从时间 $h_1$ 开始就不可用了。而从时间 $h_0$ 开始，它就可以使用了。</li>
      <li>h∈$⟦N_T ⟧_3$： 前像从时间 $h_2$ 开始可用。在此之前，从时间 $h_0$ 到时间 $h_1$，它都是可用的。</li>
    </ul>
  </h6>

The historical lookup function Λ may now be defined as:

<h6>历史查找函数 Λ 可以定义为：</h6>

$$ Λ∶ (\mathbb{A}, \mathbb{N}_T , \mathbb{H}) \to \mathbb{Y}? $$

```math
Λ(a, t, h) ≡ \left\{\begin{matrix}
a_p[h] & if \quad h ∈ \mathcal{K}(ap) ∧ I(a_l[h, ∣a_p[h]∣], t) \\
\varnothing  & otherwise
\end{matrix}\right.
```
```math
where \quad I(l, t) = \left\{\begin{matrix}
1  & if [] = l\\
x ≤ t & if [x] = l \\
x ≤ t < y  & if [x, y] = l \\
x ≤ t < y ∨ z ≤ t & if [x, y, z] = l
\end{matrix}\right.
```

**9.3. Account Footprint and Threshold Balance.** We define the dependent values i and l as the storage footprint of the service, specifically the number of items in storage and the total number of octets used in storage. They are defined purely in terms of the storage map of a service, and it must be assumed that whenever a service’s storage is changed, these change also.
<h6>9.3 账户占用空间和阈值余额。本节定义了依赖值 i 和 l，分别表示服务的存储占用空间，具体为存储的条目数量和总字节数。这两个值完全由服务的存储映射来定义，并且必须假设服务存储发生任何变化时，它们也会随之改变。</h6>

Furthermore, as we will see in the account serialization function in section C, these are expected to be found explicitly within the Merklized state data. Because of this we make explicit their set.
<h6>此外，正如我们在 C 节的账户序列化函数中将看到的那样，预计这些值将被明确地包含在 Merkle 化状态数据中。因此，我们将它们明确地定义为一个集合。</h6>

We may then define a second dependent term t, the minimum, or threshold, balance needed for any given service account in terms of its storage footprint.
<h6>然后，我们可以定义第二个依赖项 t，即任何给定服务账户所需的最低余额或阈值余额，并根据其存储占用空间进行衡量。</h6>
(91)

```math
∀_a ∈ \mathcal{V}\left\{\begin{matrix}
a_i ∈ N_{2^32} ≡ 2 ⋅ ∣ a_l ∣ + ∣ a_s ∣ \\
a_l ∈ N_{2^64} ≡ \begin{matrix}
\sum\limits_{(h,z)∈K(al)}^{} 81 + z \\
+ \sum\limits_{x∈V(a_s)}^{} 32 + ∣x∣
\end{matrix}\\
a_t ∈ N_B ≡ B_S + B_I ⋅ a_i + B_L ⋅ a_l
\end{matrix}\right. ∶
```

**9.4. Service Privileges.** Up to three services may be recognized as privileged. The portion of state in which this is held is denoted χ and has three components, each a service index. m is the index of the manager service, the service able to effect an alteration of χ from block to block. a and v are each the indices of services able to alter
φ and ι from block to block. Formally:
<h6>9.4 服务权限。最多可以识别三个服务为特权服务。状态中包含此信息的的部分记为 χ，它包含三个组件，每个组件都是一个服务索引。m 是管理器服务的索引，该服务能够逐块地改变 χ。a 和 v 分别是能够逐块地改变 φ 和 ι 的服务的索引。</h6>
(92)

$$ χ ≡(χ_m ∈ N_S, χ_a ∈ N_S, χ_v ∈ N_S) $$

<h3 align="center">10. Judgements</h3>
<h3 align="center">10. 判决 </h3>
Jam provides a means of recording a vote amongst all validators over the validity of a work-report, a unit of work done within Jam (for greater detail on the nature of a
work-report, see section 11). Such a vote is not expected to happen very often in practice (if at all), however it is an important security backstop, allowing a convenient
manner of removing troublesome keys from the validator set at short notice where there is consensus over their malfunction. It also helps coordinate the ability of unfinalized
chain-extensions to be reverted and replaced with an extension which does not contain some invalid work-report.
<h6>Jam 提供了一种在所有验证者之间对工作报告 (work-report) 有效性进行投票的方法，工作报告是 Jam 内部完成的一项工作单元 (有关工作报告性质的更多详细信息，请参阅第 11 节)。实际上，这种投票并不期望经常发生 (甚至根本不会发生)，但它是一个重要的安全保障措施，允许在验证者对某个故障密钥存在共识的情况下，方便地将其从验证者集合中移除。它还有助于协调未完成的链扩展 (chain-extension) 的回滚，并用不包含无效工作报告的扩展进行替换。</h6>

Generally speaking, judgement data will come about as a result of a dispute between validators, an off-chain process described in section 10. A judgement against a report will imply that the chain will have been reverted to immediately prior to the accumulation of that report. Placing the judgement on-chain has the effect of cancelling its accumulation. The specific strategy for chain selection is described fully in section 15.
<h6>一般来说，判断数据会由于验证者之间的争论而产生，争论是一个链下流程，在第 10 节进行了描述。对报告的判决意味着链将被回滚到累积该报告之前的状态。将判决上链会起到取消其累积的效果。链选择方面的具体策略将在第 15 节中详细描述。</h6>

In the case that a sufficient number of validator nodes do make some judgement in $E_J$ , then an indexed record of that judgement is placed on-chain (in ψ, the portion of
state handling dispute judgements).
<h6>在足够数量的验证器节点对 $E_J$ 做出判断的情况下，该判断的索引记录将被放置链上 (位于 ψ 中，状态中处理争论判决的部分)。</h6>

Having a persistent on-chain record is helpful in a number of ways. Firstly it provides a very simple means of recognizing the circumstances under which action against a validator must be taken by any higher-level validatorselection logic. Should Jam be used for a public network such as Polkadot, this would imply the slashing of the offending validator’s stake on the staking parachain.
<h6>保持一个持久的链上记录在多个方面都很有帮助。首先，它提供了一种非常简单的方法来识别任何高级验证器选择逻辑必须对验证器采取行动的情况。如果 Jam 用于像 Polkadot 这样的公共网络，这将意味着对权益质押平行链上作恶验证者的权益进行削减。</h6>

As mentioned, recording reports found to have a high confidence of invalidity is important to ensure that said reports are not allowed to be resubmitted. Conversely, recording reports found to be valid ensures that additional disputes cannot be raised in the future of the chain.
<h6>
正如前文所述，记录被认定为极有可能无效的报告非常重要，可以确保这些报告不会被再次提交。相反，记录被认定为有效的报告可以确保将来链上不会出现额外的争论。</h6>

**10.1. State.** The judgements state includes three items, an allow-set ($ψ_a$), a ban-set ($ψ_b$) and a punish-set ($ψ_p$). The allow-set contains the hashes of all work-reports
which were disputed and judged to be accurate. The banset contains the hashes of all work-reports which were disputed and whose accuracy could not be confidently confirmed. The punish-set is a set of keys of Bandersnatch keys which were found to have guaranteed a report which was confidently found to be invalid.
<h6>判断状态包含三个项目：允许集 (ψ_a)、禁止集 (ψ_b) 和惩罚集 (ψ_p)。允许集 (ψ_a) 包含所有经过争论并被判定为准确的的工作报告的哈希值。禁止集 (ψ_b) 包含所有经过争论但无法确认其准确性的工作报告的哈希值。惩罚集 (ψ_p) 是颁发了一份被确认为无效的报告的 Bandersnatch 密钥的集合。</h6>

(93)

$$ ψ ≡(ψ_a, ψ_b, ψ_p, ψ_k)$$

We store the last epoch’s validator set in $ψ_k$:
<h6>我们将最后一个纪元的验证器集存储在 $ψ_k$ 中：</h6>

(94)

```math
ψ^′_k =\left\{\begin{matrix}
\aleph    & if \quad ⌊\frac{\tau'}{E}⌋ ≠ ⌊\frac{\tau}{E}⌋\\
 ψ_k & otherwise
\end{matrix}\right.
```

10.2. Extrinsic. The judgements extrinsic, $E_J$ may contain one or more judgements as a compilation of signatures coming from exactly two-thirds plus one of either the active validator set (i.e. the Ed25519 keys of κ) or the previous epoch’s validator set (i.e. the keys of $ψ_k$):
<h6>10.2 判决 extrinsics。判决 extrinsic ($E_J$) 可以包含一个或多个判决，这些判决由来自活动验证器集或上一个 epoch 的验证器集的至少三分之二加一的签名集合编译而成。活动验证器集 (κ) - 即当前 epoch 的验证器集合，由 Ed25519 密钥标识。上一个 epoch 的验证器集 ( $ψ_k$ ) - 是指前一个 epoch 的验证器集合，其密钥集合用 $ψ_k$ 表示。</h6>

(95)

$$E_J ∈ ⟦(\mathbb{H}, ⟦({\bot , \top }, N_V,\mathbb{F})⟧_{⌊2/3V⌋}+1)⟧$$

All signatures must be valid in terms of one of the two allowed validator key-sets. Note that the two epoch’s keysets may not be mixed! Formally:
<h6>所有签名必须根据两个允许的验证器密钥集之一有效。请注意，两个纪元的密钥集不能混合！正式：</h6>
(96)

```math
\begin{matrix}
∀(r,v) ∈ E_J ,∀(v, i, s) ∈ v ∶ s ∈ E_{k[i]e} ⟨X_v \frown r⟩\\
where \quad X_⊺ = $jam\_valid \\
 and \quad X = $jam\_invalid\\
k ∈ {κ, ψ_k} \\

\end{matrix}
```

Judgements must be ordered by report hash and there may be no duplicate report hashes within the extrinsic, nor amongst any past reported hashes. Formally:
<h6>判决必须按报告哈希值排序，外部报告哈希值不得重复，过去报告哈希值之间也不得重复。形式上</h6>
(97)

```math
E_J = [r \wr \wr (r,v) ∈ E_J ]
```

(98)
```math
{r ∣ (r,v) } ⫰ ψ_a ∪ ψ_b
```
The votes of all judgements must be ordered by validator index and there may be no duplicate such indices. Formally:
<h6>所有判决的投票必须按照验证人索引排序，并且不能有重复的索引。正式：</h6>

(99)

$$∀(r,v) ∈ E_ J ∶ v = [i \wr \wr (v, i, s) ∈ v]$$

We define J as the sequence of judgements introduced in the block’s extrinsic (and ordered respectively), with the sequence of signatures substituted with the sum of votes over the signatures. We require this total to be exactly zero, two-thirds-plus-one or one-third-plus-one of the validator set indicating, respectively, that we are confident of the report’s validity, confident of its invalidity, or lacking confidence in either. This requirement may seem somewhat arbitrary, but these happen to be the decision thresholds for our three possible actions and are acceptable since the security assumptions include the requirement that at least two-thirds-plus-one validators are live (Stewart 2018 discusses the security implications in depth).
<h6>10.3 判决数据 (J)。我们将 J 定义为区块 extrinsic 中引入的判决序列（按顺序排列），并用签名总数代替签名序列。 对于验证器集合，我们要求这个总数必须正好等于零、三分之二加一或三分之一加一，分别表示我们确信报告有效、确信其无效或对两者都缺乏信心。这个要求可能看起来有些武断，但它们恰好是我们三种可能操作的决策阈值，并且是可以接受的，因为安全假设包含至少三分之二加一的验证器处于活动状态的要求（Stewart 2018 深入讨论了安全影响）。</h6>

Formally:
<h6>形式上:</h6>

(100)

$$ \mathbf{J} ∈ ⟦(\mathbb{H}, ⟦\mathbb{H}\_B⟧_{2∶3}, \mathbb{N})⟧ $$

(101)
```math

\mathbf{J} = \begin{bmatrix}
\lgroup 
(r, \sum\limits_{(v,i,s ) \in v} v
\rgroup 
\mid 
(r,v)<− E_J
\end{bmatrix}
```
(102)

$$ ∀(r, t)∈ J ∶ t ∈ \{{0, ⌊1/3V⌋, ⌊2 /3V⌋ + 1}\} $$

Note that t is the threshold of judgements that the report is valid, calculated by summing Boolean values in their implicit equivalence to binary digits of the set $\mathbb{N}_2$.
We clear any work-reports judged to be non-valid from their core:
<h6>注意：t 是报告有效性的判断阈值，它是通过将布尔值相加得到，这些布尔值隐含地等价于集合 $\mathbb{N}_2$ 的二进制位。</h6>

(103)
```math
∀c ∈ N_C ∶ ρ^† [c] = \left\{\begin{matrix}
 \varnothing & if ~ {(ρ[c]_r, t) ∈ J, t < ⌊^2/3V⌋} \\
 ρ_c & \quad otherwise
\end{matrix}\right.
```

The allow-set assimilates the hashes of any reports we judge to be valid. The ban-set assimilates any other judged report-hashes. Finally, the punish-set accumulates the guarantor keys of any report judged to be invalid:


<h6>允许集合包含了所有我们判定为有效的报告的哈希值。禁止集合包含了所有其他经过判断的报告哈希值。最后，惩罚集合累积了所有判定为无效的报告的担保人密钥。</h6>

(104)

$$ ψ^′_a ≡ ψ_a ∪ {r ∣(r, ⌊^2/3V⌋ + 1) ∈ J} $$

(105)

$$ ψ^′_b ≡ ψ_b ∪ {r ∣(r, t)∈ J, t ≠ ⌊^2  /3V⌋ + 1}$$

(106)

$$ψ^′_p ≡ ψ_p ∪ {ρ[c]_g ∣ (ρ[c]_r, 0) ∈ J} $$

Note that the augmented punish-set is utilized when determining κ′ to nullify any validator keys which appear in the punish-list.
<h6>注意：确定 κ′ 时会使用增强惩罚集合，其作用是使惩罚列表中出现的任何验证器密钥无效化</h6>

**10.3. Header. ** The judgement marker must contain exactly the sequence of report hashes judged not as confidently valid (i.e. either controversial or invalid). Formally:
<h6>10.3 区块头。判断标记必须 仅包含 被判定为非可信有效的报告哈希序列（即有争议或无效）。形式上，该序列应为：</h6>
(107)

$$H_j ≡ [r ∣(r, t) − J, t ≠ 0]$$

<h3 align="center">11. Reporting and Assurance</h3>
<h3 align="center">11. 报告和保证 </h3>

Reporting and assurance are the two on-chain processes we do to allow the results of in-core computation to make its way into the service state singleton, δ. A work-package,which comprises several work items, is transformed by validators acting as guarantors into its corresponding workreport, which similarly comprises several work outputs and then presented on-chain within the guarantees extrinsic. At this point, the work-package is erasure coded into a multitude of segments and each segment distributed to the associated validator who then attests to its availability through an assurance placed on-chain. After either enough assurances or a time-out (whichever happens first), the work-report is considered available, and the work outputs transform the state of their associated service by virtue of accumulation, covered in section 12.
<h6>报告和验证是链上操作，允许我们将内核计算结果集成到服务状态单例 (δ) 中。验证者作为担保人，将包含多个工作项的工作包转换为对应的包含多个工作输出的工作报告，然后在担保 extrinsic 中链上提交。此时，工作包会进行 erasure 编码，拆分成多个片段，并分配给相应的验证者。验证者随后通过链上证明来保证这些片段的可用性。达到足够多的证明或超时（以两者中较早者为准）后，工作报告将被视为可用，并且工作输出将通过累积的方式转换其关联服务的当前状态（见第 12 节）。</h6>

From the perspective of the work-report, therefore, the guarantee happens first and the assurance afterwards. However, from the perspective of a block’s statetransition, the assurances are best processed first since each core may only have a single work-report pending its package becoming available at a time. Thus, we will first cover the transition arising from processing the availability assurances followed by the work-report guarantees. This synchroneity can be seen formally through the requirement of an intermediate state $ρ^‡$ , utilized later in equation 134.
<h6>
从工作报告的角度来看，担保发生在验证之前。然而，从区块状态转换的角度来看，最好先处理验证，因为每个内核在等待其数据包可用时，一次只能有一个待处理的工作报告。因此，我们将首先介绍处理可用性验证产生的状态转换，然后介绍工作报告的担保。这种同步性可以通过方程 134 中稍后使用的中间状态 $ρ^‡$ 来形式化地表示。</h6>

**11.1. State.** The state of the reporting and availability portion of the protocol is largely contained within ρ, which tracks the work-reports which have been reported but not
yet accumulated and the identities of the guarantors who reported them and the time at which it was reported. As mentioned earlier, at only one report may be assigned to
a core at any given time. Formally:
<h6>11.1. 状态。协议的报告和可用性部分的状态主要包含在 ρ 中。ρ 跟踪已经报告但尚未累积的工作报告，以及报告这些报告的担保人身份和报告时间。正如之前提到的，每个内核在任何给定时间都只能分配一个报告。形式上：</h6>

$$ρ ∈ ⟦(w ∈ W, g ∈ ⟦H_E⟧_{2∶3}, t ∈ N_T)?⟧_C $$

As usual, intermediate and posterior values $(ρ^†, ρ^‡, ρ^′)$ are held under the same constraints as the prior value.
<h6>像往常一样，中间值和后验值 $(ρ^†, ρ^‡, ρ^′)$ 与先前值受到相同的约束。</h6>

*11.1.1. Work Report.* A work-report, of the set W, is defined as a tuple of authorizer hash and output, the refinement context, the package specification and the results of
the evaluation of each of the items in the package, which is always at least one item and may be no more than I items. Formally:

<h6>11.1.1. 工作报告。工作报告 (W) 被定义为一个元组，包含授权者哈希、输出、细化上下文、数据包规范以及数据包中每个项目的评估结果。数据包中至少要包含一个项目，最多可以包含 I 个项目。形式上：</h6>

(109)

$$W ≡ (a ∈ H, o ∈ Y, x ∈ X, s ∈ S, r ∈ ⟦L⟧_{1∶I})$$

The total serialized size of a work-report may be no greater than $W_R$ bytes:

<h6>工作报告的总序列化大小不得超过 $W_R$ 字节：</h6>

(110)

$$∀_w ∈ W ∶ ∣\varepsilon (w)∣ ≤ W_R$$

*11.1.2. Refinement Context.* A refinement context, denoted by the set X, describes the context of the chain at the point that the report’s corresponding work-packagewas evaluated. It identifies two historical blocks, the anchor, header hash a along with its associated posterior state-root s and posterior Beefy root b; and the lookupanchor, header hash l and of timeslot t. Finally, it identifies the hash of an optional prerequisite work-package p. Formally:

<h6>11.1.2. 细化上下文。细化上下文，用集合 X 表示，描述了报告对应的 工作包 被评估时链条的上下文。它包含以下信息：锚点 (anchor)： 历史区块之一，包含头哈希 a 及其关联的后验状态根 s 和后验 Beefy 根 b。查找锚点 (lookup anchor)： 历史区块之一，包含头哈希 l 和时隙 t。可选先决工作包哈希 (optional prerequisite work-package hash)： 用 p 表示，用于标识报告依赖的先前工作包（非必须项）。形式化描述：</h6>

(111)

```math
\mathbb{X} \equiv \Bigg\lgroup
\begin{matrix}
  a ∈ H, s ∈ H, b ∈ H, \\
l ∈ H, t ∈ NT , p ∈ H?

\end{matrix} 
 \Bigg\rgroup
```

*11.1.3. Work Package Specification.* We define the set of work-package specifications, S, as the tuple of the workpackage’s hash and serialized length together with an erasure root. Formally:
<h6>11.1.3. 工作包规范。我们将工作包规范集合定义为 S，它是一个元组，包含工作包的哈希值、序列化长度以及纠错根。形式上：</h6>

(112)

$$ S ≡(h ∈ H, l ∈ N_L, u ∈ H  )$$

*11.1.4. Work Result.* We finally come to define a work result, L, which is the data conduit by which services’ states may be altered through the computation done within a work-package.
<h6></h6>11.1.4. 工作输出 (Work Result)。我们最后定义工作输出 (L)。它是数据通道，服务的状态可以通过工作包内完成的计算进行修改。</h6>

(113)

$$\mathbb{L} ≡ (s ∈ \mathbb{N}_S, c ∈ \mathbb{H}, l ∈ \mathbb{H}, g ∈ \mathbb{Z}_G, o ∈ \mathbb{Y} ∪ \mathbb{J}) $$

Work results are a tuple comprising several items. Firstly s, the index of the service whose state is to be altered and thus whose refine code was already executed. We include the hash of the code of the service at the time of being reported c, which must be accurately predicted within the work-report according to equation 144;
<h6>工作输出由几个项目组成的一个元组。第一个是 s，它表示要更改其状态的服务的索引，也就是已经执行过细化代码的服务。我们还包含报告时服务的代码哈希值 c，根据公式 144，工作报告中必须准确预测该值。</h6>

Next, the hash of the payload (l) within the work item which was executed in the refine stage to give this result. This has no immediate relevance, but is something provided to the accumulation logic of the service. We follow with the gas prioritization ratio g used when determining how much gas should be allocated to execute of this item’s accumulate.
<h6>接下来是细化阶段执行的用于生成此结果的工作项中的有效载荷哈希 (l)。这与当前状态无关，但会提供给服务的累积逻辑。然后是用于确定分配多少 gas 来执行此项目累积的 gas 优先级比率 (g)。</h6>

Finally, there is the output or error of the execution of the code o, which may be either an octet sequence in case it was successful, or a member of the set J, if not. This
latter set is defined as the set of possible errors, formally:
<h6>最后，还有执行代码的输出或错误 o，它可以是 字节序列（如果执行成功），也可以是集合 J 的成员（如果执行失败）。集合 J 被定义为可能的错误集合，形式上为：</h6>

(114)

$$ \mathbb{J} ∈ \{{∞, ☇, BAD, BIG}\} $$

The first two are special values concerning execution of the virtual machine, ∞ denoting an out-of-gas error and ☇ denoting an unexpected program termination. Of the remaining two, the first indicates that the service’s code was not available for lookup in state at the posterior state of the lookup-anchor block. The second indicates that the code was available but was beyond the maximum size allowed S.
<h6>文本描述了虚拟机执行过程中使用的四个特殊值： ∞ 表示内存不足错误（out-of-gas error）。☇ 表示程序意外终止。剩下的两个值中，第一个表示在查找锚块的后继状态中，无法在状态中找到服务代码。第二个表示代码可用，但超过了允许的最大尺寸 S。</h6>

**11.2. Package Availability Assurances.** We first define $ρ^‡$ , the intermediate state to be utilized next in section 11.4 as well as R, the set of available work-reports, which will we utilize later in section 12. Both require the integration of information from the assurances extrinsic $E_A$.
<h6>11.2. 软件包可用性保证。首先，我们定义 $ρ^‡$ ，它将在 11.4 节作为下一个要使用的中间状态，以及 R，即可用工作报告集合，它将在后面的 12 节用到。这两个都需要整合来自外部保证 $E_A$ 的信息。</h6>

*11.2.1.* The Assurances Extrinsic.* The assurances extrinsic is a sequence of assurance values, at most one per validator. Each assurance is a sequence of binary values (i.e. bitstring), one per core, together with a signature and the index of the validator who is assuring. A value of 1 (or ⊺, if interpreted as a Boolean) at any given index implies that the validator assures they are contributing to its availability.[^11] Formally:
<h6>11.2.1. 保证外在函数 (The Assurances Extrinsic)。保证外在函数 (assurances extrinsic) 是一个包含验证者保证值的序列，每个验证者最多提供一个值。每个保证值又是一个比特串 (bitstring)，代表每个核心的状态，同时包含签名和提供保证的验证者索引。任何给定索引处的 1 值（如果解释为布尔值，则为 ⊺）表示验证器保证他们正为该索引的可用性做出贡献 [^11]。形式上：</h6>

(115)

$$E_A ∈ ⟦(a ∈ H, f ∈ B_C, v ∈ N_V, s ∈ E)⟧_{∶V} $$

The assurances must all be anchored on the parent and ordered by validator index:
<h6>这些保证必须全部锚定在父级上并按验证器索引排序：</h6>
(116) 

$$ ∀_a ∈ E_A ∶ a_a = H_p$$

(117)

$$ ∀_i ∈ {1 . . . ∣E_A∣} ∶ E_A[i − 1]_v < E_A[i]_v$$

The signature must be one whose public key is that of the validator assuring and whose message is the serialization of the parent hash $H_p$ and the aforementioned bitstring:
<h6>签名必须满足以下条件：公钥: 签名必须使用提供保证的验证者的公钥进行签名。消息: 签名消息由两个部分序列化而成：父哈希 ( $H_p$ )。前面提到的比特串</h6>
(118)

$$ ∀_a ∈ E_A ∶ a_s ∈ E_{κ[a_v]_e}⟨X_A \frown H(H_p, a_f )⟩$$

(119)

$$X_A = $jam_available $$

A bit may only be set if the corresponding core has a report pending availability on it:
<h6>只有当相应的内核有报告待提交时，该位才会被设置：</h6>

(200)

```math
∀_a ∈ E_A ∶ ∀_c ∈ N_C ∶ a_f [c] \Rightarrow  ρ^† [c] ≠ \varnothing 
```

*11.2.2. Available Reports.* A work-report is said to become available if and only if there are a clear 2/3 supermajority of validators who have marked its core as set within the block’s assurance extrinsic. Formally, we define the series of available work-reports $R$ as:
<h6>11.2.2. 可用工作报告 (Available Reports)。工作报告的可用性取决于区块保证外在函数 (assurance extrinsic) 中验证者的标记。 报告被认为可用当且仅当至少有 2/3 的验证者在其保证信息中将对应核心标记为已设置 (set)。形式上，可用工作报告集合 R 定义为</h6>

(121)

```math
R ≡ \begin{bmatrix}
ρ^†[c]_w ~~\mid  c <- N_C,\sum\limits_{a∈E_A}
a_v[c] > {^2/3}V

\end{bmatrix}
```

This value is utilized in the definition of both $δ^′$ and $ρ^‡$ which we will define presently as equivalent to $ρ^†$ except for the removal of items which are now available 
<h6>该值用于定义我们稍后将介绍的 $δ^′$ 和 $ρ^‡$ 。这两个值与 $ρ^†$ 相同，只是去掉了现在可用的条目。</h6>

(122)

```math
∀c ∈ N_C ∶ ρ^‡[c] ≡\left\{\begin{matrix}
  \varnothing  &if ~ ρ[c]w ∈ R \\
ρ^†[c] &  otherwise 
\end{matrix}\right.
```

*11.3. Guarantor Assignments.* Every block, each core has some particular number of validators uniquely assigned to it assigned to guarantee work reports for it. With V = 1, 023 validators and C = 341 cores, this results in exactly V/C = 3 validators per core. The Ed25519 keys of these validators are denoted by G:

<h6>11.3. 保证人分配 描述了验证人和核心之间的分配关系。在每个区块中，每个核心都会被分配一定数量的验证人，这些验证人专门负责为该核心担保工作报告。文档举例说明了分配规则：假设总验证人数为 V = 1023 人，核心总数为 C = 341 个，那么每个核心将被分配 V/C = 3 个验证人。这些验证人的 Ed25519 密钥用符号 G 表示。</h6>

(123)
```math
G ∈ ⟦⟦H_E⟧_{V/C}⟧_C
```

We determine the core to which any given validator is assigned through a shuffle using epochal entropy and a periodic rotation to help guard the security and liveness of the network. We use η2 for the epochal entropy rather than η1 to avoid the possibility of fork-magnification where uncertainty about chain state at the end of an epoch could
give rise to two established forks before it naturally resolves.

<h6>我们使用洗牌算法 (shuffle) 来确定每个验证人被分配到哪个核心。该算法结合了时代熵 (epochal entropy) 和周期性旋转 (periodic rotation) 以帮助保护网络的安全性和活跃性。我们使用 η2 代替 η1 作为时代熵，以避免叉链放大 (fork-magnification) 的可能性。叉链放大是指由于区块链状态在某个时代末期存在不确定性，可能导致在自然解决之前出现两个已建立的分叉链。</h6>

We define the permute function P, the rotation function R and finally the guarantor assignments G as follows:
<h6>我们定义置换函数 P、旋转函数 R 以及最后的保证人分配 G 如下：</h6>

(124)
```math
P(e, t) ≡ R(F([⌊\frac{V ⋅ i}{C }⌋ ∣i <- N_V], e), ⌊\frac {t mod E}{R}⌋) 
```

(125)
```math
R(c, n) ≡ [(x + n) mod C ∣ x <− c]
```

(126)
```math
∀_c ∈ N_C ∶ G ≡ [κ^′_i∣ i <− NV , P(η^′_2, τ^′)_i = c]
```

We also define $G^∗$, which is equivalent to the value G as it would have been under the previous rotation:

<h6>我们还定义 $G^*$，它相当于前一次旋转下的值 G：</h6>
(127)

```math
∀_c ∈ N_C ∶ G^∗≡ [k_i ∣ i −< N_V , P(e, τ' − R)_i = c]
```

(128)

```math
where ~ e =\left\{\begin{matrix}
 (η^′_2, κ) & if ~ ⌊\frac{τ^′ − R}{E}⌋ = ⌊\frac{τ^′}{E}⌋\\
 (η^′_3, λ)  & otherwise
\end{matrix}\right.
```

**11.4. Work Report Guarantees.** We begin by defining the guarantees extrinsic, $E_G$, a series of guarantees, at most one for each core, each of which is a tuple of a core index, work-report, a credential a and its corresponding timeslot t. Formally:

<h6>11.4. 工作报告保证。 我们首先定义外部保证，即 $E_G$，这是一系列保证，每个核心最多一个，每个保证都是一个核心索引、工作报告、证书 a 及其相应时隙 t 的元组：</h6>
(129)

$$ E_G ∈ ⟦(c ∈ N_C, w ∈ W, t ∈ N_T , a ∈ ⟦E?⟧_3)⟧_{∶C} $$

The credential is itself a sequence of either two or three tuples of a signature and a validator index. The core index of each guarantee must be in ascending order:
<h6>凭证本身是签名和验证器索引的两个或三个元组的序列。每个保证的核心指标必须按升序排列：</h6>
(130)

$$ E_G = [i_c \wr i ∈ E_G]$$

Credentials may only have one missing signature:
<h6>凭证可能只缺少一个签名：</h6>

(131)
$$ ∀g ∈ EG ∶ ∣{x ∈ ga ∶ x ≠ ∅}∣ ≥ 2$$

The signature must be one whose public key is that of the validator identified in the credential, and whose message is the serialization of the core index and the workreport. The signing validators must be assigned to the core in question in either this block G if the timeslot for the guarantee is in the same rotation as this block’s timeslot, or in the most recent previous set of assignments, $G^∗$:
<h6>签名要求。签名必须满足以下条件：公钥: 签名必须使用凭证中标识的验证者的公钥进行签名。消息: 签名消息由两个部分序列化而成：核心索引 (core index)工作报告 (workreport)。签名验证者:如果保证时段与当前区块的时段处于同一轮换中，则验证者必须被分配到 当前区块 对应的核心 (G)。如果保证时段不处于同一轮换中，则验证者必须被分配到 最近一轮 的核心集合 ( $G^∗$ )。</h6>

(132)

```math
\begin{matrix}
  ∀_g ∈ E_G ∶ \\
  ∀_i ∈ N_3, g_a[i] ≠ ∅ ∶\left\{\begin{matrix}
 a_s ∈ E_k[g_c]_i⟨X_G \frown \mathcal{H}(g_c, g_r)⟩\\
where ~ k =\left\{\begin{matrix}
 G & if ~ ⌊\frac{τ^′}{R}⌋ = ⌊\frac{g_t}{R}⌋ \\
G^*  & otherwise 
\end{matrix}\right.
\end{matrix}\right.
\end{matrix}
```

(133)

```math
X_G = $jam_guarantee
```
No reports may be placed on cores with a report pending availability on it unless it has timed out. In the latter case, U = 5 slots must have elapsed after the report was made. A report is invalid if the authorizer hash is not present in the authorizer pool of the core on which the work is reported. Formally:
<h6>在任何核心上提交报告之前，都必须确保该核心上没有待处理的报告，除非该报告已经超时。对于超时的情况，必须在报告生成后经过 U = 5 个时隙。如果报告缺少授权者池中对应的授权者哈希，则该报告无效。正式定义：</h6>



(134)

```math
∀_g ∈ E_G ∶ \left\{\begin{matrix}
(ρ^‡ [g_c] = ∅ ∨ H_t ≥ ρ^‡[g_c]_t + U , \\
g_a ∈ α[g_c]
\end{matrix}\right.
```
We denote w to be the set of work-reports in the present extrinsic E:
<h6>我们记作 w 是当前外部环境 E 中的工作报告集合。</h6>

(135)

$$ let w = {g_w ∣ g ∈ E_G}$$

We specify the maximum total accumulation gas requirement a work-report may imply as $G_A$, and we require the sum of all services’ minimum gas requirements to be no greater than this:
<h6>我们定义一个工作报告所暗示的最大总累积 gas 需求为 $G_A$  ，并规定所有服务的最少 gas 需求总和不得超过这个值。</h6>

(136)

$$∀_w ∈ w ∶ \sum\limits_{s∈(w_r)_s} δ[s]_m ≤ G_A$$

*11.4.1. Contextual Validity of Reports.* For convenience,we define two equivalences x and p to be, respectively, the set of all contexts and work-package hashes within the extrinsic:
<h6>
11.4.1 报告的上下文有效性。为了方便，我们定义两个等价关系 x 和 p，分别表示外部环境中的所有上下文和工作包哈希：x 表示外部环境中所有上下文的集合。p 表示外部环境中所有工作包哈希的集合。
</h6>
(137)

$$let x ≡ {wx ∣ w ∈ w} , p ≡ {(ws)h ∣ w ∈ w}$$

There must be no duplicate work-package hashes (i.e. two work-reports of the same package). Therefore, we require the cardinality of p to be the length of the workreport sequence w:
<h6>在外部环境中，不得出现重复的工作包哈希（即同一个工作包出现两次报告）。因此，我们要求集合 p 的基数（元素个数）必须等于工作报告序列 w 的长度。</h6>
(138)

$$ ∣p∣ = ∣w∣$$ 

We require that the anchor block be within the last H blocks and that its details be correct by ensuring that it appears within our most recent blocks β:
<h6>我们要求锚定区块既位于最近的 H 个区块内，同时信息也要准确无误。为了确保这一点，需要验证它也出现在我们维护的最新区块列表 β 中。</h6>

(139):

$$ ∀x ∈ x ∶ ∃ y ∈ β ∶ x_a = y_h ∧ x_s = y_s ∧ x_b = \mathcal{H}_K(\varepsilon M(y_b))$$

We require that each lookup-anchor block be within the last L timeslots:
<h6>对于每个查找锚定区块，我们要求它必须位于最近的 L 个时间槽内。</h6>

(140)

$$∀x ∈ x ∶ x_t ≥ H_t − L $$

We also require that we have a record of it; this is one of the few conditions which cannot be checked purely with on-chain state and must be checked by virtue of retaining the series of the last L headers as the ancestor set A. Since it is determined through the header chain, it is still deterministic and calculable. Formally:
<h6>除了上述要求之外，我们还需要区块的记录信息。这也是少数几个无法仅通过链上状态进行检查的条件之一，必须通过保留最近 L 个区块头作为祖先集 A 来进行检查。由于它是通过头链确定的，因此仍然是确定可计算的。形式上：</h6>

(141)

$$∀x ∈ x ∶ ∃h ∈ A ∶ h_t = x_t ∧ \mathcal{H}(h) = x_h)$$

We require that the work-package of the report not be the work-package of some other report made in the past. Since the work-package implies the anchor block, and the anchor block is limited to the most recent blocks, we need only ensure that the work-package not appear in our recent history:
<h6>对于报告的工作包，我们要求它不能是过去某个报告所包含的工作包。由于工作包隐含了锚定区块，而锚定区块又限制在最近的区块内，因此我们只需要确保工作包不在最近的历史记录中出现即可。</h6>
(142)

$$∀p ∈ p,∀x ∈ β ∶ p \notin  x_p $$

We require that the prerequisite work-package, if present, be either in the extrinsic or in our recent history:
<h6>
对于先决工作包 (prerequisite work-package)，如果存在的话，我们要求它必须满足以下条件之一：</h6>

(143) 
```math
\begin{matrix}
  ∀w ∈ w, (w_x)_p ≠ ∅ ∶ \\
  {(w)_x}_p ∈ p ∪ {x ∣ x ∈ b_p, b ∈ β}
\end{matrix}
```
We require that all work results within the extrinsic predicted the correct code hash for their corresponding service:
<h6>我们要求外部环境中所有工作的执行结果都预测了其对应服务的正确代码哈希值。换句话说，每个工作的输出应该能够准确地计算出它所服务的代码的哈希值。</h6>

(144)

$$∀w ∈ w,∀r ∈ w_r ∶ rc = δ[r_s]_c $$

**11.5. Transitioning for Reports. ** We define ρ′ as being equivalent to $ρ^‡$ , except where the extrinsic replaced an entry. In the case an entry is replaced, the new value includes the present time τ′ allowing for the value may be replaced without respect to its availability once sufficient time has elapsed (see equation 134).
<h6>1.5. 报告的转换我们定义 ρ' 等价于 $ρ^‡$，但当外部环境替换了一个条目时除外。如果一个条目被替换，新值会包含当前时间 τ'，以允许在足够的时间间隔后替换该值，而无需考虑其可用性 (参见公式 134)。</h6>

(145)

```math
 \begin{matrix}
 ∀c ∈ N_C ∶ ρ′[c] ≡ \left\{\begin{matrix}
  (w, g : G(a), t : τ^′)& \\
 ρ^‡ [c]  & otherwise
\end{matrix}\right.\\
where ~ G(a) ≡ {κ[v]_e ∣ (s, v) ∈ a}
\end{matrix}
```

This concludes the section on reporting and assurance. We now have a complete definition of ρ′ together with R to be utilized in section 12, describing the portion of the
state transition happening once a work-report is guaranteed and made available. 
<h6>这部分关于报告和保证的论述到此结束。我们现在拥有了 ρ' 和 R 的完整定义，将在第 12 部分利用它们来描述工作报告被确认并可用后，状态转换的一部分内容。</h6>

<h3 algin="center"> 12. Accumulation </h3>
<h3 align="center"> 12. 累积 </h3>

Accumulation may be defined as some function whose arguments are R and δ together with selected portions of (at times partially transitioned) state and which yields the posterior service state δ′ togWe define $δ^†$ as the state after the integration of the preimages:ether with additional state elements ι′ , φ′and χ′.
<h6>累积 (Accumulation) 可以定义为一个函数，其参数包括 R 和 δ，以及选取的部分状态 (有时部分转换过的状态)。该函数会输出后验服务状态 δ′。</h6>

The proposition of accumulation is in fact quite simple: we merely wish to execute the Accumulate logic of the service code of each of the services which has at least one work output, passing to it the work outputs and useful contextual information. However, there are three main complications. Firstly, we must define the execution environment of this logic and in particular the host functions available to it. Secondly, we must define the amount of gas to be allowed for each service’s execution. Finally, we must determine the nature of transfers within Accumulate which, as we will see, leads to the need for a second entry-point, on-transfer.
<h6>累积的原理其实很简单：我们只需要对每个至少有一个工作输出的服务，执行其服务代码中的“累积”逻辑，并将工作输出和有用的上下文信息传递给它。但是，这里存在三个主要复杂性：首先，我们需要定义这种逻辑的执行环境，特别是可供其使用的主机函数。其次，我们需要定义每个服务执行所允许的 gas 数量 (gas 代表一种用于衡量智能合约执行复杂度的单位)。最后，我们需要确定“累积”逻辑内部的转移性质，这将导致需要第二个入口点“on-transfer”，我们稍后会看到这一点。</h6>



12.1. Preimage Integration. Prior to accumulation, we must first integrate all preimages provided in the lookup extrinsic. The lookup extrinsic is a sequence of pairs of service indices and data. These pairs must be ordered and without duplicates (equation 147 requires this). The data must have been solicited by a service but not yet be provided. Formally:
<h6>12.1. 预原像集成。在进行累积之前，我们必须首先集成查找外部环境 (lookup extrinsic) 中提供的所有预处理图像。查找外部环境是一系列服务索引和数据对的序列。这些对必须按顺序排列，并且没有重复项（公式 147 需要这样）。数据必须是由某个服务请求的，但尚未提供的。形式上：</h6>

(146)

$$E_P ∈ ⟦(N_S, Y)⟧ $$

(147)

$$ E_P = [i \wr \wr i ∈ E_P ]$$

(148)
```math
 ∀(s, p)∈ E_P ∶ \left\{\begin{matrix}
 \mathcal{K}(δ[s]_p) \notni \mathcal{H}(p) ,\\
δ[s]_l[(\mathcal{H}(p), ∣p∣)] = []
\end{matrix}\right.
```

We define $δ^†$ as the state after the integration of the preimages:
<h6>我们将 $δ^†$ 定义为原像积分后的状态：</h6>

```math

δ^†= δ ex. ∀ (s, p)∈ E_P ∶\left\{\begin{matrix}
 δ^†[s]_p[\mathcal{H}(p)] = p\\
δ^†
[s]_l[\mathcal{}H(p), ∣p∣] = [τ^′]
\end{matrix}\right.

```

**12.2. Gas Accounting.** We define S, the set of all services which will be accumulated in this block; this is all services which have at least one work output within R, together with all privileged services, χ. Formally:
<h6>gas 账户：我们定义 S，它是将在本块中累积的所有服务的集合；这是 R 中具有至少一个工作输出的所有服务以及所有特权服务 χ 的集合。 形式上：</h6>

(150)

$$ \mathbf{S} ≡ {r_s ∣ w ∈ \mathbf{R}, r ∈ w_r} ∪ {χ_m, χ_a, χ_v} $$

We calculate the gas attributable for each service as the sum of each of the service’s work outputs’ share of their report’s elective accumulation gas together with the subtotal of minimum gas requirements:
<h6>对于每个服务，我们将应计 gas 计算为每个服务的工作输出与其报告的可选择累积 gas 共享的总和以及最低 gas 要求的子总计：</h6>

(151)

```math
G: \left\{\begin{matrix}
\mathbb{N}_S \to  \mathbb{Z}_G \\
s \to \sum\limits_{w∈R}\sum\limits_{r∈w_r,r_s=s}δ^†[s]_g\lfloor r_g.\frac{G_A − \sum\limits_{r∈wr}δ^†[r_s]_g}{\sum{r∈w_r}
r_g}\rfloor
\end{matrix}\right.
```

**12.3. Wrangling.** We finally define the results which will be given as an operand into the accumulate function for each service in S. This is a sequence of operand tuples O,
one sequence for each service in S. Each sequence contains one element per work output (or error) to be accumulated for that service, together with said work output’s payload hash, package hash and authorization output. The tuples are sequenced in the same order as they appear in R. Formally:
<h6>12.3. 数据整理 (Wrangling)。这一步定义了将作为每个服务在累积函数中的操作数提供的最终结果。对于集合 S 中的每个服务，我们将定义一个操作数序列 O。每个序列包含一个元素，分别对应该服务需要累积的工作输出（或错误），以及该工作输出的负载哈希、包哈希和授权输出。这些元素的顺序与它们在 R 中出现的顺序相同。</h6>

(152)

$$ O ≡(o ∈ Y ∪ J, l ∈ H, k ∈ H, a ∈ Y)$$

(153)
```math
M \left\{\begin{matrix}
\mathbb{N}_S → ⟦\mathbb{O}⟧ \\
s \to \begin{bmatrix}
\lgroup 
\begin{matrix}
o: r_o, l: r_p, \\
a: w_o, k (w_s)_h
\end{matrix}
\rgroup \mid 
\begin{matrix}
 w ∈ R,\\
 r ∈ wr,\\
 r_s = s
\end{matrix}
\end{bmatrix}
\end{matrix}\right.

```

12.4. Invocation. Within this section, we define A, the function which conducts the accumulation of a single service. Formally speaking, A assumes omnipresence of timeslot Ht and some prior state components $δ^†$ , ν, $R_d$, and takes as specific arguments the service index s ∈ S (from which it may derive the wrangled results M(s) andgas limit G(s)) and yields values for $δ^†$.[s] and staging assignments into φ, ι together with a series of lookup solicitations/forgets, a series of deferred transfers and C mapping from service index to Beefy commitment hashes.

<h6>12.4. 调用.在本节中，我们定义 A 函数，它用于执行单个服务的累积过程。形式上，A 函数假设可以访问当前时隙信息 Ht 和一些先前状态组件  ，并接收以下特定参数: $δ^†$ , ν, $R_d$ 服务索引 s ∈ S（A 函数可以从中获取整理后的结果 M(s) 和 gas 限制 G(s)）A 函数会返回以下值：δ † .[s]：更新后的服务状态（仅针对服务 s 更新）φ 和 ι 的暂存分配：这可能是用于临时存储数据的变量或结构.一系列查找请求/遗忘：这可能指示需要从其他服务获取数据或忘记先前获取的数据一系列延迟转移：这可能表示需要在稍后阶段进行数据转移的操作 。C：这是一个映射，将服务索引映射到 Beefy 承诺哈希（一种用于证明数据完整性的哈希值）</h6>

We first denote the set of deferred transfers as T, noting that a transfer includes a memo component m of 64 octets, together with the service index of the sender s, the service index of the receiver d, the amount of tokens to be transferred a and the gas limit g for the transfer. Formally:
<h6>首先，我们将延迟转移的集合记为 T。需要注意，一个转移包含以下信息：备忘录组件 (memo)：长度为 64 个字节的文本，用于描述转移的目的或附加信息。 发送方服务索引 (s)：表示发送该转移的服务的索引。 接收方服务索引 (d)：表示接收该转移的服务的索引。 转移金额 (a)：要转移的代币数量。 gas 限制 (g)：用于执行转移操作的 gas 限制。 形式上：</h6>

(154)

$$T ≡(s ∈ N_S, d ∈ N_S, a ∈ N_B, m ∈ Y_M, g ∈ Z_G)$$

We may then define A, the mapping from the index of accumulated services to the various components in terms of which we will be imminently defining our posterior state:
<h6>我们可以将 A 定义为从累积服务索引到各种组件的映射，我们稍后将使用这些组件定义后验状态。</h6>

(155)

```math
A: \begin{Bmatrix}
N_s \to \Bigg\lgroup \begin{matrix}
s ∈ A?, v ∈ K_V, t ∈ ⟦T⟧, r ∈ H,\\
c ∈ ⟦⟦H⟧+Q⟧_C, n ∈ D⟨N_S → A⟩,\\
p ∈(m ∈ N_S, a ∈ N_S, v ∈ N_S)\\


\end{matrix} \Bigg\rgroup\\
s \to Ψ_A(δ^†, s,M(s), G(s))
\end{Bmatrix}
```

As can be seen plainly, our accumulation mapping A combines portions of the prior state into arguments for a virtual-machine invocation. Specifically the service accounts $δ^†$ together with the index of the service in question s and its wrangled refine-results M(s) and gas limit G(s) are arranged to create the arguments for ΨA, itself using a virtual-machine invocation as defined in appendix B.4.
<h6>可以清楚地看到，我们的累积映射 A 将先前状态的一部分组合成用于调用虚拟机的参数。具体来说，服务账户 (δ † ) 连同所讨论服务的索引 (s)、整理后的结果 (M(s)) 和 gas 限制 (G(s)) 被组合起来创建用于 ΨA 的参数，ΨA 本身使用附录 B.4 中定义的虚拟机调用。</h6>

The Beefy commitment map is a function mapping all accumulated services to their accumulation result (the r component of the result of A). This is utilized in determining the accumulation-result tree root for the present block, useful for the Beefy protocol:
<h6>Beefy 承诺映射 (Beefy commitment map) 是一个函数，它将所有累积服务映射到它们的累积结果（A 函数结果的 r 部分）。这用于确定当前区块的累积结果树根，对 Beefy 协议很有用：</h6>

(156)
$$ C ≡ {(s, A(s)_r) ∣ s ∈ S, A(s)_r ≠ ∅}$$

Given our mapping A, which may be calculated exhaustively from the vm invocations of each accumulated service S, we may define the posterior state δ′, χ', φ′andι′ as the result of integrating A into our state.
<h6>已知映射 A，它可以由每个累积服务 S 的虚拟机调用完全计算得出。我们可以将后验状态 δ'、χ'、φ' 和 ι' 定义为将 A 集成到我们状态的结果。</h6>

*12.4.1. Privileged Transitions.* The staging core assignments, and validator keys and privileged service set are each altered based on the effects of the accumulation of
each of the three privileged services:
<h6>12.4.1  特权转换。累积过程会影响三个特权服务，分别为：调整暂存核分配、验证者密钥和特权服务集合。每个服务的效果都会导致相应的更新：</h6>

(157)

```math
χ′≡ A(χ_m)_p , φ′≡ A(χ_a)_c , ι′ ≡ A(χ_v)_v
```
*12.4.2. Service Account Transitions.* Finally, we integrate all changes to the service accounts into state.
<h6>12.4.2。服务帐户转换。*最后，我们将对服务帐户的所有更改集成到状态中。</h6>

We note that all newly added service indices, defined as $\mathcal{K}(A(s)_n)$ for any accumulated service s, must not conflict with the indices of existing services or newly added services. This should never happen, since new indices are explicitly selected to avoid such conflicts, but in the unlikely event it happens, the block would be invalid. Formally:
<h6>值得注意的是，对于任何累积服务 s，其所有新添加的服务索引（记作 $\mathcal{K}(A(s)_n)$  )）都不能与现有服务或其他新添加服务的索引冲突。由于新索引的选取会明确避免冲突，这种情况通常不会发生。但是，万一出现冲突，则该区块将会无效。</h6>

(158)

```math
\begin{matrix}
∀s ∈ S ∶ \mathcal{K}(A(s)_n) ∩ \mathcal{K}(δ^†) = ∅, \\
∀t ∈ S ∖ {s} ∶ \mathcal{K}(A(s)_n) ∩ \mathcal{K}(A(t)_n) = ∅
\end{matrix}
```

We first define $δ^‡$, an intermediate state after main accumulation but before the transfers have been credited and handled:
<h6>我们首先定义$δ^‡$，这是主要累积之后但在转账被记入和处理之前的中间状态：</h6>

(159)

```math


\begin{matrix}
 K(δ^‡) ≡ \Big(\mathcal{K}(δ^†) ∪ ⋃\limits_{s∈S} \mathcal{K}(A(s)_n)) ∖ {s \mid \begin{matrix}
s ∈ S \\
s_s = ∅
\end{matrix} }\Big) \\

δ^‡[s] ≡\left\{\begin{matrix}
A(s)_s  &if ~ s ∈ S \\
A(t)_n[s]  & if ~ ∃!t ∶ t ∈ S, s ∈ \mathcal{K}(A(t)n)\\
 δ^†[s]  &otherwise
\end{matrix}\right.

\end{matrix}

```

We denote R(s) the sequence of transfers received by a given service of index s, in order of them being sent from services of ascending index. (If some service s received
no transfers or simply does not exist then R(s) would be validly defined as the empty sequence.) Formally:

<h6>我们用 R(s) 表示给定索引 s 的服务收到的转移序列，这些转移按照发送服务的升序索引排列。(如果服务 s 没有收到任何转移，或者根本不存在，那么 R(s) 可以被合理地定义为空序列。) 形式上：</h6>

(160)

```math
R: \left\{\begin{matrix}
 \mathbb{N}_S \to ⟦\mathbb{T}⟧\\
d \to [ t ∣ s <− S, t <− A(s)t, t_d = d ]
\end{matrix}\right.
```

The posterior state δ′ may then be defined as the intermediate state with all the deferred effects of the transfersapplied:
<h6>经过应用所有延迟转移的效果后，后验状态 δ' 可以定义为中间状态。</h6>

(161)

```math
δ′= {s \to Ψ_T (δ^‡, a, R(a)) ∣ (s \to a) ∈ δ^‡}
```

Note that $Ψ_T$ is defined in appendix B.5 such that it results in $δ^‡$[d], i.e. no difference to the account’s intermediate state, if R(d) = [], i.e. said account received no
transfers.
<h6>请注意，$Ψ_T$ 在附录 B.5 中定义，因此它的结果是 $δ^‡$[d]，即如果 R(d) = []，则与帐户的中间状态没有区别，即所述帐户未收到任何信息 转移。</h6>

<h3 align="center">13. Work Packages and Work Reports</h3>
<h3>13. 工作包和工作报告</h3>

** 13.1. Honest Behavior. ** We have so far specified how to recognize blocks for a correctly transitioning Jam blockchain. Through defining the state transition function and a state Merklization function, we have also defined how to recognize a valid header. While it is not especially difficult to understand how a new block may be authored for any node which controls a key which would allow the creation of the two signatures in the header, nor indeed to fill in the other header fields, readers will note that the contents of the extrinsic remain unclear.

<h6>13.1 诚实行为。到目前为止，我们已经详细说明了如何识别 Jam 区块链正确状态转换的区块。通过定义状态转换函数和状态默克尔根函数，我们还定义了如何识别有效的区块头信息。对于任何拥有密钥的节点来说，生成新区块并创建头信息中的两个签名都并非特别困难，填写其他头信息字段也并非难事。然而，读者可能会注意到，我们尚未阐明区块内容 (extrinsic) 的细节。</h6>

We define not only correct behavior through the creation of correct blocks but also honest behavior, which involves the node taking part in several off-chain activities.
This does have analogous aspects within YP Ethereum, though it is not mentioned so explicitly in said document: the creation of blocks along with the gossiping and inclusion of transactions within those blocks would all count as off-chain activities for which honest behavior is helpful. In Jam’s case, honest behavior is well-defined and expected of at least 2/3 of validators.

<h6>我们不仅通过创建正确的区块来定义正确行为，还定义了诚实行为，这涉及节点参与链下的一些活动。这与 YP 以太坊有类似的方面，尽管在以太坊文档中没有如此明确地提及：创建区块以及对交易进行 gossip 传播并包含在区块中，都可被视为链下活动，诚实行为对此至关重要。在 Jam 区块链中，诚实行为被明确定义，并且至少需要 2/3 的验证者遵循诚实行为。</h6>

Beyond the production of blocks, incentivized honest behavior includes:
<h6>除了生产区块之外，激励诚实行为还包括：</h6>

- the guaranteeing and reporting of work-packages, along with chunking and distribution of both the chunks and the work-package itself, discussed in section 13.3;
- assuring the availability of work-packages after being in receipt of their data;
- making and submitting judgements on the correctness of work-reports;
- determining which work-reports to audit, fetching and auditing them, and creating and distributing an adverse judgement to other nodes based on the outcome of the audit;
- submitting the correct amount of work seen being done by other validators, discussed in section 16.

<ul>
<li>工作包的保证和报告，以及块和工作包本身的分块和分配，在第 13.3 节中讨论；</li>
<li>确保收到数据后工作包的可用性；</li>
<li>对工作报告的正确性做出并提交判断；</li>
<li>确定要审计哪些工作报告，获取并审计它们，并根据审计结果创建不利判断并将其分发给其他节点；</li>
<li>提交其他验证者所完成的正确工作量，如第 16 节所述。</li>
</ul>

We begin with the first of these, the guaranteeing of work-packages.
<h6>我们先来看看其中的第一项，即工作包的保证。</h6>

**13.2. Packages and Items.** We begin by defining a work-package, of set P, and its constituent work items, of set I. A work-package includes a simple blob acting as an authorization token j, a service identifier for where authorization code is hosted h, an authorization code hash c and a parameterization blob p, a context x and a sequence
of work items limited in size i:
<h6>首先，我们定义工作包 (work-package) 和工作单元 (work item)。工作包集合记为 P，工作单元集合记为 I。一个工作包包含以下信息：授权令牌 (j)：一个简单的二进制大对象 (blob)，用作授权凭证。服务标识符 (h)：指向授权代码存放位置的服务标识符。授权代码哈希 (c)：授权代码的哈希值。参数化数据 (p)：一个二进制大对象，用于参数化服务执行逻辑。上下文 (x)：与工作包相关的上下文信息。工作单元序列 (i)：一个包含有限个工作单元的序列。</h6>
(162)

$$ P ∈(j ∈ Y, h ∈ N_S, c ∈ H,p ∈ Y,x ∈ X,i ∈ I_{1l}) $$

We limit the encoded size of work-packages to a little over 6mb in order to allow for 1mb/s/core data throughput:
<h6>我们将工作包的编码大小限制在 6mb 多一点，以便实现 1mb/s/core 的数据吞吐量：</h6>

(163)

$$∣\varepsilon (P)∣ ≤ W_P$$

(164)

$$ W_P = 6 ⋅ 2 ^ {20} + 2 ^{16}$$

A work item includes the identifier of the service to which it relates s, the code hash of the service at the time of reporting, and whose preimage must be available from
the perspective of the lookup anchor block c, a payload blob y, and a gas limit g:
<h6>一个工作项包括其相关服务的标识符 s、报告时该服务的代码哈希值，其前像必须可从c 、有效载荷 blob y 和气体限制 g：</h6>

(165)

$$ I ∈(s ∈ N_S, c ∈ H,y ∈ Y, g ∈ N_G) $$

We define the work-package’s implied authorizer as $p_a$, the hash of the concatenation of the authorization code and the parameterization. We define the authorization code as pand require that it be available at the time of the lookup anchor block from the historical lookup of service h. Formally:

<h6>对于工作包，我们定义以下概念：隐含授权者 (implied authorizer)：记为 pa ，它是授权代码和参数化数据的哈希值拼接而成的哈希值。授权代码 (authorization code)：记为 p，我们要求该代码在历史查找服务 h 的查找锚定区块时可用。 形式上：</h6>

(166)
```math
∀p ∈ P ∶ \left\{\begin{matrix}
p_a ≡ \mathcal{H}(p_c \frown p_p) \\
p_c ≡ H(δ[p_h], (p_x)_t,p_c) \\
p_c ∈ Y
\end{matrix}\right.
```

(Λ is the historical lookup function defined in equation 90.)
<h6>(Λ是公式 90 中定义的历史查询函数）。</h6>

We now come to the work result computation function Ξ. This forms the basis for all utilization of cores on Jam. It operates on some work-package p for some nominated core c and results in either an error ∇ or the work result, which is deterministic and, thanks to the historical lookup functionality, can be evaluated by any node which has a recently finalized chain for up to 24 epochs after the lookup-anchor block.formally:
<h6>现在我们来讨论工作结果计算函数 Ξ。它是 Jam 区块链上所有核心利用的基础。该函数接收一个工作包 (p) 和一个指定的核心 (c) 作为输入，并返回一个错误值 (∇) 或一个确定性的工作结果。由于历史查找功能的存在，任何拥有最近完成链 (lookup-anchor block 后 24 个 epoch 内) 的节点都可以评估该工作结果。形式上：</h6>

(167)
```math
\Xi : \left\{\begin{matrix}
(P, N_C ) \to W \\
(p, c) \to \left\{\begin{matrix}
∇  & if~ o \notin Y\\
(a: p_a, o, x: p_x, s, r)  & otherwise
\end{matrix}\right.
\end{matrix}\right.
```

where:

```math
\begin{matrix}
 o = Ψ_I (p, c)\\
s =(h, l:∣\varepsilon (p)∣, u: \mathcal{M}_2([\mathcal{H}(x) ∣ x <− C(P_{2^{11}} (\varepsilon (p)))])) \\
 r = [Ψ_R(c, g, s, h, y,p_x,p_a, o) ∣(s, c,y, g) <− p_i]\\
h = \mathcal{H}(p)

\end{matrix}
```

And P is the zero-padding function to take an octet array to some multiple of n in length:
<h6>P 是零填充函数，用于将八进制数组的长度取为 n 的某个倍数：</h6>

(168)

```math
P_n∈N_1 \left\{\begin{matrix}
Y \to  Y_{k⋅n} \\
x \to x \frown [0, 0, ...]_{((∣x∣+n−1) mod n)+1...n}
\end{matrix}\right.
```

We define the binary Merklization function $M_2$ in equation 280. Note that C represents the erasure-coding function for the chunks and is defined in appendix H.

<h6>我们在公式 280 中定义了二元梅克里化函数 $M_2$。请注意，C 代表块的擦除编码函数，其定义见附录 H。</h6>

Validators are incentivized to distribute each workpackage chunk to each other validator, since they are not paid for guaranteeing unless a work-report is considered to
be available. Given our work-package p, we should therefore send chunk $ C( \varepsilon (p))v $ to each validator whose keys are$  κ_v$. In the case of a coming epoch change, they may also maximize expected reward by distributing to the new validator set (and thus also send the chunk to $ (γ_k)_v)$ .

<h6>审定者有动力将每个工作包块分配给其他审定者，因为除非工作报告被认为是可用的，否则审定者是没有报酬的。因为除非工作报告被认为是可用的，否则他们是没有报酬的。鉴于我们的工作包 p，我们应该向每个验证者发送块 $ C( \varepsilon (p))v $ ，这些验证者的密钥是 $ κ_v$。在即将到来的时代变化中，他们也可以通过向新的验证者集分发来最大化预期回报（因此也会向 $ (γ_k)_v)$ 发送块。</h6>

We will see this function utilized in the next sections, for guaranteeing, auditing and judging.

<h6>在接下来的章节中，我们将看到这一功能在保证、审计和判断方面的应用。</h6>

**13.3. Guaranteeing.** Guaranteeing work-packages involves the creation and distribution of a corresponding work-report which requires certain conditions to be met. Along with the report, a signature demonstrating the validator’s commitment to its correctness is needed. With two guarantor signatures, the work-report may be distributed to the forthcoming Jam chain block author in order to be used in the EG, which leads to a reward for the guarantors.

<h6>13.3. 工作包担保。工作包的担保涉及到创建和分发相应的 工作报告 (work-report)，该报告需要满足特定的条件。除了报告本身之外，还需要验证者签名以证明其对报告正确性的承诺。拥有两个担保者签名后，工作报告可以分发给即将到来的 Jam 链区块作者，以便用于 执行引擎 (EG)，最终使担保者获得奖励。</h6>

We presume that in a public system, validators will be punished severely if they malfunction and commit to a report which does not faithfully represent the result of Ξ
applied on a work-package. Overall, the process is:
<h6>对于一个公开的 Jam 区块链系统，我们假设验证者如果出现故障并对一个未忠实反映 Ξ 函数作用于工作包结果的报告进行签名，将会受到严厉惩罚。整个过程概述如下：</h6>

1. Evaluation of the work-package’s authorization, and cross-referencing against the authorization pool in the most recent Jam chain state.
2. Chunking of the work-package report according to the erasure codec.
3. Creation and publication of a work-package report.
4. Distributing the chunks and package as needed to other nodes.

<ol>
<li>评估工作包的授权，并与最新JAM链状态下的授权库进行交叉比对。</li>
<li>根据擦除编解码器对工作包报告进行分块。</li>
<li>编写和出版工作包报告。</li>
<li>根据需要向其他节点分发数据块和数据包。</li>
</ol>

   


For any work-package p we are in receipt of, we may determine the work result, if any, it corresponds to for the core c that we are assigned to. When Jam chain state is needed, we always utilize the chain state of the most recent block.

<h6>对于我们收到的任何工作包 (p)，如果该工作包与我们被分配的核心 (c) 相关，我们都可以确定其对应的工作结果 (如果有的话)。当我们需要用到 Jam 区块链的状态时，我们总是使用最新区块的链状态。</h6>

For any guarantor of index v assigned to core c and a work-package p, we define the work result r simply as:
<h6>对于分配到核心 c 执行任务的担保者 (guarantor) v 以及一个工作包 p，我们简单地将工作结果 r 定义为：</h6>

(169)

```math
r = Ξ(p, c)
```

Such guarantors may safely create and distribute the payload (s, v). The component s may be created according to equation 132; specifically it is a signature using the validator’s registered Ed25519 key on a payload l:

<h6>这样的担保者可以安全地创建和分发有效载荷 (s, v)。其中分量 s 可以根据公式 132 创建，具体来说，它是使用验证者注册的 Ed25519 密钥对有效载荷 l 进行签名。</h6>

(170)
```math
l = \mathcal{H}(c, r)
```

To maximize profit, the guarantor should require the work result meets all expectations which are in place during the guarantee extrinsic described in section 11.4. This
includes contextual validity, inclusion of the authorization in the authorization pool, and ensuring total gas is at most $G_A$. No doing so does not result in punishment, but will prevent the block author from including the package and so reduces rewards.
<h6>为了最大化收益，担保者应该要求工作结果满足所有在第 11.4 节担保交易描述中制定的期望条件。这包括上下文有效性、授权包含在授权池中以及确保总 gas 费用不超过 $G_A$ 。如果不满足这些条件，担保者不会受到惩罚，但区块作者将不会包含该工作包，从而减少担保者的奖励。</h6>

Advanced nodes may maximize the likelihood that their reports will be includable on-chain by attempting to predict the state of the chain at the time that the report will get to the block author. Naive nodes may simply use the current chain head when verifying the work-report. To minimize work done, nodes should make all such evaluations prior to evaluating the ΨR function to calculate the report’s work results.
<h6>高级节点可以通过尝试预测报告到达区块作者时链的状态，来最大限度地提高其报告被包含在链上的可能性。简单节点在验证工作报告时，可能只使用当前的链头状态。为了最小化工作量，节点应该在使用 ΨR 函数计算报告的工作结果之前，进行所有此类评估。</h6>

Once evaluated as a reasonable work-package to guarantee, guarantors should maximize the chance that their work is not wasted by attempting to form consensus over the core. To achieve this they should send the workpackage to any other guarantors on the same core which they do not believe already know of it.
<h6>既然担保者已经评估了工作包的担保合理性，为了最大化他们的工作不被浪费，他们应该尝试就该核心达成共识。为了实现这一点，他们应该将工作包发送给同一个核心上他们认为不知道该工作包的任何其他担保者。</h6>

In order to minimize the work for block authors and thus maximize expected profits, guarantors should attempt to construct their core’s next guarantee extrinsic from the work-report, core index and set of attestations including their own and as many others as possible.
<h6>为了使区块作者的工作量最小化并因此使预期收益最大化，担保者应该尝试用工作报告、核心索引以及包含自己和其他尽可能多的证明 (attestations) 的集合来构建他们所在核心的下一个担保交易 (guarantee extrinsic)。</h6>

In order to minimize the chance of any block authors disregarding the guarantor for anti-spam measures, guarantors should sign an average of no more than two workreports per timeslot.
<h6>为了降低区块作者将担保者视为垃圾信息而忽略的可能性，担保者平均每个时隙不应该签名超过两个工作报告。</h6>

**13.4. Availability Assurance.** Validators should issue signed statements, called assurances, when they are in possession of their corresponding erasure-coding chunk of the work-package for any corresponding work-reports which are currently pending availability.

<h6>13.4. 可用性保证。验证者应当在持有与其对应的 工作报告 (work-report) 相关的工作包的纠删码块时，签发称为 保证 (assurance) 的签名声明。这些工作报告目前正等待可用性确认。</h6>

The correct erasure-coding chunk can be determined through a proof using the commitment to the workpackage chunks Merkle root specified in the work-report.
<h6>正确的纠删码块可以通过工作报告中指定的 工作包块 Merkle 根承诺 (commitment to the work-package chunks Merkle root) 的证明来确定。</h6>

**13.5. Auditing and Judging.** The auditing and judging system is theoretically equivalent to that in Elves, introduced by Stewart 2018. For a full security analysis of the mechanism, see this work. The main differences are in terminology, whereby the terms backing and approval there refer to our guaranteeing and auditing, respectively.
<h6>13.5. 审计与裁决。审计与裁决系统 在理论上与 Stewart 在 2018 年提出的 Elves 系统中的机制等价。关于该机制的完整安全分析，请参阅相关论文。主要区别在于术语方面，Elves 系统中术语 "backing" 和 "approval" 分别对应我们这里的 "担保 (guaranteeing)" 和 "审计 (auditing)"。</h6>

*13.5.1. Overview.* The auditing process involves each node requiring themselves to fetch, evaluate and issue judgement on a random but deterministic set of workreports from each Jam chain block in which the workreport becomes available (i.e. from R). Prior to any evaluation, a node declares and proves its requirement. At specific common junctures in time thereafter the set of work-reports which a node requires itself to evaluate from each block’s R may be enlarged if any declared intentions are not matched by a positive judgement in a reasonable time or in the event of a negative judgement being seen. These enlargement events are called tranches.

<h6>13.5.1 概述。审计过程涉及每个节点都需要从包含工作报告的 Jam 链区块中获取、评估并做出判断。这些工作报告来自一个随机但确定的集合，记为 R（即工作报告变得可用的区块）。在进行任何评估之前，节点需要声明并证明其评估需求。之后，在预定的时间点，如果节点声明的评估需求没有在合理时间内得到正面判断，或者出现负面判断，那么节点需要评估的来自每个区块 R 的工作报告集合可能会被扩大。这种扩大的事件称为 tranche。</h6>

If all declared intentions for a work-report are matched by a positive judgement at any given juncture, then the work-report is considered audited. Once all of any given block’s newly available work-reports are audited, then we consider the block to be audited. One prerequisite of a node finalizing a block is for it to view the block as audited. Note that while there will be eventual consensus on whether a block is audited, there may not be consensus at the time that the block gets finalized. This does not affect the crypto-economic guarantees of this system.
<h6>如果在某个时间点，所有针对某个工作报告的声明的评估需求都得到了正面判断，那么该工作报告就被认为通过了审计。当一个区块中所有新出现的工作报告都通过审计后，那么整个区块就被认为通过了审计。节点最终确认一个区块的前提条件之一是该区块被视为已审计。需要注意的是，虽然最终会就一个区块是否通过审计达成共识，但在区块被最终确认时可能还没有达成共识。</h6>

In regular operation, no negative judgements will ultimately be found for a work-report, and there will be no direct consequences of the auditing stage. In the unlikely event that a negative judgement is found, then one of several things happens; if there are still more than 2/3V positive judgements, then validators issuing negative judgements may receive a punishment for time-wasting. If there are greater than 1/3V negative judgements, then the block which includes the work-report is ban-listed. It and all its descendants are disregarded and may not be built on. In all cases, once there are enough votes, a judgement extrinsic can be constructed by a block author and placed on-chain to denote the outcome. See section 10 for details on this.
<h6>在正常运行的情况下，审计阶段通常不会发现任何针对工作报告的负面判断，并且也不会产生任何直接后果。但是，在极少数出现负面判断的情况下，会根据具体情况采取不同措施：如果正面判断仍然多于总判断数的 2/3，那么对该工作报告做出负面判断的验证者可能会因浪费时间而受到惩罚。如果负面判断的比例超过总判断数的 1/3，则包含该工作报告的区块将被列入黑名单。这个区块及其所有后续区块都将被忽略，并且不能以此为基础继续构建新的区块。
无论哪种情况，当获得足够多的投票后，区块创建者就可以构建一个裁决交易并将它放入区块链中，用来表示最终的审计结果。有关裁决交易的细节请参见第 10 节。

</h6>

All announcements and judgements are published to all validators along with metadata describing the signed material. On receipt of sure data, validators are expected to update their perspective accordingly (later defined as J and A).

<h6>所有公告和判断 (announcements and judgements) 连同描述签名材料的元数据一起发布给所有验证者。收到确认数据后，验证者应该相应地更新他们的视角 (perspective)，具体含义将在之后定义为 J 和 A。</h6>

*13.5.2. Auditing Specifics.* Each validator shall perform auditing duties on each valid block received. Since we are entering off-chain logic, and we cannot assume consensus, we henceforth now consider ourselves a specific validator of index v and assume ourselves focused on some block B with other terms corresponding, so σ ′ is said block’s posterior state, H is its header &c. Practically, all considerations must be replicated for all blocks and multiple blocks’ considerations may be underway simultaneously.
<h6>对每个接收到的有效区块，每个验证者都应当执行审计职责。 由于我们将进入链下逻辑（off-chain logic），并且不能假设已经达成共识，因此从现在开始，我们将自己视为具有索引 v 的特定验证者，并假设自己正在处理某个区块 B（其他术语含义与此类似）。因此，σ' 表示该区块的后续状态，H 表示其区块头等等。实际上，所有这些考虑因素都必须针对所有区块进行复制，并且可能同时针对多个区块进行考虑。</h6>

We define the sequence of work-reports which we may be required to audit as Q, a sequence of length equal to the number of cores, which functions as a mapping of core index to a work-report pending which has just become available, or ∅ if no report became available on the core. Formally:

<h6>我们定义待审计工作报告序列为 Q，该序列的长度等于核心数量。Q 中每个元素对应一个核心索引，指向新出现 (pending) 等待审计的工作报告，如果没有新出现的报告则为空 (∅)。形式化定义如下：</h6>

(171)

```math
Q ∈ ⟦W?⟧_C,
```

(172)

```math
Q ≡ \begin{bmatrix}
\begin{matrix}
 [ρ[c]_w  & if ~ ρ[c]w ∈ R\\
 \varnothing  & otherwise
\end{matrix} \mid c <− N_C
\end{bmatrix}
```

We define our initial audit tranche in terms of a verifiable random quantity s0 created specifically for it:
<h6>我们用专门为其创建的可验证随机数量 s0 来定义初始审计批次：</h6>

(173)
```math
s_0 ∈ F^{[]}_{κ[v]b}⟨X_U \frown \mathcal{Y}(H_v)⟩
```

(174)
$$ X_U = $jam_audit$$

We may then define a0 as the non-empty items to audit through a verifiably random selection of ten cores:
<h6>因此，我们可以将 a0 定义为通过可验证的随机方式选取 10 个内核进行审计的非空项目：</h6>

(175)

$$ a0 = {(c, w)∣(c, w)∈ p_{⋅⋅⋅+10}, w ≠ ∅} $$

(176)

$$ where p = \mathcal{F}([(c, Q_c)∣ c ∈ N_C], r) $$

(177)

$$and r = \mathcal{Y}(s_0) $$

Every A = 8 seconds following a new time slot, a new tranche begins, and we may determine that additional cores warrant an audit from us. Such items are defined as an where n is the current tranche. Formally:
<h6>在新的时隙之后，每隔 A = 8 秒就会开始一个新的批次，我们可能会确定有更多的内核需要我们进行审计。此类项目定义为 n，其中 n 为当前批次。形式为</h6>

(178)

```math
let n = ⌊\frac{T − P ⋅ τ^′}{A}⌋
```

New tranches may contain items from Q stemming from one of two reasons: either a negative judgement has been received; or the number of judgements from the previous tranche is less than the number of announcements from said tranche. In the first case, the validator is always required to issue a judgement on the work-report. In the second case, a new special-purpose vrf must be constructed to determine if an audit and judgement is warranted from us.
<h6>新的 tranche (批次) 可能包含来自 Q 的元素，原因之一是：已经收到负面判断。另外一个原因是：前一个 tranche 中的判断数量少于来自该 tranche 的公告数量。在第一种情况下，验证器总是需要对工作报告做出判断。
在第二种情况下，需要构建一个新的特殊用途 vrf 来确定我们是否需要进行审计和判断。</h6>

In all cases, we publish a signed statement of which of the cores we believe we are required to audit (an announcement) together with evidence of the vrf signature to select them and the other validators’ announcements from the previous tranche unmatched with a judgement in order that all other validators are capable of verifying the announcement. Publication of an announcement should be taken as a contract to complete the audit regardless of any future information.

<h6>在所有情况下，我们都会发布一个签名声明，说明我们认为需要审计哪些核心（公告），同时会附上证据，这些证据包括：用于选择这些核心的 vrf 签名。前一个 tranche 中其他验证者的公告（未匹配到任何判断）。这样做的目的是为了让所有其他验证者能够验证该公告。发布公告应被视为完成审计的承诺，无论将来获得任何信息都将执行审计。</h6>

Formally, for each tranche n we ensure the announcement statement is published and distributed to all other validators along with our validator index v, evidence sn and all signed data. Validator’s announcement statements must be in the set:

<h6>形式上，对于每个 tranche n，我们确保公告声明连同我们的验证者索引 v、证据 sn 和所有签名数据一起发布并分发给所有其他验证者。验证者的公告声明必须属于以下集合：</h6>


(179)

```math
E_{κ[v]e}⟨X_I \pm n ⌢ E([\varepsilon_2(c) ⌢ H(w) ∣(c, w)∈ a_0])⟩
```

(180)

```math
X_I = $jam_announce
```

We define An as our perception of which validator is required to audit each of the work-reports (identified by their associated core) at tranche n. This comes from each other validators’ announcements (defined above). It cannot be correctly evaluated until n is current. We have absolute knowledge about our own audit requirements.

<h6>对于每个 tranche n，我们定义 An 为我们认为应该审计每个工作报告（由关联核心标识）的验证者集合。这个集合来源于其他验证者的公告（如上所述）。在 n 不是当前批次时，An 的值是无法准确评估的。我们只对自己需要审计的内容有绝对的了解。</h6>

(181)
```math
A_n ∶ W → ℘(N_V)
```

(182)
```math
∀(c, w) ∈ a_0 ∶ v ∈ q_0(w)
```

We further define $J_⊺$ and $J_\perp$ to be the validator indices who we know to have made respectively, positive and negative, judgements mapped from each work-report’s core. We don’t care from which tranche a judgement is made.
<h6>我们进一步定义 $J_⊺$ 和 $J_\perp$ 为验证人指数，我们知道他们分别做出了正面和负面的判断，这些判断是由每份工作报告的核心内容映射出来的。我们并不关心判断是由哪一批次做出的。</h6>

(183)
```math
J\{{\bot ,\top}\} ∶ W → ℘(N_V)

```

We are able to define an for tranches beyond the first on the basis of the number of validators who we know are required to conduct an audit yet from whom we have not yet seen a judgement. It is possible that the late arrival of information alters an and nodes should reevaluate and act accordingly should this happen.
<h6>对于第一个 tranche 之后的情况，我们可以根据我们知道需要进行审计的验证者数量 (但尚未收到他们的判断) 来定义后续 tranche 的 An 集合。延迟到达的信息可能会改变 An 的值，因此节点应该重新评估并根据新信息采取相应行动。</h6>

We can thus define an beyond the initial tranche through a new vrf which acts upon the set of no-show validators.
<h6>因此，我们可以通过一个新的 vrf 来定义初始批次之外的批次，该 vrf 作用于 "未显示验证器 "集。</h6>

∀n > 0 ∶

(184)

$$s_n(w) ∈ F^{[]}_{κ[v]b}⟨X_U \frown Y(Hv) \frown H(w) \pm  n⟩$$

(185)

$$a_n ≡ {w ∈ Q ∣ F_{256V} \mathcal{Y}(s_n(w))_0 < ∣A_{n−1}(w) ∖ J_⊺(w)∣}$$

We define our bias factor F = 2, which is the expected number of validators which will be required to issue a judgement for a work-report given a single no-show in the tranche before. Modeling by Stewart 2018 shows that this is optimal.

<h6>我们定义偏好因子 F 为 2，它表示在之前的一个 tranche 中没有收到某个工作报告的判断信息后，预计需要多少个验证者对该工作报告做出判断。Stewart 在 2018 年的建模表明这个值是最佳的。</h6>

Later audits must be announced in a similar fashion to the first. If audit requirements lesson on the receipt of new information (i.e. a positive judgement being returned for a previous no-show), then any audits already announced are completed and judgements published. If audit requirements raise on the receipt of new information (i.e. an additional announcement being found without an accompanying judgement), then we announce the additional audit(s) we will undertake.

<h6>后续批次的审计公告与第一批次类似。如果收到新信息后，审计需求减少（例如，之前没有收到判断信息的报告现在得到了正面判断），那么已经宣布的所有审计都将完成，并且审计结果将公布。如果收到新信息后，审计需求增加（例如，发现额外的公告但没有相应的判断），那么我们将宣布我们将进行的额外审计。</h6>

As n increases with the passage of time an becomes known and defines our auditing responsibilities. We must attempt to reconstruct all work-packages corresponding to each work-report we must audit. This may be done through requesting erasure-coded chunks from one-third of the validators. It may also be short-cutted through asking a third-party (e.g. an original guarantor) for a reverse-hash lookup using the work-package hash in the work-report’s package specification.

<h6>随着批次编号 n 的增加 (即时间的推移)，An 集合的值逐渐明朗，它将最终确定我们的审计职责。对于我们必须要审计的工作报告，我们需要尝试重建与之相对应的所有的工作包。这可以通过以下两种方式实现：方式一：向三分之一的验证者请求纠删码块。方式二：通过工作报告中包含的工作包规格信息中的工作包哈希值，向第三方 (例如原始担保者) 咨询进行反向哈希查找 (reverse-hash lookup) 来获取工作包信息。</h6>

Thus, for any such work-report w we are assured we will be able to fetch some candidate work-package encoding F(w) which comes either from reconstructing erasurecoded chunks verified through the erasure coding’s Merkle root, or alternatively from the preimage of the workpackage hash. We decode this candidate blob into a workpackage and attempt to reproduce the report on the core to give en, a mapping from cores to evaluations:
<h6>
因此，对于任何需要审计的工作报告 w，我们都可以确保能够获取一些候选工作包编码 F(w)。该编码可以通過以下两种方式之一获得：通过擦除编码的 Merkle 根验证过的擦除编码块进行重建。或者，通过工作包哈希值的原像（preimage）获得（例如，从原始担保者处通过反向哈希查找）。然后，我们将此候选数据块解码成一个工作包，并尝试在核心上重现该工作，得到 en，它是一个将核心映射到评估结果的映射：</h6>

(186)

```math
\begin{matrix}
 ∀(c, w) ∈ a_n ∶\\
e_n(w) \Leftrightarrow \left\{\begin{matrix}
w = Ξ(p, c)   & if ~ ∃p ∈ P ∶ \varepsilon (p) = F(w)\\
 \bot  & otherwise
\end{matrix}\right.
\end{matrix}
```

Note that a failure to decode implies an invalid workreport.

<h6>注意，解码失败意味着工作报告无效。</h6>

From this mapping the validator issues a set of judgements $j_n$:

<h6>根据这一映射，验证器会发出一组判断 $j_n$：</h6>

(187)
```math
j_n = {S_{κ[v]e} (X_{e(w)} \frown H(w)) ∣ (c, w) \frown a_n}
```

All judgements $j_∗$ should be published to other validators in order that they build their view of J and in the case of a negative judgement arising, can form an extrinsic
for $E_J$ .

<h6>所有判断 $j_∗$ 都应向其他验证者公布，以便他们建立自己对 J 的看法，并在出现否定判断的情况下，形成对 $E_J$ 的外在判断。
为$E_J$。</h6>

We consider a work-report as audited under two circumstances. Either, when it has no negative judgements and there exists some tranche in which we see a positive judgement from all validators who we believe are required to audit it; or when we see positive judgements for it from greater than two-thirds of the validator set.
<h6>在两种情况下，我们认为工作报告已经过审核。一种情况是，工作报告没有任何负面评价，而我们认为需要对其进行审计的所有审定者都做出了正面评价；另一种情况是，超过三分之二的审定者做出了正面评价。</h6>


(188)
```math
U(w) \Leftrightarrow  ⋁ \left\{\begin{matrix}
 J(w) = ∅ ∧ ∃_n ∶ A_n(w) ⊂ J_⊺(w)\\
J_⊺(w)∣ > ^2/3V
\end{matrix}\right.
```

Our block B may be considered audited, a condition denoted U, when all the work-reports which were made available are considered audited. Formally:
<h6>当所有已提交的工作报告都被视为经过审计时，我们的 B 块就可以被视为经过审计，这个条件用 U 表示。形式上</h6>
(189)

```math
U \Leftrightarrow  ∀_w ∈ R ∶ U(w)
```

For any block we must judge it to be audited (i.e. U = ⊺) before we vote for the block to be finalized in Grandpa. See section 15 for more information here.
<h6>对于任何区块，在我们投票让该区块通过 Grandpa  finalised 协议之前，我们必须判断该区块已经完成审计 (即 U = ⊤)。有关 Grandpa  finalised 协议的更多信息，请参见第 15 节。</h6>

Furthermore, we pointedly disregard chains which include the accumulation of a report which we know at least 1/3 of validators judge as being invalid. Any chains including such a block are not eligible for authoring on. The best block, i.e. that on which we build new blocks, is defined as the chain with the most regular Safrole blocks which does not contain any such disregarded block. Implementationwise, this may require reversion to an earlier head or alternative fork.
<h6>此外，我们还会明确地忽略包含已知至少三分之一的验证者认为无效的报告的链。任何包含此类区块的链都不可用于继续构建区块。最佳区块（即我们用来构建新区块的区块）的定义是具有最多规则 Safrole 区块且不包含任何此类被忽略区块的链。在实施方面，这可能需要回滚到更早的区块头或切换到备用分叉链。</h6>

As a block author, we include a judgement extrinsic which collects judgement signatures together and reports them on-chain. In the case of a non-valid judgement (i.e. one which is not two-thirds-plus-one of judgements confirming validity) then this extrinsic will be introduced in a block in which accumulation of the non-valid work-report is about to take place. The non-valid judgement extrinsic removes it from the pending work-reports, ρ. Refer to section 10 for more details on this.
<h6>作为区块创建者，我们包含一个裁决交易 (judgement extrinsic)，它将裁决签名集合在一起并报告给链上。如果出现无效的裁决 (例如，没有超过三分之二的裁决确认有效性)，那么该交易将包含在即将累积无效工作报告的区块中。这个无效裁决交易会将该工作报告从待处理工作报告列表 ρ 中移除。有关详细信息，请参见第 10 节。</h6>

<h3 align="center">14. Beefy Distribution</h3>
<h3 align="center">14. Beefy Distribution</h3>

For each finalized block B which a validator imports, said validator shall make a bls signature on the bls12-381 curve, as defined by Hopwood et al. 2020, affirming the Keccak hash of the block’s most recent Beefy mmr. This should be published and distributed freely, along with the signed material. These signatures may be aggregated in order to provide concise proofs of finality to third-party systems. The signing and aggregation mechanism is defined fully by Jeff Burdges et al. 2022.

<h6>每当验证者导入一个最终区块 (finalized block) B 时，它都应该使用 BLS12-381 曲线 (由 Hopwood等人于 2020 年定义) 进行 BLS 签名。 这个签名是为了确认区块最新 Beefy mmr 的 Keccak 哈希值。签名信息连同被签名数据应该被发布并自由分发。这些签名可以被聚合 (aggregated) 起来，为第三方系统提供简洁的最终性证明。签名和聚合机制的细节由 Jeff Burdges等人于 2022 年定义。</h6>

Formally, let $F_v$ be the signed commitment of validator index v which will be published:
<h6>形式上，让 $F_v$ 成为将发布的验证器索引 v 的签名承诺：</h6>
(190) 

$$ F_{v} ≡ S_{κ_v} (X_B ⌢ H_K(\varepsilon_M(last(β)b])) $$

(191)

$X_B = $jam_beefy$

<h3 align="center">15. Grandpa and the Best Chain</h3>
<h3 align="center">15. grandpa 和最好的链 </h3>

Nodes take part in the Grandpa protocol as defined by Stewart and Kokoris-Kogia 2020.
<h6>节点参与 Stewart 和 Kokoris-Kogia 2020 所定义的 Grandpa 协议。</h6>

We define the latest finalized block as $B^♮$ . All associated terms concerning block and state are similarly superscripted. We consider the best block, $B^♭$ to be that which
is drawn from the set of acceptable blocks of the following criteria:
<h6>我们将最新完成的区块定义为 $B^♮$ 。所有与区块和状态相关的术语都有类似的上标。我们认为最佳区块 $B^♭$ 是指是从符合以下标准的可接受区块集合中抽取出来的：</h6>

* Has the finalized block as an ancestor.
* Contains no unfinalized blocks where we see an equivocation (two valid blocks at the same timeslot).
* Is considered audited.

<ul>
  <li>将最终确定的区块作为祖先。</li>
  <li>不包含任何未最终确定的区块，在这些区块中，我们可以看到等价区块（同一时段的两个有效区块）。</li>
  <li>被视为经过审计。</li>
</ul>

  Formally:
<h6>形式上:</h6>

  (192)
  $$A(H^♭) ∋ H^♮ $$

  (193)

  $$U^♭ ≡ ⊺ $$

  (194)

  ```math
∃/ H_A,HB∶ ⋀ \left\{\begin{matrix}
 H^A ≠ H^B\\
H^A_T = H^B_T \\
H^A ∈ A(H^♭) \\
H^A\notin A(H^♮)

\end{matrix}\right.
  ```

Of these acceptable blocks, that which contains the most ancestor blocks whose author used a seal-key ticket, rather than a fallback key should be selected as the best
head, and thus the chain on which the participant should make Grandpa votes.

<h6>在这些可接受的区块中，应该选择包含最多祖先区块的区块作为最佳区块头 (best head)。** 这些祖先区块的创建者应该使用密封密钥票据 (seal-key ticket) 而不是备用密钥 (fallback key) 进行签名。** 因此，参与者应该在这个链上进行 Grandpa 投票。</h6>

Formally, we aim to select $B^♭$ to maximize the value m where:
<h6>从形式上看，我们的目标是选择 $B^♭$ 以最大化 m 的值：</h6>

(195)
```math
m = \sum\limits_{H^A∈A^♭}T^A
```

<h3 align="center">16. Ratings and Rewards</h3>
<h3 align="center">16. 评级和奖励 </h3>

The Jam chain does not explicitly issue rewards—we leave this as a job to be done by the staking subsystem (a system parachain—hosted without fees—in the current imagining of a public Jam network). However, much as with validator punishment information, it is important for the Jam chain to facilitate the arrival of performance information in to the staking subsystem so that it may be acted upon.
<h6>Jam 区块链本身并不直接颁发奖励 - 在当前构想中的公共 Jam 网络中，我们把这项工作留给 staking 子系统 (一个系统平行链 - 无需费用) 来完成。然而，与验证者惩罚信息类似，Jam 区块链也需要促进性能信息传送到 staking 子系统，以便其可以采取相应的行动。</h6>

Such performance information cannot directly cover all aspects of validator activity; whereas block production, guarantor reports and availability assurance can easily be tracked on-chain, Grandpa, Beefy and auditing activity cannot. In the latter case, this is instead tracked with validator voting activity: validators vote on their impression of each other’s efforts and a median may be accepted as the truth for any given validator. With an assumption of 50% honest validators, this gives an adequate means of oraclizing this information.
<h6>
性能信息并不能完全涵盖验证者活动的所有方面。例如，区块生产、担保报告和可用性保证等信息可以很容易地在链上追踪，而 Grandpa 协议、Beefy 协议和审计活动则无法直接追踪。

对于后一类活动，Jam 区块链通过验证者投票活动来进行追踪。验证者会对他们认为其他验证者所付出的努力进行投票，然后取所有投票的中位数作为每个验证者表现的真实值。假设系统中存在 50% 的诚实验证者，这种方法可以足够准确地获取这些信息的预言信息 (oraclized information)。</h6>

<h3 align="center">17. Discussion</h3>
<h3 align="center">17. 讨论 </h3>

*17.1. Technical Characteristics.* In total, with our stated target of 1,023 validators and three validators per core, along with requiring a mean of ten audits per validator per timeslot, and thus 30 audits per work-report, Jam is capable of trustlessly processing and integrating 341 work-packages per timeslot.

<h6>*17.1. 技术特点* 总体而言，我们的既定目标是 1 023 名验证员和每个核心 3 名验证员，同时要求每个验证员每个时段平均进行 10 次审核，因此每个工作报告需要进行 30 次审核，Jam 能够在每个时段无误地处理和整合 341 个工作包。</h6>

We assume node hardware is a modern 16 core cpu with 64gb ram, 1tb secondary storage and 0.5gbe networking.
<h6>我们假设节点硬件为现代 16 核 CPU、64GB 内存、1TB 二级存储和 0.5Gbe 网络。</h6>

Our performance models assume a rough split of cpu time as follows:
<h6>我们的性能模型假定计算器时间大致分为以下几部分：</h6>

|                   | *Proportion* |
|  :------------    | -----------  |
| Audits            |  10/16       |
| Merklization      |  1/16        |
| Block execution   |  2/16        |
| Grandpa and Beefy |  1/16        |
| Erasure coding    |  1/16        |
| Networking & misc |  1/16        |

Estimates for network bandwidth requirements are as follows:
<h6>网络带宽需求估算如下：</h6>

|                   |    Upload Mb/s | Downloads Mb/s |
| :---------------  | :------------: |  :----------:  |
| Guaranteeing      | 30             | 40             |
| Assuring          | 12             | 8              |
| Auditing          | 200            | 200            |
| Block publication | 42             | 42             |
| Grandpa and Beefy | 4              | 4              |
| Total             | 288            | 294            |

Thus, a connection able to sustain 500mb/s should leave a sufficient margin of error and headroom to serve other validators as well as some public connections, though the burstiness of block publication would imply validators are best to ensure that peak bandwidth is higher.
<h6>因此，能够维持 500mb/s 速度的连接应留有足够的误差和余量，以便为其他验证者和一些公共连接提供服务，尽管区块发布的突发性意味着验证者最好确保峰值带宽更高。</h6>

Under these conditions, we would expect an overall network-provided data availability capacity of 2PB, with each node dedicating at most 6tb to availability storage.
<h6>在这种情况下，我们预计网络提供的总体数据可用性容量为 2PB，每个节点最多将 6tb 用于可用性存储。</h6>

|                 |      GB             |                           |
| :-------------- | :-----------------: | :-----------------------: |
| Auditing        | 20                  | 2 × 10 pvm instances      |
| Block execution |  2                  | 1 pvm instance            |
| State cache     | 40                  |                           |
| Misc            | 2                   |                           | 
| Total           | 64                  |                           | 


As a rough guide, each parachain has an average footprint of around 2mb in the Polkadot Relay chain; a 40gb state would allow 20,000 parachains’ information to be retained in state.
<h6>粗略估算，每个 Parachain 在 Polkadot 中继链中的平均占用空间约为 2MB；40GB 的状态将允许 20,000 个 Parachain 的信息保留在状态中。</h6>

What might be called the “virtual hardware” of a Jam core is essentially a regular cpu core executing at somewhere between 25% and 50% of regular speed for the whole six-second portion and which may draw and provide 2.5mb/s average in general-purpose i/o and utilize up to 2gb in ram. The i/o includes any trustless reads from the Jam chain state, albeit in the recent past. This virtual hardware also provides unlimited reads from a semi-static preimage-lookup database.
<h6>可称为JAM核心的 "虚拟硬件 "本质上是一个普通的 CPU 核心，在整个 6 秒钟的时间里以普通速度的 25%至 50%之间的速度执行任务，并可能平均提供 2.5mb/s 的通用 i/o，以及使用高达 2GB 的内存。i/o 包括从果酱链状态进行的任何无信任读取，尽管是在最近的过去。该虚拟硬件还可从半静态预估查询数据库中进行无限次读取。</h6>

Each work-package may occupy this hardware and execute arbitrary code on it in six-second segments to create some result of at most 90kb. This work result is then entitled to 10ms on the same machine, this time with no “external” i/o beyond said result, but instead with full and immediate access to the Jam chain state and may alter the service(s) to which the results belong.
<h6每个工作包都可以占用这些硬件，并以六秒为一段执行任意代码，以产生最多 90KB 的结果。然后，该工作结果有权在同一台机器上运行 10 毫秒，这一次，除了上述结果之外，没有任何 "外部 "i/o，而是可以立即完全访问JAM链状态，并可更改结果所属的服务。</h6>

**17.2. Illustrating Performance.** In terms of pure processing power, the Jam machine architecture can deliver extremely high levels of homogeneous trustless computation. However, the core model of Jam is a classic parallelized compute architecture, and for solutions to be able to utilize the architecture well they must be designed with it in mind to some extent. Accordingly, until such usecases appear on Jam with similar semantics to existing ones, it is very difficult to make direct comparisons to existing systems. That said, if we indulge ourselves with some assumptions then we can make some crude comparisons.

<h6>**17.2. 说明性能** 就纯处理能力而言，Jam 机器架构可提供极高的同构无信任计算能力。然而，Jam 的核心模型是一个典型的并行化计算架构，要想很好地利用这一架构，解决方案的设计必须在一定程度上考虑到这一点。因此，在 Jam 上出现与现有系统语义相似的用例之前，很难与现有系统进行直接比较。尽管如此，如果我们允许自己做一些假设，我们还是可以进行一些粗略的比较。</h6>

*17.2.1. Comparison to Polkadot.* Pre-asynchronous backing, Polkadot validates around 50 parachains, each one utilizing approximately 250ms of native computation (i.e. half a second of Wasm execution time at around a 50% overhead) and 5mb of i/o for every twelve seconds of real time which passes. This corresponds to an aggregate compute performance of around parity with a native cpu core and a total 24-hour distributed availability of around 20mb/s. Accumulation is beyond Polkadot’s capabilities and so not comparable.
<h6>*17.2.1. 与 Polkadot 的比较* 在异步备份之前，Polkadot 验证了约 50 个 parachains，每个 parachains 使用约 250ms 的本地计算（即半秒的 Wasm 执行时间，开销约为 50%），每 12 秒的实时时间使用 5mb 的 i/o。这相当于总计算性能与本地 CPU 内核相当，24 小时分布式总可用率约为 20mb/s。Polkadot 的能力无法进行累积，因此不具可比性。</h6>

Post asynchronous-backing and estimating that Polkadot is at present capable of validating at most 80 parachains each doing one second of native computation in every six, then the aggregate performance is increased to around 13x native cpu and the distributed availability increased to around 67mb/s.
<h6>采用异步支持后，估计 Polkadot 目前最多能验证 80 个 parachains，每个 parachains 在每六个中进行一秒钟的本地计算，那么总体性能将提高到本地 CPU 的 13 倍左右，分布式可用性将提高到 67mb/s 左右。</h6>

For comparison, in our basic models, Jam should be capable of attaining around 85x the computation load of a single native cpu core and a distributed availability of 852mb/s.A more sophisticated model would be to use the Jam cores for balance updates as well as transaction verification. We would have to assume that state and the transactions which operate on them can be partitioned between work-packages with some degree of efficiency, and that the 15mb of the work-package would be split between transaction data and state witness data. Our basic models predict that a 4bn 32-bit account system paginated into 2 10 accounts/page and 128 bytes per transaction could, assuming only around 1% of oraclized accounts were useful, average upwards of 1.7mtps depending on partitioning and usage characteristics. Partitioning could be done with a fixed fragmentation (essentially sharding state), a rotating partition pattern or a dynamic partitioning (which would require specialized sequencing).

<h6>相比之下，在我们的基本模型中，Jam 的计算负荷应该是单个本地 CPU 内核的 85 倍左右，分布式可用性为 852mb/s。我们必须假定，状态和对其进行操作的交易可以在一定程度上高效地在工作包之间进行分割，而 15mb 的工作包将在交易数据和状态见证数据之间进行分割。我们的基本模型预测，一个 40 亿个 32 位账户系统，每页分页 2 10 个账户，每个事务 128 字节，假设只有约 1%的账户是有用的，那么根据分区和使用特点，平均处理速度可达 1.7mtps 以上。分区可采用固定分片（本质上是分片状态）、旋转分区模式或动态分区（需要专门的排序）。</h6>

Interestingly, we expect neither model to be bottlenecked in computation, meaning that transactions could be substantially more sophisticated, perhaps with more flexible cryptography or smart contract functionality, without a significant impact on performance.
<h6>有趣的是，我们预计这两种模式在计算方面都不会遇到瓶颈，这意味着交易可以更加复杂，也许可以采用更灵活的加密技术或智能合约功能，而不会对性能产生重大影响。</h6>

*17.2.3. Computation Throughput.* The tps metric does not lend itself well to measuring distributed systems’ computational performance, so we now turn to another slightly more compute-focussed benchmark: the evm. The basic YP Ethereum network, now approaching a decade old, is probably the best known example of general purpose decentralized computation and makes for a reasonable yardstick. It is able to sustain a computation and i/o rate of 1.25M gas/sec, with a peak throughput of twice that. The evm gas metric was designed to be a time-proportional metric for predicting and constraining program execution. Attempting to determine a concrete comparison to pvm throughput is non-trivial and necessarily opinionated owing to the disparity between the two platforms including word size, endianness and stack/register architecture and memory model. However, we will attempt to determine a reasonable range of values.
<h6>*17.2.3. 计算吞吐量* tps 指标本身并不能很好地衡量分布式系统的计算性能，因此我们现在转向另一个更专注于计算的基准：Evm。基本的 YP 以太坊网络现已接近十年历史，可能是通用去中心化计算的最著名范例，也是一个合理的衡量标准。它能够维持每秒 1.25 百万瓦斯的计算和 i/o 速率，峰值吞吐量是其两倍。evm 气体指标旨在成为预测和限制程序执行的时间比例指标。试图确定与 pvm 吞吐量的具体比较并不容易，而且由于两个平台之间的差异，包括字的大小、内联性、堆栈/寄存器架构和内存模型，因此必然会存在意见分歧。不过，我们将尝试确定一个合理的数值范围。</h6>


*17.2.2. Simple Transfers.* We might also attempt to model a simple transactions-per-second amount, with each transaction requiring a signature verification and the modification of two account balances. Once again, until there are clear designs for precisely how this would work we must make some assumptions. Our most naive model would be to use the Jam cores (i.e. refinement) simply for transaction verification and account lookups. The Jam chain would then hold and alter the balances in its state. This is unlikely to give great performance since almost all the needed i/o would be synchronous, but it can serve as a basis.

<h6>17.2.2 简易转账。17.2.2 小节讨论了简易转账功能。我们可能会尝试估算每秒简单转账的处理数量，其中每个转账涉及签名验证和两个账户余额的修改。和之前一样，由于缺乏明确的设计细节，我们不得不对此功能的运作方式进行一些假设。最简单的模型是仅使用 Jam 内核 (例如 refinement) 来进行交易验证和账户查询。然后，Jam 区块链将在其状态中持有并更改余额。这种方式不太可能带来高性能，因为几乎所有需要的输入/输出都将是同步的，但这可以作为一个基础模型。</h6>

A 15mb work-package can hold around 125k transactions at 128 bytes per transaction. However, a 90kb workresult could only encode around 11k account updates when each update is given as a pair of a 4 byte account index and 4 byte balance, resulting in a limit of 5.5k transactions per package, or 312k tps in total. It is possible that the eight bytes could typically be compressed by a byte or two, increasing maximum throughput a little. Our expectations are that state updates, with highly parallelized Merklization, can be done at between 500k and 1 million reads/write per second, implying around 250k-350k tps, depending on which turns out to be the bottleneck.
<h6>这段话评估了 Jam 区块链处理交易的性能上限，主要涉及以下方面：工作包 (work-package) 的容量：15MB 的工作包可以包含大约 12.5 万个交易，每个交易大小为 128 字节。工作结果 (workresult) 的编码限制：由于每个账户更新需要包含 4 字节的账户索引和 4 字节的余额，因此 90KB 的工作结果只能编码大约 1.1 万个账户更新。这导致每包的交易上限为 5.5 千个，总吞吐量约为 31.2 万笔交易每秒 (tps)。压缩提升：通过压缩账户更新数据 (可能将 8 个字节压缩成 6-7 个字节)，吞吐量可能略有提升。状态更新性能：预计使用高度并行化的 Merkle 树进行状态更新的速率可以在每秒 50 万到 100 万次读写之间。这取决于哪个环节成为瓶颈，实际的交易处理能力可能落在每秒 25 万到 35 万笔交易之间。</h6>

Evm gas does not directly translate into native execution as it also combines state reads and writes as well as transaction input data, implying it is able to process some combination of up to 595 storage reads, 57 storage writes and 1.25M gas as well as 78kb input data in eachsecond, trading one against the other.[^12] We cannot find any analysis of the typical breakdown between storage i/o and pure computation, so to make a very conservative estimate, we assume it does all four. In reality, we would expect it to be able to do on average 1/4 of each.

<h6>EVM Gas 不是直接衡量原生执行性能的指标，因为它还包含了状态读取、写入以及交易输入数据的大小。这表示 EVM 在每秒内可以处理某种组合的运算，例如最多 595 次存储读取、57 次存储写入、125 万 Gas 的运算量以及 78KB 的输入数据，这些执行类型可以相互权衡。 引用文献 [^12] 提供了这一信息。遗憾的是，我们找不到有关存储读写和纯计算之间典型划分比例的分析资料。因此，为了做出非常保守的估计，我们假设 EVM 可以同时处理这四个方面。
实际情况可能更乐观，我们预计 EVM 平均每种操作能处理其中的 1/4，即每秒可以处理 148 次存储读取、14 次存储写入、31.25 万 Gas 的运算量以及 19.5KB 的输入数据。</h6>

Our experiments[^13] show that on modern, high-end consumer hardware with a modern evm implementation, we can expect somewhere between 100 and 500 gas/µs in throughput on pure-compute workloads (we specifically utilized Odd-Product, Triangle-Number and several implementations of the Fibonacci calculation). To make a conservative comparison to pvm, we propose transcompilation of the evm code into pvm code and then reexecution of it under the Polkavm prototype.[^14]
<h6>我们的实验表明[^13]，在使用现代 EVM 实现的高端消费级硬件上，对于纯计算工作负载（我们具体使用了奇偶乘积、三角数和几种斐波那契计算的实现），吞吐量预计在 100 到 500 gas/µs 之间。为了与 PVM 进行保守的比较，我们建议将 EVM 代码转译成 PVM 代码，然后在 Polkavm 原型[^14] 下重新执行它。</h6>

To help estimate a reasonable lower-bound of evm gas/µs, e.g. for workloads which are more memory and i/o intensive, we look toward real-world permissionless deployments of the evm and see that the Moonbeam network, after correcting for the slowdown of executing within the recompiled WebAssembly platform on the somewhat conservative Polkadot hardware platform, implies a throughput of around 100 gas/µs. We therefore assert that in terms of computation, 1µs evm gas approximates to around 100-500 gas on modern high-end consumer hardware.[^15]
<h6>为了估算一个合理的 EVM gas/µs 下限 (例如对于更依赖内存和读写操作的工作负载)，我们研究了现实世界中无需许可的 EVM 部署案例。Moonbeam 网络在经过重新编译的 WebAssembly 平台（运行在相对保守的 Polkadot 硬件平台上）执行带来的速度降低因素校正后，表明其吞吐量约为 100 gas/µs。因此，从计算角度来看，在现代高端消费级硬件上，1 微秒的 EVM gas 大约相当于 100-500 gas。[^15]</h6>

Benchmarking and regression tests show that the prototype pvm engine has a fixed preprocessing overhead of around 5ns/byte of program code and, for arithmeticheavy tasks at least, a marginal factor of 1.6-2% compared to evm execution, implying an asymptotic speedup of around 50-60x. For machine code 1mb in size expected to take of the order of a second to compute, the compilation cost becomes only 0.5% of the overall time. 16 For code not inherently suited to the 256-bit evm isa, we would expect substantially improved relative execution times on pvm, though more work must be done in order to gain confidence that these speed-ups are broadly applicable.

<h6>性能测试和回归测试表明，原型 PVM 引擎具有固定的预处理开销，约为每字节程序代码 5 纳秒 (ns)。对于以运算为主的任务 (至少对于这类任务)，PVM 与 EVM 执行相比，性能开销增加的比例仅为 1.6-2%。这意味着渐近加速比大约为 50-60 倍。对于大小为 1MB 的机器码，预计需要大约一秒钟的计算时间，编译成本仅占总时间的 0.5%。对于天生不适合 256 位 EVM 指令集架构 (ISA) 的代码，我们期望在 PVM 上能获得大幅优于 EVM 的执行速度。不过，为了确信这种性能提升具有普遍适用性，还需要开展更多工作。</h6>

If we allow for preprocessing to take up to the same component within execution as the marginal cost (owing to, for example, an extremely large but short-running program) and for the pvm metering to imply a safety overhead of 2x to execution speeds, then we can expect a Jam core to be able to process the equivalent of around 1,500 evm gas/µs. Owing to the crudeness of our analysis we might reasonably predict it to be somewhere within a factor of three either way—i.e. 500-5,000 evm gas/µs.

<h6>如果我们考虑到预处理在执行过程中所占的比例与边际成本相同（例如，由于程序极其庞大但运行时间较短），并且 pvm 计量意味着执行速度的安全开销为 2 倍，那么我们可以预计一个 Jam 内核能够处理大约相当于 1,500 evm gas/µs 的工作量。由于我们的分析比较粗略，我们可以合理地预测其速度在 3 倍以内，即 500-5000 evm gas/µs。</h6>

Jam cores are each capable of 2.5mb/s bandwidth, which must include any state i/o and data which must be newly introduced (e.g. transactions). While writes come at comparatively little cost to the core, only requiring hashing to determine an eventual updated Merkle root, reads must be witnessed, with each one costing around 640 bytes of witness conservatively assuming a one-million entry binary Merkle trie. This would result in a maximum of a little under 4k reads/second/core, with the exact amount dependent upon how much of the bandwidth is used for newly introduced input data.
<h6>Jam 内核的带宽均为 2.5mb/s，其中必须包括任何状态 i/o 和必须新引入的数据（如事务）。虽然写入对内核的成本相对较低，只需要散列就能确定最终更新的 Merkle 根，但读取必须经过见证，假设有一百万条目二进制 Merkle 三元组，则每次读取的见证成本约为 640 字节。这将导致最高读取次数略低于 4k/秒/核，具体读取次数取决于有多少带宽用于新引入的输入数据。</h6>

Aggregating everything across Jam, excepting accumulation which could add further throughput, numbers can be multiplied by 341 (with the caveat that each one’s computation cannot interfere with any of the others’ except through state oraclization and accumulation). Unlike for roll-up chain designs such as Polkadot and Ethereum, there is no need to have persistently fragmented state. Smart-contract state may be held in a coherent format on the Jam chain so long as any updates are made through the 15kb/core/sec work results, which would need to contain only the hashes of the altered contracts’ state roots.
<h6>将所有内容汇总到JAM中，除了可以进一步增加吞吐量的累加之外，其他数字都可以乘以 341（需要注意的是，除了通过状态异或和累加之外，每个果酱的计算都不能干扰其他果酱的计算）。与 Polkadot 和以太坊等卷积链设计不同的是，它不需要持久的碎片状态。只要任何更新都是通过 15kb/core/sec 的工作结果进行的，智能合约的状态就可以以一致的格式保存在 Jam 链上。</h6>

Under our modelling assumptions, we can therefore summarize:
<h6>因此，根据我们的模型假设，我们可以总结出以下几点：</h6>

|                          |  Eth. L1 | Jam Core | Jam      |
| :----------------------: |  :-----: | :------: | :-----:  |
| Compute (evm gas/µs)     | $1.25^†$ | 500-5,000|0.15-1.5m |
| $State writes (s_{−1})$  | $57^†$   | n/a      | n/a      |
| $ State reads (s_{−1}) $ | $595^†$  | 4k^‡     | 1.4m^‡   |
| $Input data (s_{−1}) $   | $78kb^†$ |$2.5mb^‡$ | $852mb^‡$|

What we can see is that Jam’s overall predicted performance profile implies it could be comparable to many thousands of that of the basic Ethereum L1 chain. The large factor here is essentially due to three things: spacial parallelism, as Jam can host several hundred cores under its security apparatus; temporal parallelism, as Jam targets continuous execution for its cores and pipelines much of the computation between blocks to ensure a constant, optimal workload; and platform optimization by using a vm and gas model which closely fits modern hardware architectures.
<h6>Jam 区块链的整体性能预测表明，其性能可能相当于以太坊 L1 基础链的数千倍。造成这一巨大差异的主要因素有三点：空间并行性：Jam 的安全机制下可以容纳数百个内核，实现并行处理。时间并行性：Jam 针对其内核和流水线进行持续执行，并在区块之间尽可能多的进行计算流水化处理，以确保恒定、最佳的工作负载。平台优化：Jam 使用与现代硬件架构紧密贴合的虚拟机 (VM) 和 gas 模型进行优化。</h6>

It must however be understood that this is a provisional and crude estimation only. It is included for only the purpose of expressing Jam’s performance in tangible terms and is not intended as a means of comparing to a “full-blown” Ethereum/L2-ecosystem combination. Specifically, it does not take into account:
<h6>值得注意的是，这是一个临时性的粗略估计，仅用于用具体术语表达 Jam 区块链的性能，并不旨在与“完全成熟的”以太坊/L2 生态系统进行对比。具体来说，该估计没有考虑以下因素：</h6>

* that these numbers are based on real performance of Ethereum and performance modelling of Jam (though our models are based on real-world performance of the components);
* any L2 scaling which may be possible with either Jam or Ethereum;
* ● the state partitioning which uses of Jam would imply;
* the as-yet unfixed gas model for the pvm;
* that pvm/evm comparisons are necessarily imprecise;
* (†) all figures for Ethereum L1 are drawn from the same resource: on average each figure will be only 1/4 of this maximum.
* (‡) the state reads and input data figures for Jam are drawn from the same resource: on average each figure will be only 1/2 of this maximum.

<ul>
  <li>这些数字基于以太坊的实际性能和 Jam 的性能建模（尽管我们的模型是基于组件的实际性能）；</li>
  <li>JAM或以太坊可能进行的任何 L2 扩展</li>
  <li>Jam 的使用将意味着状态分割；</li>
  <li>尚未固定的 PVM 气体模型；</li>
  <li>pvm/evm 的比较必然是不精确的；</li>
  <li>(†) 以太坊 L1 的所有数字都来自同一资源：平均每个数字仅为最大值的 1/4 。</li>
  <li>(‡) JAM的状态读数和输入数据取自同一资源：平均而言，每个数据仅为最大值的 1/2。</li>
</ul>

We leave it as further work for an empirical analysis of performance and an analysis and comparison between Jam and the aggregate of a hypothetical Ethereum ecosystem which included some maximal amount of L2 deployments together with full Dank-sharding and any other additional consensus elements which they would require. This, however, is out of scope for the present work.
<h6>我们将性能的实证分析、Jam 与假设的以太坊生态系统性能的对比分析留待后续工作。这个假设的以太坊生态系统将包含最大数量的 L2 部署、完全的 Dank 分片以及任何他们所需要的其他共识元素。然而，这超出了本文档讨论的范围。</h6>

<h3 align="center"> 18. Conclusion </h3>
<h3 align="center"> 18. 结论 </h3>

We have introduced a novel computation model which is able to make use of pre-existing crypto-economic mechanisms in order to deliver major improvements in scalability without causing persistent state-fragmentation and thus sacrificing overall cohesion. We call this overall pattern collect-refine-join-accumulate. Furthermore, we have formally defined the on-chain portion of this logic, essentially the join-accumulate portion. We call this protocol the Jam chain. 
<h6>我们引入了一种新颖的计算模型，它能够利用现有的密码经济机制来大幅提高可扩展性，同时避免持续的状态碎片化，从而避免牺牲整体的凝聚力。我们将这种整体模式称为“收集-精炼-加入-累积”。此外，我们还正式定义了链上部分的逻辑，实质上是“加入-累积”部分。我们将此协议称为 Jam 链。</h6>

We argue that the model of Jam provides a novel “sweet spot”, allowing for massive amounts of computation to be done in secure, resilient consensus compared to fullysynchronous models, and yet still have strict guarantees about both timing and integration of the computation into some singleton state machine unlike persistently fragmented models.
<h6>我们认为 Jam 模型提供了一个新颖的“甜蜜点”，与完全同步模型相比，它可以在安全、强韧的共识下进行大量计算，同时还能像单个状态机一样对计算的时序和集成提供严格的保证，这与持续碎片化模型不同。</h6>

**18.1. Further Work. ** While we are able to estimate theoretical computation possible given some basic assumptions and even make broad comparisons to existing systems, practical numbers are invaluable. We believe the model warrants further empirical research in order to better understand how these theoretical limits translate into real-world performance. We feel a proper cost analysis and comparison to pre-existing protocols would also be an excellent topic for further work.
<h6>18.1 后续工作。虽然我们基于一些基本假设可以估算理论计算能力，甚至可以与现有系统进行大致对比，但实际数据更具价值。我们认为该模型值得进一步的实证研究，以更好地理解这些理论极限如何转化为现实世界的性能。我们认为，对成本进行适当的分析并与现有协议进行比较也将是后续工作的一个绝佳主题。</h6>

We can be reasonably confident that the design of Jam allows it to host a service under which Polkadot parachains could be validated, however further prototyping work is needed to understand the possible throughput which a pvm-powered metering system could support. We leave such a report as further work. Likewise, we have also intentionally omitted details of higher-level protocol elements including cryptocurrency, coretime sales, staking and regular smart-contract functionality.
<h6>
  我们有充分的理由相信 Jam 的设计允许它承载一项服务，该服务可以用来验证 Polkadot 的平行链。然而，还需要进一步的原型开发工作来理解基于 PVM 的计量系统所能支持的潜在吞吐量。我们把这样的报告留待以后的工作去做。同样地，我们也故意省略了关于高级协议元素的细节，例如 加密货币、核心时间销售、权益证明和常规智能合约功能。
</h6>
A number of potential alterations to the protocol described here are being considered in order to make practical utilization of the protocol easier. These include:
* Synchronous calls between services in accumulate
* Restrictions on the transfer function in order to allow for substantial parallelism over accumulation.
* The possibility of reserving substantial additional computation capacity during accumulate under certain conditions.
* Introducing Merklization into the Work Package format in order to obviate the need to have the whole package downloaded in order to evaluate its authorization.

<ul>
  <li>累积服务之间的同步调用</li>
  <li>对传递函数的限制，以便在积累过程中实现大量并行。</li>
  <li>在某些条件下，可在累积期间预留大量额外的计算能力。</li>
  <li>在 "工作包 "格式中引入 "默克尔化"（Merklization），以避免需要下载整个 "工作包 "才能对其授权进行评估。</li>
</ul>

The networking protocol is also left intentionally undefined at this stage and its description must be done in a follow-up proposal.

<h6>此时，网络协议也 故意没有明确定义，其描述必须留待后续提案进行详细阐述。</h6>

Validator performance is not presently tracked onchain. We do expect this to be tracked on-chain in the final revision of the Jam protocol, but its specific format is not yet certain and it is therefore omitted at present.
<h6>目前验证者性能数据并不会被记录在链上。我们预计在 Jam 协议的最终版本中，验证者性能将会被记录在链上，但具体的数据格式尚不明确，因此暂时没有包含在文档中。</h6>

<h3 align="center">19. Acknowledgements</h3>
<h3 align="center">19. 致谢 </h3>

Much of this present work is based in large part on the work of others. The Web3 Foundation research team and in particular Alistair Stewart and Jeff Burdges are responsible for Elves, the security apparatus of Polkadot which enables the possibility of in-core computation for Jam. The same team is responsible for Sassafras, Grandpa and Beefy.

<h6>目前的工作在很大程度上是基于其他人的工作。Web3 基金会的研究团队，尤其是阿利斯泰尔-斯图尔特（Alistair Stewart）和杰夫-伯吉斯（Jeff Burdges）负责开发 Polkadot 的安全装置精灵（Elves），它使 Jam 的内核计算成为可能。同一团队还负责 Sassafras、Grandpa 和 Beefy。</h6>

Safrole is a mild simplification of Sassafras and was made under the careful review of Davide Gallosi and Alistair Stewart.

<h6>Safrole 是对 Sassafras 的轻度简化，是在 Davide Gallosi 和 Alistair Stewart 的仔细审查下制成的。</h6>

The original CoreJam rfc was refined under the review of Bastian Köcher and Robert Habermeier and most of the key elements of that proposal have made their way into the present work.
<h6>最初的 CoreJam rfc 是在 Bastian Köcher 和 Robert Habermeier 的审查下完善的，该提案的大部分关键要素都已纳入目前的工作中。</h6>

The pvm is a formalization of a partially simplified PolkaVM software prototype, developed by Jan Bujak.Cyrill Leutwiler contributed to the empirical analysis of the pvm reported in the present work.
<h6>pvm 是 Jan Bujak 开发的部分简化 PolkaVM 软件原型的形式化。Cyrill Leutwiler 对本作品中报告的 pvm 经验分析做出了贡献。</h6>

The PolkaJam team and in particular Arkadiy Paronyan, Emeric Chevalier and Dave Emmet have been instrumental in the design of the lower-level aspects of the Jam protocol, especially concerning Merklization and i/o.

<h6>PolkaJam 团队，尤其是 Arkadiy Paronyan、Emeric Chevalier 和 Dave Emmet，在 Jam 协议的底层设计方面发挥了重要作用，特别是在 Merklization 和 i/o 方面。</h6>

And, of course, thanks to the awesome Lemon Jelly, a.k.a. Fred Deakin and Nick Franglen, for three of the most beautiful albums ever produced, the cover art of the first of which was inspiration for this paper’s background art.
<h6>当然，还要感谢令人敬畏的 "柠檬果冻 "乐队（又名弗雷德-迪肯和尼克-弗兰伦），他们制作了三张有史以来最精美的专辑，其中第一张专辑的封面设计正是本文背景设计的灵感来源。</h6>


<img width="705" alt="image" src="https://github.com/harodggg/jam/assets/31732456/1f5c3834-0c03-4adf-a2b4-fb7e4b759f60">

<img width="720" alt="image" src="https://github.com/harodggg/jam/assets/31732456/eed7afa2-caa8-4fea-aaae-1204e9d6d031">


<img width="564" alt="image" src="https://github.com/harodggg/jam/assets/31732456/e0fdb668-0b73-41e3-8129-2fc8d668e01a">


<img width="545" alt="image" src="https://github.com/harodggg/jam/assets/31732456/60d210b5-d85c-469f-b071-60cd231485ef">

<img width="606" alt="image" src="https://github.com/harodggg/jam/assets/31732456/c7c761ec-5cf8-4f94-b279-5fed38f80b70">

<img width="588" alt="image" src="https://github.com/harodggg/jam/assets/31732456/cf391980-c08e-4b1b-801a-480a4e33a828">

<img width="624" alt="image" src="https://github.com/harodggg/jam/assets/31732456/f0a865a6-70a5-4a8c-ad7d-fc8c4b9d2aed">

<img width="590" alt="image" src="https://github.com/harodggg/jam/assets/31732456/95a69df6-d44f-46af-910d-e44b7432ca53">

<img width="585" alt="image" src="https://github.com/harodggg/jam/assets/31732456/f05db8ce-6ba7-4e4b-b410-422bf7f5230a">

<img width="559" alt="image" src="https://github.com/harodggg/jam/assets/31732456/7052ca7d-f843-4630-8d8c-76b22e382ffc">

<img width="575" alt="image" src="https://github.com/harodggg/jam/assets/31732456/cbd8ba1c-a0a6-4e35-a59d-c0f078607f3f">

<img width="579" alt="image" src="https://github.com/harodggg/jam/assets/31732456/e83866fe-5c4a-44a8-a19a-3b28d8edd376">

<img width="582" alt="image" src="https://github.com/harodggg/jam/assets/31732456/e9aa10fc-9664-4c33-b474-22e76605dcff">

<img width="574" alt="image" src="https://github.com/harodggg/jam/assets/31732456/ce4aa15e-fde9-4192-8b85-e71af5d1aa5c">

<img width="570" alt="image" src="https://github.com/harodggg/jam/assets/31732456/2551054c-b867-4d5b-8f3a-e9f9a2d9cea7">

<img width="580" alt="image" src="https://github.com/harodggg/jam/assets/31732456/31d9c402-2253-4e66-9e70-019c06031f3a">

<img width="539" alt="image" src="https://github.com/harodggg/jam/assets/31732456/ab79a9a6-713b-44f6-8022-3beec820c1ec">

<img width="557" alt="image" src="https://github.com/harodggg/jam/assets/31732456/458f7519-ba5a-4c84-8b33-a63241d27f83">

<img width="547" alt="image" src="https://github.com/harodggg/jam/assets/31732456/f341fb9d-392e-46ff-b175-9728c128b2c7">

<img width="539" alt="image" src="https://github.com/harodggg/jam/assets/31732456/04ac0f14-5120-4ca0-88bb-793ae1b8eb10">

<img width="582" alt="image" src="https://github.com/harodggg/jam/assets/31732456/3465d4d2-67a6-4f1f-a4b6-e6c8fc16b143">

<img width="556" alt="image" src="https://github.com/harodggg/jam/assets/31732456/0ee87b42-3bd4-4faf-9a99-6cd9d47f0ad8">

<img width="588" alt="image" src="https://github.com/harodggg/jam/assets/31732456/9de703d7-2737-46eb-9137-d233075721c2">

<img width="585" alt="image" src="https://github.com/harodggg/jam/assets/31732456/e557b310-3196-4e40-9e9b-e66a49669616">

<img width="602" alt="image" src="https://github.com/harodggg/jam/assets/31732456/1822d305-91b5-4c8c-abe0-90e684aa8c3d">

<img width="541" alt="image" src="https://github.com/harodggg/jam/assets/31732456/f1a073db-e3dd-4f4e-8441-38e25244e1a5">

[^1]: The gas mechanism did restrict what programs can execute on it by placing an upper bound on the number of steps which may be executed, but some restriction to avoid infinite-computation must surely be introduced in a permissionless setting.
[^2]: Practical matters do limit the level of real decentralization. Validator software expressly provides functionality to allow a single instance to be configured with multiple key sets, systematically facilitating a much lower level of actual decentralization than the apparent number of actors, both in terms of individual operators and hardware. Using data collated by Dune and hildobby 2024 on Ethereum 2, one can see one major node operator, Lido, has steadily accounted for almost one-third of the almost one million crypto-economic participants.
[^3]: Ethereum’s developers hope to change this to something more secure, but no timeline is fixed.
[^4]: Some initial thoughts on the matter resulted in a proposal by Sadana 2024 to utilize Polkadot technology as a means of helping create a modicum of compatibility between roll-up ecosystems!
[^5]: In all likelihood actually substantially more as this was using low-tier “spare” hardware in consumer units, and our recompiler was unoptimized.
[^6]: Earlier node versions utilized Arweave network, a decentralized data store, but this was found to be unreliable for the data throughput which Solana required.（早期的 Solana 节点版本曾使用 Arweave 网络作为去中心化数据存储方案。然而，事实证明 Arweave 网络无法满足 Solana 所需的数据吞吐量，因此被弃用。）
[^7]: Practically speaking, blockchains sometimes make assumptions of some fraction of participants whose behavior is simply honest, and not provably incorrect nor otherwise economically disincentivized. While the assumption may be reasonable, it must nevertheless be stated apart from the rules of state-transition.（实事求是地讲，区块链有时会假设参与者中的一部分人是诚实的，他们的行为并非可证明的错误，也并非受到经济上的惩罚。尽管这种假设在一定程度上是合理的，但它仍然需要与状态转换规则分开来单独陈述。）
[^8]: 1,704,110,400 seconds after the Unix Epoch.
[^9]: This is three fewer than risc-v’s 16, however the amount that program code output by compilers uses is 13 since two are reserved for operating system use and the third is fixed as zero
[^10]: Technically there is some small assumption of state, namely that some modestly recent instance of each service’s preimages. The specifics of this are discussed in section 13.2.
[^11]: This is a “soft” implication since there is no consequence on-chain if dishonestly reported. For more information on this implication see section 13.4.
[^12]: The latest “proto-danksharding” changes allow it to accept 87.3kb/s in committed-to data though this is not directly available within state, so we exclude it from this illustration, though including it with the input data would change the results little
[^13]: This is detailed at https://hackmd.io/@XXX9CM1uSSCWVNFRYaSB5g/HJarTUhJA and intended to be updated as we get more information
[^14]: It is conservative since we don’t take into account that the source code was originally compiled into evm code and thus the pvm machine code will replicate architectural artifacts and thus is very likely to be pessimistic. As an example, all arithmetic operations in evm are 256-bit and 32-bit native pvm is being forced to honor this even if the source code only actually required 32-bit values.
[^15]: We speculate that the substantial range could possibly be caused in part by the major architectural differences between the evm is a typical modern hardware.
