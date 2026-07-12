# Retrospective & Program Post-Mortem: [Program Name]

**Date**: YYYY-MM-DD  
**Facilitator**: [Name]  
**Participants**: [List of attendees or teams represented]  

---

## 1. Program Outcome Summary
Briefly describe the final state of the program. Was it launched on time? Did it meet the goals and success metrics established in the [Project Charter](project_charter.md)?

*   **Target Launch Date**: YYYY-MM-DD
*   **Actual Launch Date**: YYYY-MM-DD
*   **Key Results**: e.g., Deployed backend v2; achieved a P99 latency of 150ms (target was < 200ms).

---

## 2. What Went Well (Successes)
Discuss what practices, decisions, or behaviors contributed to positive outcomes. What should we repeat in future programs?
- **[Practice/Event 1]**: e.g., Defining API specs using Swagger before coding saved frontend-backend alignment meetings.
- **[Practice/Event 2]**: e.g., Automated deployment pipelines worked flawlessly during the canary rollout.
- **[Practice/Event 3]**: e.g., Team collaboration and responsiveness on the shared Slack channel was excellent.

---

## 3. What Could Have Gone Better (Opportunities)
Discuss what challenges, delays, or issues were encountered. *Keep it objective and focused on processes, not individuals.*
- **[Practice/Event 1]**: e.g., We underestimated the effort required to clean legacy database records, which delayed migration by 4 days.
- **[Practice/Event 2]**: e.g., Communication gaps between the security compliance team and engineering delayed the audit sign-off.
- **[Practice/Event 3]**: e.g., Meeting overload in weeks 3 and 4 reduced developer velocity.

---

## 4. Root Cause Analysis (The "5 Whys" Framework)
Use this section to drill down into the single largest issue or delay during the program lifecycle to find its systemic cause.

> **Problem Statement**: e.g., The pilot launch was delayed by one week.
> 1.  **Why?** The staging environment was unstable.
> 2.  **Why?** A database schema change broke API endpoint routes.
> 3.  **Why?** The database schema updates were run manually instead of using automated migration scripts.
> 4.  **Why?** The team didn't have a standard automated DB migration process configured for the new database type.
> 5.  **Why? (Root Cause)**: We rushed the planning phase and did not include database schema orchestration tooling in our initial engineering tasks.

---

## 5. Action Items
List concrete, actionable changes with owners and target dates to ensure we improve.

| # | Action Item | Priority (H/M/L) | Owner | Target Date | Jira/Ticket Link |
| :- | :--- | :---: | :--- | :--- | :--- |
| 1 | Integrate Liquibase/Flyway database migration tooling into CI/CD pipelines. | High | @TechLead | YYYY-MM-DD | [ENG-1029] |
| 2 | Add compliance checks as a required gate in the initial milestone planning template. | Medium | @TPMOwner | YYYY-MM-DD | [TPM-430] |
| 3 | Reduce weekly sync meetings from 3 to 1; use asynchronous slack status reports instead. | Low | @TPMOwner | YYYY-MM-DD | N/A |
