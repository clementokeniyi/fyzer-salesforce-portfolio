# FYZER Salesforce — Technical Design Document

Version 1.0 · 2026-10-20

## 1. Architecture overview
Single Developer Edition org, Lightning Experience, configuration-first (no Apex in this portfolio). Standard objects carry the core CRM; four custom objects carry the PSA simulation; one custom object centralizes automation/integration logging.

```
                        ┌──────────────────────────── FYZER ORG ────────────────────────────┐
  Web visitor ──Web-to-Lead──▶ Lead ──convert──▶ Account ──▶ Opportunity ──▶ Quote 🧪 ──▶ Order 🧪 ──▶ Contract 🧪
                                                    │             │               ▲ approval > $50k
   Customer ──email/web──▶ Case ──▶ Support Queue  │             │ Closed Won    │
                            │  ▲ entitlement       │             ▼               │
                            │  └─ milestones       │      Handoff flows ──▶ CS Task / Support Case / Finance Task
                            ▼                      │
                     Escalation (4h)               └──▶ FYZER Project 🧪 ──▶ Resources / Timesheets / Milestones
  External API ◀──Named Credential── Flow HTTP Callout        │
  Workbench    ──REST API──▶ FYZER_Automation_Log__c ◀── fault paths from every flow
  Einstein AI  ──Prompt Builder──▶ Account brief (FYZER Account Summary) · Agentforce agent (draft)
                        └──────────────────────────────────────────────────────────────────────┘
```

## 2. Security design
- Private OWDs on Lead/Account/Opportunity/Case/Contract; role hierarchy grants management visibility; one sharing rule opens sales-owned accounts Read Only to the CS Manager role.
- Access is composed: job-role permission set groups bundle focused permission sets (Sales, Sales Manager, CS, Support, Support Manager, Finance). Profiles stay near Minimum Access; the System Administrator profile is reserved for the org owner. FLS corrections are delivered through permission sets where the audience is a job role (account lifecycle/SLA fields), and directly on a profile only where the audience is the administrator (Case Entitlement field fix).
- Credentials: the demo Named Credential uses the External Credential model with a Named Principal; a dedicated permission set grants principal access — no credentials are stored in flows, code, or documents.

## 3. Automation design principles
1. **Transition-only triggers.** Every after-save flow enters on a *change into* a state (prior value comparison) — the standard defense against duplicate side effects on later edits. Idempotency was proven by edit tests on both Opportunity and Account.
2. **Recursion guards by construction.** Handoff flows never update their own triggering record; the auto-entitlement flow is create-only with an "Entitlement is empty" entry condition, so it cannot loop or overwrite a manual selection.
3. **Validation-rule awareness.** The Opportunity-Won flow branches on the account's SLA date so it can never violate FYZER_Active_Customer_Requires_SLA — it routes to Onboarding instead.
4. **Fault paths everywhere they're possible.** After-save/screen/autolaunched flows route faults to FYZER_Automation_Log__c with flow name, record ID, and `$Flow.FaultMessage`. Before-save flows cannot create records (platform limit); the one before-save flow is a pure field update with no fault surface. Subflow elements expose no fault connector — mitigated by the subflow's own internal fault path.
5. **Reusable subflows.** Onboarding task creation exists once (FYZER_Create_Onboarding_Task) and is called by both the screen flow and the won-handler.

## 4. Data design
- Lifecycle as the spine: `Customer_Lifecycle_Stage__c` drives onboarding, handoffs, segments in reports, and AI grounding.
- PSA math: per-entry formula fields price hours at resource rates (billable-aware); project totals use a true roll-up for hours and a totals flow for money — because roll-up summaries cannot aggregate formula fields containing cross-object references (platform limit, see Governance).
- Simulation boundary: Quote/Order/Contract are standard objects used with "(SIMULATION)" labeling; PSA uses custom objects whose descriptions state "Not Certinia."

## 5. Integration design
- Pattern: Named Credential + External Service + Flow HTTP Callout for declarative outbound calls; response status and a mapped payload field are written to the Automation Log for every demo call.
- REST evidence (Workbench): SOQL query (200), create (201), retrieve (200), invalid create (400, `INVALID_FIELD`).
- **Retry design (documented standard for consumers of this pattern):** retry only transient failures — HTTP 429, 5xx, and timeouts — with exponential backoff (1s → 2s → 4s, max 3 attempts, plus jitter). Never retry 4xx client errors (e.g., `INVALID_FIELD`); fix the payload instead. POST creates must carry an idempotency key in production. Every terminal failure writes one Automation Log record with source name, related record, and the full error body for triage; alerting keys off new log records.
- Sample data only; the demo endpoint is a public test API carrying no personal or business data.

## 6. Reporting design
Reports live in four FYZER folders; five dashboards aggregate them by audience (Sales, Service, Executive, CS, Admin). Measures that dashboards chart on are explicitly summarized in their source reports (a chart cannot consume an unsummarized column).

## 7. AI design
- Prompt template "FYZER Account Summary" (Field Generation, Account → Description) is grounded on three account fields and instructed to use only provided facts; preview-verified output was factual but generic — richer grounding (open cases, SLA status, renewal date via additional merge resources) is the documented next iteration.
- Agentforce: "FYZER Demo Agent" from the standard Employee Agent template, left in Draft. Capability finding: default subagents have only a knowledge-search tool, so CRM questions fail honestly ("I do not have direct access…"). Production design: add actions (invoke the prompt template and/or a Get-Records flow action), ground with the Data Library, keep a human in the loop for any write action, and review conversations in Agent Analytics before channel deployment.
- Governance: AI outputs are drafts for human review; no AI component writes records autonomously in this design; prompts forbid inventing facts.

## 8. Deployment design
See `release-notes.md` for the change inventory and `governance.md` for rollback and technical-debt positions. Source representation lives in the SFDX project (`sfdx/`) with a manifest; production-style validation is `sf project deploy start --dry-run --manifest manifest/package.xml` against an authorized org (documented; CLI authorization is an owner action).
