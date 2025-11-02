
## IDL 

The fundamental unit of data structure in [[Protobuf]] is the message. A message is a logical record of information, analogous to a class in Java or a struct in C++ or Go.

### Syntax:

A message is declared using the message keyword, followed by its name and a pair of curly braces enclosing its field definitions.29

  

```Protocol Buffers
message SongRequest {  
  string song_name = 1;  
  int64 artist_id = 2;  
}  
```
  

### Naming Conventions:

The official Protobuf style guide recommends using **TitleCase**  **for message names** and **lower_snake_case for field names**. Following these conventions ensures that the generated code is idiomatic in the target languages, as the compiler often transforms lower_snake_case into the conventional style of the target language (e.g., camelCase in Java).

### Nested Messages:

Messages can be defined within the scope of other messages. This is useful for grouping related data structures and creating a logical namespace.

```Protocol Buffers

message SearchResponse {  
  message Result {  
    string url = 1;  
    string title = 2;  
  }  
  repeated Result results = 1;  
}  
```  

Here, Result is a nested message type that can only be accessed within the context of SearchResponse (e.g., SearchResponse.Result in generated code).

### Importing:

Messages can be defined out of the scope of the message, i.e in a different file, this can be later imported into the required message. 

> **result.proto**
```Protocol Buffers
syntax = "proto3";

package search;

// Define Result independently
message Result {
  string url = 1;
  string title = 2;
}
```

> **search_response.proto**
``` Protocol Buffers
syntax = "proto3";

package search;

import "result.proto"; // Import the external message definition

message SearchResponse {
  repeated Result results = 1; // Use it directly since it shares the same package
}
```


## Field Number/Tag

### Syntax:

Every field in a message must be assigned a unique positive integer number.

> type field_name = 1;

### Function:

These field numbers, not the field names, are what identify the fields in the compact binary wire format. When a message is serialized, the output is a series of key-value pairs where the key is derived from the field number and a wire type. This design is a primary reason for Protobuf's efficiency and compactness, as it avoids sending verbose string keys (like in JSON) over the network.

### Uniqueness and Immutability:

The rules governing field numbers are strict and are the bedrock of Protobuf's schema evolution capabilities:

- The number must be unique within that message.
- Once a field number is used in a production system, it must never be changed.
- A field number from a deleted field should never be reused for a new field. Instead, the old number should be marked as reserved to prevent future accidents.


Violating these rules can lead to severe data corruption, as a parser using a newer schema could completely misinterpret data written by an older one. The stability of these numbers is the central pillar of Protobuf's compatibility contract. This prioritization of long-term stability over short-term developer convenience necessitates strong governance over .proto files, including strict code review policies and the use of automated tools to prevent accidental changes to field numbers.

### Performance Implications:

The choice of field number has a direct impact on the size of the serialized message. Field numbers in the range 1 through 15 require only one byte to encode the key (tag + wire type). Numbers in the range 16 through 2047 take two bytes. For this reason, the most frequently used or repeated fields should be assigned the lowest available field numbers.

Reserved Ranges:

The field numbers from 19,000 to 19,999 are reserved for the internal implementation of Protocol Buffers and cannot be used.


## Field Cardinality


Each field in a message must have a cardinality, which specifies how many times that field can appear.

- Singular Fields: The field can appear at most once in a message. The handling of singular fields differs significantly between Protobuf versions and is a key concept to master.

- optional (Explicit Presence): This is the recommended approach for modern Protobuf (proto3 with the optional keyword, and the default in editions). It means the system can distinguish between a field that was not set and a field that was explicitly set to its default value (e.g., 0 for an integer). The generated code includes a has_...() method to perform this check.

- Implicit Presence: This was the original default for scalar fields in proto3. A field holding its default value (e.g., 0, false, "") is not serialized. This makes it impossible for the receiver to know if the sender intentionally set the value to 0 or simply omitted the field. This ambiguity can lead to bugs and is why explicit presence is now favored.    

- repeated Fields: The field can be repeated any number of times (including zero). This is used to represent lists, arrays, or sequences of values. The order of the elements is preserved. The style guide recommends using pluralized names for repeated fields (e.g., repeated Song songs).

- required Fields (proto2 only, Deprecated): This keyword, available only in proto2, specifies that a value for the field must be provided. While seemingly useful for validation, it proved to be extremely problematic for schema evolution. A field marked as required can never be safely made optional or removed without breaking older clients, which will reject messages that are missing the field. For this reason, its use is strongly discouraged, and it was removed entirely from proto3 and editions.


## Services for RPC

**In addition to defining data structures, .proto files are the standard way to define service interfaces for RPC frameworks like gRPC.**



### Syntax: 
A service is defined using the service keyword, containing a list of rpc methods.  

```Protocol Buffers  
service SearchService {  
  rpc Search(SearchRequest) returns (SearchResponse);  
}  
 ``` 
### RPC Methods:
Each rpc method specifies its name, its request message type, and its response message type. The compiler uses this definition to generate client stubs and server interface skeletons.    
### Streaming: 
The stream keyword can be prepended to the request and/or response types to define different communication patterns, which are a core feature of gRPC: 
- Unary RPC: rpc Method(Request) returns (Response); (the default)
    
- Server Streaming RPC: rpc Method(Request) returns (stream Response);
    
- Client Streaming RPC: rpc Method(stream Request) returns (Response);
    
- Bidirectional Streaming RPC: rpc Method(stream Request) returns (stream Response);
    

**

