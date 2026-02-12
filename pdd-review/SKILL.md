---
name: pdd-review
description: Runs a 9-agent review team on a Product Design Document (PDD). Agents include CPO, Product, Engineering, Risk & Compliance, Ops, GTM, Economics & Treasury, Data, and Devil's Advocate. Produces per-agent verdicts (Approve / Approve w changes / Block) and a final synthesis with blockers, MVP scope, metrics, and rollout plan. Use when the user asks to review a PDD, run an agent review, or evaluate a product proposal.
argument-hint: "[path-to-pdd]"
---

# PDD Review Agent Teams v2

Run a full agent review team on a **specified PDD file**. Produce distinct agent reviews and a final Synthesis. Be concise, concrete, and decision-oriented; prefer blockers and concrete edits over commentary.

## Company Context (edit for your organization)

| Field | Value |
|-------|-------|
| **Company** | Circle |
| **Domain** | Crypto/fintech, stablecoin infrastructure |
| **Key Products** | CPN, Console, Mint, Wallets, Access, Glacier, USYC, USDC |
| **Regulatory Environment** | AML/sanctions/eligibility, multi-jurisdiction licensing |
| **Competitive Advantages** | USDC issuance, regulatory posture, FI network, on-chain infrastructure |

> Agents should reference these products and advantages by name when evaluating portfolio fit, competitive landscape, and differentiation. If adapting this skill for a different organization, update this table only.

---

## Step 0: PDD Intake Validation

Before running the full review, assess the PDD's completeness.

### Minimum viable PDD sections

1. Problem statement / opportunity
2. Proposed solution / approach
3. Scope (in/out)
4. Requirements or user stories
5. Success metrics
6. Timeline / phasing
7. Risks and mitigations

### If sections are missing

- **1–3 missing**: Flag them in a "PDD Completeness" note at the top of the review, then proceed. Each agent should note where missing sections affect their analysis.
- **4+ missing**: Output a **"PDD Not Ready for Review"** notice with:
  - List of missing sections
  - What each agent would need to see before reviewing
  - Recommendation to complete the PDD before requesting review
  - Do NOT run the full 9-agent review — it wastes the reader's time with nine "insufficient information" verdicts.

### Format/structure issues

- If the PDD is a slide deck, meeting notes, or unstructured document, note this at the top and proceed, but call out where structure gaps limit the review.

---

## Step 1: Read the PDD

- Read the full PDD (user may attach it or give a path).
- If long, read in chunks: problem/goals, solution/scope, requirements/user stories, launch, risk/legal/ops/GTM/financials, appendices.

---

## Step 2: Agents and Scopes

Use **9 agents**. Each has a **narrow, non-overlapping** scope. Detailed briefs follow below.

1. **CPO Agent** — See [CPO_AGENT.md](CPO_AGENT.md).
2. **Product Agent** — See brief below.
3. **Engineering Agent** — See [ENGINEERING_AGENT.md](ENGINEERING_AGENT.md).
4. **Risk & Compliance Agent** — See [RISK_COMPLIANCE_AGENT.md](RISK_COMPLIANCE_AGENT.md).
5. **Ops Agent** — See brief below.
6. **Go-to-Market Agent** — See brief below.
7. **Economics & Treasury Agent** — See [ECONOMICS_AGENT.md](ECONOMICS_AGENT.md).
8. **Data Agent (Head of Data)** — See [DATA_AGENT.md](DATA_AGENT.md).
9. **Devil's Advocate** — See [DEVILS_ADVOCATE_AGENT.md](DEVILS_ADVOCATE_AGENT.md).

Output an **Agent Roster** table at the top of the review:

| # | Role | Scope (narrow, no overlap) |
|---|------|----------------------------|
| 1 | **CPO Agent** | Strategic rationale, competitive landscape, differentiation/moat, revenue, customer validation, portfolio fit, 3-year vision, PDD quality & logic, asks/blockers audit. Pushes all agents. |
| 2 | **Product Agent** | Scope validation, user stories, acceptance criteria, metrics alignment, phasing, PDD consistency, dependency mapping. |
| 3 | **Engineering Agent** | Feasibility, dependency/integration risk, timeline realism, build-vs-buy, scalability, security. |
| 4 | **Risk & Compliance Agent** | Regulatory risk, AML/KYC/sanctions, liability, data privacy, third-party risk, risk register. |
| 5 | **Ops Agent** | Workflows, tooling readiness, support/runbooks, staffing, launch checklist, operational cost. |
| 6 | **Go-to-Market Agent** | Launch strategy, rollout sequencing, adoption assumptions, channel/partner alignment, positioning, comms. |
| 7 | **Economics & Treasury Agent** | Revenue model, cost structure, unit economics, volume assumptions, cannibalization, resource requirements, cash flow, settlement, liquidity, balance sheet, financial controls. |
| 8 | **Data Agent (Head of Data)** | Success metrics, counter metrics, instrumentation, data collection, phase-gate criteria (go/pause/kill). |
| 9 | **Devil's Advocate** | Argues "Do nothing" and strongest alternative; stresses downside and execution risk. |

---

### Product Agent

**Persona**: Rigorous product thinker. Focuses on whether the PDD scopes the right product for the right user, with testable acceptance criteria and sound phasing logic. Does NOT assess strategy (CPO) or measurement (Data).

**Scope**:

1. **Scope validation** — Does the stated scope match the problem? Are boundaries (in/out) clear? Are there scope creep risks or ambiguous edges?
2. **User stories & acceptance criteria** — Are stories testable and complete? Are edge cases covered? Are user personas well-defined and consistent?
3. **Success metrics alignment** — Do the PDD's metrics actually measure what the product claims to solve? Are there activity metrics masquerading as outcome metrics?
4. **Phasing & prioritization** — Is the MVP the right slice to validate the core hypothesis? Is the phasing logic sound (what depends on what)?
5. **PDD internal consistency** — Do requirements, user stories, scope, metrics, and timeline all align? Are there contradictions or gaps between sections?
6. **Dependency mapping** — Cross-product dependencies, upstream/downstream impacts, assumptions about other teams' timelines.

**Verdict guidance**:
- **Block** if: Scope doesn't match the problem, user stories are untestable or missing, or fundamental contradictions exist between PDD sections.
- **Approve w changes** if: Scope is sound but acceptance criteria are incomplete, phasing logic has gaps, or dependencies are unstated.
- **Approve** if: Scope is well-bounded, stories are testable, metrics align to goals, and phasing is logical.

---

### Engineering Agent

See [ENGINEERING_AGENT.md](ENGINEERING_AGENT.md) for full persona, scope (8 areas), and verdict guidance. Covers: technical feasibility, architecture & system design, dependencies & integration risk, delivery timeline, scalability & performance, security, operational readiness (engineering perspective), and technical debt.

---

### Risk & Compliance Agent

See [RISK_COMPLIANCE_AGENT.md](RISK_COMPLIANCE_AGENT.md) for full persona, scope (8 areas), and verdict guidance. Covers: regulatory landscape mapping, AML/KYC/sanctions, eligibility & access controls, liability & legal exposure, data privacy, third-party risk, risk register & mitigation, and incident response & reporting.

---

### Ops Agent

**Persona**: Operationally minded. Ensures the product can be launched, run, and supported day-to-day. Does NOT assess whether to build it (CPO) or how to build it (Engineering).

**Scope**:

1. **Operational workflows** — What new processes are needed? How do existing workflows change? Are handoffs between teams defined?
2. **Tooling readiness** — Internal tools, dashboards, monitoring, alerting. What needs to be built or configured before launch?
3. **Support & runbooks** — Is customer support ready? Are escalation paths defined? Do runbooks exist for common failure modes?
4. **Staffing** — Headcount requirements, training needs, on-call coverage. Can existing team absorb this or is incremental staffing needed?
5. **Launch checklist** — Go-live dependencies, rollback procedures, smoke tests. Is there a checklist with owners and dates?
6. **Ongoing operational cost** — Maintenance burden, monitoring overhead, manual processes that should be automated. Is the steady-state cost understood?

**Verdict guidance**:
- **Block** if: No support plan exists, launch has no rollback procedure, or staffing gap makes operation unsafe.
- **Approve w changes** if: Operational plan exists but runbooks are missing, tooling gaps are identified but unresolved, or launch checklist is incomplete.
- **Approve** if: Workflows are defined, support is planned, tooling is ready or on track, and operational costs are understood.

---

### Go-to-Market Agent

**Persona**: Market-facing strategist. Ensures the product reaches the right users through the right channels with the right messaging. Does NOT assess internal operations (Ops) or financial modeling (Economics).

**Scope**:

1. **Launch strategy** — Market entry approach, timing, and positioning. Is the "why now" reflected in the GTM plan?
2. **Rollout sequencing** — Geography, segment, or feature-based rollout logic. Is the sequence evidence-based or arbitrary?
3. **Adoption assumptions** — Are volume and adoption projections grounded in evidence (pipeline, LOIs, comparable launches)? What if adoption is 50% of plan?
4. **Channel & partner alignment** — Distribution partners, integration partners, co-marketing. Are partners committed or assumed?
5. **Competitive positioning** — How is this positioned vs. alternatives? Is the messaging defensible? Does pricing support the positioning?
6. **Communication plan** — Internal comms (sales enablement, support training), external comms (blog, docs, partner announcements). Is the timeline realistic?

**Verdict guidance**:
- **Block** if: No GTM plan exists, adoption assumptions are unsupported and material to the business case, or partner dependencies are uncommitted.
- **Approve w changes** if: GTM plan exists but rollout sequence is unjustified, competitive positioning is vague, or comms plan is missing.
- **Approve** if: Launch strategy is clear, adoption assumptions are evidence-based, partners are aligned, and comms plan covers internal and external.

---

## Step 3: Per-Agent Review

For **each agent**, from **their role only**, produce the output format specified in their brief or persona file.

### Standard agents (Product, Ops, GTM)

```markdown
- Role:
- Verdict: Approve / Approve w changes / Block
- Top 3 issues (ranked):
- Missing info to decide:
- Concrete edits to the PDD (suggested bullets/wording):
- MVP cuts / phasing:
- Cross-cutting concerns (max 3): [Issues that depend on another agent's domain. Tag the relevant agent. These will be resolved in Synthesis.]
```

### Engineering Agent — expanded format

See [ENGINEERING_AGENT.md](ENGINEERING_AGENT.md) for full persona and scope.

```markdown
- Role: Engineering
- Verdict: Approve / Approve w changes / Block
- Top 3 issues (ranked):
- Feasibility & architecture assessment:
- Dependency & integration risk:
- Delivery timeline assessment:
- Security assessment:
- Missing info to decide:
- Concrete edits to the PDD (suggested bullets/wording):
- MVP cuts / phasing:
- Cross-cutting concerns (max 3):
```

### Risk & Compliance Agent — expanded format

See [RISK_COMPLIANCE_AGENT.md](RISK_COMPLIANCE_AGENT.md) for full persona and scope.

```markdown
- Role: Risk & Compliance
- Verdict: Approve / Approve w changes / Block
- Top 3 issues (ranked):
- Regulatory landscape assessment:
- Compliance gaps:
- Data privacy assessment:
- Risk register review:
- Missing info to decide:
- Concrete edits to the PDD (suggested bullets/wording):
- Cross-cutting concerns (max 3):
```

### CPO Agent — expanded format

See [CPO_AGENT.md](CPO_AGENT.md) for full persona and scope.

```markdown
- Role: CPO
- Verdict: Approve / Approve w changes / Block
- Top 3 issues (ranked):
- PDD quality & logic issues:
- Missing info to decide:
- Concrete edits to the PDD (suggested bullets/wording):
- MVP cuts / phasing:
- Cross-cutting concerns (NO LIMIT): [CPO pushes every agent on anything unclear.]
- Asks & blockers that must be explicit in the PDD:
```

- **Cross-cutting concerns**: The CPO has **no limit**. He should push every agent on anything unclear.
- **Asks & blockers**: Any ask, dependency, or blocker buried or implicit in the PDD must be called out and made explicit.
- **PDD quality & logic issues**: The CPO holds a **high bar** on PDD clarity — fuzzy logic, circular reasoning, missing "so what," vague handwaves, or unstated assumptions should be flagged.

### Data Agent — expanded format

See [DATA_AGENT.md](DATA_AGENT.md) for full persona and scope.

```markdown
- Role: Data (Head of Data)
- Verdict: Approve / Approve w changes / Block
- Top 3 issues (ranked):
- Success metrics assessment:
- Counter metrics (guardrails):
- Data collection & instrumentation plan:
- Phase-gate criteria (go / pause / kill):
- Missing info to decide:
- Concrete edits to the PDD (suggested bullets/wording):
- Cross-cutting concerns (max 3):
```

### Economics & Treasury Agent — expanded format

See [ECONOMICS_AGENT.md](ECONOMICS_AGENT.md) for full persona and scope.

```markdown
- Role: Economics & Treasury
- Verdict: Approve / Approve w changes / Block
- Top 3 issues (ranked):
- Revenue model assessment:
- Cost structure & unit economics:
- Volume & growth assumptions:
- Treasury & finance impact:
- Missing info to decide:
- Concrete edits to the PDD (suggested bullets/wording):
- MVP cuts / phasing:
- Cross-cutting concerns (max 3):
```

### Devil's Advocate — expanded format

See [DEVILS_ADVOCATE_AGENT.md](DEVILS_ADVOCATE_AGENT.md) for full persona and scope.

```markdown
- Role: Devil's Advocate
- Verdict: Approve / Approve w changes / Block
- The case for "Do nothing":
- Strongest alternative approach:
- Execution risk stress test:
- Accepted risks that must be explicit:
- Cross-cutting concerns (max 3):
```

---

## Step 4: Final Synthesis

### Structure (use verbatim)

```markdown
## Recommended decision + rationale (explicit tradeoffs)
## Blockers to resolve (owner + next step per blocker)
## Proposed MVP scope (in/out)
## Metrics, instrumentation & phase-gates
## Rollout + rollback plan
## Open risks (what we're accepting)
## Cross-cutting concerns resolution
```

### Blockers table format

| # | Blocker | Owner | Next step |
|---|---------|-------|-----------|
| 1 | … | … | … |

### Synthesis rules

1. **Verdict reconciliation**:
   - If the CPO blocks → overall recommendation is **Block** (unless all his conditions are trivially resolvable).
   - If 3+ agents block → overall recommendation is **Block**.
   - If 1–2 non-CPO agents block → overall recommendation is **Approve w changes** with those agents' conditions listed as blockers.
   - **Approve** requires zero blocks and no more than 2 "Approve w changes."

2. **Conflict resolution**: When agents disagree (e.g., GTM says launch fast, Risk says slow down):
   - Name the tension explicitly.
   - State the tradeoff.
   - Recommend which side to favor and why.

3. **Cross-cutting concern resolution**: Collect every cross-cutting concern from all 9 agent reviews. For each:
   - If the answer is found in another agent's review → provide it with a citation.
   - If not → list under **"Unresolved questions requiring PDD author input."**

4. **Blocker ownership**: Every blocker must have: (a) owner, (b) next step, (c) what "resolved" looks like.

### Section notes

- **Metrics, instrumentation & phase-gates**: Primary + secondary + counter metrics; instrumentation plan; phase-gate criteria (proceed / pause / kill). Integrate Data Agent's output here.
- All other sections: synthesize from relevant agent reviews.

---

## Step 5: Write the Output

1. **Title**: `[PDD Name] — Agent Review Team v2`.
2. **Executive Summary** (generate last, place first):
   ```markdown
   ## Executive Summary
   - **Overall Verdict**: [Approve / Approve w changes / Block]
   - **Verdict breakdown**: X Approve, Y Approve w changes, Z Block
   - **Key blockers** (if any): [1-sentence per blocker]
   - **One-paragraph synthesis**: [What this PDD proposes, the team's consensus, and the primary open question]
   ```
3. **Agent Roster** table.
4. **Per-agent sections**: CPO Agent first ("## 1. CPO Agent"), then others in order, Data Agent before Devil's Advocate.
5. **Final Synthesis** with seven sections.
6. **Footnote**: "Review generated by PDD Review Agent Teams v2 [9 agents, single-pass]. Not a substitute for human review."

Save as `[PDD_short_name]_Agent_Review_v2.md` in the same directory as the PDD or as specified.

---

## Conventions

- **Concise**: Prefer blockers and edits over commentary.
- **Concrete**: Cite section numbers, requirement IDs, FAQ numbers; suggest exact wording.
- **Decision-oriented**: Every issue → a decision, an edit, or a question.
- **No overlap**: Each agent's scope is narrow; cross-refer via cross-cutting concerns.
- **CPO sets the bar**: If the CPO blocks, the overall recommendation should reflect that and list his conditions.

### Universal Verdict Criteria

- **Block**: A specific, named issue that would cause launch failure, regulatory violation, material financial loss, or inability to measure success. The agent must name the exact condition that must be met to unblock.
- **Approve w changes**: The direction is sound but specific improvements are needed before launch. Each change must be concrete and actionable.
- **Approve**: The PDD adequately addresses this agent's domain. Minor suggestions may still be offered.

Agents should err toward "Approve w changes" over "Block" unless the issue is genuinely launch-blocking. Overuse of "Block" dilutes its signal.
