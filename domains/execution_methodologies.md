# Program Execution Methodologies & Playbook — A TPM Guide

> This guide provides a framework for program execution, from planning complex software lifecycles to managing technical risks, mapping dependencies, and defining operational metrics.

---

## 📋 Table of Contents
- [The Program Lifecycle](#the-program-lifecycle)
- [Software Development Frameworks](#software-development-frameworks)
  - [Scrum Framework](#scrum-framework)
  - [Kanban Framework](#kanban-framework)
  - [Waterfall vs. Agile Trade-offs](#waterfall-vs-agile-trade-offs)
- [Dependency Management & Critical Path](#dependency-management--critical-path)
  - [The Critical Path Method (CPM)](#the-critical-path-method-cpm)
  - [Dependency Mapping & Types](#dependency-mapping--types)
  - [Mitigating Technical Dependencies](#mitigating-technical-dependencies)
- [Risk Management Framework](#risk-management-framework)
  - [The Risk Matrix](#the-risk-matrix)
  - [Risk Mitigation Strategies](#risk-mitigation-strategies)
  - [Structuring Escalations](#structuring-escalations)
- [Program Delivery Metrics](#program-delivery-metrics)

---

## The Program Lifecycle

Unlike a standalone project, a technical program consists of multiple interconnected workstreams driving toward a unified strategic goal.

```
┌──────────────┐      ┌──────────────┐      ┌──────────────┐      ┌──────────────┐      ┌──────────────┐
│  Initiation  │ ───► │   Planning   │ ───► │  Execution   │ ───► │  Monitoring  │ ───► │   Closing    │
│ (Charter,    │      │ (Architecture│      │ (Milestones, │      │ (Rollouts,   │      │ (Retro, Hand-│
│ Stakeholders)│      │  WBS, CPM)   │      │  Risk Log)   │      │  Canary, SLA)│      │  off, Docs)  │
└──────────────┘      └──────────────┘      └──────────────┘      └──────────────┘      └──────────────┘
```

1.  **Initiation**: Defining scope, writing the [Project Charter](../templates/project_charter.md), mapping stakeholders, and securing executive sponsors.
2.  **Planning**: System design alignment, detailing the Work Breakdown Structure (WBS), mapping dependencies, and calculating the Critical Path.
3.  **Execution**: Standard execution cadence (sprint syncs, program reviews), managing the [Risk Register](../templates/risk_register.md), and clearing blockers.
4.  **Monitoring & Launch**: Multi-phase deployment (canary, rolling, dark launches), monitoring telemetry metrics, and validating SLAs.
5.  **Closing**: Holding a blameless post-mortem/retrospective, updating architecture docs, handing off operations to support, and archiving resources.

---

## Software Development Frameworks

A TPM must choose the right framework (or hybrid approach) depending on the team structure, release coupling, and level of ambiguity.

### Scrum Framework

Best for teams shipping iterative software with a stable, prioritized product backlog.

*   **Key Roles:**
    *   *Product Owner*: Owns the product backlog (what to build and why).
    *   *Scrum Master*: Facilitates processes and coaches the team (clears process impediments).
    *   *Development Team*: Designs, builds, and tests the increments.
*   **Ceremonies:**
    *   *Sprint Planning*: Commit to a sprint goal and define sprint backlog tasks.
    *   *Daily Standup*: 15-minute sync on yesterday's work, today's plans, and blockers.
    *   *Sprint Review (Demo)*: Demonstrate working software to stakeholders.
    *   *Sprint Retrospective*: Reflect on team dynamics, processes, and improvements.
*   **Key Metrics**: Velocity (average story points completed per sprint), Burndown Chart (daily sprint progress).

### Kanban Framework

Best for operational, support, platform, or infrastructure teams handling high-churn, reactive, or continuous intake pipelines.

*   **Core Pillars:**
    *   *Visualize Workflow*: Board tracking states (Backlog -> In Progress -> Code Review -> QA -> Done).
    *   *Limit Work-in-Progress (WIP)*: Prevent bottlenecks by limiting the number of active tasks in any state.
    *   *Manage Flow*: Measure and optimize Lead Time (total time from ticket creation to completion) and Cycle Time (time spent actively coding/testing a ticket).

### Waterfall vs. Agile Trade-offs

| Dimension | Waterfall (Linear) | Agile (Iterative) |
| :--- | :--- | :--- |
| **Requirements** | Rigid, locked at initiation. | Adaptive, evolving with feedback. |
| **Release Cadence** | Single big-bang release at the end. | Continuous, incremental updates. |
| **Risk Profile** | High risk of late-stage integration failure. | Low risk of total failure; bugs caught early. |
| **Best For** | Physical hardware rollouts, compliance-bound migrations. | SaaS products, web/mobile app features. |

---

## Dependency Management & Critical Path

Managing cross-functional programs requires mapping how the delays in one team affect others.

### The Critical Path Method (CPM)

The **Critical Path** is the longest sequence of dependent tasks that determines the shortest possible duration of the program. Any delay on the critical path directly pushes out the launch date.

```
[Task A: Database Setup] (4d) ──► [Task B: API Development] (6d) ──► [Task C: Frontend Integration] (5d)
                                         ▲
[Task D: Design Assets]  (2d) ───────────┘
```
*In this simple example, the path A -> B -> C takes 15 days, whereas D -> B -> C takes 13 days. The critical path is **A -> B -> C**. Task D has 2 days of "slack" (float) time.*

> [!TIP]
> **TPM Action**: Always prioritize your engineering resources on Critical Path tasks. If a task *not* on the critical path is delayed, monitor its "float" (slack time) to ensure it doesn't become the new critical path.

### Dependency Mapping & Types

A dependency describes the relationship between two tasks:
*   **Finish-to-Start (FS)**: Task B cannot start until Task A finishes (Most common; e.g., cannot deploy code until QA is complete).
*   **Start-to-Start (SS)**: Task B cannot start until Task A starts (e.g., database migration begins, sync job starts).
*   **Finish-to-Finish (FF)**: Task B cannot finish until Task A finishes (e.g., testing cannot conclude until documentation is complete).
*   **Start-to-Finish (SF)**: Task B cannot finish until Task A starts (Rarely used).

### Mitigating Technical Dependencies

When working with external teams:
1.  **Define API Contracts Early**: Use OpenAPI/Swagger specs to align interface expectations. Do not wait for code to finish to design APIs.
2.  **Integration Mocking**: Build mock endpoints so team B can develop and test independently of team A’s deployment.
3.  **Feature Flags (Toggle-Based Integration)**: Deploy code early in disabled states, allowing systems to integrate continuously behind flags.
4.  **Double-Routing (Dark Launch)**: Run new systems in parallel with old systems, comparing output data silently before cutting over.

---

## Risk Management Framework

Every program faces uncertainty. A TPM’s job is to catalog, quantify, and mitigate these risks.

### The Risk Matrix

Log risks in a central register. Calculate severity score as: **Probability × Impact (Score 1-5)**.

```
       5 | [Medium]   [High]     [Critical] [Critical] [Critical]
I      4 | [Low]      [Medium]   [High]     [Critical] [Critical]
m      3 | [Low]      [Medium]   [Medium]   [High]     [Critical]
p      2 | [Low]      [Low]      [Medium]   [Medium]   [High]
a      1 | [Low]      [Low]      [Low]      [Low]      [Medium]
c        └────────────────────────────────────────────────────────
t                  1          2          3          4          5
                         P r o b a b i l i t y
```

*   **Low (1–4)**: Track internally; no action required.
*   **Medium (5–9)**: Assign owners; establish watch lists.
*   **High (10–15)**: Actively mitigate; report in weekly updates.
*   **Critical (16–25)**: Escalation required; direct executive path.

### Risk Mitigation Strategies

*   **Avoid**: Change the program plan to eliminate the risk (e.g., bypass a buggy third-party service by building a basic internal solution).
*   **Mitigate**: Reduce the probability or impact of the risk (e.g., write automated fallback routes if the dependency goes offline).
*   **Transfer**: Shift the risk impact to a third party (e.g., buy cloud hosting SLAs rather than hosting database servers locally).
*   **Accept**: Acknowledge the risk and do nothing, useful for low-impact risks or where mitigation is cost-prohibitive.

### Structuring Escalations

When a blocker cannot be resolved at the engineering level, escalate to leadership immediately. Use this structured format:
1.  **The Blocker (One-liner)**: Clear summary of the issue.
2.  **Business & Technical Impact**: What happens to users/metrics/schedules if unresolved.
3.  **Options Explored**: What the engineering team has tried.
4.  **Proposed Path / Recommendation**: What you think the solution is.
5.  **Required Action**: The specific decision or resources needed from leadership.

---

## Program Delivery Metrics

To measure execution success, track the following metrics monthly:

*   **Say-Do Ratio**: The percentage of committed roadmap features delivered in a cycle. (Target: >85%).
*   **Velocity Trends**: Is team throughput stable, increasing, or erratic?
*   **SLA Breach Rates**: Frequency of production systems breaching uptime or response time limits.
*   **Bug Escape Rate**: The ratio of bugs discovered in production vs. pre-release environments.
*   **Time-to-Resolve (MTTR)**: Average time elapsed between incident trigger and incident resolution.
