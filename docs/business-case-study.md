# FYZER | Salesforce Business Transformation Case Study

**Type:** Fictional organization / hands-on Salesforce Developer Edition implementation  
**Role demonstrated:** Salesforce Administrator, solution designer, platform automation and governance  
**Scope:** Sales Cloud, Service Cloud, Flow, reporting, integrations, Quote-to-Cash simulation, PSA simulation  
**Evidence:** [30 screenshot index](../fyzer-salesforce-portfolio-evidence/README.md) · [Test results](../fyzer-salesforce-portfolio-evidence/reports/testing-evidence.md) · [Known issues](../fyzer-salesforce-portfolio-evidence/reports/known-issues.md)

## 1. Business background

FYZER is a **fictional** B2B software business selling platform subscriptions, implementation services, and premium support. The case assumes that initial commercial growth outpaces operational processes. Leads arrive without clear ownership, post-sale onboarding depends on messages between teams, support activity is not consistently tied to contractual response expectations, and financial reporting is scattered. This is a scenario designed to demonstrate administrator judgment, **not a real client engagement or an observed incident**.

## 2. Business problems and risks

| Problem | Business risk | What success must look like |
|---|---|---|
| Unowned or inconsistently qualified inbound leads | Missed opportunities and unreliable pipeline | Lead source determines owner; qualification stages have usable guidance; conversion creates linked CRM records |
| Sales-to-service handoff is manual | Onboarding steps lost; invoice and support setup delayed | Closed Won triggers governed customer lifecycle changes and discrete responsibilities for CS, Support, Finance |
| Support cases lack consistent triage | Slow response and poor SLA visibility | Cases enter a queue; urgent subjects are flagged; eligible cases attach entitlements; overdue work is visible |
| Quote exceptions not governed | Unapproved pricing | Approval trail exists above the threshold, while smaller quotes do not enter the high-value process |
| Delivery profitability cannot be reconciled | Margin leakage | Time entries feed billable and cost calculations, utilization and profitability views |
| Weak data quality and auditability | Duplicate contacts/leads, over-access and silent automation faults | Private sharing, least privilege, duplicate alerts and central automation logging |

## 3. Investigation and design choices

The planned solution favors Salesforce standard objects and platform configuration, because the budget is $0 and licensing is limited to Developer Edition. The Account lifecycle picklist is the connective field across Sales and Customer Success; a validation rule prevents `Active Customer` without an SLA expiration date. Transition-only Flow conditions, not broad update triggers, help prevent repeated tasks and cases. Permission set groups encode job responsibilities and OWDs remain Private. Integrations use a Named Credential rather than embedding secrets in Flow logic.

The proposed system design and its boundaries are recorded in the [technical design](technical-design.md), [architecture](architecture.md), [data dictionary](data-dictionary.md) and [governance log](governance.md).

## 4. Implemented response and acceptance evidence

### A. Capture and convert demand

**Intervention:** Web-to-Lead, `FYZER - Sales Leads` queue, active assignment rule, Lead Qualification Path.  
**Test:** Web lead Ava Novak routed to queue; partner referral routed to Daniel Brooks. Ava converted into Vertex Analytics Account, Contact and Opportunity.  
**Evidence:** [Assignment](../fyzer-salesforce-portfolio-evidence/screenshots/10-lead-assignment.png) · [Conversion](../fyzer-salesforce-portfolio-evidence/screenshots/11-lead-conversion.png).

### B. Make Closed Won the start of onboarding

**Intervention:** Opportunity-Won Handler and Account Active-Customer Handoff Flows. The first checks SLA date before changing lifecycle; the second coordinates CS welcome task, Support case, Finance invoice task, and audit feed post.  
**Test:** Closing a demo opportunity produced the expected artifacts. Editing that opportunity and the customer Account again produced **zero duplicate handoff artifacts**. Separately, the no-SLA account branch stayed in Onboarding without activating the CS/Support/Finance Active-Customer handoff.  
**Evidence:** [Handoff](../fyzer-salesforce-portfolio-evidence/screenshots/22-cross-functional-automation.png) · [Tests](../fyzer-salesforce-portfolio-evidence/reports/testing-evidence.md).

### C. Organize service response and escalation

**Intervention:** Service Console, Case Queue, assignment/escalation rules, Case Priority Guard, Entitlement Process, automatic entitlement attachment.  
**Test:** Queue routing, priority guard and entitlement attachment passed. **Exception:** CaseMilestone records did not complete after case closure, including a reproduced test; don't describe this as resolved.  
**Evidence:** [Service console](../fyzer-salesforce-portfolio-evidence/screenshots/16-service-console.png) · [Entitlement](../fyzer-salesforce-portfolio-evidence/screenshots/18-entitlement-sla.png) · [KI-1](../fyzer-salesforce-portfolio-evidence/reports/known-issues.md).

### D. Govern commercial and project economics

**Intervention:** Quote line items and over-$50K approval (simulation), Order/Contract demonstration; PSA custom objects and margin computations (simulation, not Certinia).  
**Test:** A $98,500 expansion quote was submitted and approved with Approval History. BluePeak sample project matched 44 hours, $6,800 billings, $4,320 cost, $2,480 profit and 36.47% margin. **Exception:** Stored monetary totals need reconciliation if timesheet records are deleted.  
**Evidence:** [Approval](../fyzer-salesforce-portfolio-evidence/screenshots/23-quote-to-cash.png) · [PSA](../fyzer-salesforce-portfolio-evidence/screenshots/24-psa-profitability.png).

### E. Improve control and observability

**Intervention:** Private OWD, role/permission-set model, duplicate matching and warn/report, Automation Log, dashboards, Named Credential and test callout.  
**Test:** Duplicate warning surfaced; API calls produced expected successful and invalid-request responses; five dashboards rendered; metadata check-only deployment recorded 65/65 components and zero errors. **Qualification:** The non-admin users screenshot proves user/profile setup, **not record visibility by itself**.  
**Evidence:** [Sharing](../fyzer-salesforce-portfolio-evidence/screenshots/05-owd-sharing-rules.png) · [Duplicate match](../fyzer-salesforce-portfolio-evidence/screenshots/26-duplicate-detection.png) · [Validation report](../fyzer-salesforce-portfolio-evidence/reports/deployment-validation.md).

## 5. Outcomes, honestly reported

**Demonstrated:** deterministic lead routing, end-to-end conversion, automatic cross-functional handoff with edit-based duplicate checks, pricing approval history, per-project sample profitability, integration success/error handling and check-only metadata validation.  
**Not established:** production ROI, reduced response time, real organizational adoption, actual reduction in lost leads, or live business revenue gains. Those would require production telemetry and an actual customer implementation.

## 6. Remaining operational risks and next iteration

- SLA milestone completion after Case close remains unresolved (KI-1).
- Project monetary totals can drift after deletion; a reconciliation approach is designed but not deployed.
- Agentforce prompt is demoed; CRM-enabled agent remains a future enhancement.
- Knowledge activation was reported manually by the org owner, but the repository QA documentation predates verification.
- Screenshot #06 is administration-user evidence, not a substitute for a real non-admin record visibility test.

See [Known Issues](../fyzer-salesforce-portfolio-evidence/reports/known-issues.md) and [Final QA](../fyzer-salesforce-portfolio-evidence/reports/final-qa-results.md).

## 7. What this portfolio demonstrates

The emphasis is on **problem definition → configuration design → implementation → test evidence → limitations and change control**. It is deliberately more than a list of Salesforce features, and deliberately less than an unsupported claim of a production transformation.
