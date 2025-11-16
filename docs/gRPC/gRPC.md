
> It is highly recommend to go through the [[docs/ProtoBuf/ProtoBuf|ProtoBuf]] module before reading gRPC
### Introduction 

gRPC originally stood for **Google Remote Procedure Call**. It is a modern, open-source, high-performance remote procedure call framework. It is designed to operate in any environment. It is especially **good for servers-to-servers communication, which may be within or across data centers**. This applicability extends to the **last mile of the distributed computing** which serves a wide range of clients which include **IoT devices, web browsers, mobile applications**, etc.

The [[Foundation of gRPC|fundamental concept of gRPC]] is built upon the established paradigm of RPC. It allows client applications to directly invoke a method on a server application which may be on a different machine or in a different data center as if it were a local object or a function call. This abstraction is the corner store of distributed systems as it makes the underlying complexity of network communication, data serialization, and error handling thereby simplifying the development of distributed applications and services.

### How it came into being
It was an internal project of google called **stubby**. its powered the core of vast and planet scale infrastructure. It was built to manage the communication layer for system that handle 10s of billion request per second. 

This internal system provided Google with the extreme scalability, performance, and cross-language functionality required to operate its services.

Google made this project open-source in 2015. This release effectively democratized access to a level of distributed systems technology that was previously only available within hyperscale companies.

The primary design goals for this project were:

- **Polyglot Environments**: How to make services written in C++, Java, and Python communicate seamlessly.

- **Strict Contracts**: How to enforce rigid service contracts to prevent "brittle" integrations when thousands of teams iterate independently.

- **High-Performance Serialization**: How to minimize the network footprint and CPU cost of serializing and deserializing data billions of times a second.


#grpc
