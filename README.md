# Boardroom: AI Board of Advisors

Simulate a boardroom session where 10 advisors with distinct expertise challenge, validate, and sharpen your business idea. Two rounds of feedback (individual + cross-reactions) followed by an actionable synthesis.

Built by [Mission Relearn](https://missionrelearn.com), an AI-literacy consultancy in the Netherlands.

## Install

### Option A: Claude Code (CLI)

Requires [Claude Code](https://docs.anthropic.com/en/docs/claude-code), the terminal tool.

**Step 1:** Open a terminal and start Claude Code:
```bash
claude
```

**Step 2:** Add this repository as a plugin marketplace:
```
/plugin marketplace add annetklo/boardroom
```

**Step 3:** Install the plugin:
```
/plugin install boardroom@boardroom
```

**Step 4:** Enable the plugin:
```
/plugin enable boardroom
```

Done. The `/boardroom` skill is now available in all your Claude Code sessions.

### Option B: Claude.ai (browser, no install)

You can use this skill directly in claude.ai without installing anything.

**Step 1:** Go to [claude.ai](https://claude.ai) and create a new Project (sidebar > Projects > New Project).

**Step 2:** Open the project, click "Set project instructions", and paste the contents of [`skills/boardroom/SKILL.md`](skills/boardroom/SKILL.md) into the instructions field. Also paste the contents of [`skills/boardroom/references/advisors.md`](skills/boardroom/references/advisors.md) below it.

**Step 3:** Start a conversation in the project and ask your question:

```
Should we pivot from consulting to a SaaS product?
```

Claude follows the same structured workflow and produces a boardroom report.

## Usage

Interactive (Claude asks what it needs):
```
/boardroom
```

With a question:
```
/boardroom "Should we offer a subscription model instead of per-project pricing?"
```

From a file:
```
/boardroom path/to/business-plan.md
```

In English:
```
/boardroom --lang en "Should we expand internationally?"
```

## What it does

You present a business question, challenge, pitch, or strategy. 10 advisors with distinct perspectives respond:

**Round 1: Individual perspectives** (each 100-200 words)

| # | Advisor | Focus |
|---|---------|-------|
| 1 | De Strateeg | Market positioning, competitive advantage |
| 2 | De Klant | End-user perspective, pain points |
| 3 | De Investeerder | Scalability, market size, unit economics |
| 4 | De CFO | Cash flow, break-even, financial risk |
| 5 | De Technoloog | Technical feasibility, build vs. buy |
| 6 | De Marketeer | Brand, messaging, go-to-market |
| 7 | De Operator | Processes, execution, scaling |
| 8 | De Insider | Industry trends, regulations, competitors |
| 9 | De Peoplemanager | Team, culture, hiring |
| 10 | De Advocaat van de Duivel | Risks, blind spots, uncomfortable truths |

**Round 2: Boardroom dynamics** -- 4-6 cross-reactions where advisors challenge and build on each other's points.

**Synthesis** -- consensus points, key tensions, critical questions, and prioritized next steps.

Output is saved as a markdown file: `Boardroom_[Topic]_[Date].md`

## Example output (fragment)

```
### 3. De Investeerder

The subscription model changes your unit economics fundamentally.
Right now you sell hours at EUR 150, which caps your revenue at
available hours. A subscription at EUR 500/month per organization
needs 40 subscribers to match your current revenue, but scales
without adding headcount.

The question is retention. B2B training subscriptions see 15-25%
annual churn in this segment. At what subscriber count do you
break even after accounting for churn and support costs?

My recommendation: model three scenarios (pessimistic, base,
optimistic) with realistic churn rates before committing.
```

```
### Boardroom Dynamics

**De CFO reageert op De Investeerder:** "Those subscriber
projections assume steady acquisition, but you haven't accounted
for the 3-6 month sales cycle in enterprise education. Cash flow
will dip before subscriptions compensate. Build a bridge budget."
```

## Options

| Flag | Description | Default |
|------|-------------|---------|
| `--lang` | Output language: `nl` or `en` | Matches input language |
| `--output-dir` | Where to save the report | Current directory |

## Language support

Default output matches the language of your input. Dutch input produces Dutch output, English input produces English output. Use `--lang nl` or `--lang en` to override.

## Requirements

**Option A (CLI):**
- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) installed

**Option B (browser):**
- A [claude.ai](https://claude.ai) account (free or paid)

## Troubleshooting

**"command not found" when running `claude`:** Install Claude Code first: `npm install -g @anthropic-ai/claude-code`

**Plugin install fails:** Make sure you completed step 2 first (`/plugin marketplace add annetklo/boardroom`). The marketplace must be added before you can install.

**Using claude.ai in the browser?** Skip the CLI steps entirely, use Option B above.

## License

MIT
