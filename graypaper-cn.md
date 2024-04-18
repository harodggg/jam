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
<h6>摘要。我们提出了jam全面，并且正式的定义，即一种结合了ethereum和polkadot两种的元素的协议。Jam 既提供了类似于以太坊首创的智能合约环境的全局单例无许可对象环境，又结合了 Polkadot 开创的、可安全地在可扩展节点网络上并行处理的侧带计算功能。</h6>
<h6>( sideband:边带。 一般而言，术语“边带”用于指代用于传输附加信息的次要或辅助信道。这可以是在无线电通信、计算或其他领域的背景下。在这里指的可能是polkadot中继-平行链模型，中继通过hrmp或者xcmp 处理平行链的信息 )</h6>
<p>Jam introduces a decentralized hybrid system offering smart-contract functionality structured around a secure and
scalable in-core/on-chain dualism. While the smart-contract functionality implies some similarities with Ethereum’s
paradigm, the overall model of the service offered is driven largely by underlying architecture of Polkadot.</p>
<h6> </h6>
<p>Jam is permissionless in nature, allowing anyone to deploy code as a service on it for a fee commensurate with the
resources this code utilizes and to induce execution of this code through the procurement and allocation of core-time,
a metric of resilient and ubiquitous computation, somewhat similar to the purchasing of gas in Ethereum. We already
envision a Polkadot-compatible CoreChains service.</p>
