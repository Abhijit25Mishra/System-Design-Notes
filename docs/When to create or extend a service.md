There is no universal answer. It must be based on care evaluation of architectural principles and long term consequences. 

### Reasons to create a new service
1. **Divergent Responsibilities and the Single Responsibility Principle**
2. **Independent Scaling Requirements**
3. **Different Technology Stack**
4. **Team Ownership and Autonomy**
### Reasons to extend a new service
1. **High Cohesion and Domain Proximity**
2. **Avoiding Premature Decomposition** : every new service that gets introduces operational overhead, including deployment pipelines, monitoring, security configurations, and network communication latency.

This decision-making process is fundamentally about preserving the [[Architectural choice|architectural integrity]] of the system by protecting the boundaries of each service.

The first step when created a new microservice starts with [[Domain Driven Design]].