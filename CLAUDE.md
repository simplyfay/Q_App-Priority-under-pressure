# Q — Priority Under Pressure
## Claude Code Project Context

This file is read automatically at the start of every Claude Code session.
Do not skip or summarise it. Read it fully before executing any command.

---

## What this product is

Q is an AI-powered triage tool for overwhelmed knowledge workers.
It activates at the moment of overwhelm — when the plan has failed and decision fatigue has set in.
Q surfaces one clear, justified next action and steps back when the user regains clarity.

**Core tagline:** "Most days, you've got this. On the days you don't, Q does."

---

## The persona

**Jordan** — Account Manager, 4 years experience, SaaS company, 80-person team.
Manages 18 client accounts. Day fragmented by Slack, email, calls, internal requests.
Tools: Asana, Notion, Slack, Google Calendar, sticky notes.
Goal: end every day knowing the most important work got done — not just whatever felt urgent.
Voice: "I know what needs to get done. I just can't always figure out what to start with when things get hectic."

The persona's name is Jordan throughout. Not Fay. Not "the user." Jordan.

---

## The four system states — these are the product

Every screen, every component, every interaction maps to one of these states.
Do not invent states. Do not add states. Do not rename states.

### DETECT
Behavioural signals (rapid task switching, idle time, interruption count) trigger a quiet offer to help.
No manual activation. Q comes to Jordan.
Entry point: floating pill — "Q has a suggestion."
Copy names the signal, not a mood: "You've switched tasks 4 times in 20 minutes."
Two exits only: Accept / Dismiss.

### ACTIVATE
Q runs a context-aware recalculation using four weighted signals (see below).
Surfaces ONE task. Not three. Not a ranked list. One.
'Why' leads 'what': rationale tags appear ABOVE the task title.
Copy that earns trust: "12 other tasks are waiting. They can."
Primary: Start task. Secondary: View all tasks.

### FOCUS
Focus protection mode. Task list eliminated — active design decision, not simplification.
Notifications held — visible as a count, not surfaced.
Progress bar with calm copy: "Great progress. Keep going."
Exit always accessible, never prominent.
This is the quietest screen in Q. It should feel like it disappears.

### DISRUPT + REORIENT
Triggered when an interruption arrives during Focus.
Current task remains visible, dimmed behind the prompt.
Q has already recalculated BEFORE the prompt appears.
Exact copy: "Q has recalculated. Your current task is still the priority." (or "...priority has shifted.")
Binary: "Continue current task" / "Switch to new priority."
Jordan always makes the final call.

### CLEAR (bonus state)
No action required. Q has stepped back.
Copy: "You're clear. Nothing needs you right now."
Maximum whitespace. The absence of UI is the message.

### AMBIENT (companion state)
Always-on passive view. Under 10 seconds to read.
Quiet signal: load level, task count, free time, what's building.
Copy: "Your day looks manageable." / "Things are building."
Q is present but not directing.

---

## The four priority signals (used in ACTIVATE recalculation)

| Signal | Source | What it measures |
|--------|--------|-----------------|
| Deadline proximity | Calendar, task tools | How soon is this due vs remaining hours today? |
| Source authority | Email, Slack | Who is requesting? Manager > client > peer > automated |
| Task age | Task tools, email timestamps | How long has this been waiting? Stale = more urgent |
| Calendar pressure | Calendar | How many free hours remain? Less time = tighter triage |

---

## Design principles — enforce these in every component

### Confident Ally
Q is a trusted colleague who has assessed the situation and arrives with one clear recommendation.
Not a list. Not an alarm. Not a dashboard.
This principle governs copy, colour, and interaction design.

### No warning colours
Do not use red, orange, or amber for urgency signals anywhere in Q.
Urgency is communicated through copy and hierarchy, not colour panic.
Q communicates confidence, not alarm.

### No gamification
No streaks. No points. No celebration animations. No confetti.
Progress copy is calm: "Great progress. Keep going." — not "🎉 Amazing!"

### One thing at a time
Hick's Law: decision time increases with the number and complexity of choices.
More options at peak overload add load, not help.
In Activate: one task. In Reorient: two buttons. In Focus: one title.

### Transparent reasoning
Jordan understands why before committing.
Rationale tags are not decorative — they are the reason Jordan trusts Q.

---

## Design token system

Source of truth: `variables.css` in the project root.
All colours, typography, spacing, radius, and semantic values live there.
Do NOT hardcode any hex values, font sizes, or spacing values in components.
Always reference CSS custom properties: `var(--token-name)`.

### Token hierarchy
Primitives → Semantic → Component
Example derivation chain:
`#457B9D` → `--color-slate-500` → `--action-primary` → button background

### Emotional register in tokens
- `--ambient-*` tokens: calm, low-load states
- `--state-focus` / `--color-navy-*`: deep focus, active triage
- `--action-primary` (`#457B9D`): the one accent colour — used sparingly

---

## Figma file

URL: `https://www.figma.com/design/aWwqnVauFDa3e2Yawbk3Af/Test`
Page: `Test` (node-id `0-1`)
When pushing to Figma: derive all values from `tokens/system/tokens.json`.
Never re-read `variables.css` directly at push time — the JSON is the verified source.

---

## What is deliberately out of scope (do not build)

- Onboarding / OAuth integrations (Phase 2)
- Settings and preference configuration (Phase 2)
- Post-triage reflection (Phase 2)
- Team / collaborative features (Phase 2)
- Mobile app (architecture ready, deferred pending desktop validation)
- Mood detection of any kind (rejected in problem framing — Q detects behaviour, not emotion)

---

## Guardrails for every session

1. Read this file before running any command.
2. All token values must trace back to `variables.css`. No invented values.
3. The persona is Jordan. Not "the user." Not Fay.
4. The product is Q. Screens are system states. States are: Detect, Activate, Focus, Reorient, Clear, Ambient.
5. Copy follows the Confident Ally register: calm, direct, one recommendation.
6. If a command conflicts with this file, flag the conflict before proceeding.
