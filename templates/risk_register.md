# Risk Register & Mitigation Log: [Program Name]

> **Risk Scoring Guide:**
> Calculate **Risk Exposure** as: **Probability (1–5) × Impact (1–5)**.
> - **1–4**: Low Risk (Monitor)
> - **5–9**: Medium Risk (Assign owner, plan mitigation)
> - **10–15**: High Risk (Execute mitigation, track weekly)
> - **16–25**: Critical Risk (Escalate to leadership immediately)

---

## ⚠️ Active Risk Register

| Risk ID | Date Logged | Risk Description | Category | Prob (1-5) | Imp (1-5) | Exposure Score | Mitigation Strategy & Action Plan | Owner | Status | Target Date |
| :---: | :--- | :--- | :--- | :---: | :---: | :---: | :--- | :--- | :---: | :--- |
| **R-01** | YYYY-MM-DD | e.g., Third-party payment gateway API documentation is incomplete, risking delay in integration. | Technical | 4 | 3 | **12** | Mitigate: Request shared Slack channel with vendor engineers; create local mock endpoints. | @TPMOwner | Open | YYYY-MM-DD |
| **R-02** | YYYY-MM-DD | e.g., Primary database administrator is going on parental leave during migration phase. | Resource | 5 | 4 | **20** | Avoid/Transfer: Train backup engineer; document migration runbook early. | @TechLead | Open | YYYY-MM-DD |
| **R-03** | YYYY-MM-DD | e.g., Cloud cost may exceed the program's budget if auto-scaling rules are configured too aggressively. | Budget | 2 | 3 | **6** | Accept: Set up billing alarms at 80% and 90% budget thresholds. | @DevOpsLead | Open | YYYY-MM-DD |

---

## 🕒 Change History & Resolution Log

Use this section to track how risks evolved, when they were closed, or what actual outcomes occurred.

| Risk ID | Update Date | Status Shift | Actions Taken & Results |
| :---: | :---: | :---: | :--- |
| **R-01** | YYYY-MM-DD | Open ──► Mitigated | Vendor Slack channel created. API mock testing complete. Code verified. |
| **R-02** | YYYY-MM-DD | Open ──► Closed | Backup engineer onboarded and completed dry-run database migration. |

---

## 📣 Escalation SOP (Standard Operating Procedure)
When an exposure score enters the **Critical** zone (16–25) and cannot be mitigated locally, initiate this escalation immediately:
1.  **Format the escalation message** using the template in [Execution Methodologies Guide](../domains/execution_methodologies.md).
2.  **Schedule a 15-minute sync** with the Executive Sponsor, Product Lead, and Engineering Lead.
3.  **Establish clear options** (e.g., Delay launch by 2 weeks, cut feature X, or assign 2 engineers from team Y).
