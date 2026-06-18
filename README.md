# Language

English

# Wedding Control Board: Make Wedding Planning Decisions Before Small Errors Become Expensive

![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)
![Platform](https://img.shields.io/badge/Platform-Excel%20%7C%20Single--File%20HTML-051C2C)
![Tool Type](https://img.shields.io/badge/Tool%20Type-Decision%20Support%20Workbook-2251FF)

**A lightweight wedding planning control board that connects tasks, vendors, guests, budgets, and decision notes so planning risks are visible before they become last-minute costs.**

[Live Demo](https://hyvoid.github.io/wedding-planner/) · [Download Workbook](https://github.com/HyVoid/wedding-planner/blob/main/Wedding_Control_Board.xlsx) 

![Wedding Control Board hero image placeholder](./docs/hero-placeholder.png)

## What Decision Does This Help You Make?

1. Which planning tasks require attention this week because they are overdue or blocked?
2. Which vendor commitments are creating unpaid balance exposure before the wedding date?
3. Whether the current guest count still fits the budget and table planning assumptions.
4. Which verbal decisions need to be linked back to a task or vendor before they are forgotten.

## About The Builder

This project follows a strict operating philosophy.

Decision support over system replacement. Productized reasoning over feature accumulation. Low-friction execution over implementation complexity.

The workbook does not try to replace a planner, a CRM, a budgeting app, or a venue management system. It does one job: keep the planning facts connected so the next decision is based on the current state of the wedding, not on memory, chat history, or scattered spreadsheet tabs.

No enterprise setup. No database dependency. No onboarding ceremony. The Excel workbook works as a structured planning file. The HTML version works as a standalone browser app with local autosave, JSON backup export, JSON import, and reset.

## Who This Is For

| User | Typical Situation | Decision Supported | Why Existing Methods Fail |
|---|---|---|---|
| Couple planning without a full-service planner | Tasks, vendor payments, and guest replies are spread across chats and notes | What needs attention this week | Messaging apps preserve conversation, not decision state |
| Planner managing a lightweight wedding | The client needs a simple source of truth without project-management overhead | Which milestone, payment, or RSVP issue should be escalated | Generic dashboards show status but rarely connect tasks to payment and guest assumptions |
| Family member coordinating guest execution | RSVP counts and table assignments change frequently | Whether attendance assumptions are still reliable | Static guest lists do not surface the planning impact of uncertain replies |
| Budget-conscious buyer | Vendor contracts are signed at different times with deposits and balances | Whether actual commitments are still inside planned budget | A total budget number hides category-level overrun risk |

## Why Most Wedding Planning Errors Aren't Judgment Errors

Most wedding planning failures are not caused by poor judgment. They are caused by broken information flow.

An intelligent person can still make a bad planning decision if the number underneath the decision is stale. A guest count may exclude tentative replies. A budget may include planned spend but not signed vendor contracts. A task may look harmless until its due date passes and blocks three downstream actions.

The failure usually looks like this:

```text
Chat message -> memory -> manual update later -> partial dashboard -> decision
```

The corrected workflow is:

```text
Task / vendor / guest / budget input -> linked calculation -> dashboard signal -> decision
```

| Before | After |
|---|---|
| "I think most vendors are paid." | Deposit paid, final balance, and payment status are visible by vendor. |
| "Guest count should be around 80." | Confirmed, tentative, declined, and not contacted counts are separated. |
| "We are mostly on schedule." | Overdue tasks are counted and flagged against current date. |

The operational implication is simple. Verification reduces the number of decisions made from memory. Memory is not auditable. A linked workbook is.

## Three Traps That Catch Even Experienced Planners

### Trap 1: Treating Planned Budget As Actual Exposure

| Sequence | What Happened |
|---|---|
| A decision was made | The couple approved another floral upgrade because the planned budget still showed room. |
| Faulty number | Actual vendor contracts were not grouped back into budget categories. |
| Changed recommendation | The upgrade looked acceptable before vendor commitments were included; it became an over-budget decision after actual contracts were mapped. |
| Why incorrect | A plan is not exposure. Exposure starts when contracts are signed. |
| Corrected approach | Group `Vendor_Tracker[Contract_Amt]` into `Budget_Manager[Actual_Cost]` by category mapping. |
| Corrected outcome | The upgrade is delayed until a lower-priority category is reduced or approved as an explicit overrun. |

```text
Wrong workflow:
Plan_Budget -> remaining room -> approve upgrade

Correct workflow:
Vendor contracts -> mapped actual cost -> variance -> approve / defer / cut
```

<details>
<summary>Formula logic</summary>

```excel
Actual_Cost =
SUMPRODUCT(
  (XLOOKUP(tbl_Vendor_Tracker[Category],
           tbl_Config_Category_Map[Vendor_Category],
           tbl_Config_Category_Map[Budget_Category],
           "NO MATCH")=[@Category])
  * tbl_Vendor_Tracker[Contract_Amt]
)

Variance = Plan_Budget - Actual_Cost
Status = IF(Variance < 0, "OVER BUDGET", "ON TRACK")
```

</details>

### Trap 2: Counting Invited Guests As Confirmed Attendance

| Sequence | What Happened |
|---|---|
| A decision was made | Table count was locked using the total invited guest list. |
| Faulty number | Tentative and not-contacted guests were treated as reliable attendance. |
| Changed recommendation | The venue layout looked stable at 90 invited guests but unstable after separating confirmed and tentative responses. |
| Why incorrect | Invitation volume is not attendance demand. It is a pipeline. |
| Corrected approach | Split RSVP status into confirmed, tentative, declined, and not contacted. Use confirmed guests for commitments and tentative guests for risk review. |
| Corrected outcome | The table plan remains flexible until tentative guests convert or decline. |

```text
Wrong workflow:
Invited guests -> table count -> layout locked

Correct workflow:
Confirmed guests + tentative buffer -> table range -> layout held open
```

<details>
<summary>Formula logic</summary>

```excel
Confirmed Guests =
SUMIFS(tbl_Guest_RSVP[Act_Guests],
       tbl_Guest_RSVP[RSVP_Status],
       "Confirmed")

Tentative Guests =
SUMIFS(tbl_Guest_RSVP[Est_Guests],
       tbl_Guest_RSVP[RSVP_Status],
       "Tentative")
```

</details>

### Trap 3: Treating Completed Tasks As The Same Thing As Unblocked Execution

| Sequence | What Happened |
|---|---|
| A decision was made | The week was considered under control because completion rate looked acceptable. |
| Faulty metric | The planner looked only at percentage complete and ignored overdue open tasks. |
| Changed recommendation | A 75% completion rate looked healthy, but one overdue guest execution task blocked invitation follow-up. |
| Why incorrect | Completion percentage is a lagging metric. Overdue open work is an action signal. |
| Corrected approach | Use completion rate and overdue count together. Sort open tasks by due date. |
| Corrected outcome | The next meeting focuses on blocked tasks, not general progress. |

```text
Wrong workflow:
Completion rate -> confidence

Correct workflow:
Completion rate + overdue count + next due tasks -> action list
```

<details>
<summary>Formula logic</summary>

```excel
Completion Rate =
IF(COUNTA(tbl_Timeline_Tasks[Task_ID])>0,
   COUNTIF(tbl_Timeline_Tasks[Status],"Completed")
   / COUNTA(tbl_Timeline_Tasks[Task_ID]),
   0)

Upcoming Task Feed =
LET(
  mask,
  (tbl_Timeline_Tasks[Task_ID]<>"")
  * (tbl_Timeline_Tasks[Status]<>"Completed"),
  open,
  FILTER(tbl_Timeline_Tasks[[Task_ID]:[Due_Date]], mask),
  dates,
  FILTER(tbl_Timeline_Tasks[Due_Date], mask),
  IFERROR(TAKE(SORTBY(open, dates, 1), 8), "No open tasks")
)
```

</details>

## Example Scenario

A couple has five weeks left before the wedding. The planning file contains four active tasks, three vendors, four guest records, and five budget categories.

Raw inputs:

| Input | Value |
|---|---:|
| Planned total budget | ¥450,000 |
| Banquet venue contract | ¥180,000 |
| Photo / video contract | ¥52,000 |
| Floral design contract | ¥28,000 |
| Deposits paid | ¥65,000 |
| Confirmed guests | 2 |
| Tentative guests | 1 |
| Overdue tasks | 1 |

Intermediate calculations:

| Calculation | Result |
|---|---:|
| Actual committed cost | ¥260,000 |
| Final unpaid balance | ¥195,000 |
| Budget used | 57.8% |
| Task completion rate | 25.0% |

The raw budget still looks safe. That is not the decision. The decision is whether the current planning state is controlled enough to approve additional spending or lock operational commitments.

The workbook interpretation is more conservative. One task is overdue. Final balances remain large. RSVP volume is too small to lock attendance assumptions. The recommendation is to delay optional upgrades, clear overdue guest execution work, and reconcile vendor balances before approving any nonessential add-ons.

The decision implication is practical. No panic. No speculation. The next action is not "review everything." It is "clear the overdue task, confirm RSVP conversion, then revisit discretionary spend."

## What The Workbook Actually Delivers (Not How It Calculates)

- A single control board for wedding planning decisions.
- A task tracker that separates progress from overdue risk.
- A vendor tracker that exposes deposits, final balances, payment status, and contract archive status.
- A guest tracker that separates confirmed attendance from tentative demand.
- A budget manager that compares planned budget with signed vendor exposure.
- A decision log for calls, family meetings, site visits, and verbal commitments.
- A standalone HTML version with local autosave, backup export, backup import, and reset.
- A reusable operating rhythm for weekly planning review.

## Workbook Logic (For the Skeptical Reviewer)

The workbook uses a layered architecture.

| Layer | Sheet / View | Role |
|---|---|---|
| Setup | `SETUP_Dictionary` | Dropdown lists and category mapping |
| Data | `Timeline_Tasks` | Task source table |
| Data | `Vendor_Tracker` | Vendor and payment source table |
| Data | `Guest_RSVP` | Guest and RSVP source table |
| Data / Calc | `Budget_Manager` | Budget inputs plus calculated actuals |
| Data | `Decision_Notes` | Decision audit trail |
| Calculation | `CALC_Summary` | Dashboard metric dependencies |
| Presentation | `Dashboard` | KPI and decision view |

Data flows from operational tables into calculation logic, then into the dashboard. The HTML version follows the same logic in front-end JavaScript. Inputs update state. State triggers recalculation. Recalculation updates visible KPIs, data bars, badges, and anomaly rows.

Validation flow is intentionally simple. Dropdowns restrict recurring categories. Date fields drive overdue checks. Numeric fields drive budget, payment, and guest calculations. Status fields control whether a row is normal, complete, or action-required.

Output dependencies are explicit. Dashboard completion rate depends on task status. Budget exposure depends on vendor contract values and category mapping. RSVP shape depends on guest status and headcount fields. Decision notes do not inflate metrics; they preserve the audit trail.

## Implementation Notes (For Those Who Want to Look Under the Hood)

<details>
<summary>Core workbook formulas</summary>

```excel
Final_Balance = Contract_Amt - Deposit_Paid

Variance = Plan_Budget - Actual_Cost

Completion Rate =
IF(COUNTA(tbl_Timeline_Tasks[Task_ID])>0,
   COUNTIF(tbl_Timeline_Tasks[Status],"Completed")
   / COUNTA(tbl_Timeline_Tasks[Task_ID]),
   0)
```

</details>

<details>
<summary>Validation rules</summary>

```text
Task status:
Not Started | In Progress | Pending Confirmation | Completed

RSVP status:
Confirmed | Declined | Tentative | Not Contacted

Payment status:
Unpaid | Deposit Paid | Paid in Full
```

The purpose is not to make data entry restrictive. The purpose is to make aggregation reliable.

</details>

<details>
<summary>HTML implementation details</summary>

```text
Single file.
No external dependencies.
No backend.
No account system.

Browser storage:
localStorage key = wedding-control-board-v1

Backup:
Export JSON -> Import JSON -> Reset sample data
```

This is suitable for individual use, lightweight product demos, and low-friction sharing. It is not a multi-user database.

</details>

## Get The Workbook

Download the Excel workbook or open the standalone HTML app.

Use the Excel workbook for spreadsheet-native workflows. Use the HTML version for a browser-based single-file demo with local autosave and portable JSON backups.

## Limitations

This tool is not an enterprise planning system, accounting ledger, contract management platform, or legal record. It does not replace professional wedding planning judgment. It does not synchronize data across devices unless the user exports and imports a backup. Browser storage can be cleared by the user or the browser. Budget categories and vendor mappings should be reviewed before real use. Guest counts, vendor terms, taxes, deposits, cancellation policies, and venue constraints must be validated independently.

The workbook supports better decisions. It does not guarantee better outcomes without disciplined updates.

## Other Decision Tools You Might Need

- Vendor comparison scorecard for quote evaluation.
- Seating plan optimizer for table assignment constraints.
- Cash flow forecast for wedding payment timing.
- Contract obligation checklist for cancellation, deposit, and deliverable review.
- Post-event budget variance report.

## License

This project is licensed under the Apache License 2.0.

See the `LICENSE` file for the full Apache License 2.0 terms. If no license file is included in a distribution package, refer to the official Apache License 2.0 text published by the Apache Software Foundation.
