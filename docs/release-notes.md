# FYZER Salesforce — Release Notes

## Release 1.0 — 2026-10-20 (Developer Edition portfolio build)

### Sales Cloud
- Lead capture: Web-to-Lead enabled, FYZER - Sales Leads queue, FYZER Lead Assignment rule active (Web → queue; catch-all → Sales Rep). Default Lead Creator set to the org owner.
- Lead qualification: FYZER Lead Qualification path on Lead Status; conversion verified end to end (account record-type selection, contact, opportunity, primary contact role).
- Pipeline: FYZER Opportunity Path; products FYZER Platform Professional/Enterprise, Implementation Services, Premium Support; Standard Price Book (activated) plus a 10%-discounted FYZER - Enterprise Price Book.
- Data: 9 accounts, 9 contacts, 6 opportunities incl. 2 Closed Won ($85,000 won; $225,000 open pipeline).

### Service Cloud
- FYZER Service Console app; FYZER - Support Queue; FYZER Case Assignment rule active.
- FYZER Case Escalation rule active (High priority, not closed → 4-hour reassign + owner notification) with the FYZER Case Escalated Notification template.
- FYZER Case Resolution path; FYZER Case Record Page with the Path component (org + Service Console default).
- Entitlements: FYZER Standard Support process (First Response 4h, Resolution 2 business days); BluePeak entitlement active through 12/31/2027.
- Bug fix (root cause): Case Entitlement field was hidden for every profile (FLS) — set Visible for System Administrator; field now renders on the Case layout.

### Automation (Flow)
- FYZER_Customer_Onboarding screen flow + FYZER_Create_Onboarding_Task subflow + "Start Onboarding" account action.
- FYZER_Case_Priority_Guard (before-save urgency guard) · FYZER_Case_Auto_Entitlement (after-save, create-only, never overwrites) · FYZER_Case_Close_SLA_Note (close-transition Chatter audit).
- FYZER_Opportunity_Won_Handler + FYZER_Account_Active_Customer_Handoff — cross-functional handoffs with transition-only guards; idempotency proven by edit tests (zero duplicates).
- FYZER_Automation_Log__c central logging; live fault test produced exactly one log record.
- FYZER_Update_Project_Totals (PSA totals) · FYZER_API_Demo_Callout (saved inactive, debug-verified).

### Simulations (clearly labeled — not licensed products)
- Quote-to-Cash 🧪: quotes with line discounts, FYZER Quote Approval (> $50,000; positive and negative paths tested), Order 00000100 and Contract 00000100 activated, Finance billing-handoff task.
- PSA 🧪: 4 custom objects (Project, Project Resource, Timesheet Entry, Project Milestone) with rate-based formulas; BluePeak project totals verified exactly (44h, $6,800 billed, $4,320 cost, $2,480 profit, 36.47% margin); profitability + utilization reports.

### Integrations & Data Quality
- Named Credential FYZER_Demo_API (External Credential model + principal + access permission set); Flow HTTP Callout verified (HTTP 200, mapped response logged).
- Workbench REST evidence: query 200, create 201, retrieve 200, invalid create 400 `INVALID_FIELD`.
- FYZER Lead Matching Rule + FYZER Lead Duplicate Rule active (warn-and-report); live duplicate test fired the alert during entry and created Duplicate Record Set DRS-0000000001.

### Analytics
- Reports: pipeline, leads by source, opportunities by owner, won revenue, accounts by lifecycle, cases by status/priority/origin/account, automation log by flow, PSA profitability + utilization.
- Dashboards: Sales, Service, Executive, Customer Success, Admin — all verified rendering with data.

### AI
- Einstein Generative AI and Agentforce confirmed ON; no credit/license walls encountered.
- Prompt template FYZER Account Summary (Field Generation) activated; live grounded preview verified.
- FYZER Demo Agent created from the standard template (Draft); documented limitation — default template has no CRM record actions.

### Known limitations carried forward
- Salesforce Knowledge awaits one manual enable click by the org owner (button unresponsive to automation).
- Omni-Channel is enabled org-wide; no routing was built (out of scope).
- PSA money totals don't recalculate on timesheet deletion (see Governance TD-1).
- Milestone panel countdown may keep displaying after case close (documented platform behavior).
- Duplicate test data intentionally retained: unlinked "NoSLA Test Corp" account; duplicate Liam Osei lead (evidence for duplicate management).
