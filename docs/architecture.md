# FYZER Salesforce — Architecture

## System context
```
                         ┌──────────────────────────────────────────────────────────────┐
   Website visitor       │                 FYZER Salesforce Org (DE)                    │
        │ Web-to-Lead    │                                                              │
        └───────────────▶│  SALES            SERVICE            DELIVERY 🧪            │
                         │  Lead → Account   Case → Queue       Project → Timesheet     │
   Customer email/web ──▶│       │             │  ▲                  │                  │
                         │       ▼             ▼  │ milestones       ▼                  │
                         │  Opportunity    Entitlement          Profit / Margin       │
                         │       │                                                      │
                         │       ▼ Closed Won                                           │
                         │  Quote 🧪 → Order 🧪 → Contract 🧪      CROSS-FUNCTIONAL     │
                         │       │                              Handoff flows:        │
                         │       └──────────────┬─────────────── CS task · Support      │
                         │                      │                 case · Finance task │
                         │  AUTOMATION LAYER: 9 Flows · assignment/escalation rules ·   │
                         │  approval process · duplicate rules · validation rule        │
                         │                                                              │
                         │  INSIGHT: 13 reports · 5 dashboards                          │
                         │  AI: Prompt Builder template · Agentforce agent (draft)      │
                         └──────┬───────────────────────────────────────▲──────────────┘
              Named Credential│ HTTP Callout                      │ REST API
                              ▼                                       │
                    jsonplaceholder.typicode.com            Workbench (admin tooling)
                              │
                     FYZER_Automation_Log__c ◀── every flow fault + integration result lands here
```

## Layer summary
| Layer | Contents | Key design decisions |
|---|---|---|
| Data | Standard objects (Lead, Account, Contact, Opportunity, Case, Quote🧪, Order🧪, Contract) + 5 custom objects (Automation Log; Project/Resource/Timesheet/Milestone 🧪) | Lifecycle stage on Account is the spine; fictional data only |
| Security | 14-role hierarchy, 6 permission sets in 5 groups, Private OWDs, 1 sharing rule, FLS via permission sets | Least privilege; profiles stay minimal; credential access via its own permission set |
| Automation | 9 flows, 2 assignment rules, 1 escalation rule, 1 approval process, 1 validation rule, matching + duplicate rules | Transition-only triggers, fault logging, subflow reuse, validation-rule-aware branching |
| Integration | Named/External Credential, External Service, Flow HTTP Callout, REST via Workbench | No secrets in config; retry standard documented (backoff on 429/5xx only, log terminal failures) |
| Analytics | 4 report folders, 5 dashboards by audience | Measures summarized at the report level so charts render |
| AI | Prompt template (active) + agent (draft) | Grounded prompts, human review, no autonomous writes |

## Record lifecycle (Account)
Prospect → Qualified → Onboarding → Active Customer → At Risk / Renewal / Churned
- Guard: Active Customer requires SLA Expiration Date (validation rule).
- Drivers: manual edits, the onboarding screen flow, and the Opportunity-Won flow (which routes to Onboarding when no SLA date exists so it can never break the guard).
- Side effects on entering Active Customer: CS welcome task, Support health-check case, Finance invoice task, Chatter audit post — exactly once (transition guard, edit-tested).

## Integration & error-handling pattern
Every automation fault and integration outcome converges on `FYZER_Automation_Log__c` (Flow Name, Related Record, Error Message). The Admin Dashboard surfaces it, making the log the single pane for "did anything break?"
