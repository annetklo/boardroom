---
name: boardroom
description: Simulate an AI board of advisors providing multi-perspective feedback on a business question, challenge, pitch, or strategy. Use when the user wants strategic feedback, a sounding board, advisory input, or wants to stress-test a business idea from multiple angles. Triggers on phrases like "boardroom", "advisory board", "board of advisors", "raad van advies", "sparringpartners", "strategic feedback", "stress-test my idea".
argument-hint: "[question or file path] [--output-dir DIR] [--lang nl|en]"
allowed-tools: Read, Write, Glob
license: MIT
---

# Boardroom: AI Board of Advisors

Simulate a boardroom session where 10 advisors with distinct expertise provide feedback on the user's business question. The advisors respond individually and then react to each other, creating realistic boardroom dynamics. Output is a structured markdown report.

## Usage

```
/boardroom "Should we offer a subscription model for AI training?"
/boardroom path/to/briefing.md
/boardroom --lang en "Should we expand internationally?"
/boardroom "Moeten we een freemium model lanceren?"
```

Minimal (interactive):
```
/boardroom
```

## Workflow

### Step 1: Gather the business question

Accept the input in one of three ways:

1. **Direct text**: User provides the question/pitch/challenge as text
2. **File path**: User points to a document (.md, .txt, .docx, .pdf) -- read it and extract the core question
3. **Interactive**: If no input provided, ask in the user's language:
   - Dutch: "Wat is de vraag, uitdaging, pitch of strategie die je wilt voorleggen aan je raad van advies?"
   - English: "What is the question, challenge, pitch, or strategy you want to put before your board of advisors?"

Determine the output language:
- If `--lang` flag is provided, use that language
- Otherwise, match the language of the input (Dutch input = Dutch output)
- When in doubt, default to Dutch

### Step 2: Load advisor personas

Read the file `references/advisors.md` (relative to this skill's directory) to load all 10 advisor profiles.

### Step 3: Set the context

Before generating advisor responses, internally establish:
- **The core question** -- one sentence summary of what the user is asking
- **The domain** -- education, tech, consulting, etc.
- **The stage** -- idea, early-stage, growth, pivot, etc.
- **Key assumptions** -- what the user is taking for granted

### Step 4: Round 1 -- Individual perspectives

Generate a response from each of the 10 advisors, in this order:

1. **De Strateeg** -- sets the strategic frame
2. **De Klant** -- grounds the discussion in reality
3. **De Investeerder** -- investment and scalability lens
4. **De CFO** -- financial reality check
5. **De Technoloog** -- technical feasibility
6. **De Marketeer** -- go-to-market and messaging
7. **De Operator** -- execution and processes
8. **De Insider** -- domain context and trends
9. **De Peoplemanager** -- team and culture implications
10. **De Advocaat van de Duivel** -- challenges everything said so far

Each advisor response must:
- Stay in character (use their specific lens, vocabulary, and priorities)
- Be 100-200 words (concise, not lectures)
- Open with their most important observation or concern
- Include at least one specific, actionable question or recommendation
- Reference the user's actual situation, not generic advice

### Step 5: Round 2 -- Boardroom dynamics

Generate 4-6 cross-reactions where advisors respond to each other. This is what makes the boardroom feel alive.

Rules:
- Each reaction is 50-100 words
- Format: **[Advisor] reageert op [Other Advisor]:** "reaction text"
- Include at minimum: 1 disagreement, 1 reinforcement, 1 build-on
- The Contrarian should react to at least one other advisor

Natural tension pairs to draw from:
- Investeerder vs. CFO (growth vs. prudence)
- Strateeg vs. Operator (vision vs. execution)
- Technoloog vs. Marketeer (build vs. sell)
- Klant vs. Insider (user needs vs. market trends)
- Advocaat van de Duivel vs. anyone
- Peoplemanager vs. Operator (team wellbeing vs. efficiency)

### Step 6: Synthesis

Create an actionable summary with four sections:

**Consensuspunten / Consensus Points** -- 3-5 points where 3+ advisors agree

**Belangrijke spanningen / Key Tensions** -- 2-3 genuine disagreements with both sides articulated

**Kritische vragen / Critical Questions** -- The 5 most important questions the user should answer, synthesized across all advisor input

**Aanbevolen vervolgstappen / Recommended Next Steps** -- 5-7 concrete, prioritized actions. Each with:
- What to do
- Why (which advisor perspective supports this)
- Priority: hoog / middel (or high / medium in English)

### Step 7: Save the output

Save the report as a markdown file.

**Filename:** `Boardroom_[Topic-slug]_[YYYY-MM-DD].md`
Example: `Boardroom_Subscription-Model-AI-Training_2026-03-01.md`

**Location:** Current working directory, or `--output-dir` if specified.

After saving, display a brief summary in chat:
- The core question discussed
- Top 3 takeaways (one sentence each)
- Path to the saved file

## Output Template

Use this structure for the saved report:

```markdown
# Boardroom Report

**Vraag / Question:** [the business question]
**Datum / Date:** [current date]
**Taal / Language:** [NL/EN]

---

## De Adviseurs / The Advisors

| # | Adviseur | Perspectief |
|---|----------|-------------|
| 1 | De Strateeg | Marktpositionering, concurrentievoordeel |
| 2 | De Klant | Eindgebruikersperspectief, pijnpunten |
| 3 | De Investeerder | Schaalbaarheid, marktomvang |
| 4 | De CFO | Financiele gezondheid, kasstromen |
| 5 | De Technoloog | Technische haalbaarheid, innovatie |
| 6 | De Marketeer | Merk, boodschap, go-to-market |
| 7 | De Operator | Processen, efficientie, opschaling |
| 8 | De Insider | Domeintrends, regelgeving |
| 9 | De Peoplemanager | Team, cultuur, talent |
| 10 | De Advocaat van de Duivel | Risico's, blinde vlekken |

---

## Ronde 1: Individuele Adviezen

### 1. De Strateeg
[response]

### 2. De Klant
[response]

### 3. De Investeerder
[response]

### 4. De CFO
[response]

### 5. De Technoloog
[response]

### 6. De Marketeer
[response]

### 7. De Operator
[response]

### 8. De Insider
[response]

### 9. De Peoplemanager
[response]

### 10. De Advocaat van de Duivel
[response]

---

## Ronde 2: Boardroom Dynamiek

[cross-reactions]

---

## Synthese

### Consensuspunten
- [point]

### Belangrijke Spanningen
1. **[Tension]**: [Advisor A] vs. [Advisor B]: [description]

### Kritische Vragen
1. [question]

### Aanbevolen Vervolgstappen
1. **[Action]**: [why] | Prioriteit: hoog/middel

---

*Generated by the Boardroom Skill*
```

When output language is English, use English section headers and advisor names.

## Tone Guidelines

- Advisors speak directly, not academically
- They use "je/jij" (Dutch) or "you" (English), addressing the user personally
- Each advisor has a distinct voice (the VC talks about TAM and burn rate, the Customer talks about their actual experience, the Contrarian challenges with pointed questions)
- The synthesis is written in the voice of a neutral facilitator summarizing the session
- **Writing quality rules**: no throat-clearing ("It's important to note that..."), no emphasis crutches ("It's crucial/essential/vital"), no filler adverbs, no binary contrasts ("It's not just X, it's Y"), no false agency ("This approach unlocks..."), no passive voice, no vague declaratives. Use active voice, specific language, named actors.

## Skill Chain (optional)

After a boardroom session, these follow-up skills work well if you have them installed:
- `/generate-proposal` -- if the boardroom validated a client approach
- `/meeting-prep` -- use boardroom insights to prepare for an actual meeting
- `/docx` -- convert the report to a formatted Word document for sharing
