# TPM Interview Preparation & Study Roadmap

> A comprehensive study plan, frameworks, and sample questions to help Technical Program Managers prepare for loops at major tech companies (FAANG, high-growth startups, and enterprise companies).

---

## 📋 Table of Contents
- [The TPM Interview Loop Structure](#the-tpm-interview-loop-structure)
- [6-Week Preparation Timeline](#6-week-preparation-timeline)
- [Frameworks for Success](#frameworks-for-success)
  - [System Design Framework](#system-design-framework)
  - [Behavioral & STAR Framework](#behavioral--star-framework)
  - [Program Management Framework](#program-management-framework)
- [Sample Interview Questions](#sample-interview-questions)
  - [System Design Questions](#system-design-questions)
  - [Program Execution Scenarios](#program-execution-scenarios)
  - [Behavioral & Influence Questions](#behavioral--influence-questions)
  - [Fermi Estimation / Capacity Questions](#fermi-estimation--capacity-questions)
- [Study Checklist](#study-checklist)

---

## The TPM Interview Loop Structure

While exact rounds vary by company (e.g., Meta focuses on Technical Program Sense, Google on Technical Depth/System Design, Amazon on Leadership Principles), the standard TPM loop covers four core areas:

1.  **Technical Screen / System Design**: Evaluates technical breadth. You must explain how to design a distributed system, choose components (DBs, caches, brokers), and scale it under high loads.
2.  **Program Management & Execution**: Evaluates your planning, execution, risk mitigation, and dependency management capabilities.
3.  **Leadership & Behavioral**: Evaluates how you resolve conflicts, handle failures, manage stakeholder relationships, and influence without authority.
4.  **Analytical & Estimation (Fermi Problems)**: Evaluates quantitative capabilities. You will estimate storage, bandwidth, queries per second (QPS), and cost for a system.

---

## 6-Week Preparation Timeline

```
Week 1-2: Core System Design  ──►  Week 3-4: Program Execution  ──►  Week 5: Behavioral Storytelling  ──►  Week 6: Mock Loops
```

*   **Week 1 & 2: Technical & System Design Fundamentals**
    *   Study scaling, load balancing, caching, databases, and microservice communications. (Read Kleppmann's *Designing Data-Intensive Applications*).
    *   Practice designing 3 standard systems (e.g., URL shortener, Chat app, Video streaming).
*   **Week 3 & 4: Program Management & Lifecycle Case Studies**
    *   Review Scrum, Kanban, and Waterfall.
    *   Document 3–4 detailed stories from your career about: a delayed dependency, a critical risk realization, a major scope change, and a production outage.
*   **Week 5: Behavioral Prep & STAR Structuring**
    *   Map your career history to behavioral categories (Conflict, Influence, Failure, Teamwork).
    *   Refine stories using the STAR framework.
*   **Week 6: Capacity Estimation & Mocks**
    *   Practice Fermi estimation math (conversions, bytes, memory, traffic metrics).
    *   Run at least 2 mock interviews for System Design and Program Management.

---

## Frameworks for Success

### System Design Framework

When asked: *"Design [System X]"*, do not start drawing blocks immediately. Follow this structured flow:

```
1. Clarify Requirements (5 mins) ──► 2. High-Level Design (10 mins) ──► 3. Deep Dive (15 mins) ──► 4. Scale & Bottlenecks (10 mins)
```

1.  **Clarify Requirements (Functional & Non-Functional)**:
    *   *Functional*: What features are in scope? (e.g., Can users post images? Can they search?)
    *   *Non-Functional*: Scale (DAU, write/read ratio), Latency SLAs (e.g., API < 200ms), Availability (e.g., 99.99%).
2.  **High-Level Design**:
    *   Draw the client, load balancer, API gateway, databases, and core application blocks.
    *   Define core API endpoints (e.g., `POST /v1/tweet`).
3.  **Detailed Component Deep Dive**:
    *   Define the database schema (SQL vs NoSQL).
    *   Explain data flow for key actions. Show where caching and message queues fit in.
4.  **Scale, Failure, & Bottleneck Resolution**:
    *   Address single points of failure.
    *   How does the system handle database bottlenecks? (Replicas, sharding, indexes).
    *   Explain geo-distribution (CDNs, multi-region routing).

### Behavioral & STAR Framework

Use the **STAR** method to keep behavioral answers structured and concise:
*   **S — Situation**: Set the context (Company, program size, high stakes). Keep it under 20% of your response time.
*   **T — Task**: What was the challenge or conflict? What was *your* specific responsibility?
*   **A — Action**: What did *you* do? Explain your technical investigations, stakeholder negotiations, or change management. Focus on "I", not "We". (50% of response time).
*   **R — Result**: What was the outcome? Use quantitative metrics where possible (e.g., *“Delivered the migration 3 days ahead of schedule, saving $200k in monthly infrastructure bills.”*).

### Program Management Framework

For scenario questions (e.g., *"How would you launch a new payment gateway?"*), structure the delivery lifecycle:
1.  **Initiation**: Project charter, stakeholder mapping (RACI), aligning on business objectives.
2.  **Architecture**: High-level design sign-off, API contract agreements.
3.  **Resource & Scheduling**: WBS, calculating Critical Path, identifying cross-team dependencies.
4.  **Execution & Risk**: Setting weekly sprint processes, tracking risks in a register, clear escalation channels.
5.  **Launch Operations**: Canary deployment gates, rollback procedures, alerting systems, and metrics tracking.

---

## Sample Interview Questions

### System Design Questions
*   *Design WhatsApp / Messenger*: Focus on real-time bidirectional communication, persistent TCP connections (WebSockets), presence indicator (online/offline), message storage (NoSQL/Cassandra), and delivery status updates.
*   *Design Netflix / YouTube Video Streaming*: Focus on video ingestion, encoding/transcoding pipelines, global content delivery via CDNs, and dynamic adaptive streaming (HLS/DASH).
*   *Design an E-Commerce Cart & Checkout System*: Focus on transactional consistency (ACID, databases, transaction boundaries), concurrency handling during flash sales (queues, locks), and third-party payment gateway integration.

### Program Execution Scenarios
*   *"A partner team informs you that the API service you depend on will be delayed by 4 weeks. What do you do?"*
    *   **Ideal Answer Steps**: Check the critical path. Evaluate float time. Can we build mock endpoints to unblock local testing? Can we negotiate a reduced API schema for phase 1? Escalate only if float is exceeded and timeline is directly threatened.
*   *"Product Management wants to add three new features to a release that is currently in QA phase. How do you handle this?"*
    *   **Ideal Answer Steps**: Evaluate impact. How much additional QA testing is required? What is the impact on launch SLAs? Present options to PM: (1) Launch on time without features, release them in Phase 2; (2) Delay launch by X weeks to include them. Make it a joint decision, but highlight resource/timeline costs.

### Behavioral & Influence Questions
*   *"Tell me about a time you had to influence an engineering team to change their technical design."*
    *   **Ideal Answer Steps**: Focus on data and user impacts. Do not just debate code. Show how you gathered performance metrics, cloud cost estimates, or customer SLAs to present an objective trade-off analysis.
*   *"Describe a program that was a failure. What went wrong and what did you learn?"*
    *   **Ideal Answer Steps**: Take accountability. Avoid blaming individuals. Focus on systemic or process gaps (e.g., underestimating legacy database migration complexity) and explain the specific process changes you implemented in the retrospective to prevent it from happening again.

### Fermi Estimation / Capacity Questions
*   *"Estimate the storage required to support photos uploaded to Instagram daily."*
    *   **Sample Calculation**:
        1.  Assume **500 million daily active users (DAU)**.
        2.  Assume **10% of users** upload a photo daily = 50 million photos.
        3.  Assume average photo size is **2 Megabytes (MB)**.
        4.  $50\text{ million} \times 2\text{ MB} = 100\text{ Terabytes (TB)}$ of raw data per day.
        5.  Add replication factor (e.g., 3 copies for backup) = $300\text{ TB}$ of storage needed *per day*.
        6.  Annually: $300\text{ TB/day} \times 365\text{ days} \approx 110\text{ Petabytes (PB)}$ of storage per year.

---

## Study Checklist

- [ ] Read *Designing Data-Intensive Applications* (Martin Kleppmann).
- [ ] Understand difference between SQL indexes (B-Trees vs. LSM-Trees).
- [ ] Master basic computer networking (TCP/IP handshake, DNS lookup process, HTTPS encryption).
- [ ] Practice 10 behavioral questions using the STAR framework.
- [ ] Practice 5 Fermi capacity problems (Storage, bandwidth, QPS).
- [ ] Complete 2 live mock interviews with other TPMs/Engineers.
