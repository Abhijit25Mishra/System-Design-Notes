## Monolithic vs. Microservices

#### Monolithic
- A single, unified codebase.
- All business functions are contained in a single application.
- All components are interdependent.
- The application is deployed as a single unit.

**Pros:**
1. Better for small-scale teams.
2. Simplicity and speed. Development is faster and deployment is straightforward.
3. Tracing a request is easier.

**Cons:** As it scales, all the pros turn into cons.
1. Slow development, unintended changes.
2. Scaling becomes inefficient.
3. A single error can compromise the availability of the entire application.
4. New technology adoption is blocked.

#### Microservices
- A set of applications.
- Independent of each other.
- Loosely coupled, i.e., each service handles a different business function.
- Communicates through APIs.

**Pros:**
1. Distinct scaling requirements can be handled.
2. Enables technological diversity.
3. Dramatically improves fault isolation.

Microservices solve a lot of problems that are present in a monolithic architecture. Adopting microservices is often driven by the need to scale the organization, not just the application. A large team working on a single monolithic codebase inevitably faces high coordination overhead, frequent merge conflicts, and a bottlenecked release pipeline.
Microservices minimize cross-team communication overhead, enabling parallel development and deployment. Data consistency is one area where a monolithic architecture is often preferred, as it is easier to maintain strong consistency.

[[When to create or extend a service]] ?



| Dimension                | Monolithic Architecture                                                                            | Microservices Architecture                                                                                                     |
| ------------------------ | -------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| **Team Size**            | Ideal for small teams (<10-15 developers) where communication is simple.                           | Suitable for larger organizations with multiple teams that need to work independently.                                         |
| **Development Speed**    | High initial velocity due to simplicity. Slower long-term velocity as complexity grows.            | Slower initial setup due to distributed system complexity. Higher long-term velocity for large teams.                          |
| **Scalability**          | Coarse-grained. The entire application must be scaled, even if only one component is a bottleneck. | Fine-grained. Each service can be scaled independently, optimizing resource usage.                                             |
| **Deployment**           | Simple. A single artifact is deployed.                                                             | Complex. Requires mature automation (CI/CD) and container orchestration for many services.                                     |
| **Operational Overhead** | Low. Fewer moving parts to manage, monitor, and secure.                                            | High. Requires managing a distributed system, including service discovery, distributed tracing, and complex networking.        |
| **Data Consistency**     | Strong consistency is easily achieved with a single, transactional database (ACID).                | Eventual consistency is the default. Achieving strong consistency across services is complex and requires patterns like Sagas. |
| **Fault Isolation**      | Low. An error in one module can bring down the entire application.                                 | High. Failure in one service is isolated and typically does not cascade to the entire system.                                  |
| **Technology Stack**     | Homogeneous. Constrained by the technologies chosen at the outset.                                 | Heterogeneous (Polyglot). Each service can use the most appropriate language, framework, and database.                         |


