# /generate-components

Generate React components for the Q app from the verified token system.

**Reads:** `tokens/system/semantic.json`, `tokens/system/tokens.css`
**Writes:** `components/*.tsx`

---

## Pre-flight

1. Confirm `tokens/system/tokens.css` exists. If not, run `/build-token-system` first.
2. Read `tokens/system/semantic.json` to understand available token names.
3. Do not hardcode any hex values. All values must use `var(--token-name)`.

---

## Components to generate

Generate these components in order. Each maps to one or more Q screens.

### Button.tsx
Variants: `primary` | `secondary` | `ghost`
Sizes: `sm` | `md` | `lg`

- primary: background `var(--action-primary)`, text `var(--action-primary-text)`, radius `var(--radius-md)`
- secondary: background `var(--action-secondary)`, text `var(--action-secondary-text)`, border `1px solid var(--line-strong)`, radius `var(--radius-md)`
- ghost: background transparent, text `var(--content-muted)`, border `1px solid var(--line-default)`, radius `var(--radius-md)`
- All sizes minimum 44px height (touch target requirement)
- Font: DM Sans, weight medium (500)
- Hover state via useState + onMouseEnter/onMouseLeave

### Card.tsx
- background `var(--surface-raised)`
- border `1px solid var(--line-default)`
- radius `var(--radius-xl)` (12px)
- padding `var(--spacing-6)` (24px)
- No hardcoded shadows

### Input.tsx
- background `var(--surface-raised)`
- border `1px solid var(--line-default)`
- radius `var(--radius-md)` (6px)
- text `var(--content-primary)`
- placeholder text `var(--content-muted)`
- focus: border-color `var(--action-primary)`, outline none, box-shadow glow
- Minimum 44px height

### Tag.tsx
Variants: `urgent` | `impact` | `meta`
- All use `var(--radius-full)` (pill shape)
- Font: DM Mono, size 11px
- urgent: background rgba of action-primary at 15% opacity, text `var(--content-accent)`
- impact: background rgba of state-success at 15% opacity, text green
- meta: background `var(--surface-elevated)`, text `var(--content-muted)`

### Avatar.tsx
- Circle shape: `var(--radius-full)`
- Background: `var(--color-navy-700)` (hardcoded exception — this is a primitive, not a component decision)
- Border: `2px solid var(--action-primary)`
- Text: `var(--content-accent)`, DM Sans semibold
- Props: `initial` (string), `size` (sm | md | lg)

### DetectPill.tsx
The animated detect pill for the Detect state screen.

- Shape: pill (`var(--radius-full)`)
- Background: `var(--surface-elevated)`
- Border: `1px solid var(--line-strong)`
- Text: `var(--content-secondary)`, DM Sans 13px
- Pulsing dot: 6px circle, background `var(--action-primary)`, animation pulse 2s infinite

Rotating halo animation:
```css
@keyframes rotate-halo {
  from { transform: rotate(0deg); }
  to   { transform: rotate(360deg); }
}

.detect-halo {
  position: absolute;
  inset: -3px;
  border-radius: inherit;
  background: conic-gradient(
    var(--action-primary) 0deg,
    transparent 60deg,
    transparent 300deg,
    var(--action-primary) 360deg
  );
  animation: rotate-halo 3s linear infinite;
  z-index: -1;
}
```

Wrap the pill in a `position: relative` container to contain the halo.

### ProgressBar.tsx
- Track: `var(--surface-elevated)`, height 5px, radius `var(--radius-full)`
- Fill: `var(--state-success)`, transitions width 0.5s ease
- Props: `percent` (0-100), `variant` (calm | active)
- calm copy shown below: DM Sans 13px italic, `var(--content-secondary)`

### Toggle.tsx
- Track on: `var(--action-primary)`, off: `var(--surface-overlay)`
- Knob: white circle, transitions position
- Props: `on` (boolean), `onChange` (function)
- Minimum 44px touch target wrapper

### Nav.tsx
Left sidebar — icon-only vertical navigation for the app shell.

- Background: `var(--surface-subtle)`
- Border right: `1px solid var(--line-default)`
- Width: 56px
- Q logo mark at top: 26px square, background `var(--action-primary)`, text white bold "Q"
- Nav items: icon buttons for each state (Ambient, Detect, Activate, Focus, Reorient, Clear)
- Active state: background `var(--surface-elevated)`, radius `var(--radius-lg)`
- All nav items minimum 44×44px

### TopBar.tsx
Top bar for the app shell.

- Height: 52px
- Background: `var(--surface-subtle)`
- Border bottom: `1px solid var(--line-default)`
- Left: Q logo text "Priority Under Pressure", DM Sans 15px semibold
- Centre: search input (uses Input component, smaller variant)
- Right: notification icon + Avatar component (initial "S")
- Theme toggle button (sun/moon icon) — sets `data-theme="dark"` or `"light"` on `document.documentElement`

---

## Component conventions

- Functional components, TypeScript
- Inline style prop — no CSS files per component
- No external UI libraries
- Cast CSS vars where TypeScript needs a specific type:
  `'var(--font-weight-semibold)' as React.CSSProperties['fontWeight']`
- Hover: useState + onMouseEnter/onMouseLeave
- Focus: outline none on element, visible ring via box-shadow on wrapper
- Export: `export default ComponentName`

---

## After generating

List all components created. Confirm no hardcoded hex values:
```bash
grep -r "#[0-9a-fA-F]" components/
```
Only acceptable hardcoded hex: none. If any found, fix before proceeding.

Print:
```
Components generated: N
  Button · Card · Input · Tag · Avatar · DetectPill · ProgressBar · Toggle · Nav · TopBar
  No hardcoded hex values found.

Ready for /build-web-app
```
