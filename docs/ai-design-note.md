# FYZER Salesforce — AI Design & Governance Note

## What was verified in the org (2026-10-20)
- Einstein Generative AI: **ON**. Agentforce: **ON**. Prompt Builder: available, 40+ standard templates active. No credit or license wall was encountered.
- **Working demo:** prompt template *FYZER Account Summary* (Field Generation, Account → Description, GPT 5 Mini), activated. Preview on BluePeak Industries resolved only three facts (name, industry, lifecycle stage) and produced a factual, if generic, 3-sentence brief — no invented data. Generation: 1.96s, 470 tokens, 0 warnings.
- **Agent:** *FYZER Demo Agent* built from the standard Agentforce Employee Agent template, left in **Draft**. Asked to summarize an account and its cases, it answered honestly that it has no direct access to account summaries or case lists — the default template's subagents carry only a knowledge-search tool, with no CRM record actions.

## Design (how FYZER would productionize AI here)
1. **Grounding first.** Prompts resolve from CRM fields only and instruct the model to use only provided facts. The next iteration of the account brief adds open-case count, SLA status, and renewal date as merge resources so output stops being generic.
2. **Agents get tools, not wishes.** The demo agent's gap is the design lesson: an agent is its actions. Production build: add (a) an action invoking the FYZER Account Summary prompt template, (b) a Get-Records flow action for account/case lookup, (c) Data Library grounding for policy content.
3. **Human in the loop.** AI output is a draft. No AI component writes records, sends customer email, or changes lifecycle stages autonomously in this design; a user reviews and commits.
4. **Data boundaries.** Only fictional data exists in this org; the same build in production would restrict prompt grounding to fields the running user can already see (Prompt Builder respects user permissions) and keep PII out of external model calls per the Einstein Trust Layer.
5. **Observability.** Agent conversations are reviewable in Agent Analytics / Testing Center before any channel deployment; prompt performance insights (latency, tokens, warnings) are checked at each template change.

## Governance rules adopted
- AI features ship in Draft/inactive and are activated only after a human-reviewed test against known records.
- Prompts must state their factual boundaries ("use only the facts provided; do not invent…").
- Any new agent action is reviewed like any other automation: transition guards, fault logging, and a rollback (deactivate) path.
- Platform advisories are tracked (e.g., OpenAI GPT-4o retirement in March 2027 for legacy agents — upgrade path is the new Agentforce Builder or a newer reasoning model).
