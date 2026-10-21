# FYZER Salesforce Portfolio — 30-Item Checklist

Last updated: 2026-10-19 (corrections phase running)

Legend: ✅ Done (tested) · 🔄 In progress · ⏳ Not started · ⚠️ Blocked / needs attention · 🧪 Simulation

## Phase 0 — Foundation (Items 1–9, verified by read-only audit 2026-10-19)

| # | Item | Status | Notes |
|---|------|--------|-------|
| 1 | Org setup: fyzer.co DE, timezone, company info | ✅ | Pacific TZ verified; My Domain orgfarm-1f5ecc6632-dev-ed (not fyzer-related, cosmetic) |
| 2 | Executive + operational role hierarchy (14 roles) | ✅ | CEO→Clement Okeniyi; 11 roles unassigned (by design, future hiring) |
| 3 | Six FYZER departmental permission sets | ✅ | Verified; FLS matrix correction in progress |
| 4 | Five FYZER permission set groups | ✅ | 2 groups have assigned users; FYZER - Finance Access in no group (no finance users yet) |
| 5 | Private OWD (Lead/Account/Opportunity/Case/Contract) + CSM sharing rule | ✅ | Rule FYZER_Sales_Accounts_to_Customer_Success verified |
| 6 | Test users: Daniel Brooks, Olivia Carter | ✅ | Both active; share same Gmail (Olivia unverified — test artifact) |
| 7 | Customer Lifecycle Stage custom field | ✅ | 7 picklist values, default Prospect |
| 8 | Validation rule FYZER_Active_Customer_Requires_SLA | ✅ | Active; formula verified; fired correctly in test |
| 9 | Record types (Prospect/Customer Account) + FYZER Account Management Lightning page | ✅ | 2026-10-19: Customer Account record type made available to System Administrator + Minimum Access - Salesforce (default Prospect Account); FLS granted via permsets (Sales: Read+Edit; CS: Read on Lifecycle Stage + SLA Expiration Date); validation rule tested passing both directions |

## Phase 1 — Sales Cloud

| # | Item | Status | Notes |
|---|------|--------|-------|
| 10 | Lead capture (Web-to-Lead), assignment rules, lead queue | ✅ | 2026-10-19: Queue "FYZER - Sales Leads" (member: Daniel Brooks); rule "FYZER Lead Assignment" ACTIVE (Web→queue, catch-all→Daniel Brooks; "Standard" rule deactivated); Web-to-Lead enabled, Default Lead Creator→Clement Okeniyi, form generated (First/Last Name, Email, Company, Phone, Lead Source); reCAPTCHA off (default); tests: Web lead→queue ✅, Partner Referral lead→Daniel Brooks ✅ |
| 11 | Lead qualification + conversion process (status path, field mapping) | ✅ | 2026-10-19: Path "FYZER Lead Qualification" ACTIVE on Lead Status (--Master--) with key fields + guidance per stage; Converted flag verified on "Closed - Converted"; field-mapping check: 5 pre-existing custom lead fields (Current Generator(S), Number Of Locations, Primary, Product Interest, SIC Code) all mapped to None, nothing pending; converted Ava Novak → Account "Vertex Analytics" (Prospect Account) + Contact Ava Novak + Opportunity "Vertex Analytics - FYZER Platform" (Stage Qualification, Close Date 1/17/2027, Ava Novak primary contact role); Liam Osei lead left unconverted |
| 12 | Accounts/Contacts model + realistic fictional FYZER customer data | ✅ | 2026-10-19: 4 prospect accounts created (Meridian Logistics, Atlas Health Systems, Summit Financial Partners, Brightline Media) with 5 contacts (Priya Raman, Elena Torres, James Whitfield, Nora Adeyemi, Sofia Lindqvist); all Lifecycle Stage=Qualified, SLA Silver w/ expiration 12/31/2027; Northstar→Qualified, BluePeak→Prospect→Qualified; all example.com-style fictional domains/emails |
| 13 | Opportunity stages, pipeline, forecast categories | ✅ | 2026-10-19: standard 10 stages + probabilities reviewed (no FYZER sales process existed — left as-is); Path "FYZER Opportunity Path" ACTIVE with key fields + stage guidance; 3 pipeline opportunities created (Northstar $48k Qualification, BluePeak $120k Proposal/Price Quote, Meridian $75k Needs Analysis); Kanban verified all 4 opportunities with correct stages/amounts; Priya Raman PRIMARY contact role on Meridian opp |
| 14 | Products + standard/custom price books | ✅ | 2026-10-19: 4 Active products (FYZ-PRO $15k, FYZ-ENT $45k, FYZ-IMPL $12k, FYZ-SUP $8.5k); Standard Price Book entries added (the true standard book was inactive → activated); custom "FYZER - Enterprise Price Book" (Active) with 10% discounts; Vertex opp: Standard book + FYZ-ENT + FYZ-IMPL line items → Amount $57,000 |
| 15 | Sales reports + sales dashboard | ✅ | 2026-10-19: folder "FYZER Sales Reports" + 3 reports (Open Pipeline by Stage — 4 rows, $300k total; Leads by Source — 2 rows incl. converted lead; Opportunities by Owner); folder "FYZER Dashboards" + "FYZER Sales Dashboard" (pipeline bar chart, leads donut, opportunities table — all render data) |

## Phase 2 — Service Cloud

| # | Item | Status | Notes |
|---|------|--------|-------|
| 16 | Case management + Service Console app | ✅ | 2026-10-19: "FYZER Service Console" Lightning app (Console navigation; tabs Cases/Accounts/Contacts/Reports/Dashboards; assigned to System Administrator + Minimum Access); console layout verified rendering cases |
| 17 | Case queues, assignment rules, escalation rules | ✅ | 2026-10-19: Queue "FYZER - Support Queue" (no members — no support users hired yet); rule "FYZER Case Assignment" ACTIVE (Web→queue, catch-all→queue; old "Standard" auto-deactivated); escalation rule "FYZER Case Escalation" ACTIVE (Priority=High AND Status≠Closed, 4 business hours → reassign to queue + notify via "FYZER Case Escalated Notification" template; old "Standard" escalation auto-deactivated); tests: 2 cases created, both routed to queue ✅; escalation action fires via hourly job — criteria verified, 4h wait not performed |
| 18 | Entitlements/SLA tracking (where available in DE) | ✅ | 2026-10-19: Entitlement Management already enabled; process "FYZER Standard Support" ACTIVE (milestones: First Response 240 min, Resolution 2880 min, exit when Case Closed); BluePeak Industries → Customer Account + Active Customer + SLA Exp 12/31/2027 (validation rule satisfied); entitlement "FYZER Standard Support - BluePeak" Active 10/19/2026–12/31/2027; test case 00001028 did NOT auto-attach entitlement or milestones → gap: Entitlement Name not on Case layout + no auto-assignment; fix planned in Flow phase (item 20) |
| 19 | Service dashboard + knowledge check | ✅ | 2026-10-19: folder "FYZER Service Reports" + 3 reports (Open Cases by Status/Priority, Cases by Origin — 2 rows each); "FYZER Service Dashboard" renders all 3 components; Omni-Channel is enabled (no routing built — documented); Salesforce Knowledge: available in DE but the enable toggle does not respond to browser automation → ⚠️ NEEDS MANUAL CLICK in Setup > Knowledge Settings |

## Phase 3 — Automation (Salesforce Flow)

| # | Item | Status | Notes |
|---|------|--------|-------|
| 20 | Record-triggered Flows (before-save + after-save), fault paths, recursion safeguards | ✅ | 2026-10-19/20: FYZER_Case_Priority_Guard (before-save, tested), FYZER_Case_Auto_Entitlement (after-save; tested end to end: auto-attach ✅, no-entitlement negative ✅), FYZER_Case_Close_SLA_Note (after-save on close transition, tested ✅ — note: milestone countdown kept ticking in panel after close, documented); fault paths → automation log; live fault test produced exactly 1 log record |
| 21 | Screen Flow + reusable subflows, error handling, logging | ✅ | Subflow FYZER_Create_Onboarding_Task + Screen Flow FYZER_Customer_Onboarding ACTIVE + tested; fault paths → FYZER_Automation_Log__c |
| 22 | Cross-functional automation: Sales → CS → Support → Finance | ✅ | 2026-10-20: "FYZER - Opportunity Won Handler" + "FYZER - Account Active Customer Handoff" ACTIVE; Meridian won-deal test produced all 5 artifacts (Active Customer stage, CS welcome task, Support health-check case 00001033, Finance invoice task, onboarding task, 2 Chatter posts); idempotency tests C+E passed (opp + account edits created zero duplicates — counts identical); Automation Log still 1; caveat: no-SLA Onboarding branch verified by logic inspection only (layout required SLA values on the test account); two "NoSLA Test Corp" accounts exist (one unlinked duplicate — left in place, no-delete rule) |
| 22 | Cross-functional automation: Sales → CS → Support → Finance | ⏳ | After Flow part 1 lands |

## Phase 4 — Business Processes (simulations, clearly labeled)

| # | Item | Status | Notes |
|---|------|--------|-------|
| 23 | 🧪 Quote-to-Cash simulation (quotes, line items, discounts, approvals, orders, contracts, billing handoff) | ✅ | 2026-10-20: Quotes enabled; BluePeak quote Grand Total $49,000 (10% line discount); small-quote negative test ✅; POSITIVE approval path ✅ via expansion quote 00000003 ($98,500, submitted → pending step → approved by Clement Okeniyi with comment, final status Accepted, full Approval History trail); Order 00000100 + Contract 00000100 Activated; Finance billing-handoff task on Order |
| 24 | 🧪 PSA simulation (projects, staffing, timesheets, milestones, budgets, utilization, profitability) | ✅ | 2026-10-20: 4 custom objects (FYZER_Project__c, FYZER_Project_Resource__c, FYZER_Timesheet_Entry__c, FYZER_Project_Milestone__c) + tabs; BluePeak project verified EXACT: 44h, Billed $6,800, Cost $4,320, Profit $2,480, Margin 36.47%; profitability + utilization reports run correctly. Deviation: roll-ups can't aggregate cross-object formulas → Total Billed/Cost are stored fields maintained by flow "FYZER Update Project Totals" (create/update only, not delete — documented) |

## Phase 5 — Integrations & Data

| # | Item | Status | Notes |
|---|------|--------|-------|
| 25 | REST API demo (Named/External Credentials), error logging, retry design, non-sensitive test | ✅ | 2026-10-20: Named Credential FYZER_Demo_API (new External Credential UI + principal + "FYZER Demo API Access" permset); Flow HTTP Callout EXISTS — debug run HTTP 200, log record "HTTP 200 - delectus aut autem"; Workbench REST: query 200 (BluePeak), POST 201, GET 200, invalid POST 400 INVALID_FIELD; Automation Log now 3 records. Retry design goes in the integration design note (docs phase) |
| 26 | Matching rules, duplicate rules, data-quality controls | ✅ | 2026-10-20: Pre-existing standard matching + duplicate rules all Active (inventory recorded). New "FYZER Lead Matching Rule" ACTIVE (Email Exact AND (Last Name Fuzzy OR Company Fuzzy), logic "1 AND (2 OR 3)"); "FYZER Lead Duplicate Rule" ACTIVE (Create Allow+Alert+Report; Edit Allow+Alert; enforce sharing rules). Live test ✅: duplicate Liam Osei lead (same email/company) triggered "Similar Records Exist" during entry, saved without block, record shows "We found 1 potential duplicate"; Duplicate Record Set DRS-0000000001 contains both leads (set attributed to the Standard rule — both rules active; noted). Duplicate rules expose no Unique Name field (UI fact) |
| 27 | Data dictionary | ✅ | 2026-10-20: docs/data-dictionary.md — all customized standard objects, 5 custom objects with fields/formulas, 9-flow automation inventory, security model, reports/dashboards, integrations, AI components |

## Phase 6 — AI & Analytics

| # | Item | Status | Notes |
|---|------|--------|-------|
| 28 | Executive, sales, service, CS, admin dashboards | ✅ | 2026-10-20: Sales + Service dashboards existed (items 15/19). New reports: FYZER Won Revenue ($85,000 ✓), FYZER Accounts by Lifecycle Stage (9 accts: Prospect 2 / Qualified 3 / Active Customer 4), FYZER Cases by Account (8), FYZER Automation Log by Flow (3). New dashboards in FYZER Dashboards: Executive (Won Revenue table $85k — Metric chart unavailable for tabular source, documented substitution; pipeline $225k by stage; lifecycle donut; cases by priority), Customer Success, Admin — all opened and verified rendering with data |
| 29 | Agentforce exploration / practical AI demo or documented AI design w/ governance | ✅ | 2026-10-20: Einstein Generative AI ON, Agentforce ON, Prompt Builder works, NO credit/license walls. Built + ACTIVATED prompt template "FYZER Account Summary" (Field Generation, Account→Description, GPT 5 Mini) — live preview on BluePeak generated a grounded 3-sentence brief (1.96s, 470 tokens, 0 warnings). Built "FYZER Demo Agent" (Employee Agent template, Draft) — honest limitation: default template has NO CRM record lookup actions, so it cannot summarize accounts/cases without added actions (verbatim response + builder trace captured). Model Builder node absent. AI design + governance note in docs phase |

## Phase 7 — DevOps, Governance, Documentation, GitHub

| # | Item | Status | Notes |
|---|------|--------|-------|
| 30 | SFDX project + git, validation, rollback plan, release notes; governance doc; full docs bundle (BRD, stories, TDD, architecture diagram, SOP, training guide); GitHub repo prep (no push without approval) | ✅ | 2026-10-20: sfdx/ project (sfdx-project.json valid; manifest inventories all verified FYZER components; 38 source XML files well-formed; CLI parses project — deploy validation stops at org authorization, an owner action, documented). Git repo initialized locally: 6 commits + feature-branch merge, clean tree. Docs bundle complete in docs/ (10 files). Rollback plan in governance.md §5. NOT pushed anywhere — awaiting C approval |

## Requires my attention ⚠️

- Salesforce Knowledge: available in this DE org and the enable toggle exists, but the submit button does not respond to browser automation (tried Lightning + Classic pages, multiple click methods). Manual action for C: Setup > Knowledge Settings > check the acknowledge box > "Enable Salesforce Lightning Knowledge". Once enabled, no further build is required for this portfolio (article types/articles were out of scope).
- Entitlement auto-attach: RESOLVED 2026-10-20 — root cause was the Case Entitlement field being hidden for every profile (FLS); fixed, and FYZER_Case_Auto_Entitlement now attaches entitlements automatically (tested end to end). No action needed from C.

## Risks / dependencies log

- Customer Account record type unavailable to all profiles (found 2026-10-19) → fixing now.
- SLAExpirationDate__c invisible to sales/CS users while validation rule requires it → fixing now via permission sets.
- FYZER - Finance Access in no group and no finance users → revisit when finance role is staffed or during Finance automation (item 22).
- My Domain not fyzer-branded → cosmetic; changing post-setup is risky, documenting only.
- Org is an OrgFarm-provisioned Agentforce DE; some features (Knowledge, Omni-Channel, Agentforce) may behave differently than a standard DE — verify availability in items 16–19 and 29.

## Final QA annotations (2026-10-20)

- CLI authorized via JWT (External Client App "FYZER CLI Access", api+refresh_token, admin pre-authorized only). Web-login flow is environmentally impossible here (localhost callback unreachable) — JWT is the documented headless path.
- Metadata retrieved (66 files) and check-only deployment validation PASSED (65/65, 0 errors). Repo SFDX source synced from the org retrieve (68 files incl. entitlement processes); sourceApiVersion aligned to 68.0.
- QA-6 milestone countdown: root-caused to Developer Edition entitlement-engine behavior (milestones never complete on closure; reproduced on test case 00001034; process config verified correct from retrieved XML). Known issue KI-1 — no safe config fix.
- QA-7 PSA delete gap: record-triggered flows on delete are REJECTED by this org in validation (both trigger phases). Remediation = nightly scheduled reconciliation flow (designed; build in Flow Builder pending owner approval). TD-1 updated.
- QA-8 no-SLA branch: LIVE TEST PASSED via API-created account "QA NoSLA Branch Test Co" (Onboarding + 1 task + 0 cases + no Finance task).
- QA-9 Agentforce: FYZER_Get_Account_Summary_Data authored and check-only VALIDATED (picklist TEXT() defect found+fixed during validation). Deployment + agent wiring pending owner approval.
- Evidence package: fyzersalesforce-portfolio-evidence/ (documentation + reports complete; screenshots 27 & 30 captured as local renders; the 28 live-org captures are blocked on owner approval to add the "web" OAuth scope — api-only tokens cannot open UI sessions).
