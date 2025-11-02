
# SAGA Pattern

The SAGA pattern is the most common solution for managing data consistency across services in distributed systems. A saga is a sequence of local transactions that are coordinated to execute a complete business process. Each step in the saga is a local, atomic transaction within a single service. Upon successful completion, it triggers the next step in the sequence, typically by publishing an event or sending a command.

## Handling Failures

The key to the SAGA pattern's atomicity is its handling of failures. If any local transaction in the sequence fails, the saga executes a series of compensating transactions to undo the work of the preceding, successfully completed steps. 

For example, if a `CreateOrder` saga involves:
1. `ProcessPayment`
2. `UpdateInventory`
3. `CreateShipment`

And the `CreateShipment` step fails, compensating transactions for:
- `RefundPayment`
- `RestoreInventory`

would be executed. This approach does not provide the immediate consistency of an ACID transaction but instead achieves eventual consistency, where the system converges to a consistent state over time.

## Implementation Approaches

There are two primary approaches to implementing sagas:

### 1. Choreography
This is a decentralized approach where:
- Services communicate directly with each other
- Communication happens through publishing and subscribing to events
- No central coordinator is needed
   
### 2. Orchestration
This is a centralized approach where:
- A dedicated orchestrator service manages the entire saga
- The orchestrator sends commands to each participant service
- It waits for replies and decides the next steps in the workflow
- If a step fails, the orchestrator triggers the necessary compensating transactions

The orchestration approach is easier to monitor and manage for complex workflows but has two main drawbacks:
- Introduces the orchestrator as a potential single point of failure
- Creates a degree of coupling between the participant services and the orchestrator


