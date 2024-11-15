# CD²N
In order to achieve decentralization and open network, CD²N, like many decentralized cache networks, uses blockchain to formulate a series of rules such as node joining the network, exiting, rewards and penalties to drive the whole system to operate well and develop in an orderly manner. These are the "conventions" of decentralized networks. Any node joining the network needs to abide by these "conventions" and make efforts (computing power, storage and other resources) to get rewards. A good working protocol always allows nodes that work honestly to get due rewards and can identify and punish malicious nodes. However, the uncertainty brought by decentralized open networks makes the construction of protocols more complex and challenging. Most of the existing decentralized CDN solutions require complex proof and audit mechanisms, or specialized networks designed based on the characteristics of their underlying architecture, and most of them are limited to their own ecosystems, making it difficult to achieve flexible expansion and cross-platform applications.

CD²N solves the above problems through EVM smart contracts and modular working protocols. CD²N provides a set of protocol guidelines and a liberalized decentralized resource trading platform, and fully open-sources the supporting development tool chain. CD²N supports any group or individual to publish new work agreements to realize the value of various node resources. Nodes can plug and unplug different functional modules to meet the requirements of different work agreements, join the agreement and obtain benefits from it. At present, CD²N has implemented a cache work agreement, which mainly serves the data cache and retrieval functions of the CESS network. The cache agreement stipulates that the main functions of participating nodes are as follows:

● CESS data cache function: cache CESS data shards from storage nodes or other cache nodes, and provide paid data shard download services;

● CESS data sharing function: allow cache nodes to share data shards with each other, and offset each other's download costs through equal data exchange;

● CESS data retrieval function: that is, it has a sound gateway function and can provide paid download services for complete files;

CESS cache nodes implement data caching and sharing functions, and DeOSS implements data retrieval functions based on cache nodes. The cache agreement stipulates that nodes adopt the principle of service first and then billing. Nodes first open a certain download qualification to users. After the user reaches the preset download limit, they need to pay the node to supplement the credit in order to continue to obtain services and obtain higher download qualifications. Users pay fees to nodes by calling the cache protocol contract to create caches or retrieve orders. The cache protocol contract will indirectly count the workload of the nodes based on the number of orders and issue rewards to the nodes according to the workload ratio.

The cache protocol contract also standardizes specific work processes such as node registration, order collection, reward claiming, and exiting the network. Nodes complete a series of processes such as joining the network, accepting user requests, and obtaining income by calling contracts. Adhering to the principles of decentralization, openness, and open source, no matter how the node is implemented internally, as long as it meets the rules set by the work protocol and provides effective services, it can be regarded as a member of the node in CD²N and obtain income by selling resources. Anyone can add a new work protocol to the CESS network to expand CD²N and create unlimited possibilities for the DePIN ecosystem.