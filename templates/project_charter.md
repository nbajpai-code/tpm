# Project Charter: [Program Name]

> **Document Version**: 1.0  
> **Status**: [Draft / Under Review / Approved]  
> **Last Updated**: YYYY-MM-DD  
> **Program Owner**: [Name] | **Tech Lead**: [Name] | **Product Owner**: [Name]  

---

## 1. Executive Summary
Provide a brief, high-level summary (3–5 sentences) of what this program is, why it is necessary, and the strategic value it brings to the organization.

---

## 2. Business Problem & Goals

### 2.1 Problem Statement
Define the specific problem, pain point, or opportunity this program addresses. *Why should we solve this now?*

### 2.2 Program Goals
Specify the goals of the program using the SMART framework (Specific, Measurable, Achievable, Relevant, Time-bound).
1.  **[Goal 1]**: e.g., Migrate 100% of user data to the new database with zero downtime.
2.  **[Goal 2]**: e.g., Reduce page load latency by 20% by Q4.

### 2.3 Success Metrics (KPIs)
List the key performance indicators that will prove the program was successful.

| Metric | Baseline | Target | Measurement Method |
| :--- | :--- | :--- | :--- |
| e.g., Core Web Vital LCP | 3.5s | < 2.0s | Real User Monitoring (RUM) dashboard |
| e.g., System Uptime | 99.9% | 99.99% | Datadog availability alerts |

---

## 3. Scope & Boundaries

### 3.1 In Scope
List the specific features, services, and tasks that are included in this program:
*   [Feature/Task 1]
*   [Feature/Task 2]
*   [Feature/Task 3]

### 3.2 Out of Scope
Clearly state what is excluded from the program to prevent scope creep:
*   [Feature/Task 4] - *Planned for phase 2.*
*   [Feature/Task 5] - *Owned by the Security Infrastructure team.*

---

## 4. Key Stakeholders

| Role | Name / Team | Contact (Slack/Email) |
| :--- | :--- | :--- |
| **Executive Sponsor** | [Name] | [Contact] |
| **Product Lead** | [Name] | [Contact] |
| **Technical Lead** | [Name] | [Contact] |
| **Security Lead** | [Name] | [Contact] |
| **UX/Design Lead** | [Name] | [Contact] |

---

## 5. High-Level Timeline & Milestones

Provide key target dates for major program phases:

```
[Phase 1: Architecture Sign-Off] ──► [Phase 2: Code Freeze] ──► [Phase 3: Canary Release] ──► [Phase 4: GA]
           Target: Q1                              Target: Q2                      Target: Q3            Target: Q4
```

| Milestone | Description | Target Date | Status |
| :--- | :--- | :--- | :--- |
| **Milestone 1** | Architecture and API design approval | YYYY-MM-DD | [Not Started] |
| **Milestone 2** | Core service development complete | YYYY-MM-DD | [Not Started] |
| **Milestone 3** | Integration testing & security audit | YYYY-MM-DD | [Not Started] |
| **Milestone 4** | 100% Traffic Cutover (GA Launch) | YYYY-MM-DD | [Not Started] |

---

## 6. Assumptions, Constraints & Risks

### 6.1 Assumptions
Statements taken as true for planning purposes (e.g., *Infrastructure team will provision K8s clusters by Q2*).

### 6.2 Constraints
Limitations within which the program must operate (e.g., *Fixed budget, strict security compliance deadline of Oct 1st*).

### 6.3 Initial Key Risks
List the top high-level risks identified at initiation (detailed risks go to the [Risk Register](risk_register.md)).
1.  **Risk**: [Description] | **Mitigation**: [Plan]
2.  **Risk**: [Description] | **Mitigation**: [Plan]

---

## 7. Approval Signatures
By signing below, the stakeholders agree to the scope, goals, and resources defined in this charter.

*   **Executive Sponsor**: _______________________ Date: _________
*   **Product Lead**: ___________________________ Date: _________
*   **Technical Lead**: __________________________ Date: _________
