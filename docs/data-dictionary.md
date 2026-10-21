# FYZER Salesforce — Data Dictionary

Org: fyzer.co Developer Edition (Org ID redacted) · Compiled 2026-10-20 from the verified org configuration.
All sample data is fictional. Items marked 🧪 are simulations built with standard/custom Salesforce features — never licensed CPQ, Certinia, or enterprise production tooling.

## 1. Standard objects — customizations

### Account
| Element | API / Name | Detail |
|---|---|---|
| Custom field | `Customer_Lifecycle_Stage__c` (picklist) | Prospect (default), Qualified, Onboarding, Active Customer, At Risk, Renewal, Churned |
| Standard field used | `SLA__c`, `SLAExpirationDate__c`, `SLASerialNumber__c` | SLA tier, expiry date, serial — used by the onboarding flow and validation rule |
| Validation rule | `FYZER_Active_Customer_Requires_SLA` (active) | Blocks Lifecycle = "Active Customer" when `SLAExpirationDate__c` is blank. Message: "An SLA expiration date is required before an account can be marked as an Active Customer." |
| Record types | Prospect Account (default), Customer Account | Customer Account available to System Administrator + Minimum Access - Salesforce profiles |
| Lightning page | FYZER Account Management | Activated for Sales app, both record types, Desktop; carries the "Start Onboarding" flow action |

### Lead
| Element | Detail |
|---|---|
| Statuses | Open - Not Contacted, Working - Contacted, Closed - Converted (converted flag), Closed - Not Converted |
| Path | FYZER Lead Qualification — key fields + guidance per status |
| Queue | FYZER - Sales Leads (member: Daniel Brooks) |
| Assignment rule | FYZER Lead Assignment (active): Web → queue; catch-all → Daniel Brooks |
| Web-to-Lead | Enabled; Default Lead Creator = Clement Okeniyi; form generated (not hosted) |
| Matching rule | FYZER Lead Matching Rule (active): Email Exact AND (Last Name Fuzzy OR Company Fuzzy); MatchBlank = false |
| Duplicate rule | FYZER Lead Duplicate Rule (active): Create = Allow + Alert + Report; Edit = Allow + Alert; enforce sharing rules |
| Field mapping | 5 pre-existing template custom fields map to None on conversion (verified, unchanged) |

### Opportunity
| Element | Detail |
|---|---|
| Stages | 10 standard stages with standard probabilities (Prospecting 10% … Closed Won / Closed Lost) |
| Path | FYZER Opportunity Path — key fields + guidance per stage |
| Contact roles | Primary contact role used on converted/won deals (Meridian: Priya Raman, PRIMARY) |

### Case
| Element | Detail |
|---|---|
| Path | FYZER Case Resolution on Case Status |
| Lightning page | FYZER Case Record Page — clone of the system page with the Path component; org default (Desktop) + Service Console app default |
| Queue | FYZER - Support Queue (no members — no support users hired yet) |
| Assignment rule | FYZER Case Assignment (active): Web → queue; catch-all → queue |
| Escalation rule | FYZER Case Escalation (active): Priority = High AND Status ≠ Closed; Age Over 4 hours → reassign to Support Queue + notify case owner (template "FYZER Case Escalated Notification") |
| Layout | Case Information section includes Entitlement Name (`EntitlementId`) — FLS for this field was hidden for ALL profiles in this org; set Visible for System Administrator (root-cause fix). Future support users should receive it via the FYZER - Support Access permission set |

### Quote / Order / Contract 🧪 (Quote-to-Cash simulation, standard objects)
| Element | Detail |
|---|---|
| Quotes | Enabled in Quote Settings. Demo quotes numbered 00000001–00000003, all labeled "(SIMULATION)" |
| Approval process | FYZER Quote Approval (active): entry `Total Price > 50,000` → approver Clement Okeniyi; final actions set Status Approved / Rejected (note: the wizard offers Total Price, not Grand Total) |
| Order 00000100 | BluePeak, Activated, $49,000 (matches source quote) |
| Contract 00000100 | BluePeak, 12 months, Activated |

### Entitlements
| Element | Detail |
|---|---|
| Process | FYZER Standard Support (active): milestones First Response 240 min (Order 1), Resolution 2880 min (Order 2); enters on case creation, exits when Case Closed = true |
| Record | "FYZER Standard Support - BluePeak" — Active, 10/19/2026 → 12/31/2027 |

## 2. Custom objects

### FYZER Automation Log (`FYZER_Automation_Log__c`)
Central error/result log for flows and integrations.
| Field | Type | Notes |
|---|---|---|
| `Flow_Name__c` | Text(80), required | Source flow or integration pattern name |
| `Related_Record__c` | Text(18) | Record ID context (may be null for API demos) |
| `Error_Message__c` | Long Text(32768) | Full fault message or result summary |

### FYZER Project (`FYZER_Project__c`) 🧪 PSA simulation object — not Certinia
| Field | Type | Notes |
|---|---|---|
| Name | Text | e.g. "BluePeak Platform Implementation (SIMULATION)" |
| `Account__c` | Lookup(Account) | Client account |
| `Status__c` | Picklist | Planning, Active, On Hold, Completed, Cancelled |
| `Start_Date__c` / `End_Date__c` | Date | |
| `Budget__c` | Currency | |
| `Total_Hours__c` | Roll-Up Summary | SUM of Timesheet Hours |
| `Total_Billed__c` / `Total_Cost__c` | Currency (stored) | Roll-ups cannot aggregate cross-object formula fields, so these are maintained by the flow FYZER_Update_Project_Totals (create/update; not on delete) |
| `Profit__c` | Formula (Currency) | `Total_Billed__c - Total_Cost__c` |
| `Margin_Percent__c` | Formula (Percent) | `IF(Total_Billed__c = 0, 0, Profit__c / Total_Billed__c)` |

### FYZER Project Resource (`FYZER_Project_Resource__c`) 🧪
`Project__c` Master-Detail(FYZER Project) · `Resource_User__c` Lookup(User) · `Role__c` picklist (Project Manager, Architect, Consultant, Developer, QA Analyst) · `Bill_Rate__c` Currency · `Cost_Rate__c` Currency · `Allocation__c` Percent.

### FYZER Timesheet Entry (`FYZER_Timesheet_Entry__c`) 🧪
Name = Auto Number `TS-{0000}` · `Project__c` Master-Detail · `Project_Resource__c` Lookup · `Entry_Date__c` Date · `Hours__c` Number(5,2) · `Billable__c` Checkbox (default true) · `Billable_Amount__c` Formula = `IF(Billable__c, Hours__c * Project_Resource__r.Bill_Rate__c, 0)` · `Cost_Amount__c` Formula = `Hours__c * Project_Resource__r.Cost_Rate__c` · `Notes__c` Text(255).

### FYZER Project Milestone (`FYZER_Project_Milestone__c`) 🧪
`Project__c` Master-Detail · `Due_Date__c` Date · `Status__c` picklist (Not Started, In Progress, Complete) · `Amount__c` Currency.

## 3. Automation inventory (Flows, all FYZER-prefixed)

| Flow | Type | Trigger / Purpose |
|---|---|---|
| FYZER_Case_Priority_Guard | Record-triggered (before-save) | Case create only: Subject contains "outage"/"down" AND Priority ≠ High → Priority = High. No fault path possible (before-save flows cannot DML) |
| FYZER_Case_Auto_Entitlement | Record-triggered (after-save) | Case create: Account set + Entitlement empty → attach account's active entitlement; fault paths → Automation Log |
| FYZER_Case_Close_SLA_Note | Record-triggered (after-save) | Case transitions into Closed with an entitlement → Chatter SLA audit post; fault path → log |
| FYZER_Create_Onboarding_Task | Autolaunched subflow | Inputs AccountId, OwnerId → creates "Onboarding welcome call" task (due +7d); fault path → log |
| FYZER_Customer_Onboarding | Screen Flow | "Start Onboarding" account action: collects SLA date/serial, sets Active Customer, calls subflow; fault → log + error screen |
| FYZER_Opportunity_Won_Handler | Record-triggered (after-save) | Opp transitions to Closed Won → account to Active Customer (SLA date present) or Onboarding (absent), subflow onboarding task, Chatter post; fault → log |
| FYZER_Account_Active_Customer_Handoff | Record-triggered (after-save) | Account transitions to Active Customer → CS welcome task (+3d), Support health-check case (Support Queue), Finance invoice task (+5d, interim owner Clement Okeniyi), Chatter post; fault → log |
| FYZER_Update_Project_Totals | Record-triggered (after-save) 🧪 | Timesheet create/update → recompute project Total Billed / Total Cost |
| FYZER_API_Demo_Callout | Autolaunched (saved, inactive; debug-tested) | HTTP Callout GET /todos/1 via Named Credential → writes result to Automation Log |

## 4. Security model
- OWD: Private on Lead, Account, Opportunity, Case, Contract.
- Role hierarchy (14 roles) from CEO → CRO/CTO/… with named users: Clement Okeniyi (CEO, System Administrator), Daniel Brooks (Sales Representative), Olivia Carter (Customer Success Manager). 11 roles intentionally unassigned.
- Permission sets: FYZER - Sales Access, Sales Manager Access, Customer Success Access, Support Access, Support Manager Access, Finance Access — composed into 5 permission set groups mapped to job roles. FYZER - Finance Access is in no group yet (no finance users).
- Sharing rule: FYZER_Sales_Accounts_to_Customer_Success — Sales-owned accounts shared Read Only to the Customer Success Manager role.
- FYZER Demo API Access permission set: grants Clement Okeniyi principal access to the FYZER external credential (least-privilege credential access).

## 5. Reports & dashboards
- Folders: FYZER Sales Reports, FYZER Service Reports, FYZER PSA Reports (SIMULATION), FYZER Dashboards.
- Dashboards (all verified rendering): FYZER Sales Dashboard, FYZER Service Dashboard, FYZER Executive Dashboard, FYZER Customer Success Dashboard, FYZER Admin Dashboard.

## 6. Integrations
- Named Credential `FYZER_Demo_API` → https://jsonplaceholder.typicode.com (External Credential "FYZER Demo External Credential", No Authentication, Named Principal "FYZERDemoPrincipal") + Remote Site Setting FYZER_JSONPlaceholder.
- External Service: FYZERJSONPlaceholderService (Flow HTTP Callout action "FYZER Get Todo").
- REST API exercised via Workbench (query 200 / create 201 / retrieve 200 / invalid 400 INVALID_FIELD).

## 7. AI
- Prompt template: FYZER Account Summary (FYZER_Account_Summary), Field Generation on Account → Description, model GPT 5 Mini, ACTIVE.
- Agent: FYZER Demo Agent (FYZER_Demo_Agent), Agentforce Employee Agent template, Version 1 Draft — default subagents only; no CRM record actions (documented limitation).
