# QA Phase — Reconciled Screenshot Mapping (checklist.md order takes priority)

C's example table preserved where topics match; renumbered/renamed to the actual 30 checklist items.

| # | Checklist item | Filename | Evidence to capture |
|---|---|---|---|
| 1 | Org setup | 01-org-setup.png | Setup > Company Information (locale, currency, timezone) — Org ID redacted |
| 2 | Role hierarchy | 02-role-hierarchy.png | Setup Roles tree, expanded |
| 3 | Six permission sets | 03-permission-sets.png | Permission Sets list showing the six FYZER sets |
| 4 | Five permission set groups | 04-permission-set-groups.png | Permission Set Groups list / one group detail with assigned sets |
| 5 | Private OWD + sharing rule | 05-owd-sharing-rules.png | Sharing Settings: OWD Private + FYZER sharing rule |
| 6 | Test users | 06-least-privilege-test-users.png | Users list (Daniel Brooks, Olivia Carter) / user detail with Minimum Access profile + group |
| 7 | Customer Lifecycle Stage field | 07-lifecycle-stage-field.png | Account field detail with 7 picklist values |
| 8 | Validation rule | 08-account-security-validation.png | FYZER_Active_Customer_Requires_SLA detail (Active, formula, message) |
| 9 | Record types + Lightning page | 09-record-types-lightning-page.png | Account record types or FYZER Account Management page in App Builder |
| 10 | Lead capture/assignment/queue | 10-lead-assignment.png | FYZER Lead Assignment rule (Active) + queue |
| 11 | Lead qualification + conversion | 11-lead-conversion.png | Converted lead evidence (Ava Novak → Vertex Analytics) / Lead Path |
| 12 | Accounts/Contacts data | 12-accounts-contacts-data.png | Accounts list with fictional FYZER customers |
| 13 | Opportunity pipeline | 13-opportunity-pipeline.png | Opportunity Kanban populated |
| 14 | Products + price books | 14-products-price-books.png | Products list / FYZER - Enterprise Price Book with entries |
| 15 | Sales dashboard | 15-sales-dashboard.png | FYZER Sales Dashboard |
| 16 | Service Console | 16-service-console.png | FYZER Service Console with case list |
| 17 | Case routing/escalation | 17-case-routing-escalation.png | FYZER Case Escalation rule detail |
| 18 | Entitlements/SLA | 18-entitlement-sla.png | Case with Entitlement Name + live Milestones panel |
| 19 | Service dashboard + Knowledge | 19-service-dashboard.png | FYZER Service Dashboard (Knowledge limitation in caption + QA report) |
| 20 | Record-triggered flows | 20-record-triggered-flow.png | Flow Builder: FYZER - Case Auto-Entitlement (Active) |
| 21 | Screen flow + subflow | 21-customer-onboarding-screen-flow.png | FYZER - Customer Onboarding flow (builder or run screen) |
| 22 | Cross-functional automation | 22-cross-functional-automation.png | Meridian account: handoff tasks/case/Chatter (composite if needed) |
| 23 | Quote-to-Cash sim | 23-quote-to-cash.png | Quote 00000003 with line items + Approval History |
| 24 | PSA sim | 24-psa-profitability.png | PSA Project record (margin) + profitability (composite ok) |
| 25 | REST API demo | 25-rest-api-integration.png | Named Credential detail + Automation Log "HTTP 200" record (composite ok) |
| 26 | Duplicate rules | 26-duplicate-detection.png | Duplicate Liam Osei lead banner / matching rule (composite ok) |
| 27 | Data dictionary | 27-data-dictionary.png | Rendered data dictionary document |
| 28 | Dashboards (Exec/CS/Admin) | 28-executive-dashboard.png | FYZER Executive Dashboard |
| 29 | Agentforce AI | 29-agentforce-ai.png | Prompt Builder: FYZER Account Summary preview result |
| 30 | SFDX/git/docs/GitHub prep | 30-devops-documentation.png | Composite: git log graph + CLI validation output + README preview |

C's example-table topics absorbed: least-privilege test → #06; git → #30 composite; deployment validation → #30 composite + reports/; architecture diagram → documentation/ (also rendered as its own file in documentation/, and the diagram image included in evidence documentation); GitHub README preview → #30 composite; permission security audit → reports/ (text) + #03/#04/#06 images.
