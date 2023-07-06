# System Architecture

## Overall System Architecture

CESS is a decentralized, high speed, secure and scalable cloud data storage network. CESS proposes a novel Random Rotational Selection(R²S) consensus mechanism to achieve low gas fees and rapid transaction processing throughput (10,000 TPS). CESS offers large-scale storage capacity, managing billions of data files with up to 100PB space, to meet enterprise level demands. At the same time, CESS provides data services including data rights confirmation and protection. Therefore, our platform not only provides expandable data storages for DAPPs, but also strongdata owner rights protection.

As shown below, CESS adopts a layered and loosely coupled systemarchitecture, which is divided into blockchain service layer, distributed storage resource layer, distributedcontent delivery layer and application layer.

![Layered-system architecture](../assets/concepts/system-architecture/layered-system-architecture.png)

Among them, a Blockchain service layer provides blockchain service of the entire CESS network, including encouraging unused storage resources and computational resources to join the CESS network to provide data storage, data rights confirmation and other services for the
application layer. The Distributed storage resource layer uses virtualization technology to realize the integration and pooling of storage resources. The infrastructure consists of storage capacityminers and storage scheduling miners. The distributed content delivery layer uses content cachingtechnology to achieve fast delivery of stored data, which is composed of data index miners anddata delivery miners. The application layer provides API/SDK tools to support data storage service, blockchain service, network drive service, enterprise level SDK, AI applications, and etc.
