# FYZER Salesforce — Business Requirements Document (BRD)

Version 1.0 · 2026-10-20 · Org: fyzer.co Developer Edition (portfolio implementation)

## 1. Purpose
Build a complete, working Salesforce Administrator portfolio inside a single Developer Edition org that demonstrates Sales Cloud, Service Cloud, automation, integrations, data quality, analytics, and AI — using fictional data, zero budget, and clearly labeled simulations where a licensed product (CPQ, Certinia) would normally be required.

## 2. Business context
FYZER is a fictional B2B software company selling a subscription platform (Professional and Enterprise editions), implementation services, and premium support. The org models its revenue and service motions: marketing-captured leads, a sales pipeline, customer onboarding, support with SLAs, project delivery, and finance handoffs.

## 3. Scope
**In scope:** org security model (roles, permission sets/groups, sharing), lead capture and conversion, pipeline management, products and price books, reporting and dashboards, Service Console, case routing and escalation, entitlements and milestones, Flow automation (record-triggered, screen, subflow), cross-functional handoffs (Sales → CS → Support → Finance), quote-to-cash and PSA *simulations* on standard/custom objects, a REST API demo with error logging, duplicate management, Agentforce/Prompt Builder exploration.

**Out of scope:** any paid license or feature; hosting the Web-to-Lead form; Salesforce Knowledge articles (Knowledge articles are out of scope; the owner reports manually enabling Knowledge); Omni-Channel routing configuration; real customer data of any kind.

## 4. Functional requirements (summary)
| # | Requirement | Delivered as |
|---|---|---|
| FR-1 | Least-privilege access aligned to job roles | 6 permission sets composed into 5 permission set groups; Private OWDs; one targeted sharing rule |
| FR-2 | Leads from the web route to the sales team automatically | Web-to-Lead + FYZER - Sales Leads queue + active assignment rule |
| FR-3 | Reps follow a consistent qualification and sales process | Lead and Opportunity Paths with key fields and guidance |
| FR-4 | A customer cannot be "Active" without an SLA commitment date | Validation rule FYZER_Active_Customer_Requires_SLA |
| FR-5 | Customers are onboarded consistently | Screen Flow "FYZER - Customer Onboarding" + reusable subflow |
| FR-6 | Cases route to Support, and urgent cases escalate | Support queue, assignment rule, 4-hour escalation rule with email notification |
| FR-7 | Entitled customers' cases carry SLA milestones | Entitlement process (First Response 4h, Resolution 2 business days) + auto-entitlement flow |
| FR-8 | Case urgency is protected even when mis-triaged | Before-save Priority Guard flow |
| FR-9 | Winning a deal triggers onboarding and cross-functional handoffs without duplicates | Opportunity-Won and Active-Customer flows with transition-only entry conditions (idempotency tested) |
| FR-10 | Quotes above a threshold require approval before acceptance | Approval process over $50,000 |
| FR-11 | Delivery work is tracked with budgets and profitability | PSA simulation objects with bill/cost rates, rollups, profit/margin formulas |
| FR-12 | Integrations fail loudly and leave evidence | FYZER Automation Log + fault paths on every flow + REST error-contract test |
| FR-13 | Duplicate leads are caught at entry | FYZER matching + duplicate rules (warn-and-report) |
| FR-14 | Leadership sees pipeline, service health, and automation health | 5 dashboards (Sales, Service, Executive, CS, Admin) |
| FR-15 | AI assists must be grounded and honest about limits | Prompt template grounded on account fields; agent capability gap documented verbatim |

## 5. Non-functional requirements
- **Cost:** $0 — Developer Edition and included features only.
- **Safety:** no destructive changes without owner approval; existing org configuration preserved unless a change is necessary and documented.
- **Evidence:** no item is "done" without Salesforce evidence (records, run results, approval trails) recorded in the task log.
- **Honesty in labeling:** simulations are labeled "(SIMULATION)" in record names and object descriptions; limitations are documented, never hidden.
- **Maintainability:** FYZER- naming prefix, flow descriptions explaining triggers/recursion guards, centralized error logging.

## 6. Constraints and known limitations
Developer Edition limits apply throughout. Specific platform behaviors discovered during the build (Case Entitlement field FLS hidden org-wide; roll-ups unable to aggregate cross-object formulas; before-save flows unable to create records; milestone panel countdown continuing after case close; Knowledge enable button unresponsive to automation) are catalogued in the task log and governance document.

## 7. Success criteria
All 30 checklist items verified with evidence; end-to-end tests passing for lead routing, conversion, entitlement assignment (positive and negative), closure behavior, fault logging, cross-functional handoff with duplicate-prevention, quote approval (positive and negative), PSA totals, REST calls (success and error), and duplicate detection.
