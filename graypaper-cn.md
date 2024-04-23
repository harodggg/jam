<h1 align="center">JAM</h1>

<h5 align="center">JOIN-ACCUMULATE MACHINE: A SEMI-COHERENT SCALABLE TRUSTLESS VM</h5>
<h5 align="center">聚合-累计 机器: 一个半相干的可以扩展的无需信任的虚拟机 </h5>
<h6 align="center">( JOIN: 表示多个实体，可能是不同种类的，不同用途的实体一起来使用。)</h6>
<h6 align="center">( ACCUMULATE： 表示这些实体的使用该机器的结果，是依赖于前置的，历史的，类似于状态机，当前结果是依赖于前一个结果。)</h6>
<h6 align="center">( semi-coherent: semi一半，coherent一致，相关，相干。也许是不一定需要强一致性，类似于一个区块，只有唯一的历史结果，只有一个世界树，这个也许可以包含多个历史结果，类似平行宇宙。 )</h6>

<p>Abstract. We present a comprehensive and formal definition of Jam, a protocol combining elements of both Polkadot
and Ethereum. In a single coherent model, Jam provides a global singleton permissionless object environment—much
like the smart-contract environment pioneered by Ethereum—paired with secure sideband computation parallelized
over a scalable node network, a proposition pioneered by Polkadot.</p>
<h6>摘要。我们提出了全面，并且正式的JAM定义，即一种同时结合了ethereum和polkadot两种的元素的协议。JAM既提供了类似于以太坊首创的全局单例无许可对象智能合约环境，又结合了 Polkadot 开创的、可安全地在可扩展节点网络上并行处理的侧带计算功能。</h6>
<h6>( sideband:边带。 一般而言，术语“边带”用于指代用于传输附加信息的次要或辅助信道。这可以是在无线电通信、计算或其他领域的背景下。在这里指的可能是polkadot中继-平行链模型，中继通过hrmp或者xcmp 处理平行链的信息 )</h6>
<p>Jam introduces a decentralized hybrid system offering smart-contract functionality structured around a secure and
scalable in-core/on-chain dualism. While the smart-contract functionality implies some similarities with Ethereum’s
paradigm, the overall model of the service offered is driven largely by underlying architecture of Polkadot.</p>
<h6>JAM 提出了一种去中心化混合系统，拥有围绕在安全和可扩展性 "核心内"/"在链上" 的二元性的智能合约功能。虽然智能合约功能与以太坊的范例有一些相似之处，但所提供服务的整体模型主要由 Polkadot 的底层架构驱动。</h6>
<h6> ( in-core/on-chain dualism，二元性，有点像一个硬币的两面，或者量子的概念。即可能是在核心里，也可能是在链上，关键取决于你怎么看待这件事情。可以即是在核心里，也可以在链上。 )</h6>
<p>Jam is permissionless in nature, allowing anyone to deploy code as a service on it for a fee commensurate with the
resources this code utilizes and to induce execution of this code through the procurement and allocation of core-time,
a metric of resilient and ubiquitous computation, somewhat similar to the purchasing of gas in Ethereum. We already
envision a Polkadot-compatible CoreChains service.</p>
<h6> JAM 在本质上是无需许可的，允许任何人去部署代码提供服务，费用和代码所使用的资源相当，并且去引发代码执行通过获取和分配核心时间，有点类似以太坊中购买gas。我们已经设想了一种兼容polkadot的核心链服务</h6>


<h3 align="center"> 1. Introduction </h3>

<p><strong>1.1. Nomenclature. </strong>In this paper, we introduce a decentralized, crypto-economic protocol to which the Polkadot Network could conceivably transition itself in a major revision. Following this eventuality (which must not be taken for granted since Polkadot is a decentralized network) this protocol might also become known as Polkadot or some derivation thereof. However, at this stage this is not the case, therefore our proposed protocol will for the present be known as Jam. <p>

<h6><strong>1.1. 命名</strong> 在这篇论文中，我们提出了一个去中心化，加密经济协议，对于polkadot 网络在设想中，可以通过重大修订过渡到该协议。如果该事件发生（由于波卡是一个去中心化网络，因此这并非板上钉钉），该协议也可能被称为 Polkadot 或类似名称。然而，目前情况并非如此，因此我们提出的协议暂时将被命名为 Jam。</h6>

&nbsp;&nbsp;<p>An early, unrefined, version of this protocol was first proposed in Polkadot Fellowship rfc31, known as CoreJam. CoreJam takes its name after the collect/refine/join/accumulate model of computation at the heart of its service proposition. While the CoreJam rfc suggested an incomplete, scope-limited alteration to the Polkadot protocol, Jam refers to a complete and coherent overall blockchain protocol.</p>
<h6>该协议的早期、未经完善的版本最初在 Polkadot Fellowship rfc31 中提出，名为 CoreJam。CoreJam 这个名字来源于其服务核心——收集/精炼/加入/累积计算模型。虽然 CoreJam RFC 提出了对波卡协议不完整、范围有限的修改，但 Jam 指的是一个完整且连贯的整体区块链协议。</h6>
<h6>( collect/refine/join/accumulate model,这个模型听起来很抽象，但是我们也许可以类比成我们熟悉的区块链，collect->从rpc，fullnode收集交易，refine->对收集的交易进行处理，将不合理的交易给丢弃掉，合理的交易被保留到内存池，join -> 区块生成过程，从内存池选择交易，开始构建区块， accumulate -> 生产一个个区块。 )</h6>

<h6>$$\left( \sum{k=1}^n a_k b_k \right)^2 \leq \left( \sum{k=1}^n ak^2 \right) \left( \sum{k=1}^n b_k^2 \right)$$</h6>



