This section describes the message design between nodes, including message exchange formats and protocols.

# Message Exchange Format

[**protobuf**](https://protobuf.dev/) is the message exchange format between nodes. protobuf is a flexible and efficient data serialization method for structured data. It has the advantage of high-speed efficiency and spatial efficiency, e.g., small encode volume, fast encoding and decoding speed.

# Message Protocol

A general message is the information that must be carried in all node communication. The message includes the following details of a peer node: version number, timestamp, message ID, whether to gossip this message to neighboring nodes, node ID, node public key, and node data signature. The primary function of a general message is to verify the node's identity and data signature. The general message protocol design is as follows:

```protobuf
message MessageData {
  string clientVersion = 1; // client version
  int64 timestamp = 2;      // unix time
  string id = 3;            // allows requesters to use request data when processing a response
  bool gossip = 4;          // true to have receiver peer gossip the message to neighbors
  string nodeId = 5;        // id of node that created the message (not the peer that may have sent it). =base58(multihash(nodePubKey))
  bytes nodePubKey = 6;     // Authoring node Secp256k1 public key (32bytes) - protobufs serielized
  bytes sign = 7;           // signature of message data + method specific data by message authoring node.
}
```

# Write Data Message Protocol

The write data message protocol describes how a node writes its data to another node, including request and response message formats. The write data message protocol is defined as follows:

```protobuf
message WritefileRequest {
  MessageData messageData = 1;
  // Roothash uniquely identifies a user data
  string Roothash =2;
  // Datahash is the currently written data hash value
  string Datahash = 3;
  // Length is the length of the data written this time
  uint32 Length = 4;
  // Offset is the offset of this write
  uint32 Offset = 5;
  // Data is the data written this time
  bytes Data = 6;
}

message WritefileResponse {
  MessageData messageData = 1;
  // Code indicates the result of this transfer
  uint32 Code = 2;
  // Offset is the write offset the receiver wants
  uint32 Offset =3;
}
```

`WritefileRequest` defines the data offset of writing, which enables breakpoint and continuation setup in the writing process. As long as the peer informs the writer about the offset in `WritefileResponse` message, the writer can start writing from the position specified by the offset without having to start writing from the starting position every time.

# Read Data Message Protocol

The read data message protocol describes how a node reads the desired data from another node, including request and response message formats. The read data message protocol is defined as follows:

```protobuf
message ReadfileRequest {
  MessageData messageData = 1;
  // Roothash uniquely identifies a user data
  string Roothash =2;
  // Datahash is the currently written data hash value
  string Datahash = 3;
  // Offset is the offset that the reader wants to read
  uint32 Offset = 4;
}

message ReadfileResponse {
  MessageData messageData = 1;
  // Code indicates the result of this transfer
  uint32 Code = 2;
  // Offset is the data offset returned by the peer
  uint32 Offset =3;
  // Length is the returned data length
  uint32 Length = 4;
  // Data is the returned data
  bytes Data = 5;
}
```

`ReadfileRequest` defines the data offset of reading, which enables breakpoint and continuation setup in the reading process. As long as the reader informs the peer about the offset in `ReadfileResponse` message, the peer can start reading from the position specified by the offset.
