# yugho_message_chain

# Overview
A simple, decentralized, trustless message system

The message system serves 3 purposes:
- Ensure that each received "action" message is sent to all nodes that are up and open
- Add the message to the message chain and assign a unique messageId that is recognized by all nodes in the network
- As possible, balance request traffic across member nodes in the system

The message id establishes a confirmed order.  Each node should carry the same message chain subset even if each node does not contain the entire message chain.  It should be possible for any node to determine the if a messgeId is already in the message chain and sync the message chain up to the specified messageId

A node can either participate in a "pull", "push", or "verification" way. If a node chooses to "pull" only, then the node will not receive updates.  If a node chooses for "push", then the node is included in the notifications so that the local message chain should grow automatically without any effort.

At any time, a small number of nodes that wish to participate in "verification" are selected as a verifier.  The verifiers are responsible for updating the message chain and for sharing updates with the nodes who have ofted for "push".

The only centralized component is a list of nodes that are available which can be used to find nodes.  

The set of nodes ("member nodes") or any other nodes that exists in the network can receive requests which are either "actions" which are intended to be added to the message chain or "queries" which are not intended to be added to the message chain.

Each request should receive a receipt which serves as proof that the action or query was properly implemented.

The following requests should be supported by all "member nodes"
1. **Broadcast**: this is a message that is intended to go to all nodes.  This is a message that gets added to the message chain.  A broadcast by a non-member node will typically require a fee that is either transaction-based (one-time fee) or is part of a subscription.
2. **Message Chain**: this is a message that can be handled by any node.  It should be possible to specify a start and an end.  Depending on the size of the data, it is up to the node to determine the fee.  For member nodes, the message chain request is free so long as it is small so the free way to get the message chain is to distribute the request.  
3. **List of Nodes**: this is a request which is free where any node can return a small number of neighbor nodes.  The response time can vary based on the traffic received.  
4. **Verification**: This is a request to verify when a broadcast occurred
5. **Latest Message Id***: Query for the latest messageId.  This is a free call.

The data structure possessed by all nodes should consist of the following:
- Neighbor list (validated with timestamp and ordered by recency)
- Message Chain: either full message chain or a subset of the message chain with links to other parts which have been validated with timestamp
- A Registry of nodes with public keys for validation including a list of nodes that were previously verifiers
- Status with timestamp (each time a node comes back up, it needs to rejoin the network or when it is requested by another node.

The following processes are defined for each node:
- Joining the community and syncing up
- Identifying down nodes and invalid nodes (nodes not following expected protocol)
- Broadcast protocol
- Push Protocol
- Pull Protocol
- Fee Management -  initially free until fee management system is in place
- Request Handling (Actions and Queries)

# Features
1. **Bootstrap**:  This is used for the first node and for test enviroments.  For testing, it is possigle to have multiple virtual nodes on the same node.  For production, each node must have a unique IP address.
2. **Node Start Up**: A node starts up, If it is has not yet joined, it will join the network.  If it has joined, it will sync up.  For "verificaiton" or "push", it will sync up completely.  For "pull", it will sync up to a specific message Id.  
3. **Handle Broadcast Request**: A node receives a broadcast request and submits it to the verification process.
4. **Broadcast Verification**: If a node supports "verification", it will need to be in "push" mode and it will need be ready to serve as a verifier when selected.
5. **Handle Query Request**: A node is required to handle a query request
6. **Report Node**: Handle reports of a node being down or a node operating in an invalid way

# Configuring Node
- All configuration are done through the yaml file which can be found in configuration.  The yaml files is self-documented.
