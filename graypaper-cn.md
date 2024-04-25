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

<h6>$$\left( \sum{k=1}^n a_k b_k \right)^2 \leq \left( \sum{k=1}^n ak^2 \right) \left( \sum{k=1}^n b_k^2 \right)$$</h6>

**1.2. Driving Factors.** Within the realm of blockchain and the wider Web3, we are driven by the need first and foremost to deliver resilience. A proper Web3 digital system should honor a declared service profile—and ideally meet even perceived expectations—regardless of the desires, wealth or power of any economic actors including individuals, organizations and, indeed, other Web3 systems. Inevitably this is aspirational, and we must be pragmatic over how perfectly this may really be delivered. Nonetheless, a Web3 system should aim to provide such radically strong guarantees that, for practical purposes, the system may be described as unstoppable.
<h6><strong>1.2. 驱动因素。 </strong>在区块链和更广泛的Web3领域中，我们最优先的驱动因素是提供韧性(弹性)。在理想的 Web3 数字系统中，服务质量承诺应当得到尊重，并尽力满足用户即使是潜在的期望，这与任何经济参与者的意愿、财富或权力无关，这些参与者包括个人、组织，当然也包括其他 Web3 系统。不可避免地，这是一个理想化的目标，我们必须务实地看待它能被完美实现的程度。尽管如此，Web3 系统仍应致力于提供如此强有力的保障，以至于在实际操作中，该系统可以被描述为不可阻挡的(不会停机的)。 </h6>

While Bitcoin is, perhaps, the first example of such a system within the economic domain, it was not general purpose in terms of the nature of the service it offered. A rules-based service is only as useful as the generality of the rules which may be conceived and placed within it. Bitcoin’s rules allowed for an initial use-case, namely a fixedissuance token, ownership of which is well-approximated and autonomously enforced through knowledge of a secret, as well as some further elaborations on this theme.

<h6>虽然比特币可能是经济领域内这种系统最早的例子之一，但它所提供的服务性质使其并非通用型。 </h6>

Later, Ethereum would provide a categorically more general-purpose rule set, one which was practically Turing complete.[^1] In the context of Web3 where we are aiming to deliver a massively multiuser application platform, generality is crucial, and thus we take this as a given.

Beyond resilience and generality, things get more interesting, and we must look a little deeper to understand what our driving factors are. For the present purposes,we identify three additional goals:
1. Resilience: highly resistant from being stopped,corrupted and censored.
2. Generality: able to perform Turing-complete computation.
3. Performance: able to perform computation quickly and at low cost.
4. Coherency: the causal relationship possible between different elements of state and how thus how well individual applications may be composed.
5. Accessibility: negligible barriers to innovation;easy, fast, cheap and permissionless.
   
As a declared Web3 technology, we make an implicit assumption of the first two items. Interestingly, items 3 and 4 are antagonistic according to an information theoretic principle which we are sure must already exist in some form but are nonetheless unaware of a name for it. For argument’s sake we shall name it *size-synchrony antagonism*.

**1.3. Scaling under Size-Synchrony Antagonism.** Size-synchrony antagonism is a simple principle implying that as the state-space of information systems grow, then the system necessarily becomes less synchronous. The argument goes:
1. The more state a system utilizes for its dataprocessing, the greater the amount of space this state must occupy.
2. The more space used, then the greater the mean and variance of distances between statecomponents.
3. As the mean and variance increase, then interactions become slower and subsystems must manage the possibility that distances between interdependent components of state could be materially different, requiring asynchrony.

This assumes perfect coherency of the system’s state.Setting the question of overall security aside for a moment,we can avoid this rule by applying the divide and conquer maxim and fragmenting the state of a system, sacrificing its coherency. We might for example create two independent smaller-state systems rather than one large-state system. This pattern applies a step-curve to the principle;intra-system processing has low size and high synchrony,inter-system processing has high size but low synchrony.It is the principle behind meta-networks such as Polkadot,Cosmos and the predominant vision of a scaled Ethereum(all to be discussed in depth shortly).

The present work explores a middle-ground in the antagonism, avoiding the persistent fragmentation of statespace of the system as with existing approaches. We do this by introducing a new model of computation which pipelines a highly scalable element to a highly synchronous element. Asynchrony is not avoided, but we do open the possibility for a greater degree of granularity over how it is traded against size. In particular fragmentation can be made ephemeral rather than persistent, drawing upon a coherent state and fragmenting it only for as long as it takes to execute any given piece of processing on it.


Unlike with snark-based L2-blockchain techniques for scaling, this model draws upon crypto-economic mechanisms and inherits their low-cost and high-performance profiles and averts a bias toward centralization.

**1.4. Document Structure.** We begin with a brief overview of present scaling approaches in blockchain technology in section 2. In section 3 we define and clarify the notation from which we will draw for our formalisms.

We follow with a broad overview of the protocol in section 4 outlining the major areas including the Polka Virtual Machine (pvm), the consensus protocols Safrole and Grandpa, the common clock and build the foundations of the formalism.

We then continue with the full protocol definition split into two parts: firstly the correct on-chain state-transition formula helpful for all nodes wishing to validator the chain state, and secondly, in sections 13 and 15 the honest strategy for the off-chain actions of any actors who wield a validator key.

The main body ends with a discussion over the performance characteristics of the protocol in section 17 and finally conclude in section 18.

The appendix contains various additional material important for the protocol definition including the pvm in appendices A & B, serialization and Merklization in appendices C & D and cryptography in appendices F, G & H. We finish with an index of terms which includes the values of all simple constant terms used in the work in appendix I, and close with the bibliography.

<h3 align="center">2. Previous Work and Present Trends</h3>

In the years since the initial publication of the Ethereum *YP* , the field of blockchain development has grown immensely. Other than scalability, development has been done around underlying consensus algorithms, smart-contract languages and machines and overall state environments. While interesting, these latter subjects are mostly out scope of the present work since they generally do not impact underlying scalability.

**2.1. Polkadot.** In order to deliver its service, Jam coopts much of the same game-theoretic and cryptographic machinery as Polkadot known as Elves and described by Stewart 2018. However, major differences exist in the actual service offered with Jam, providing an abstraction much closer to the actual computation model generated by the validator nodes its economy incentivizes

It was a major point of the original Polkadot proposal, a scalable heterogeneous multichain, to deliver highperformance through partition and distribution of the workload over multiple host machines. In doing so it took an explicit position that composability would be lowered. Polkadot’s constituent components, parachains are, practically speaking,highly isolated in their nature. Though a message passing system (xcmp) exists it is asynchronous,coarse-grained and practically limited by its reliance on a high-level slowly evolving interaction language xcm.

As such, the composability offered by Polkadot between its constituent chains is lower than that of Ethereum-like smart-contract systems offering a single and universal object environment and allowing for the kind of agile and innovative integration which underpins their success. Polkadot, as it stands, is a collection of independent ecosystems with only limited opportunity for collaboration, very similar in ergonomics to bridged blockchains though with a categorically different security profile. A technical proposal known as spree would utilize Polkadot’s unique shared-security and improve composability, though blockchains would still remain isolated.

Implementing and launching a blockchain is hard, timeconsuming and costly. By its original design, Polkadot limits the clients able to utilize its service to those who are both able to do this and raise a sufficient deposit to win an auction for a long-term slot, one of around 50 at the present time. While not permissioned per se, accessibility is categorically and substantially lower than for smart-contract systems similar to Ethereum.

Enabling as many innovators to participate and interact, both with each other and each other’s user-base, appears to be an important component of success for a Web3 application platform. Accessibility is therefore crucial.

**2.2. Ethereum.** The Ethereum protocol was formally defined in this paper’s spiritual predecessor, the Yellow Paper, by Wood 2014. This was derived in large part from the initial concept paper by Buterin 2013. In the decade since the YP was published, the de facto Ethereum protocol and public network instance have gone through a number of evolutions, primarily structured around introducing flexibility via the transaction format and the instruction set and “precompiles” (niche, sophisticated bonus instructions) of its scripting core, the Ethereum virtual machine(evm).

Almost one million crypto-economic actors take part in the validation for Ethereum.[^2] Block extension is done through a randomized leader-rotation method where the physical address of the leader is public in advance of their block production.[^3] Ethereum uses Casper-FFG introduced by Buterin and Griffith 2019 to determine finality, which with the large validator base finalizes the chain extension around every 13 minutes.

Ethereum’s direct computational performance remains broadly similar to that with which it launched in 2015, with a notable exception that an additional service now allows 1mb of commitment data to be hosted per block (all nodes to store it for a limited period). The data cannot be directly utilized by the main state-transition function, but special functions provide proof that the data(or some subsection thereof) is available. According to Ethereum Foundation 2024b, the present design direction is to improve on this over the coming years by splitting responsibility for its storage amongst the validator base in a protocol known as *Dank-sharding.*

According to Ethereum Foundation 2024a, the scaling strategy of Ethereum would be to couple this data availability with a private market of roll-ups, sideband computation facilities of various design, with zk-snark-based roll-ups being a stated preference. Each vendor’s roll-up design, execution and operation comes with its own implications.

One might reasonably assume that a diversified marketbased approach for scaling via multivendor roll-ups will allow well-designed solutions to thrive. However, there are potential issues facing the strategy. A research report by Sharma 2023 on the level of decentralization in the various roll-ups found a broad pattern of centralization, but notes that work is underway to attempt to mitigate this. It remains to be seen how decentralized they can yet be made.

Heterogeneous communication properties (such as datagram latency and semantic range), security properties(such as the costs for reversion, corruption, stalling and censorship) and economic properties (the cost of accepting and processing some incoming message or transaction) may differ, potentially quite dramatically, between major areas of some grand patchwork of roll-ups by various competing vendors. While the overall Ethereum network may eventually provide some or even most of the underlying machinery needed to do the sideband computation it is far from clear that there would be a “grand consolidation” of the various properties should such a thing happen. We have not found any good discussion of the negative ramifications of such a fragmented approach.[^4]

*2.2.1. Snark Roll-ups.* While the protocol’s foundation makes no great presuppositions on the nature of roll-ups, Ethereum’s strategy for sideband computation does centre around snark-based rollups and as such the protocol is being evolved into a design that makes sense for this. Snarks are the product of an area of exotic cryptography which allow proofs to be constructed to demonstrate to a neutral observer that the purported result of performing some predefined computation is correct. The complexity of the verification of these proofs tends to be sub-linear in their size of computation to be proven and will not give away any of the internals of said computation,nor any dependent witness data on which it may rely.

Zk-snarks come with constraints. There is a trade-off between the proof’s size, verification complexity and the computational complexity of generating it. Non-trivial computation, and especially the sort of general-purpose computation laden with binary manipulation which makes smart-contracts so appealing, is hard to fit into the model of snarks.

To give a practical example, risc-zero (as assessed by Bögli 2024) is a leading project and provides a platform for producing snarks of computation done by a risc-v virtual machine, an open-source and succinct risc machine architecture well-supported by tooling. A recent benchmarking report by PolkaVM Project 2024 showed that compared to risc-zero’s own benchmark, proof generation alone takes over 61,000 times as long as simply recompiling and executing even when executing on 32 times as many cores, using 20,000 times as much ram and an additional state-of-the-art gpu. According to hardware rental agents https://cloud-gpus.com/, the cost multiplier of proving using risc-zero is 66,000,000x of the cost[^5] to execute using our risc-v recompiler.

Many cryptographic primitives become too expensive to be practical to use and specialized algorithms and structures must be substituted. Often times they are otherwise suboptimal. In expectation of the use of snarks (such as plonk as proposed by Gabizon, Williamson, and Ciobotaru 2019), the prevailing design of the Ethereum project’s Dank-sharding availability system uses a form of erasure coding centered around polynomial commitments over a large prime field in order to allow snarks to get acceptably performant access to subsections of data. Compared to alternatives, such as a binary field and Merklization in the present work, it leads to a load on the validator nodes orders of magnitude higher in terms of cpu usage.

In addition to their basic cost, snarks present no great escape from decentralization and the need for redundancy,leading to further cost multiples. While the need for some
benefits of staked decentralization is averted through their verifiable nature, the need to incentivize multiple parties to do much the same work is a requirement to ensure that
a single party not form a monopoly (or several not form a cartel). Proving an incorrect state-transition should be impossible, however service integrity may be compromised in other ways; a temporary suspension of proof-generation, even if only for minutes, could amount to major economic ramifications for real-time financial applications.

Real-world examples exist of the pit of centralization giving rise to monopolies. One would be the aforementioned snark-based exchange framework; while notionally serving decentralized exchanges, it is in fact centralized with Starkware itself wielding a monopoly over enacting trades through the generation and submission of proofs, leading to a single point of failure—should Starkware’s service become compromised, then the liveness of the system would suffer.

It has yet to be demonstrated that snark-based strategies for eliminating the trust from computation will ever be able to compete on a cost-basis with a multi-party crypto-economic platform. All as-yet proposed snarkbased solutions are heavily reliant on crypto-economic systems to frame them and work around their issues. Data availability and sequencing are two areas well understood as requiring a crypto-economic solution.

We would note that snark technology is improving and the cryptographers and engineers behind them do expect improvements in the coming years. In a recent article by Thaler 2023 we see some credible speculation that with some recent advancements in cryptographic techniques, slowdowns for proof generation could be as little as 50,000x from regular native execution and much of this could be parallelized. This is substantially better than the present situation, but still several orders of magnitude greater than would be required to compete on a cost-basis with established crypto-economic techniques such as elves.

**2.3. Fragmented Meta-Networks.** Directions for general-purpose computation scalability taken by other projects broadly centre around one of two approaches; either what might be termed a fragmentation approachor alternatively a centralization approach. We argue that neither approach offers a compelling solution.

The fragmentation approach is heralded by projects such as Cosmos (proposed by Kwon and Buchman 2019) and Avalanche (by Tanana 2019). It involves a system fragmented by networks of a homogenous consensus mechanic, yet staffed by separately motivated sets of validators. This is in contrast to Polkadot’s single validator set and Ethereum’s declared strategy of heterogeneous rollups secured partially by the same validator set operating under a coherent incentive framework. The homogeneity of said fragmentation approach allows for reasonably consistent messaging mechanics, helping to present a fairly unified interface to the multitude of connected networks.

However, the apparent consistency is superficial. The networks are trustless only by assuming correct operation of their validators, who operate under a crypto-economic security framework ultimately conjured and enforced by economic incentives and punishments. To do twice as much work with the same levels of security and no special coordination between validator sets, then such systems essentially prescribe forming a new network with the same overall levels of incentivization.

Several problems arise. Firstly, there is a similar downside as with Polkadot’s isolated parachains and Ethereum’s isolated roll-up chains: a lack of coherency due to a persistently sharded state preventing synchronous composability.

More problematically, the scaling-by-fragmentation approach, proposed specifically by Cosmos, provides no homogenous security—and therefore trustlessness—guarantees. Validator sets between networks must be assumed to be independently selected and incentivized with no relationship, causal or probabilistic, between the Byzantine actions of a party on one network and potential for appropriate repercussions on another. Essentially, this means that should validators conspire to corrupt or revert the state of one network, the effects may be felt across other networks of the ecosystem.

That this is an issue is broadly accepted, and projects propose for it to be addressed in one of two ways. Firstly, to fix the expected cost-of-attack (and thus level of security) across networks by drawing from the same validator set. The massively redundant way of doing this, as proposed by Cosmos Project 2023 under the name replicated security, would be to require each validator to validate on all networks and for the same incentives and punishments. This is economically inefficient in the cost of security provision as each network would need to independently provide the same level of incentives and punishment-requirements as the most secure with which it wanted to interoperate. This is to ensure the economic proposition remain unchanged for validators and the security proposition remained equivalent for all networks. At the present time, replicated security is not a readily
available permissionless service. We might speculate that these punishing economics have something to do with it.

The more efficient approach, proposed by the OmniLedger team, Kokoris-Kogias et al. 2017, would be to make the validators non-redundant, partitioning them between different networks and periodically, securely and randomly repartitioning them. A reduction in the cost to attack over having them all validate on a single network is implied since there is a chance of having a single network accidentally have a compromising number of malicious validators even with less than this proportion overall. This aside it presents an effective means of scaling under a basis of weak-coherency.

Alternatively, as in Elves by Stewart 2018, we may utilize non-redundant partitioning, combine this with a proposal-and-auditing game which validators play to weed out and punish invalid computations, and then require that the finality of one network be contingent on all causally-entangled networks. This is the most secure and economically efficient solution of the three, since there is a mechanism for being highly confident that invalid transitions will be recognized and corrected before their effect is finalized across the ecosystem of networks. However, it requires substantially more sophisticated logic and their causal-entanglement implies some upper limit on the number of networks which may be added.

**2.4. High-Performance Fully Synchronous Networks.** Another trend in the recent years of blockchain development has been to make “tactical” optimizations over data throughput by limiting the validator set size or diversity, focusing on software optimizations, requiring a higher degree of coherency between validators, onerous requirements on the hardware which validators must have, or limiting data availability.

The Solana blockchain is underpinned by technology introduced by Yakovenko 2018 and boasts theoretical figures of over 700,000 transactions per second, though according to Ng 2024 the network is only seen processing a small fraction of this. The underlying throughput is still substantially more than most blockchain networks and is owed to various engineering optimizations in favor of maximizing synchronous performance. The result is a highlycoherent smart-contract environment with an api not unlike that of *YP* Ethereum (albeit using a different underlying VM), but with a near-instant time to inclusion and finality which is taken to be immediate upon inclusion.

Two issues arise with such an approach: firstly, defining the protocol as the outcome of a heavily optimized codebase creates structural centralization and can undermine resilience. Jha 2024 writes “since January 2022, 11 significant outages gave rise to 15 days in which major or partial outages were experienced”. This is an outlier within the major blockchains as the vast majority of major chains have no downtime. There are various causes to this downtime, but they are generally due to bugs found in various subsystems.

Ethereum, at least until recently, provided the most contrasting alternative with its well-reviewed specification, clear research over its crypto-economic foundations and multiple clean-room implementations. It is perhaps no surprise that the network very notably continued largely unabated when a flaw in its most deployed implementation was found and maliciously exploited, as described by Hertig 2016.

The second issue is concerning ultimate scalability of the protocol when it provides no means of distributing workload beyond the hardware of a single machine.

In major usage, both historical transaction data and state would grow impractically. Solana illustrates how much of a problem this can be. Unlike classical blockchains, the Solana protocol offers no solution for the archival and subsequent review of historical data, crucial if the present state is to be proven correct from first principle by a third party. There is little information on how Solana manages this in the literature, but according to Solana Foundation 2023, nodes simply place the data onto a centralized database hosted by Google.[^6]

Solana validators are encouraged to install large amounts of ram to help hold its large state in memory (512 gb is the current recommendation according to Solana Labs 2024). Without a divide-and-conquer approach, Solana shows that the level of hardware which validators can reasonably be expected to provide dictates the upper limit on the performance of a totally synchronous, coherent execution model. Hardware requirements represent barriers to entry for the validator set and cannot grow without sacrificing decentralization and, ultimately, transparency.

<h3 align="center">3. Notational Conventions</h3>
<h6 align="center">3. 符号约定 </h6>

Much as in the Ethereum Yellow Paper, a number of notational conventions are used throughout the present work. We define them here for clarity. The Ethereum Yellow Paper itself may be referred to henceforth as the *YP*.

<h6> 就像以太坊黄皮书一样，在该工作中使用了大量约定的符号。为了清楚起见，我们在这里定义他们。以太坊黄皮书本身在以后用 YP 缩写。</h6>
**3.1. Typography.** We use a number of different typefaces to denote different kinds of terms. Where a term is used to refer to a value only relevant within some localized section of the document, we use a lower-case roman letter e.g. x, y (typically used for an item of a set or sequence) or e.g. i, j (typically used for numerical indices). Where
we refer to a Boolean term or a function in a local context, we tend to use a capitalized roman alphabet letter such as A, F. If particular emphasis is needed on the fact a term
is sophisticated or multidimensional, then we may use a bold typeface, especially in the case of sequences and sets
<h6>3.1. 术语的字体运用。为了表示不同种类的术语，我们使用多种不同的字体。术语如果仅在文档的局部范围内引用，表示局部值，那么我们使用小写罗马字母，例如 x、y（通常用于集合或序列中的元素）或 i、j（通常用于数值索引）。当我们在局部上下文中引用布尔值或函数时，则倾向于使用大写罗马字母，例如 A、F。如果特别需要强调术语的复杂性或多维性，则可以使用粗体字，尤其是在序列和集合的情况下。</h6>

For items which retain their definition throughout the present work, we use other typographic conventions. Sets are usually referred to with a blackboard typeface, e.g. N refers to all natural numbers including zero. Sets which may be parameterized may be subscripted or be followed by parenthesized arguments. Imported functions, used by the present work but not specifically introduced by it, are written in calligraphic typeface, e.g. H the Blake2 cryptographic hashing function. For other non-context dependent functions introduced in the present work, we use upper case Greek letters, e.g. Υ denotes the state transition function
<h6> 对于贯穿整部著作的项目，我们使用其他的排版约定。集合通常用黑板字体表示，例如 N 表示所有自然数，包括零。可参数化的集合可以用下标表示，也可以用带括号的参数表示。引用函数时，如果该函数在著作中使用但未特别引入，则使用书法字体，例如 H 表示 Blake2 加密散列函数。对于著作中引入的其他与上下文无关的函数，我们使用大写希腊字母，例如 Υ 表示状态转换函数。</h6>


[^1]: The gas mechanism did restrict what programs can execute on it by placing an upper bound on the number of steps which may be executed, but some restriction to avoid infinite-computation must surely be introduced in a permissionless setting.
[^2]: Practical matters do limit the level of real decentralization. Validator software expressly provides functionality to allow a single instance to be configured with multiple key sets, systematically facilitating a much lower level of actual decentralization than the apparent number of actors, both in terms of individual operators and hardware. Using data collated by Dune and hildobby 2024 on Ethereum 2, one can see one major node operator, Lido, has steadily accounted for almost one-third of the almost one million crypto-economic participants.
[^3]: Ethereum’s developers hope to change this to something more secure, but no timeline is fixed.
[^4]: Some initial thoughts on the matter resulted in a proposal by Sadana 2024 to utilize Polkadot technology as a means of helping create a modicum of compatibility between roll-up ecosystems!
[^5]: In all likelihood actually substantially more as this was using low-tier “spare” hardware in consumer units, and our recompiler was unoptimized.
[^6]: Earlier node versions utilized Arweave network, a decentralized data store, but this was found to be unreliable for the data throughput which Solana required.
