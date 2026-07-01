# /build-token-system

Convert `tokens/raw.json` into a verified, structured token system with light-first CSS output.

**Reads:** `tokens/raw.json`
**Writes:** `tokens/system/primitives.json`, `tokens/system/semantic.json`, `tokens/system/tokens.css`, `tokens/system/token-conventions.md`

---

## Mode strategy

Q is light-first. Light mode is the default experience (`：root`).
Dark mode is an optional toggle (`[data-theme="dark"]`).
This is not negotiable — do not invert this.

---

## Step 1 — Read and validate

Read `tokens/raw.json`. Confirm it was extracted from Figma (not a webpage).
If `extractionMethod` is not `figma-variables`, stop and tell the user to run `/extract-tokens` first.

Count and report:
- Primitive tokens by group (color, spacing, radius, font, shadow)
- Semantic tokens by group (surface, content, line, action, state, ambient)
- Both light and dark values present for each semantic token

---

## Step 2 — Build primitives.json

One entry per primitive token. Raw values only. No semantic meaning.

```json
{
  "color": {
    "black": { "100": "#000000" },
    "gray": { "950": "#0A0A0A", "900": "#111111", "800": "#1A1A1A" },
    "slate": { "500": "#457B9D", "300": "#A8C4D8" }
  },
  "spacing": { "1": 4, "2": 8, "3": 12, "4": 16, "6": 24, "8": 32 },
  "radius": { "none": 0, "sm": 4, "md": 6, "lg": 8, "xl": 12, "2xl": 16, "full": 9999 },
  "shadow": { "sm": "0 1px 2px rgba(0,0,0,0.4)", "md": "0 2px 8px rgba(0,0,0,0.5)" },
  "typography": {
    "family": { "body": "DM Sans", "mono": "DM Mono" },
    "size": { "xs": 11, "sm": 12, "base": 14, "md": 15, "lg": 16, "xl": 18, "2xl": 20, "3xl": 24 },
    "weight": { "light": 300, "regular": 400, "medium": 500, "semibold": 600, "bold": 700 }
  }
}
```

---

## Step 3 — Build semantic.json

Every semantic token needs both light and dark values.
Values reference primitives using `{group.name.step}` syntax.

```json
{
  "surface": {
    "default":  { "light": "{color.white.100}",  "dark": "{color.black.100}" },
    "subtle":   { "light": "{color.gray.50}",    "dark": "{color.gray.950}" },
    "raised":   { "light": "{color.gray.100}",   "dark": "{color.gray.800}" },
    "elevated": { "light": "{color.gray.200}",   "dark": "{color.gray.700}" }
  },
  "content": {
    "primary":   { "light": "{color.gray.900}",  "dark": "{color.white.100}" },
    "secondary": { "light": "{color.gray.600}",  "dark": "{color.gray.400}" },
    "muted":     { "light": "{color.gray.400}",  "dark": "{color.gray.500}" },
    "accent":    { "light": "{color.slate.500}", "dark": "{color.slate.300}" }
  },
  "action": {
    "primary":       { "light": "{color.slate.500}", "dark": "{color.slate.500}" },
    "primary-hover": { "light": "{color.slate.300}", "dark": "{color.slate.300}" },
    "primary-text":  { "light": "{color.white.100}", "dark": "{color.white.100}" }
  }
}
```

Use the actual values from `raw.json` — the above is the structure, not the values.
Do not invent values. If a light value is missing from raw.json, flag it and ask before proceeding.

---

## Step 4 — WCAG contrast audit

Check every text/surface pairing for both modes.

Required pairs:
- content/primary on surface/default — both light and dark
- content/secondary on surface/default — both light and dark
- content/muted on surface/default — both light and dark (known flag: may fail AA for normal text)
- action/primary-text on action/primary — both light and dark

Thresholds:
- Normal text (< 18px regular / < 14px bold): 4.5:1 minimum
- Large text / UI components: 3:1 minimum

Flag failures. Do not block writing files for known acceptable failures
(content/muted is documented as large-text/metadata only — 3.94:1 on dark surface is acceptable).

---

## Step 5 — Write tokens.css

Light-first structure. This is the CSS file the app imports.

```css
/* ── Primitives — static, no modes ── */
:root {
  --color-black-100: #000000;
  --color-white-100: #FFFFFF;
  --color-gray-950: #0A0A0A;
  /* ... all primitives ... */
  --radius-sm: 4px;
  --radius-md: 6px;
  --radius-lg: 8px;
  --radius-xl: 12px;
  --radius-full: 9999px;
  --spacing-1: 4px;
  --spacing-2: 8px;
  /* ... */
}

/* ── Semantic — Light (default) ── */
:root,
[data-theme="light"] {
  --surface-default: var(--color-white-100);
  --surface-raised: var(--color-gray-100);
  --content-primary: var(--color-gray-900);
  --content-secondary: var(--color-gray-600);
  --content-muted: var(--color-gray-400);
  --action-primary: var(--color-slate-500);
  --action-primary-text: var(--color-white-100);
  /* ... all semantic light values ... */
}

/* ── Semantic — Dark (toggle) ── */
[data-theme="dark"] {
  --surface-default: var(--color-black-100);
  --surface-raised: var(--color-gray-800);
  --content-primary: var(--color-white-100);
  --content-secondary: var(--color-gray-400);
  --content-muted: var(--color-gray-500);
  --action-primary: var(--color-slate-500);
  --action-primary-text: var(--color-white-100);
  /* ... all semantic dark values ... */
}
```

Use actual values from semantic.json — the above is the structure only.

---

## Step 6 — Write token-conventions.md

Include:
- Primitive → semantic derivation table (every semantic token with its light and dark primitive source)
- Contrast audit results for both modes
- Mode strategy: light-first, dark via [data-theme="dark"]
- content/muted restriction: metadata and large text only, never body copy

---

## Step 7 — Confirm

Print:
```
Token system built.
  Primitive tokens: N
  Semantic tokens: N (N light values, N dark values)
  Contrast pairs audited: N
  Flags: list any
  Mode strategy: light-first — [data-theme="dark"] for dark mode

Ready for /generate-components
```
