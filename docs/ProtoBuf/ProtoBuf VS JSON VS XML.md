
# Protocol Buffers vs JSON vs XML

A comparison of different data serialization formats and their characteristics:

| Feature             | Protocol Buffers ([[ProtoBuf]])                                                                                      | JSON (JavaScript Object Notation)                                                                                     | XML (eXtensible Markup Language)                                                            |
| ------------------- | -------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- |
| Format Type         | Binary                                                                                                               | Text-based                                                                                                            | Text-based                                                                                  |
| Performance (Speed) | Very fast serialization/deserialization; up to 6x faster than JSON in server-to-server contexts.                     | Slower than Protobuf due to text parsing overhead.                                                                    | Generally the slowest due to high verbosity and complex parsing rules.                      |
| Performance (Size)  | Highly compact; significantly smaller payloads than JSON/XML.                                                        | More compact than XML but larger than Protobuf due to text-based keys and syntax.                                     | Most verbose, resulting in the largest file sizes.                                          |
| Schema Requirement  | Strictly required; defined in a .proto file. Enforces a strong contract.                                             | Schema-less by default; schema can be added via external standards like JSON Schema or OpenAPI.                       | Schema is optional (DTD, XSD) but commonly used in enterprise systems.                      |
| Human Readability   | Not human-readable; requires tools for inspection.                                                                   | Human-readable and easy to inspect, aiding in debugging.                                                              | Human-readable, though verbosity can make it complex to parse visually.                     |
| Data Type Support   | Rich set of scalar types, including various integer sizes and encodings; supports enums and complex nested messages. | Limited data types (string, number, boolean, array, object, null).                                                    | Supports a wide range of data types through schema definitions (XSD).                       |
| Compatibility       | Excellent backward and forward compatibility features built into the protocol design.                                | No built-in compatibility features; relies on application-level conventions.                                          | Schema evolution is possible but can be complex to manage.                                  |
| Primary Use Cases   | High-performance internal microservices (gRPC), IoT, data storage, and performance-critical systems.                 | Public web APIs, client-side web applications, configuration files, and scenarios valuing simplicity and readability. | Enterprise systems, legacy applications, complex document formats, and standards like SOAP. |


**Example Payload for different format**

| Format | Payload Example | Size (bytes) | Notes                                 |
| ------ | --------------- | ------------ | ------------------------------------- |
| JSON   | {"id":42}       | 9 bytes      | Assumes ASCII, no whitespace.         |
| XML    | <id>42</id>     | 11 bytes     | Assumes ASCII, no whitespace.         |
| Protob | 0x08 0x2a       | 2 bytes      | Schema-dependent, not human-readable. |

Payload size difference between JSON and Protobuf

| Payload      | Raw JSON Size | Protobuf Size | Protobuf Size (%) |
| ------------ | ------------- | ------------- | ----------------- |
| 0 tickers    | 58 bytes      | 13 bytes      | 22.4%             |
| 10 tickers   | 396 bytes     | 133 bytes     | 33.6%             |
| 200 tickers  | 6,578 bytes   | 2,413 bytes   | 36.7%             |
| 2000 tickers | 65,250 bytes  | 24,013 bytes  | 36.8%             |



  #protobuf #preformance
  