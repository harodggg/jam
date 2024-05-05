<h1 align="center">JAM</h1>

<h5 align="center">JOIN-ACCUMULATE MACHINE: A SEMI-COHERENT SCALABLE TRUSTLESS VM</h5>
<h5 align="center">聚合-累计 机器: 一个半相干的可以扩展的无需信任的虚拟机 </h5>
<h6 align="center">( JOIN: 表示多个实体，可能是不同种类的，不同用途的实体一起来使用。)</h6>
<h6 align="center">( ACCUMULATE： 表示这些实体的使用该机器的结果，是依赖于前置的，历史的，类似于状态机，当前结果是依赖于前一个结果。)</h6>
<h6 align="center">( semi-coherent: semi一半，coherent一致，相关，相干。也许是不一定需要强一致性，类似于一个区块，只有唯一的历史结果，只有一个世界树，这个也许可以包含多个历史结果，类似平行宇宙。 )</h6>

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
3.8.1 哈希（Hashing). $\mathcal{H}$ 表示一组通常由密码函数产生的 256 位值的集合，这与 $\mathbb{Y}_{32}$ 等价，其中 H0 等于 $[0]_{32}$。$[0]_{32}$ 表示由 32 个 0 组成的字节串。
$\mathcal{H}(m ∈ \mathbb{Y}) ∈ \mathbb{H}$ 表示一个函数 $\mathcal{H}$，它接受集合 $\mathbb{Y}$ 中的元素 m 为输入，并输出一个属于集合 $\mathbb{H}$ 的 256 位哈希值。Blake2b由 Saarinen 和 Aumasson 在 2015 年提出的密码散列函数，可以生成 256 位的哈希值。“散列”和“哈希”在该语境下可以互换使用. $\mathcal{H}_K(m ∈ \mathbb{Y}) ∈ H$ 表示另一个函数 $\mathcal{H}_K$ ，它也接受集合 $\mathbb{Y}$ 中的元素 m 为输入，并输出一个属于集合 $\mathbb{H}$ 的 256 位哈希值。 Keccak: 由 Bertoni 等人在 2013 年提出的密码散列函数，Keccak 也是 SHA-3 算法的基础，可以生成 256 位的哈希值。 Wood 2014: 推测 Wood 在 2014 年的某份研究或文档中使用了 Keccak 哈希函数。</h6>

We may sometimes wish to take only the first x octets of a hash, in which case we denote $\mathcal{H}_x(m) ∈ \mathbb{Y}_x$ to be the first x octets of $\mathcal{H}(m)$ . The inputs of a hash function are generally assumed to be serialized with our codec $\upvarepsilon(x) ∈ Y$ , however for the purposes of clarity or unambiguity we may also explicitly denote the serialization. Similarly, we may wish to interpret a sequence of octets as some other kind of value with the assumed decoder function $\upvarepsilon\_{−1} (x ∈ \mathbb{Y})$ . In both cases, we may subscript the transformation function with the number of octets we expect the octet sequence term to have. Thus, r = $\upvarepsilon\_4(x ∈ N)$ would assert x ∈ $\mathbb{N}\_{2^{32}}$ and r ∈ $\mathbb{Y}_4$, whereas $s = \upvarepsilon^{−1}_8 (y)$ would assert $y ∈ \mathbb{Y}\_8$ and $s ∈ \mathbb{N}\_{2^{64}}$ .

<h6>有时我们可能只想取哈希值的前 x 个八位节，记为 $\mathcal{H}_x(m) ∈ \mathbb{Y}_x$，表示提取哈希函数 $\mathcal{H}(m)$ 的前 x 个八位节的结果为一个长度为 x 的八位节序列。哈希函数的输入通常假定使用我们的编码器 $\upvarepsilon(x) ∈ \mathbb{Y}$ 进行序列化。但是为了清晰或避免歧义，我们也可以明确表示序列化过程。类似地，我们可能希望将一个八位节序列解释为某种其他类型的值，并使用解码器函数 $\upvarepsilon\_{−1} (x ∈ \mathbb{Y})$ 。在这两种情况下，我们可以用期望的八位节序列的长度作为下标来标记转换函数。例如，r = $\upvarepsilon\_4(x ∈ N)$  表示 x ∈  $\mathbb{N}\_{2^{32}}$ 且 r ∈ $\mathbb{Y}_4$ ，这意味着将一个 32 位无符号整数 ( $\mathbb{N}$ ) 编码成一个 4 位节序列 $\mathbb{Y}_4$ 。另一种情况是 $s = \upvarepsilon^{−1}_8 (y)$ ，表示 y ∈ Y8 且 s ∈ N264，即解码一个 8 位节序列 ($y ∈ \mathbb{Y}\_8$ ) 成一个 64 位无符号整数 ( $s ∈ \mathbb{N}\_{2^{64}}$ )。</h6>

[^1]: The gas mechanism did restrict what programs can execute on it by placing an upper bound on the number of steps which may be executed, but some restriction to avoid infinite-computation must surely be introduced in a permissionless setting.
[^2]: Practical matters do limit the level of real decentralization. Validator software expressly provides functionality to allow a single instance to be configured with multiple key sets, systematically facilitating a much lower level of actual decentralization than the apparent number of actors, both in terms of individual operators and hardware. Using data collated by Dune and hildobby 2024 on Ethereum 2, one can see one major node operator, Lido, has steadily accounted for almost one-third of the almost one million crypto-economic participants.
[^3]: Ethereum’s developers hope to change this to something more secure, but no timeline is fixed.
[^4]: Some initial thoughts on the matter resulted in a proposal by Sadana 2024 to utilize Polkadot technology as a means of helping create a modicum of compatibility between roll-up ecosystems!
[^5]: In all likelihood actually substantially more as this was using low-tier “spare” hardware in consumer units, and our recompiler was unoptimized.
[^6]: Earlier node versions utilized Arweave network, a decentralized data store, but this was found to be unreliable for the data throughput which Solana required.

