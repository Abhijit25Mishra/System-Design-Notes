# When to Create or Extend a Service

There is no universal answer. The decision must be based on careful evaluation of architectural principles and long-term consequences. 

## Reasons to Create a New Service

1. **Divergent Responsibilities and the Single Responsibility Principle**
   - Service has clearly separate and distinct business functionality
   - Follows single responsibility principle

2. **Independent Scaling Requirements**
   - Different performance characteristics
   - Unique resource demands

3. **Different Technology Stack**
   - Requires specific technologies or frameworks
   - Incompatible with current service architecture

4. **Team Ownership and Autonomy**
   - Clear ownership boundaries
   - Independent development and deployment cycles

## Reasons to Extend an Existing Service

1. **High Cohesion and Domain Proximity**
   - Functionality is closely related to existing service
   - Shares common domain concepts and data models

2. **Avoiding Premature Decomposition**
   - Every new service introduces operational overhead:
     - Deployment pipelines
     - Monitoring systems
     - Security configurations
     - Network communication latency

## Decision Making Process

This decision-making process is fundamentally about preserving the [[Architectural choice|architectural integrity]] of the system by protecting the boundaries of each service.

When creating a new microservice, always start with [[Domain Driven Design]] to properly define its boundaries and responsibilities.