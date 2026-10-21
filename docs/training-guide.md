# FYZER Salesforce — End-User Training Guide

Welcome to FYZER on Salesforce. This guide covers the everyday workflows for Sales, Customer Success, and Support. All data in the training org is fictional.

## 1. Finding your way around
- Use the **App Launcher** (grid icon) to switch apps: **Sales** (leads, accounts, opportunities), **FYZER Service Console** (cases), and the FYZER PSA tabs (Projects, Timesheets) for delivery work.
- Your home dashboards: **FYZER Sales Dashboard** (Sales), **FYZER Service Dashboard** (Support), **FYZER Customer Success Dashboard** (CS).

## 2. Working leads (Sales)
1. New web leads wait in the **FYZER - Sales Leads** queue — open the Leads list view for the queue, take ownership, and start working.
2. Follow the **FYZER Lead Qualification** path at the top of the lead: each stage shows the key fields to complete and guidance text.
3. If you see **"Similar Records Exist"** while creating a lead, stop and open the existing record — duplicates slow everyone down. If you save anyway, you'll see a "potential duplicate" banner; your admin reviews these weekly.
4. To convert: open the lead → **Convert** → choose the Account record type (**Prospect Account** for new business) → create the Contact and Opportunity → set yourself as owner and add the new contact as the **Primary** contact role on the opportunity.

## 3. Working opportunities (Sales)
- Move deals along the **FYZER Opportunity Path**; complete the key fields shown at each stage (e.g., Next Step in the middle stages).
- Add products from the price book so the Amount reflects the real quote (the Enterprise Price Book carries a 10% discount for strategic deals).
- When you set a deal to **Closed Won**, automation takes over: the account advances (Active Customer if it has an SLA date, otherwise Onboarding), Customer Success gets an onboarding task, and there's nothing for you to chase manually. Editing the opportunity afterwards will **not** re-trigger anything.

## 4. Accounts & onboarding (Customer Success)
- The **Customer Lifecycle Stage** tells you where an account stands: Prospect → Qualified → Onboarding → Active Customer → (At Risk / Renewal / Churned).
- An account can only become **Active Customer** with an **SLA Expiration Date** — Salesforce will block the save without one. That's deliberate.
- To onboard: open the account → **Start Onboarding** action → enter the SLA expiration date (and serial if you have it). The flow sets the stage, and creates an "Onboarding welcome call" task due in 7 days.
- When an account becomes Active Customer you may also see a **Welcome call** task (due in 3 days) — that's yours; Support and Finance get their own handoffs automatically.

## 5. Working cases (Support)
1. Cases arrive in the **FYZER - Support Queue** — pick up from the queue list view in the Service Console.
2. Follow the **FYZER Case Resolution** path across the top of the case.
3. If a case subject mentions an **outage** or something being **down**, its priority is automatically raised to **High** — work it first.
4. High-priority cases left open **4 hours** escalate automatically (reassigned to the queue + owner notified).
5. For entitled customers (e.g., BluePeak), the case shows **Entitlement Name** and a **Milestones** panel with live First Response / Resolution countdowns. Work to the countdown, not the calendar.
6. Closing an entitled case automatically posts an SLA note to the case feed — that's the audit record; leave it in place.

## 6. Quotes (Sales) 🧪 simulation
- Create quotes from the opportunity; the Enterprise price book rules still apply.
- Quotes **over $50,000** must be submitted for approval (**Submit for Approval** button) and approved before you mark them **Accepted**. Smaller quotes skip approval — if you see "No applicable approval process was found," that's expected for under-threshold quotes, not an error.

## 7. Projects & timesheets (Delivery) 🧪 simulation
- Log time on **FYZER Timesheet Entries** against your project staffing record; tick **Billable** only for client-billable work — the project's billed/cost/profit totals update themselves.
- Project managers: watch the **FYZER PSA Project Profitability** report and the project's Margin %.

## 8. Getting help
- Something automated went wrong? Check with your admin and mention the record you were on — every automation failure is captured in the FYZER Automation Log with the flow name and error.
- AI brief button: the **Account Description** can be drafted by the FYZER Account Summary AI template. It's a draft — read it before you use it; it only knows the account's name, industry, and lifecycle stage.
