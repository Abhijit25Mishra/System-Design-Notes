
The Database per Service pattern is a core tenet of microservices architecture which dictates that each microservice must own and manage its own private database (or a private schema within a shared database server). Crucially, this data store is considered part of the service's internal implementation and must not be directly accessed by any other service. All communication and data exchange must occur exclusively through the service's public API.

Pros
1. **Loose Coupling**
2. **Technology Freedom (Polyglot Persistence)**
3. **Independent Scalability and Fault Isolation**


Cons:
1. **Distributed Transactions**: **Implementing business transactions that span multiple services becomes highly complex. Traditional ACID (Atomicity, Consistency, Isolation, Durability) transactions, which rely on two-phase commits, are often not feasible or supported by modern NoSQL databases and are generally avoided in microservices due to their negative impact on availability and performance, as described by the CAP theorem.**
2. **Cross-Service Queries: Performing queries that require joining data from multiple, now-separate databases is challenging. Such operations cannot be handled by a simple SQL join and require more complex patterns.**
3. **Operational Complexity: The need to deploy, manage, monitor, and back up a multitude of different database technologies increases the operational burden on the organization.**


To address the challenge of distributed transactions, the [[SAGA pattern]] is the most common solution.