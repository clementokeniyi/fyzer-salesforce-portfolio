# FYZER Salesforce — User Stories & Acceptance Criteria

## Sales
**US-1 — Web lead routing.** As a Sales Manager, I want web leads to land with the sales team automatically so no enquiry waits for manual triage.
- AC: Web-to-Lead is enabled; a Web lead is assigned to the FYZER - Sales Leads queue by the active assignment rule; any other lead falls back to the named sales rep. *(Verified: Ava Novak → queue; Liam Osei → Daniel Brooks.)*

**US-2 — Guided qualification.** As a Sales Rep, I want a visual path with key fields per lead status so I qualify consistently.
- AC: FYZER Lead Qualification path is active on Lead Status with key fields and guidance text per stage; conversion uses "Closed - Converted" and lets the user pick the Account record type. *(Verified by converting Ava Novak into Vertex Analytics with contact + opportunity + primary contact role.)*

**US-3 — Pipeline visibility.** As a Sales Manager, I want pipeline by stage and by owner so I can coach and forecast.
- AC: FYZER Opportunity Path active; "FYZER Open Pipeline by Stage" report and Sales Dashboard render live data; Kanban shows all open deals. *(Verified: 4 opps, $300,000 pipeline at build; $225,000 open after two wins.)*

## Customer data integrity
**US-4 — SLA guardrail.** As an Admin, I want to prevent accounts being marked Active Customer without an SLA expiration date.
- AC: Validation rule blocks the save with a clear message; saving with a date succeeds. *(Verified both ways on Northstar Technologies.)*

**US-5 — Duplicate prevention.** As a Sales Rep, I want a warning when I create a lead that already exists.
- AC: FYZER matching rule (email exact + name/company fuzzy) and duplicate rule (Allow + Alert + Report) are active; creating a look-alike lead shows "Similar Records Exist" during entry and a potential-duplicate banner after save; a Duplicate Record Set is created. *(Verified with a second Liam Osei lead.)*

## Service
**US-6 — Case routing and escalation.** As a Support Manager, I want cases routed to the support queue and high-priority cases escalated if untouched for 4 hours.
- AC: FYZER Case Assignment rule active (Web + catch-all → queue); FYZER Case Escalation rule active with Age Over 4 hours action (reassign + notify owner with the FYZER template). *(Verified: cases 00001026/00001027 routed to queue; high-priority case met escalation criteria.)*

**US-7 — SLA milestones.** As a Support Agent, I want entitled customers' cases to show First Response and Resolution countdowns.
- AC: Entitlement process active with 240-minute First Response and 2880-minute Resolution milestones; new cases on entitled accounts auto-attach the entitlement and show a live milestone. *(Verified: case 00001030 auto-attached and counted down; a case on a non-entitled account saved cleanly with no milestones.)*

**US-8 — Urgency guardrail.** As a Support Manager, I want any case whose subject signals an outage to be High priority even if logged as Medium.
- AC: Before-save flow raises Priority to High on create when the subject contains "outage"/"down". *(Verified: case 00001029.)*

**US-9 — Closure audit.** As a Support Manager, I want an audit note when an entitled case closes.
- AC: Closing an entitled case posts "Case closed under entitlement SLA…" to the case feed, only on the transition into Closed. *(Verified on 00001030. Note: the milestone panel kept its countdown visible after close — documented platform behavior.)*

## Cross-functional automation
**US-10 — Win handoff.** As a CS Manager, when a deal is won I want the account advanced and onboarding started without manual chasing — and never twice.
- AC: On transition to Closed Won, the account moves to Active Customer (or Onboarding if no SLA date), an onboarding task is created, and a Chatter post records the event; on transition to Active Customer, CS welcome task, Support health-check case, and Finance invoice task are created. Editing either record afterwards creates NO duplicates. *(Verified on Meridian Logistics, including the edit/idempotency tests.)*

**US-11 — Fault visibility.** As an Admin, I want automation failures logged centrally instead of vanishing.
- AC: Every flow has fault paths to FYZER_Automation_Log__c; a forced fault produced exactly one log record with flow name and full error. *(Verified via the onboarding subflow debug run.)*

## Quote-to-Cash 🧪 (simulation)
**US-12 — Quote approval.** As a Sales Manager, I want quotes over $50,000 approved before they go to the customer.
- AC: Approval process active; a $98,500 quote entered approval, was approved with a comment, and its status was set to Approved by the process, then Accepted; a $12,000 quote was refused entry ("No applicable approval process was found"). Order and Contract activate at the quoted totals; a Finance billing-handoff task is created. *(Verified: quotes 00000001–00000003, Order 00000100, Contract 00000100.)*

## PSA 🧪 (simulation)
**US-13 — Project profitability.** As a Delivery Lead, I want billed, cost, profit, and margin per project from timesheets.
- AC: Timesheet formulas price hours at the resource's bill/cost rates; project shows totals matching hand calculation exactly (44h; $6,800 billed; $4,320 cost; $2,480 profit; 36.47% margin); utilization report groups hours by resource. *(Verified on the BluePeak implementation project.)*

## Integrations
**US-14 — API demo with error contract.** As an Admin, I want a working Named Credential callout and proof of how the REST API behaves on success and failure.
- AC: Flow HTTP Callout returns 200 and logs the mapped result; Workbench REST query/create/retrieve return 200/201/200; an invalid create returns 400 with errorCode INVALID_FIELD. *(Verified; responses recorded verbatim in the task log.)*

## AI
**US-15 — Grounded account brief.** As a Sales Rep, I want a one-click AI brief of an account that uses only our data.
- AC: "FYZER Account Summary" prompt template is active; preview on BluePeak Industries produced a factual 3-sentence brief from Name/Industry/Lifecycle with no invented data. Known limit: output is generic without richer grounding; the Agentforce demo agent cannot yet query CRM records (default template has no record actions) — documented, not hidden.
