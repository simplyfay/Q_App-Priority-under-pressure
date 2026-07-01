# /build-web-app

Build the complete Q — Priority Under Pressure React web app across all nine screens.

**Reads:** `tokens/system/tokens.css`, `components/*.tsx`, `SESSION-SUMMARY.md`
**Writes:** `q-app/`

---

## Pre-flight

1. Confirm `components/` has at least Button, Card, Input, Tag, Avatar, DetectPill, Nav, TopBar.
   If missing, run `/generate-components` first.
2. Confirm `tokens/system/tokens.css` exists.
3. Re-read SESSION-SUMMARY.md sections 3, 6, 7, 8, and 9 before writing any screen.
4. Print: "Building Q web app — 9 screens, Sam Roy persona, light-first. Proceed? (yes/no)"
   Wait for approval.

---

## Project structure

```
q-app/
├── index.html
├── vite.config.js
├── package.json
└── src/
    ├── main.jsx
    ├── App.jsx              ← navigation state + layout shell
    ├── styles/
    │   ├── tokens.css       ← copied from tokens/system/tokens.css
    │   └── global.css       ← reset, body, font import
    └── screens/
        ├── Landing.jsx
        ├── Login.jsx
        ├── Ambient.jsx
        ├── Detect.jsx
        ├── Activate.jsx
        ├── Focus.jsx
        ├── Reorient.jsx
        ├── Clear.jsx
        └── Profile.jsx
```

---

## Global setup

### index.html
```html
<!DOCTYPE html>
<html lang="en" data-theme="light">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Q — Priority Under Pressure</title>
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link href="https://fonts.googleapis.com/css2?family=DM+Sans:wght@300;400;500;600&family=DM+Mono:wght@400;500&display=swap" rel="stylesheet" />
</head>
<body>
  <div id="root"></div>
  <script type="module" src="/src/main.jsx"></script>
</body>
</html>
```

Note `data-theme="light"` on `<html>` — light is the default.

### global.css
```css
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
body {
  font-family: 'DM Sans', system-ui, sans-serif;
  background: var(--surface-default);
  color: var(--content-primary);
  min-height: 100vh;
  font-size: 14px;
  line-height: 1.5;
  -webkit-font-smoothing: antialiased;
}
```

### App.jsx
- Manages `currentScreen` state (string)
- Manages `theme` state ('light' | 'dark') — sets `data-theme` on `document.documentElement`
- Landing and Login screens: full-width, no sidebar
- All other screens: sidebar + topbar shell layout, content area to the right
- Pass `navigate` function as prop to all screens

---

## Screen specifications

Build each screen as a separate file. Use exact copy from SESSION-SUMMARY.md section 6.
Use components from `components/`. No hardcoded hex values.

### Landing.jsx
Marketing page. No sidebar. Full-width layout.

Top nav:
- Logo: "Q" mark + "Priority Under Pressure" text
- Links: How it works · Use cases · Research · Pricing
- Right: "Log in" ghost button + "Get started" primary button
- Both buttons navigate to Login screen

Hero section:
- Heading (large, semibold, tight tracking): "Most days, you've got this."
- Subheading: "On the days you don't, Q does."
- Body: "Q recognises the moment your plan has broken down and surfaces one clear next action — so you stop deciding and start doing."
- CTA: "Get started free" primary button → Login

How it works section (4 steps in a row):
- Detect: "Q notices when you're overwhelmed — no tap required."
- Activate: "One task surfaces. The most important one."
- Focus: "Everything else waits. You work."
- Reorient: "Disruption arrives. Q recalculates. You decide."

Footer: minimal — "Q · Priority Under Pressure · 2026"

### Login.jsx
No sidebar. Centred card layout.

- Card: max-width 400px, centred, Card component
- Heading: "Welcome back"
- Sub: "Sign in to your Q workspace"
- Email input: label "Email", placeholder "sam.roy@email.com"
- Password input: label "Password", type password
- Primary button: "Log in" (full width)
- Ghost link: "Forgot password?" (no action needed)
- Demo note (small, muted): "Demo: sam.roy@email.com · password: sam"
- On submit: check email === "sam.roy@email.com" AND password === "sam"
  - Correct → navigate to Ambient
  - Wrong → show inline error below password field: "Incorrect email or password. Try the demo credentials above."
  - Never use alert()

### Ambient.jsx
Dashboard. Uses Nav + TopBar shell.

Content:
- Status line: small dot (green, var(--state-success)) + "MONITORING" in DM Mono uppercase, muted
- Day card (Card component):
  - Heading: "Your day looks manageable."
  - Sub: "3 tasks, 2 meetings, 4 hours free."
  - Progress label: "Day progress"
  - ProgressBar at 40%
  - Today's focus label
  - Pill row: "Review Q3 proposal draft" · "Prep for 2pm standup" · "Reply to client email"
- Stat row (3 columns):
  - Tasks today: 3
  - Meetings: 2
  - Focus time: 4h
- Card: "What Q is watching"
  - Body: "Task-switching rate · Idle time · Interruption frequency. No action needed right now. Q will surface an offer if signals change."

### Detect.jsx
Uses Nav + TopBar shell.

Content (centred column, max-width 480px):
- DetectPill component with rotating halo animation: "Q has a suggestion"
- Signal line (DM Mono, uppercase, muted): "SIGNAL — 4 TASK SWITCHES IN 20 MINUTES"
- Heading (large): "Things have shifted."
- Body: "You've switched tasks 4 times in 20 minutes. Want help deciding what matters right now?"
- Two stacked buttons (full width, max 340px):
  - Primary: "Yes, help me focus" → navigate to Activate
  - Ghost: "I'm fine, thanks" → navigate to Ambient

### Activate.jsx
Uses Nav + TopBar shell.

Content:
- Label (DM Mono, uppercase, muted): "ACTIVATE — ONE TASK SURFACED"
- Task card (Card component):
  - Tag row (above title): Tag urgent + Tag impact + Tag meta ("Due 5pm · from your manager")
  - Task title (large, semibold): "Review Q3 proposal draft"
  - Context (secondary): "Sarah flagged this 2 hours ago. Client presentation is at 5pm today."
  - Button row: "Start task" primary → Focus | "View all tasks" secondary (toggles list below)
- Quiet italic text (muted, centred): "12 other tasks are waiting. They can."
- Collapsible task list (hidden by default, shown on "View all tasks"):
  - 5 sample tasks with checkboxes
  - Checking a task marks it done (strikethrough + muted)

### Focus.jsx
Uses Nav + TopBar shell. Maximum whitespace. Centred layout.

Content:
- Label (DM Mono, uppercase, muted): small dot (action-primary colour) + "FOCUS · ACTIVE"
- Task title (26px, semibold, tight tracking): "Review Q3 proposal draft"
- ProgressBar (green variant) at starting 25%
- Progress label below bar (DM Mono, muted): "25% complete"
- Calm copy (italic, secondary): "Great progress. Keep going."
- Held pill (DM Mono, surface-raised, border): "4 notifications held"
- Button row:
  - Primary: "Mark 25% done" — clicking advances progress (25→50→75→100), updates label and copy
  - Secondary: "Disruption arrived" → navigate to Reorient
- Text link (muted, small): "Exit focus mode" → navigate to Clear

When progress reaches 100%:
- Copy: "Complete. Q is stepping back."
- Primary button changes to: "Complete — exit focus" → navigate to Clear

### Reorient.jsx
Uses Nav + TopBar shell.

Layout: two-panel stacked card (Card component wrapping both):

Top panel (dimmed at 45% opacity):
- Label: "FOCUS · ACTIVE (dimmed)"
- Task title: "Review Q3 proposal draft"
- Small progress bar at 25%
- Italic: "Great progress. Keep going."

Bottom panel (full opacity, separated by a line):
- From label (DM Mono, uppercase, muted): "NEW MESSAGE FROM SARAH"
- Message: "Re: Q3 proposal — can we push to 6pm?"
- Q recalculation note (left border 2px var(--action-primary), background rgba action-primary 8%):
  "Q has recalculated. Your current task is still the priority."
- Button row:
  - Primary: "Continue current task" → navigate to Focus
  - Ghost: "Switch to new priority" → navigate to Activate

### Clear.jsx
No sidebar. Full-screen centred layout.

- Background: var(--surface-default) — whatever the current theme's default surface is
- Three lines only, centred vertically and horizontally:
  - Heading (28px, semibold): "You're clear."
  - Sub (14px, secondary): "Nothing needs you right now."
  - Note (12px, muted, DM Mono): "Q will let you know when that changes."
- No buttons. No cards. Nothing else.

### Profile.jsx
Uses Nav + TopBar shell.

Profile hero:
- Avatar component: initial "S", size lg
- Name: "Sam Roy" (semibold, 20px)
- Role: "Account Manager · SaaS · 18 client accounts" (muted)
- Username: "@sam.roy" (DM Mono, action-primary colour)
- Status indicator: green dot + "Active"

Editable fields (Input component, 2-column grid):
- Full name: "Sam Roy"
- Username: "@sam.roy"
- Email: "sam.roy@email.com"
- Role: "Account Manager"
- Company size: "80-person SaaS team"
- Tools used: "Asana · Notion · Slack · Google Calendar"

Save/Cancel buttons below fields.

Q Preferences section (Card):
Three Toggle components:
- "Automatic detection" — Q monitors task-switching and idle patterns
- "Hold notifications during focus" — interruptions queued until session ends
- "Transparent reasoning" — show rationale tags on every surfaced task
All three default to on.

Token reference grid (small swatches at bottom):
- Show first 12 semantic colour tokens as 18×18px swatches with token name in DM Mono

---

## Navigation rules

- Landing and Login: no sidebar, no topbar
- All other screens: full shell (Nav sidebar + TopBar)
- Active nav item highlights with var(--surface-elevated) background
- Navigating via sidebar updates currentScreen in App.jsx
- All button navigations also update currentScreen

---

## After build

Run:
```bash
cd q-app && npm install && npm run dev
```

Verify checklist:
1. All 9 screens render without errors
2. `grep -r "#[0-9a-fA-F]" src/` returns nothing (or only acceptable primitives in tokens.css)
3. Log in with sam.roy@email.com / sam → navigates to Ambient
4. Wrong credentials → inline error, no alert()
5. Detect → Activate → Focus → Reorient → Clear flow works
6. Profile shows "Sam Roy" and avatar "S"
7. Detect pill has rotating halo animation
8. Theme toggle switches between light and dark
9. No screen has more than 3 type sizes
10. All interactive elements minimum 44px height

Print:
```
Q web app built.
  Screens: Landing · Login · Ambient · Detect · Activate · Focus · Reorient · Clear · Profile
  Theme: light-first, dark toggle available
  Dev server: http://localhost:5173
  Run: cd q-app && npm install && npm run dev
```
