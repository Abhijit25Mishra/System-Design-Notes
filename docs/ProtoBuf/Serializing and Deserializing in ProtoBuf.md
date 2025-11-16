
# Serializing and Deserializing in Protocol Buffers

## Serializing Messages

The process of serializing a message typically involves three steps:

1. **Instantiation**
   - Create an instance of the generated message class
   - Often uses a builder pattern for clarity
   - Ensures the object is immutable once created

2. **Population**
   - Use the generated setter methods
   - Populate the fields of the message with application data

3. **Serialization**
   - Call one of the serialization methods (e.g., `SerializeToString()` or `toByteArray()`)
   - Executes highly optimized serialization logic
   - Converts the in-memory object into a compact byte array
   - Ready for transmission or storage

### Binary Format

The resulting binary data is not self-describing:
- Stream of key-value pairs
- "Key" is a combination of:
  - Field number
  - Wire type identifier
- "Value" is the encoded data payload
- Field names and precise data types are not included

This structure is a fundamental reason for [[Serializing and Deserializing in ProtoBuf|Protobuf's efficiency]], but it requires the exact same schema definition (or the code generated from it) to correctly interpret the data on the receiving end.

## Deserializing Messages

The deserialization process is the mirror image of serialization:

1. **Receive Data**
   - Obtain the serialized byte array
   - Can come from:
     - Network socket
     - File
     - Other data sources

2. **Parsing**
   - Call a static parsing method on the corresponding generated message class
   - Pass the byte array as an argument
   - Example: `Person.parseFrom(data)`

3. **Usage**
   - Returns a fully populated, read-only instance of the message object
   - Use the generated getter methods to access the data
   - Object is immutable to ensure thread safety

#protobuf 