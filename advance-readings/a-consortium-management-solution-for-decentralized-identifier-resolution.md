# CMS: A Consortium Management Solution for Decentralized Identifier Resolution

## Authors
- Zhiping Li , Bo Zhang

## Abstract
The advent of decentralized identity (DID) technologies aims to return control of identity to users. However, existing DID resolution schemes often rely on centralized management, which can lead to single points of failure, security vulnerabilities, insufficient scalability, and privacy concerns. To address these challenges, this paper introduces a novel solution named CMS. CMS leverages blockchain smart contracts and InterPlanetary File System (IPFS) decentralized storage to implement a consortium management approach. It drives the management of a universal resolver through a voting mechanism of smart contracts and utilizes IPFS for distributed storage, thereby enhancing system transparency, stability, and the persistent security of data. Although this approach increases the technical complexity of the system and may impact performance, it significantly enhances management transparency and data security.

## Introduction
The existing DID technology utilizes a universal resolver [1] released by the Decentralized Identity Foundation (DIF), managed and audited by DIF members via GitHub and stored on the centralized platform DockerHub. Although this model supports rapid integration of various DID method resolution drivers and achieves unified resolution of different DID methods, it also introduces issues related to opaque management processes and the risk of data loss.

In summary, To address the challenges present in the unified resolution process of existing DIDs, the primary contribution of this paper is the proposal of a consortium management scheme for DID universal resolvers, employing blockchain smart contracts and IPFS decentralized storage technologies.

## Proposed Scheme

### Design Goals
1. *Decentralization and Transparency*: The primary goal is to decentralize the management of the DID universal resolver by leveraging blockchain smart contracts and IPFS technologies. This aims to enhance the transparency and fairness of the submission, review, and auditing processes for DID resolution drivers.
2. *Enhanced Security and Data Stability*: CMS aims to reduce the risks associated with centralized management and storage, such as malicious modifications, deletions, and data loss. By utilizing blockchain's immutability and IPFS's distributed storage, the scheme seeks to ensure the security, reliability, and stability of the DID resolution drivers.
3. *Consortium Governance*: The scheme introduces a consortium governance model for managing the DID universal resolver. The goal is to establish a consortium governance committee that oversees the decision- making process through a voting mechanism implemented via smart contracts. This approach aims to foster community participation, consensus-based outcomes, and adaptability to meet the diverse needs of stakeholders.

### Architecture
Figure 1 illustrates the architecture of the CMS scheme, comprising an off-chain distributed storage system, an on-chain parsing-driven indexed smart contract, and a consortium governance smart contract.

![fig1](https://github.com/caict-develop-zhangbo/my-image/blob/main/CMS.svg "fig1")

1. *On-Chain Parsing-Driven Indexed Contract*: The parsing-driven indexed contract stores indices in IPFS of all DID resolution drivers that have been approved through audit voting. Users monitoring storage events can thus obtain real-time updates on the latest driver information.
2. *On-Chain Consortium Governance Contract*: The consortium governance contract integrates the governance models of both the consortium governance committee and DID resolution driver governance, achieving consensus-based uniform outcomes through voting. In this contract, the consortium governance committee assumes roles of oversight and decisionmaking, with their votes directing the system's developmental trajectory. All voting records are documented on the blockchain, ensuring transparency and preventing opaque operations within the governance consortium. 

## Conclusion

This paper analyzes the issues currently faced by generic DID resolvers and proposes a consortium management solution utilizing blockchain smart contracts and IPFS technology. This approach not only enhances the transparency, data permanence, and security of the submission and review processes for DID resolution drivers but also offers new perspectives for overcoming performance limitations and technical challenges through technological innovation.

## References

[1] “decentralized-identity/universal-resolver: Universal Resolver implementation and drivers.,” *GitHub*. https://github.com/decentralized-identity/universal-resolver