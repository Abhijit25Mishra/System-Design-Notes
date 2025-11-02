
# Best Practices for Protocol Buffers

The long-term success of a system built on [[ProtoBuf]] depends heavily on the quality and maintainability of its `.proto` files. Adhering to best practices is not merely a matter of style but a crucial element of future-proofing the API.

## File Organization

Logically group related messages and services within the same `.proto` file. For message types that are expected to be widely used across many different services or projects (e.g., a common `Money` or `Timestamp` message), it is best to place them in their own separate file with no other dependencies. This promotes reusability and minimizes dependency bloat.

## Style Guide

Consistency is key. The official Google Protobuf Style Guide provides a set of conventions that should be followed:

- **Formatting**: Use a standard line length (80 characters) and indentation (2 spaces).
- **Naming**: Use `TitleCase` for message, service, and enum names. Use `lower_snake_case` for field and oneof names. Use `UPPER_SNAKE_CASE` for enum values.    
- **Comments**: Use `//`-style comments to document the purpose of messages, fields, and services. A well-commented `.proto` file is a form of living documentation for the API.

## Unique Request/Response Messages

A critical best practice for RPC service design is to always define unique message types for each method's request and response, even if a method initially takes no parameters or returns nothing. Instead of using a shared or empty type like `google.protobuf.Empty`, define a specific message (e.g., `CreateUserRequest`, `CreateUserResponse`). 

This practice future-proofs the API. If, in the future, a parameter needs to be added to the request or a new field needs to be returned in the response, it can be done by adding a field to the existing message—a non-breaking change. If `google.protobuf.Empty` were used, adding any parameter would require changing the method signature, which is a breaking change.

This discipline of creating purpose-built messages ensures the API remains extensible for its entire lifetime.




| Schema Change                         | Backward Compatible? | Forward Compatible? | Notes                                                                                                      |
| ------------------------------------- | -------------------- | ------------------- | ---------------------------------------------------------------------------------------------------------- |
| Add a new optional or repeated field  | Yes                  | Yes                 | Safest and most common change. New code uses default values for old data; old code ignores the new field.  |
| Remove a field                        | Yes                  | No (Potentially)    | Old code may fail if it relies on the field. The field number and name should be marked as reserved.       |
| Rename a field                        | Yes (Wire Format)    | Yes (Wire Format)   | Breaking change for source code. Requires coordinated code updates across all clients and servers.         |
| Change a field's number               | No                   | No                  | Never do this. This is a catastrophic breaking change that leads to data corruption.                       |
| Change type (int32 to int64)          | Yes                  | Yes                 | Compatible numeric types can be changed.                                                                   |
| Change type (int32 to string)         | No                   | No                  | Incompatible wire formats will cause deserialization errors.                                               |
| Change from singular to repeated      | No                   | No                  | The wire format is incompatible.                                                                           |
| Add a field to an existing oneof      | Yes                  | No                  | Old clients will not recognize the new oneof variant, leading to potential data loss or misinterpretation. |
| Remove a field from an existing oneof | No                   | Yes                 | New clients may misinterpret old data containing the removed variant, leading to cascading data loss.      |
