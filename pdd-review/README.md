# PDD Review — Claude Code Skill

A multi-agent review skill for Product Design Documents (PDDs). Runs 9 specialized agents that evaluate a PDD from different perspectives and produce a structured review with per-agent verdicts and an actionable synthesis.

## Agents

| # | Agent | Focus |
|---|-------|-------|
| 1 | **CPO** | Strategic rationale, competitive landscape, differentiation, portfolio fit, PDD quality & logic |
| 2 | **Product** | Scope validation, user stories, acceptance criteria, phasing, consistency |
| 3 | **Engineering** | Technical feasibility, architecture, dependencies, timeline, security |
| 4 | **Risk & Compliance** | Regulatory, AML/KYC, liability, data privacy, third-party risk |
| 5 | **Ops** | Workflows, tooling, support readiness, staffing, launch checklist |
| 6 | **Go-to-Market** | Launch strategy, rollout sequencing, adoption assumptions, comms |
| 7 | **Economics & Treasury** | Revenue model, unit economics, cash flow, settlement, balance sheet |
| 8 | **Data** | Success metrics, counter metrics, instrumentation, phase-gate criteria |
| 9 | **Devil's Advocate** | "Do nothing" case, strongest alternative, execution risk stress test |

## Output

The review produces:

1. **Executive Summary** — overall verdict, blocker count, one-paragraph synthesis
2. **Per-Agent Reviews** — each agent's verdict, top issues, concrete edits, cross-cutting concerns
3. **Final Synthesis** — recommended decision, blockers table, MVP scope, metrics & phase-gates, rollout plan, open risks, cross-cutting concern resolution

## Installation

### Option 1: Clone to skills directory

```bash
git clone https://github.com/YOUR_USERNAME/pdd-review-skill.git ~/.claude/skills/pdd-review
```

### Option 2: Add to a project

```bash
cd your-project
git clone https://github.com/YOUR_USERNAME/pdd-review-skill.git .claude/skills/pdd-review
```

## Usage

```
/pdd-review path/to/your-pdd.md
```

Or simply ask Claude:

> Review this PDD: path/to/your-pdd.md

## Customization

### Company Context

Edit the **Company Context** table at the top of `SKILL.md` to match your organization:

```markdown
| Field | Value |
|-------|-------|
| **Company** | Your Company |
| **Domain** | Your industry |
| **Key Products** | Your product portfolio |
| **Regulatory Environment** | Your regulatory landscape |
| **Competitive Advantages** | Your differentiators |
```

All agents reference this table, so a single edit adapts the entire review to your context.

### Agent Personas

Each agent with a dedicated file can be customized independently:

| File | Agent | Scope Areas |
|------|-------|-------------|
| `CPO_AGENT.md` | CPO | 9 areas |
| `ENGINEERING_AGENT.md` | Engineering | 8 areas |
| `RISK_COMPLIANCE_AGENT.md` | Risk & Compliance | 8 areas |
| `ECONOMICS_AGENT.md` | Economics & Treasury | 9 areas |
| `DATA_AGENT.md` | Data | 6 areas |
| `DEVILS_ADVOCATE_AGENT.md` | Devil's Advocate | 6 areas |

Product, Ops, and GTM agents are defined as inline briefs in `SKILL.md`.

## File Structure

```
pdd-review/
├── SKILL.md                    # Main orchestration (364 lines)
├── CPO_AGENT.md                # CPO agent persona
├── ENGINEERING_AGENT.md        # Engineering agent persona
├── RISK_COMPLIANCE_AGENT.md    # Risk & Compliance agent persona
├── ECONOMICS_AGENT.md          # Economics & Treasury agent persona
├── DATA_AGENT.md               # Data agent persona
├── DEVILS_ADVOCATE_AGENT.md    # Devil's Advocate agent persona
├── README.md
└── LICENSE
```

## License

MIT
