# FYZER Salesforce Portfolio — Evidence Package

A complete Salesforce Administrator portfolio built in a Developer Edition org for **FYZER**, a fictional B2B software company: Sales Cloud, Service Cloud, Flow automation, integrations, data quality, analytics, and Agentforce AI — every item tested against live org evidence, with simulations labeled and limitations documented honestly.

**How this package is organized**
- `screenshots/` — 30 numbered screenshots, one per build-checklist item, in checklist order.
- `documentation/` — BRD, technical design, architecture, SOP, training guide, AI governance note, data dictionary.
- `reports/` — final QA results, deployment validation, known issues, testing evidence, completion checklist, screenshot mapping.

**Privacy:** screenshots are captured from a fictional demo org; the Salesforce Org ID is redacted in screenshot 01, and no credentials, tokens, or personal email addresses appear anywhere in this package.

## Screenshot index

| # | File | What it shows | Business value |
|---|------|---------------|----------------|
| 01 | 01-org-setup.png | Company Information for the FYZER Developer Edition org (Org ID redacted). | Proves the portfolio lives in a real, dedicated Salesforce org with correct locale/timezone settings. |
| 02 | 02-role-hierarchy.png | The 14-role FYZER hierarchy (CEO → CRO/CTO/CS/Support/Finance/Marketing/Product, down to Sales Rep, CSM, Support Agent). | Role hierarchy mirrors the company's org chart, so record visibility follows real reporting lines. |
| 03 | 03-permission-sets.png | The FYZER permission sets list, including the Demo API Access set. | Access is granted by composable permission sets, not profile edits — least-privilege and easy to audit. |
| 04 | 04-permission-set-groups.png | The five FYZER permission set groups (Sales Representative, Sales Manager, Customer Success, Support Agent, Support Manager). | Job-role bundles: onboarding a hire is one group assignment instead of six separate grants. |
| 05 | 05-owd-sharing-rules.png | Sharing Settings: Private OWDs for Lead/Account/Opportunity/Case/Contract plus the Sales→Customer Success account sharing rule. | Data is private by default and opened deliberately — the core of the FYZER security model. |
| 06 | 06-least-privilege-test-users.png | Users list with the least-privilege test users (Sales Rep, CSM) on the Minimum Access profile. | Shows the named test users and their Minimum Access profiles; it does not independently prove record-level visibility. |
| 07 | 07-lifecycle-stage-field.png | Account fields including Customer Lifecycle Stage (Prospect → Qualified → Onboarding → Active Customer → At Risk → Renewal → Churned). | One field drives the whole customer journey — reporting, automation, and CS handoffs key off it. |
| 08 | 08-account-security-validation.png | Account validation rules, incl. FYZER Active Customer Requires SLA (active). | Data quality is enforced by the platform: an account cannot become an Active Customer without an SLA date. |
| 09 | 09-record-types-lightning-page.png | Composite: Account record types (Prospect / Customer) + the FYZER Account Management Lightning page on BluePeak Industries. | Different account kinds get tailored pages and fields instead of one cluttered layout for everyone. |
| 10 | 10-lead-assignment.png | FYZER Lead Assignment rules — Web leads route to the FYZER - Sales Leads queue; catch-all assigns to the sales rep. | No web lead ever sits unowned: every inquiry lands with a team or a named rep automatically. |
| 11 | 11-lead-conversion.png | Converted lead Ava Novak, linked to the Vertex Analytics account, contact, and opportunity it created. | Lead conversion is demonstrated end to end, producing clean account/contact/opportunity data. |
| 12 | 12-accounts-contacts-data.png | Vertex Analytics account with its related contact and opportunity data. | The demo dataset is coherent across objects — the output of real conversion, not hand-typed rows. |
| 13 | 13-opportunity-pipeline.png | Opportunity pipeline view: $225,000 open (Qualification $105k, Proposal $120k) with the FYZER Opportunity Path. | Sales leadership sees stage-by-stage pipeline with guided selling built in. |
| 14 | 14-products-price-books.png | FYZER - Enterprise Price Book (10% below standard: $15k/$45k/$12k/$8.5k standard products). | Pricing governance: reps quote from controlled price books, with an enterprise tier pre-approved. |
| 15 | 15-sales-dashboard.png | FYZER Sales Dashboard (open pipeline, won revenue, stage mix). | One-screen sales performance for the team, fed by live opportunity data. |
| 16 | 16-service-console.png | A support case open in the Service Console experience on the FYZER Case Record Page (Path + milestones). | Agents work queues, cases, and SLAs from a single console instead of tab-hopping. |
| 17 | 17-case-routing-escalation.png | Composite: FYZER Case Assignment rules (all cases → Support Queue) + Case Escalation rules (High priority, 4-hour age trigger). | Every case is routed instantly, and aging high-priority cases escalate themselves before SLAs burn. |
| 18 | 18-entitlement-sla.png | Open entitled case with the live milestone panel, showing an overdue First Response milestone. The case-closure completion defect is separately documented. | Support commitments are tracked on the record; see reports/known-issues.md (KI-1) for the milestone-completion platform limitation. |
| 19 | 19-service-dashboard.png | FYZER Service Dashboard (case volume, status, priority). | Support leadership monitors load and backlog at a glance. (Knowledge was manually enabled according to the org owner; verify the setting and publish articles separately.) |
| 20 | 20-record-triggered-flow.png | FYZER Case Auto-Entitlement flow in Flow Builder (after-save; attaches the matching entitlement on case creation). | SLA coverage is automatic — no agent has to remember to attach an entitlement. |
| 21 | 21-customer-onboarding-screen-flow.png | FYZER Customer Onboarding screen flow in Flow Builder, launched from the account "Start Onboarding" action. | A guided, consistent onboarding checklist replaces ad-hoc manual steps. |
| 22 | 22-cross-functional-automation.png | Meridian Logistics account after the won-deal handoff: Active Customer with CS/Support/Finance follow-up work attached. | Winning a deal automatically mobilizes three departments — no handoff email ever gets forgotten. |
| 23 | 23-quote-to-cash.png | Quote 00000003 ($98,500, Accepted) with its Approval History — Quote-to-Cash **simulation** on standard objects. | Deals above $50k cannot be quoted without recorded management approval. (Simulation — not Salesforce CPQ.) |
| 24 | 24-psa-profitability.png | PSA **simulation**: BluePeak project — 44h, billed $6,800, cost $4,320, profit $2,480, margin 36.47%, with resources/timesheets. | Delivery profitability is visible per project, computed automatically from timesheets. (Simulation — not Certinia.) |
| 25 | 25-rest-api-integration.png | Composite: FYZER Demo API Named Credential + the Automation Log record evidencing the REST/HTTP calls (200/201). | Integrations authenticate through Named Credentials (no hard-coded secrets) and every call outcome is logged. |
| 26 | 26-duplicate-detection.png | Composite: duplicate alert on the second Liam Osei lead + the FYZER Lead Duplicate Rule in Setup. | Dirty data is stopped at the door: matching on email + fuzzy name/company, warn-and-report. |
| 27 | 27-data-dictionary.png | Rendered FYZER data dictionary (objects, fields, types, purpose). | Anyone can understand the data model without opening Setup — documentation as a deliverable. (Local document render.) |
| 28 | 28-executive-dashboard.png | FYZER Executive Dashboard, incl. Won Revenue $85,000. | Leadership gets revenue and pipeline truth in one view. |
| 29 | 29-agentforce-ai.png | Prompt Builder with the active FYZER Account Summary template (Einstein/Agentforce). | Shows the configured, active grounded prompt template; the generated output is documented in the AI governance note, not visible in this screenshot. |
| 30 | 30-devops-documentation.png | Composite local render: git history (9 commits, branch + merge), CLI retrieve + check-only deployment validation results, portfolio README preview. | The work is version-controlled and its deployment was validated before anyone trusted it. (Local repository evidence.) |

## Verification status (2026-10-20)
- All 30 checklist items were built and evidence-tested in the org (see `reports/testing-evidence.md`).
- Metadata was retrieved with the Salesforce CLI and passed check-only deployment validation: **65/65 components, 0 errors** (`reports/deployment-validation.md`).
- Known limitations are documented, not hidden (`reports/known-issues.md`).

## Final evidence review notes

- **#06:** User administration screenshot only; an actual restricted-record view requires a separate visual capture. Earlier live access testing was reported, but is not visually demonstrated here.
- **#18:** Milestone panel accurately shows an overdue SLA. Milestones failing to complete after case closure remain an open issue (KI-1); no successful closure is claimed.
- **#29:** Image shows Prompt Builder configuration, not the actual generated response. The AI preview result is described in the AI governance documentation.
- **Knowledge:** The project owner reports manually enabling Lightning Knowledge on 2026-10-20; Setup confirmation should be captured when feasible.

The screenshots document visible configurations, not an independent re-execution of every functional test.
