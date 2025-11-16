gRPC is a **"contract-first" philosophy**, which is enforced by its default [[Interface Definition Language|Interface Definition Language (IDL)]], [[ProtoBuf | protocol buffers]].

It uses protobuf by default for both its IDL for defining service contracts and underlying message interchange format for serialization. 

### Contract first philosophy

Many REST-based architectures, the API contract (such as an OpenAPI/Swagger specification) is often treated as an afterthought. It is frequently written after the code is implemented, simply to document what has been built. This "code-first" approach inevitably leads to "schema drift," where the documentation and the implementation become out of sync, causing integration failures.

The gRPC workflow  makes this class of error impossible. The .proto file  is the single, indisputable source of truth. This "strongly typed contract" catches data structure and service errors at compile-time, not at runtime.

### Devlopers workflow

> Read [[ProtoBuf Workflow]] , [[Protoc Compiler]] and [[Serializing and Deserializing in ProtoBuf]] might be helpful in understanding the complexity beneath this.

The .proto file is the starting point for a rigid, automated development workflow that is central to gRPC's design.

1. **Step 1: Define**: The developer authors the .proto file, defining all data message types and service RPCs. This file becomes the single, authoritative source of truth for the API contract.

2. **Step 2: Compile:** The developer runs the Protocol Buffer compiler, protoc. This compiler is used with a language-specific gRPC plugin (e.g., protoc-gen-go-grpc).

3. **Step 3: Generate:** The compiler generates source code in the target programming language. This code is not trivial; it is a comprehensive set of tools, including     
	- **Data Access Classes**: For each message type, the compiler generates a class (e.g., HelloRequest in Java or C++) with all the necessary methods for populating fields, serializing the message to binary, and parsing binary data back into the object.
	
	- **Server Skeleton:** A "service base class" or interface (e.g., GreeterServer) that defines the abstract methods the server-side application must implement.
	
	- **Client Stub:** A client-side "stub" (e.g., GreeterClient) that provides concrete methods for the client application to call. This stub handles all the work of serializing the request, sending it to the server, and parsing the response.

4. **Step 4: Implement:** The server-side developer's task is reduced to implementing the generated service interface. The client-side developer's task is reduced to instantiating and calling the methods on the generated client stub.

#grpc 