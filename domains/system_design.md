# System Design & Technical Architecture — A TPM Study Guide

> This guide outlines the essential technical concepts that a Technical Program Manager (TPM) needs to understand to lead architecture discussions, estimate effort accurately, identify technical risks, and align teams on scalability decisions.

---

## 📋 Table of Contents
- [Why System Design Matters for TPMs](#why-system-design-matters-for-tpms)
- [Distributed Systems Core Concepts](#distributed-systems-core-concepts)
  - [Load Balancing & Traffic Distribution](#load-balancing--traffic-distribution)
  - [Caching Strategies](#caching-strategies)
  - [Content Delivery Networks (CDNs)](#content-delivery-networks-cdns)
- [Database Architecture & Data Storage](#database-architecture--data-storage)
  - [SQL vs. NoSQL Databases](#sql-vs-nosql-databases)
  - [The CAP Theorem](#the-cap-theorem)
  - [Database Scaling: Replication, Partitioning & Sharding](#database-scaling-replication-partitioning--sharding)
- [Application Communication & Protocols](#application-communication--protocols)
  - [API Paradigms (REST, GraphQL, gRPC, WebSockets)](#api-paradigms-rest-graphql-grpc-websockets)
  - [Asynchronous Messaging & Event-Driven Architecture](#asynchronous-messaging--event-driven-architecture)
- [Cloud Infrastructure & Modern DevOps](#cloud-infrastructure--modern-devops)
  - [Monoliths vs. Microservices](#monoliths-vs-microservices)
  - [Containers & Orchestration (Docker & Kubernetes)](#containers--orchestration-docker--kubernetes)
  - [CI/CD & Deployment Strategies](#cicd--deployment-strategies)
  - [Observability & Monitoring](#observability--monitoring)
- [Security, Compliance & Privacy](#security-compliance--privacy)
  - [AuthN vs. AuthZ (OAuth2, OIDC, SAML)](#authn-vs-authz-oauth2-oidc-saml)
  - [Data Encryption (At-Rest, In-Transit)](#data-encryption-at-rest-in-transit)
  - [Compliance Frameworks](#compliance-frameworks)

---

## Why System Design Matters for TPMs

As a TPM, you are not writing production code. However, your technical depth is your primary tool for:
1. **Mitigating Execution Risk**: Anticipating single points of failure (SPOFs), network latencies, or database performance limits *before* code is deployed.
2. **Resource & Schedule Estimation**: Recognizing that a change from a single DB write to an asynchronous transaction queue involves major schema, testing, and latency changes.
3. **Cross-Team Alignment**: Brokering API contracts between frontend and backend teams, or capacity requirements between product engineering and platform infrastructure.

---

## Distributed Systems Core Concepts

Distributed systems scale horizontally by distributing computations and data across multiple machines connected via networks.

### Load Balancing & Traffic Distribution

Load Balancers (LBs) act as the entry point to your services, routing traffic to backend instances.

*   **Algorithms:**
    *   *Round Robin*: Routes sequentially. Simple, but assumes equal server capacity and request cost.
    *   *Least Connections*: Routes to the server with fewest active sessions. Ideal for long-lived connections (e.g., chat apps).
    *   *Consistent Hashing*: Maps client IP/ID to specific servers. Crucial for stateful backends or cache coherence.
*   **TPM Trade-off Analysis:**
    *   **Hardware LBs (F5)** offer high throughput but are expensive and slow to provision.
    *   **Software/Cloud LBs (AWS ALB, NGINX, HAProxy)** are highly configurable, support auto-scaling, and handle SSL termination, but consume virtual compute resources.

### Caching Strategies

Caching stores copies of frequently accessed data in memory (e.g., Redis, Memcached) to reduce database load and lower read latencies.

```
                   Cache Hit (Fast)
              ┌────────────────────────┐
              ▼                        │
Client ──► Gateway ──► Application ──► Cache
                        │
                        ├────────────────────────► Database
                           Cache Miss (Slow, Writes to Cache)
```

*   **Caching Patterns:**
    *   *Cache-Aside (Lazy Loading)*: App queries cache; on miss, reads from DB and writes to cache. Minimizes cache footprint, but first read is slow.
    *   *Write-Through*: App writes to cache, which writes to DB. Cache is always consistent, but write latency is higher.
    *   *Write-Back (Write-Behind)*: App writes to cache; cache asynchronously writes to DB in batches. Fast writes, but risks data loss if the cache fails.
*   **Gotchas for TPMs**: Watch out for **Cache Stampede** (many requests query the DB concurrently on cache expiration) and **Cache Invalidation** (updating cache values when the source database changes).

### Content Delivery Networks (CDNs)

CDNs (Cloudflare, Akamai) are globally distributed networks of proxy servers caching static content (images, JS, videos) close to end-users.
*   **Push CDN**: Content is pushed to CDN edge nodes whenever it changes. Good for predictable, low-churn files.
*   **Pull CDN**: Edge node fetches content from your origin server on the first user request. Low management overhead, but first-load latency exists.

---

## Database Architecture & Data Storage

Choosing the right database dictates a program’s scalability limits, schema flexibility, and operational costs.

### SQL vs. NoSQL Databases

| Feature | Relational Databases (SQL) | Non-Relational Databases (NoSQL) |
| :--- | :--- | :--- |
| **Examples** | PostgreSQL, MySQL, Oracle | MongoDB, Cassandra, DynamoDB, Neo4j |
| **Schema** | Rigid, predefined columns & tables | Dynamic, document/key-value/graph |
| **Scaling** | Vertical (Larger servers), Horizontal (Read replicas) | Horizontal (Partitioning/Sharding across nodes) |
| **Transactions** | Strict ACID (Atomicity, Consistency, Isolation, Durability) | BASE (Basically Available, Soft State, Eventual Consistency) |
| **Best For** | E-commerce checkouts, financial ledger systems, complex joins | User profiles, real-time analytics, high-velocity clickstreams |

### The CAP Theorem

In any distributed data store, you can only guarantee **two** of the following three properties:

```
                  Consistency (C)
                 /               \
                /     Distributed \
               /       Databases   \
              /                     \
Availability (A)───────────────────Partition Tolerance (P)
```

1.  **Consistency (C)**: Every read receives the most recent write or an error.
2.  **Availability (A)**: Every non-failing node returns a response (without guarantee that it contains the latest write).
3.  **Partition Tolerance (P)**: The system continues to operate despite network partition/message loss.

> [!IMPORTANT]
> Because physical networks will always experience partitions (P), distributed databases must choose between **Consistency (CP)** or **Availability (AP)**.
> - **CP systems** (e.g., MongoDB, Etcd) block writes during partition to guarantee accuracy.
> - **AP systems** (e.g., DynamoDB, Cassandra) accept writes during partition, resolving conflicts later via eventual consistency.

### Database Scaling: Replication, Partitioning & Sharding

When a single database server hits compute, disk, or memory capacity:
*   **Read Replicas (Replication)**: Primary database handles writes; copies data to secondary databases for reads. Excellent for read-heavy systems, but secondary reads may suffer from replication lag.
*   **Horizontal Partitioning (Sharding)**: Breaking tables into smaller subsets (shards) across physical hardware based on a key (e.g., User ID % N).
    *   *TPM Risk*: Sharding is highly complex. Cross-shard joins are extremely slow, and re-sharding an active system is a multi-month risk-heavy operation.

---

## Application Communication & Protocols

### API Paradigms

```
REST:       GET /users/123  ──►  Returns static JSON payload
GraphQL:    POST /graphql   ──►  Query specified fields: { user { name } }
gRPC:       Binary payload  ──►  High-performance RPC over HTTP/2
WebSockets: Bidirectional   ──►  Long-lived TCP connection for real-time updates
```

*   **REST**: Standard, cacheable, stateless. But prone to over-fetching or under-fetching (requiring multiple round trips).
*   **GraphQL**: Single endpoint; client requests exactly what they need. Solves over-fetching but shifts query complexity to the server (risking DB performance degradation).
*   **gRPC**: Uses Protocol Buffers and HTTP/2. Highly efficient binary serialization. Ideal for internal microservice-to-microservice communication.
*   **WebSockets**: Full-duplex TCP communication. Best for real-time applications (chat, live feeds, dashboard tickers).

### Asynchronous Messaging & Event-Driven Architecture

In event-driven architectures, services communicate by publishing and consuming events asynchronously via a message broker.

*   **Pub/Sub (Publish-Subscribe)**: Producers publish messages to a "Topic". All subscribed services receive copies of that message (e.g., order processed -> inventory service & email service receive notification).
*   **Message Queues**: Producers push messages to a queue. Exactly **one** consumer processes each message. Useful for background job processing.
*   **Technologies:**
    *   **Kafka**: Log-structured, high-throughput, retains message history, guarantees order per partition. Excellent for event sourcing and large-scale data pipelines.
    *   **RabbitMQ**: Advanced routing capabilities, lightweight, messages are deleted once consumed. Excellent for transactional tasks and complex routing rules.

---

## Cloud Infrastructure & Modern DevOps

### Monoliths vs. Microservices

*   **Monolith**: Single codebase and deployment unit. Simple to develop and deploy initially, but code becomes coupled and hard to scale independently.
*   **Microservices**: Application split into loosely coupled services communicating via APIs. Enables independent team deployments and scaling.
    *   *TPM Impact*: Microservices shift architectural complexity to the network. Program managers must align API contracts, deploy integration testing, and set up robust tracing.

### Containers & Orchestration

*   **Docker (Containers)**: Packages application code, runtime, system tools, and libraries into a lightweight, portable container. Ensures consistency from dev to production.
*   **Kubernetes (K8s)**: Container orchestrator. Manages container lifecycle, scaling, load balancing, health checks, self-healing, and service discovery.
    *   *K8s Terms to Know*: Pod (smallest deployable unit), Node (physical/virtual machine running Pods), Deployment (specifies how to run pods).

### CI/CD & Deployment Strategies

Continuous Integration/Continuous Deployment pipelines automate the build, test, and release lifecycle.

*   **Deployment Patterns:**
    *   *Blue/Green*: Two identical environments (Blue active, Green idle). Deploy code to Green, run verification, then swap the load balancer target. Easy rollback, but requires double the infrastructure cost.
    *   *Canary*: Route a small percentage of traffic (e.g., 2%) to the new build. If error rates are normal, scale up gradually. Limits blast radius.
    *   *Rolling*: Gradually update instances one-by-one. Zero-downtime, but running mixed-version code simultaneously can cause schema issues.

### Observability & Monitoring

You cannot manage what you do not measure. A robust system requires the three pillars of observability:
1.  **Metrics (System level)**: Quantitative measurements (CPU, memory, request count, error rates, latencies). Tools: Prometheus, Datadog.
2.  **Logs (App level)**: Timestamped text outputs describing application events (errors, debug statements). Tools: ELK Stack (Elasticsearch, Logstash, Kibana), Splunk.
3.  **Traces (Request level)**: End-to-end path of a request through microservices. Essential for debugging latency issues. Tools: Jaeger, Zipkin, OpenTelemetry.

---

## Security, Compliance & Privacy

### AuthN vs. AuthZ
*   **Authentication (AuthN)**: Verification of identity ("Who are you?"). Verified via passwords, biometrics, or MFA.
*   **Authorization (AuthZ)**: Verification of permissions ("What are you allowed to do?"). Role-Based Access Control (RBAC).
*   **Standards:**
    *   **OAuth 2.0**: Framework for delegation of authorization (letting apps access resources on behalf of user).
    *   **OIDC (OpenID Connect)**: Identity layer built on top of OAuth 2.0 (standardizing authentication).
    *   **SAML**: XML-based standard for Single Sign-On (SSO) in enterprise organizations.

### Data Encryption
*   **Encryption In-Transit**: Protects data moving across networks. Implemented via **TLS** (Transport Layer Security) protocols.
*   **Encryption At-Rest**: Protects data on disk (drives, databases, cloud storage buckets). Uses symmetric encryption algorithms like **AES-256**, managed through Key Management Services (KMS).

### Compliance Frameworks

TPMs are frequently accountable for ensuring programs comply with legal and security standards:

| Framework | Target Domain | Key Requirements |
| :--- | :--- | :--- |
| **GDPR / CCPA** | Personal Data & Privacy | Right to be forgotten, data minimization, consent logging. |
| **SOC 2 Type II** | Security & Availability | Audited evidence showing secure handling of customer data over time. |
| **HIPAA** | Healthcare Information | Encryption of Protected Health Information (PHI), audit logs. |
| **PCI-DSS** | Credit Card Payments | Dedicated payment zones, network isolation, tokenization. |

---

## Technical program alignment check
When starting a technical program, use these discussion prompts with your tech leads:
1. *What are the throughput (QPS) and latency SLA requirements?*
2. *Is our database read-heavy or write-heavy? How does it scale?*
3. *What is the disaster recovery plan? (Active-Active or Active-Passive)?*
4. *How are API versions managed? Are we introducing breaking changes?*
5. *What monitoring, alarms, and dashboards will be created to verify release health?*
