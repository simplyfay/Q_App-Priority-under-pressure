# Q — Priority Under Pressure
## Session Summary — Full Context for Claude Code
### Last updated: July 2026

---

## 1. The Product

**Name:** Q
**Type:** Web app (browser-based, not desktop — explicitly scoped to desktop browser in MVP)
**Tagline:** "Most days, you've got this. On the days you don't, Q does."
**Problem:** Professionals lose productive time not because they can't work, but because they can't decide what to work on when cognitive load peaks.
**HMW (short):** "How might we help professionals decide what to do next when cognitive load peaks?"
**HMW (long):** "How might we help professionals identify and commit to their most actionable next task during moments of peak cognitive load, so they spend less time deciding and more time doing?"

---

## 2. The Persona

**Name:** Sam Roy (previously Jordan — updated everywhere, no exceptions)
**Username:** @sam.roy
**Avatar initial:** S
**Test login credentials:** sam.roy@email.com / password: sam
**Role:** Account Manager · 4 years experience · SaaS company · 80-person team
Manages 18 client accounts. Day fragmented by Slack, email, calls, internal requests.
Tools: Asana, Notion, Slack, Google Calendar, sticky notes.
Goal: end every day knowing the most important work got done — not just whatever felt urgent.
Voice: "I know what needs to get done. I just can't always figure out what to start with when things get hectic."

After building, search entire codebase for "Jordan" and replace every instance with "Sam Roy".

---

## 3. The Six System States

Every screen maps to one of these. Do not invent, add, or rename states.

### AMBIENT
- Always-on passive view, under 10 seconds to read
- Copy: "Your day looks manageable." / "Things are building."
- Q present but not directing
- Day status card, progress bar at 40%, today's focus pills, stat row, "What Q is watching" card

### DETECT
- Behavioural signals (rapid task switching, idle time, interruption count) trigger a quiet offer
- No manual activation — Q comes to Sam
- Entry: floating pill — "Q has a suggestion"
- ANIMATION: rotating conic-gradient halo in --action-primary (#457B9D), full rotation 3s ease infinite, calm not urgent — communicates Q is actively working
- Copy names the signal: "You've switched tasks 4 times in 20 minutes"
- Two exits only: Accept / Dismiss

### ACTIVATE
- Q runs context-aware recalculation using four weighted signals
- Surfaces ONE task — not three, not a list
- 'Why' leads 'what': rationale tags appear ABOVE the task title
- Trust copy: "12 other tasks are waiting. They can."
- Primary: Start task / Secondary: View all tasks (collapsible)

### FOCUS
- Task list eliminated — active design decision, not simplification
- Notifications held — visible as count, not surfaced
- Progress bar advances: 25% → 50% → 75% → 100%, calm copy updates each step
- "Great progress. Keep going."
- Exit always accessible, never prominent
- Quietest screen in Q — designed to disappear

### DISRUPT + REORIENT
- Triggered when interruption arrives during Focus
- Current task remains visible, dimmed at 45% opacity behind prompt
- Q recalculates BEFORE the prompt appears
- Exact copy: "Q has recalculated. Your current task is still the priority."
- Binary only: "Continue current task" / "Switch to new priority"
- Sam always makes the final call

### CLEAR
- Copy: "You're clear. Nothing needs you right now."
- "Q will let you know when that changes."
- Maximum whitespace — the absence of UI is the message
- Three lines only. No buttons. No cards. Background: var(--surface-default) pure black.

---

## 4. The Four Priority Signals (used in ACTIVATE)

| Signal | Source | What it measures |
|--------|--------|-----------------|
| Deadline proximity | Calendar, task tools | How soon is this due vs remaining hours today? |
| Source authority | Email, Slack | Manager > client > peer > automated |
| Task age | Task tools, email timestamps | How long has this been waiting? |
| Calendar pressure | Calendar | How many free hours remain? |

---

## 5. Design Principles (enforce in every component)

### Confident Ally
Q is a trusted colleague with one clear recommendation. Not a list. Not an alarm. Not a dashboard.

### No warning colours
Never use red, orange, or amber for urgency. Urgency communicated through copy and hierarchy only.

### No gamification
No streaks, points, celebration animations, or confetti.
Progress copy is calm: "Great progress. Keep going."

### One thing at a time
Hick's Law: decision time increases with number and complexity of choices.
Activate: one task. Reorient: two buttons. Focus: one title.

### Transparent reasoning
Sam understands why before committing.
Rationale tags are not decorative — they are the reason Sam trusts Q.

### Minimalism over ambition
Prioritise functionality and minimalism over ambitious but broken design.
If a component is not needed for the flow, do not build it.

---

## 6. Exact Copy (do not paraphrase)

| Location | Copy |
|----------|------|
| Ambient heading | "Your day looks manageable." |
| Ambient sub | "3 tasks, 2 meetings, 4 hours free." |
| Detect pill | "Q has a suggestion" |
| Detect signal | "SIGNAL — 4 TASK SWITCHES IN 20 MINUTES" (DM Mono, uppercase) |
| Detect heading | "Things have shifted." |
| Detect body | "You've switched tasks 4 times in 20 minutes. Want help deciding what matters right now?" |
| Detect CTAs | "Yes, help me focus" / "I'm fine, thanks" |
| Activate label | "ACTIVATE — ONE TASK SURFACED" (DM Mono, uppercase) |
| Activate tags | "Urgent" · "High impact" · "Due 5pm · from your manager" |
| Activate title | "Review Q3 proposal draft" |
| Activate context | "Sarah flagged this 2 hours ago. Client presentation is at 5pm today." |
| Activate quiet | "12 other tasks are waiting. They can." (italic, muted) |
| Activate CTAs | "Start task" (primary) · "View all tasks" (secondary) |
| Focus progress | "Great progress. Keep going." |
| Focus held | "4 notifications held" (DM Mono pill) |
| Focus exit | "Exit focus mode" (text link, not button) |
| Reorient source | "New message from Sarah" |
| Reorient recalc | "Q has recalculated. Your current task is still the priority." |
| Reorient CTAs | "Continue current task" (primary) · "Switch to new priority" (ghost) |
| Clear title | "You're clear." |
| Clear sub | "Nothing needs you right now." |
| Clear note | "Q will let you know when that changes." |

---

## 7. UI Standards and Radius Decisions

Derived from reference UI (learning platform screenshot) and industry standards (Linear, Notion, Figma).

### Radius — apply these exactly
| Element | Token | Value |
|---------|-------|-------|
| Cards | radius/xl | 12px |
| Buttons (primary, secondary) | radius/md or radius/lg | 6–8px |
| Inputs | radius/md | 6px |
| Pills / tags / badges | radius/full | 9999px |
| Detect pill (with animation) | radius/full | 9999px |
| Sidebar nav icons | radius/lg | 8px |
| Avatar | radius/full | 9999px |

Never use exaggerated radius on rectangular interactive elements.

### Spacing — 8px grid
Use spacing tokens only: spacing/2 (8px), spacing/3 (12px), spacing/4 (16px), spacing/6 (24px)

### Typography
- Never more than 3 type sizes on one screen
- Category labels: small + uppercase + letter-spacing wide (DM Mono)
- Titles: semibold, tight tracking
- Body: regular, --content-secondary
- Metadata: small, --content-muted (never body copy — see contrast flag in Section 10)

### Touch targets
Minimum 44px height for all interactive elements.

### Elevation
- Cards: var(--surface-raised)
- Dropdowns/overlays: var(--surface-elevated)
- Sidebar: var(--surface-subtle)
- Page background: var(--surface-default)

---

## 8. Navigation Structure

### App navigation (inside the product)
Left sidebar — icon-only vertical nav, active state highlighted with --surface-elevated background
Icons represent the six states: Ambient · Detect · Activate · Focus · Reorient · Clear

Top bar:
- Left: Q logo mark
- Centre: search field
- Right: notification icon · Sam's avatar (initial "S", --color-navy-700 circle, --action-primary border)

### Landing page navigation (marketing)
Top bar, horizontal
Items: How it works · Use cases · Research · Pricing | Log in · Get started
Log in = ghost/outline button
Get started = filled primary button (--action-primary)

---

## 9. Screens to Build

Build in this order. Do not skip any screen.

### 1. Landing page
- Top nav with: How it works · Use cases · Research · Pricing | Log in · Get started
- Hero: tagline "Most days, you've got this. On the days you don't, Q does."
- How it works section: four states as visual flow (Detect → Activate → Focus → Reorient)
- CTA section
- Footer (minimal)

### 2. Log in page
- Email field with hint: sam.roy@email.com
- Password field
- Primary "Log in" button
- "Forgot password?" ghost link (no destination needed)
- Demo credentials shown subtly: "Demo: sam.roy@email.com / sam"
- On correct credentials → navigate to Dashboard (Ambient state)
- On wrong credentials → inline error message, no alert()
- Consistent with app visual language

### 3. Dashboard / Ambient state
- Top bar: Q logo left · search centre · notification icon · Sam avatar (S) right
- Left sidebar: icon nav, six states, active state highlighted
- Day status card: "Your day looks manageable." + progress bar at 40%
- Today's focus pills (non-actionable display)
- Stat row: Tasks · Meetings · Focus time · Alerts held
- "What Q is watching" card at bottom

### 4. Detect state
- Floating pill: "Q has a suggestion"
- Rotating conic-gradient halo animation around pill (--action-primary, 3s ease infinite)
- Signal line in DM Mono uppercase: "SIGNAL — 4 TASK SWITCHES IN 20 MINUTES"
- Heading: "Things have shifted."
- Body: "You've switched tasks 4 times in 20 minutes. Want help deciding what matters right now?"
- Two stacked buttons max-width 340px:
  Primary "Yes, help me focus" → Activate
  Ghost "I'm fine, thanks" → Ambient

### 5. Activate state
- Label in DM Mono uppercase: "ACTIVATE — ONE TASK SURFACED"
- Rationale tags ABOVE task title: "Urgent" · "High impact" · "Due 5pm · from your manager"
- Task title: "Review Q3 proposal draft"
- Context: "Sarah flagged this 2 hours ago. Client presentation is at 5pm today."
- Primary "Start task" → Focus
- Secondary "View all tasks" → collapsible task list below
- Italic muted below: "12 other tasks are waiting. They can."

### 6. Focus state
- Centred layout, maximum whitespace
- Task title only at large size
- Progress bar: 25% → 50% → 75% → 100% on button click, calm copy updates each step
- DM Mono pill: "4 notifications held"
- "Disruption arrived" button → Reorient
- "Exit focus mode" text link → Clear

### 7. Reorient state
- Current task visible at 45% opacity above prompt
- Interruption source label: "New message from Sarah"
- Q recalculation note (left border --action-primary, subtle blue tint background):
  "Q has recalculated. Your current task is still the priority."
- Primary "Continue current task" → Focus
- Ghost "Switch to new priority" → Activate

### 8. Clear state
- Three lines only, centred, no buttons, no cards
- Background: var(--surface-default) pure black
- "You're clear."
- "Nothing needs you right now."
- "Q will let you know when that changes."

### 9. Profile page
- Avatar: "S" initial, --color-navy-700 fill, --action-primary border
- Name: Sam Roy
- Role: Account Manager · SaaS · 18 client accounts
- Username: @sam.roy
- Editable fields: Name, Username, Role, Company size, Tools used
- Three preference toggles: Automatic detection · Hold notifications · Transparent reasoning
- Token reference grid at bottom (colour swatches from token system)

---

## 10. Token System

### Source of truth
Figma file: https://www.figma.com/design/aWwqnVauFDa3e2Yawbk3Af/Q---Priority-under-pressure
Never hardcode hex values. Always use CSS custom properties: var(--token-name).

### Token hierarchy
Primitives → Semantic → Component
Example: #457B9D → --color-slate-500 → --action-primary → button background

### Primitive tokens (58 total)
- Color: 21 tokens
- Typography: 25 tokens (1 family, 10 sizes, 5 weights, 5 tracking, 4 leading)
- Spacing: 12 tokens (named with numbers: 1, 2, 3, 4, 5, 6, 8, 10, 12, 16, 20, 24)
- Radius: 7 tokens (none, sm, md, lg, xl, 2xl, full)
- Shadow: 4 tokens (sm, md, lg, glow)

### Semantic tokens (29 total)
- surface: 5 — default (#000), subtle (#0A0A0A), raised (#1A1A1A), elevated (#2A2A2A), overlay (#3A3A3A)
- content: 5 — primary (#FFF), secondary (#BCBBC0), muted (#6B6B6B), inverse (#000), accent (#A8C4D8)
- line: 3 — default (#2A2A2A), subtle (#1A1A1A), strong (#3A3A3A)
- action: 7 — primary (#457B9D), primary-hover (#A8C4D8), primary-text (#FFF), secondary (#2A2A2A), secondary-hover (#3A3A3A), secondary-text (#D4D4D4), destructive (#7A3B1E)
- state: 5 — urgent (#457B9D), moderate (#BCBBC0), low (#3A3A3A), success (#2A7A4B), focus (#1D3557)
- ambient: 3 — calm (#1A1A1A), building (#2E4F7A), heavy (#1D3557)

### Window control tokens (decorative only — for macOS frame presentation)
Primitives:
- color/red/500 → #FF5F57
- color/yellow/500 → #FEBC2E
- color/green/500 → #28C840

Semantic (in action group):
- action/window/close → color/red/500
- action/window/minimise → color/yellow/500
- action/window/fullscreen → color/green/500

Note: Q is a web app. These tokens are decorative only if presenting inside a macOS frame.

### Contrast audit results (WCAG 2.1)
- ✓ content-primary (#FFF) on surface-default (#000): 21:1
- ✓ content-secondary (#BCBBC0) on surface-default: 11.01:1
- ✗ content-muted (#6B6B6B) on surface-default: 3.94:1 — FLAGGED
  → Only for large text (≥18px regular / ≥14px bold) and metadata
  → Restricted to: timestamps, meta labels, helper text, token labels, Clear screen note
  → NEVER use for body copy
- ✓ action-primary-text (#FFF) on action-primary (#457B9D): 4.59:1 — passes AA, low margin
  → Do not lighten --action-primary in future iterations without re-checking
- ✓ content-accent (#A8C4D8) on surface-raised (#1A1A1A): 9.57:1
- ✓ content-inverse (#000) on content-primary (#FFF): 21:1

### Figma variable naming convention
- Use slashes as separators: radius/sm, color/gray/800, spacing/1
- No double dashes in Figma (CSS syntax only)
- No hyphens as separators in Figma variable names
- Group name provides first path level — token name is second level only
  → Token "sm" inside group "radius" = full path radius/sm
  → Do NOT name it "radius/sm" inside the radius group — creates radius/radius/sm
- Spacing tokens named with numbers: 1, 2, 3 — never words (one, two, three)
- Typography lives in Text Styles, not standalone semantic variables
  → Text Style naming: {Scale}/{Weight} e.g. Label/Medium, Body/Regular, Display/Black

---

## 11. Token Rules — Non-Negotiable

- Read all tokens from Figma file using figma-use before writing any component
- Every colour, font size, spacing, radius must reference a CSS custom property
- No hardcoded hex values anywhere in the codebase
- --content-muted only for metadata/labels, never body copy
- No red, orange, or amber for urgency — use --action-primary and copy hierarchy only
- No gamification anywhere

---

## 12. Repository

**URL:** https://github.com/simplyfay/Q_App-Priority-under-pressure

### Folder structure
```
Q_App-Priority-under-pressure/
├── CLAUDE.md                        ← read automatically every session
├── SESSION-SUMMARY.md               ← this file — read every session
├── variables.css                    ← token source of truth
├── README.md
├── tokens/
│   └── system/
│       ├── primitives.css
│       ├── semantic.css
│       ├── tokens.json
│       └── token-conventions.md
├── components/
│   └── *.tsx
├── q-app/
└── .claude/
    └── commands/
        ├── extract-tokens.md        ← reads Figma file variables, not a URL
        ├── build-token-system.md
        ├── generate-components.md
        ├── build-web-app.md
        └── push-to-figma.md
```

---

## 13. Full Workflow Sequence

```bash
git clone https://github.com/simplyfay/Q_App-Priority-under-pressure.git
cd Q_App-Priority-under-pressure
claude
```

Inside Claude Code, run in this order:

```
/extract-tokens https://www.figma.com/design/aWwqnVauFDa3e2Yawbk3Af/Q---Priority-under-pressure
```
Reads Figma file variables (primitives → semantic → component). NOT a webpage scrape.
Outputs: tokens/raw.json

```
/build-token-system
```
Verifies derivation chain, runs WCAG contrast audit, outputs tokens/system/ files.

```
/generate-components
```
Scaffolds React components from token system and Figma component structures.

```
/build-web-app
```
Builds the full Q React app across all nine screens.

```
/push-to-figma https://www.figma.com/design/aWwqnVauFDa3e2Yawbk3Af/Q---Priority-under-pressure
```
Publishes token system and components back to the Q Figma file.

```bash
# Second terminal after /build-web-app completes
cd q-app && npm install && npm run dev
# Open http://localhost:5173
```

---

## 14. MCP Setup

### Connected servers
- Figma MCP: http://127.0.0.1:3845/mcp (HTTP) — connected (READ-ONLY)
- Figma plugin (write access): installed from claude-plugins-official
  Skills: figma-generate-library, figma-generate-design, figma-code-connect,
  figma-use-slides, figma-use-figjam, figma-generate-diagram, figma-create-new-file, figma-use
- Vercel MCP: https://mcp.vercel.com — needs authentication (not needed for Q)

### Critical note on Figma MCP
The local Figma MCP server (127.0.0.1:3845) is READ-ONLY.
Write access requires the figma-use skill from the Anthropic plugin.
Always load figma-use skill before any use_figma call.

---

## 15. Tech Stack

- React with Vite
- CSS custom properties only — no Tailwind, no MUI, no Chakra
- DM Sans (Google Fonts) for body copy
- DM Mono for system labels, metadata, token names
- React useState for navigation — no router library
- Deploy-ready: npm install && npm run dev

---

## 16. Out of Scope

- Onboarding / OAuth integrations
- Real authentication logic
- Settings and preference configuration
- Post-triage reflection
- Team / collaborative features
- Mobile app (architecture ready, deferred)
- Mood detection of any kind

---

## 17. Verification Checklist (run after every build)

1. All screens render without errors
2. grep -r "#[0-9a-fA-F]" src/components returns nothing
3. Navigation works across all screens
4. Log in with sam.roy@email.com / sam → navigates to Dashboard
5. Wrong credentials → inline error, no alert()
6. Detect → Activate → Focus → Reorient → Clear flow works end to end
7. Profile shows "Sam Roy" and avatar initial "S" — not Jordan, not Fay
8. Detect pill has rotating conic-gradient halo animation
9. Cards use radius/xl (12px), buttons use radius/md or radius/lg (6–8px)
10. No screen has more than 3 type sizes
11. --content-muted not used on any body copy
12. All touch targets minimum 44px height

---

## 18. Key Decisions

| Decision | Rationale |
|----------|-----------|
| One task surfaced, not three | Hick's Law — more choices at peak load adds load |
| Automatic detection, not manual toggle | Overloaded user can't recognise their own overload |
| GPS recalculation pattern | Q recalculates silently, surfaces one instruction |
| No warning colours | Q communicates confidence, not alarm |
| No semantic radius/spacing tokens at MVP | Primitives sufficient; semantic layer adds complexity without benefit |
| Typography in Text Styles not variables | Figma's native pattern for bundling size + weight + tracking |
| Web app not desktop | Problem occurs at the desk in a browser, not in a native app |
| Window control tokens decorative only | Q is a web app; macOS dots only appear in presentation frame mockups |
| Sam Roy not Jordan | Persona renamed — enforced everywhere including CLAUDE.md |
| Minimalism over ambition | Functionality and clarity first; broken ambitious design rejected |
| /extract-tokens reads Figma not a URL | Token source is the Q Figma file, not a scraped webpage |
