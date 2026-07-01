# /extract-tokens

Extract all design tokens from the Q Figma file into `tokens/raw.json`.

**Source:** https://www.figma.com/design/aWwqnVauFDa3e2Yawbk3Af/Q---Priority-under-pressure
**Output:** `tokens/raw.json`

This command reads from Figma only. It does NOT scrape a webpage or curl a URL.

---

## Pre-flight

1. Load the `figma:figma-use` skill — mandatory before any `use_figma` call.
2. Confirm Figma MCP is connected. If not, stop and tell the user to run:
   `! claude mcp add --transport http figma http://127.0.0.1:3845/mcp`
3. Open the Q Figma file using the URL above.

---

## Phase 1 — Inspect (one read call)

Run one inspection call. Report back:
- All variable collections found: name, mode count, mode names, variable count
- Text style count
- Do NOT assume collection names or token counts — log what the file actually contains

Expected but not required:
- A `Primitives` collection (single mode)
- A `Tokens(semantic)` or `Design Tokens` collection (two modes: Light and Dark)

If names differ, adapt. Never stop because a name doesn't match expectations.

---

## Phase 2 — Extract Primitives

One `use_figma` call. Read every variable in the Primitives collection.

```js
const cols = await figma.variables.getLocalVariableCollectionsAsync();
const prim = cols.find(c => c.name === "Primitives");
const vars = await figma.variables.getLocalVariablesAsync();

const toHex = c => "#" + [c.r, c.g, c.b]
  .map(x => Math.round(x * 255).toString(16).padStart(2, "0")).join("").toUpperCase();

const primitives = {};
const primIdToName = {};
for (const v of vars) {
  if (v.variableCollectionId !== prim.id) continue;
  primIdToName[v.id] = v.name;
  const modeId = prim.modes[0].modeId;
  const raw = v.valuesByMode[modeId];
  primitives[v.name] = (v.resolvedType === "COLOR") ? toHex(raw) : raw;
}
return { primitives, primIdToName };
```

Group by first path segment: `color`, `spacing`, `radius`, `font`, `shadow`.

---

## Phase 3 — Extract Semantic tokens (both modes)

One `use_figma` call. Find the semantic collection (may be named `Tokens(semantic)` or `Design Tokens`).

Q has Light and Dark modes. Light is primary — it is the default app experience.
Dark mode is available as an optional toggle.

Use `defaultModeId` as the Light (primary) mode.
Capture both modes faithfully. Do not invent values for either mode.

```js
const dt = cols.find(c =>
  c.name === "Tokens(semantic)" || c.name === "Design Tokens"
);
const lightModeId = dt.defaultModeId;
const darkMode = dt.modes.find(m => m.modeId !== lightModeId);
const darkModeId = darkMode ? darkMode.modeId : null;

async function resolve(val) {
  if (val && val.type === "VARIABLE_ALIAS") {
    const target = await figma.variables.getVariableByIdAsync(val.id);
    const tCol = cols.find(c => c.id === target.variableCollectionId);
    const tVal = target.valuesByMode[tCol.modes[0].modeId];
    return { primitive: target.name, hex: toHex(tVal) };
  }
  return { primitive: null, hex: toHex(val) };
}

const semantic = {};
for (const v of vars) {
  if (v.variableCollectionId !== dt.id) continue;
  semantic[v.name] = {
    light: await resolve(v.valuesByMode[lightModeId]),
    dark: darkModeId ? await resolve(v.valuesByMode[darkModeId]) : null,
  };
}
return semantic;
```

Group by first path segment: `surface`, `content`, `line`, `action`, `state`, `ambient`.

---

## Phase 4 — Extract component tokens and text styles

**Component tokens:** if a third collection exists, read it the same way.
If not, derive component entries from semantic tokens:
```
buttonPrimary.background    → action/primary
buttonPrimary.textColor     → action/primary-text
buttonPrimary.borderRadius  → radius/md
card.background             → surface/raised
card.radius                 → radius/xl
input.background            → surface/raised
input.borderRadius          → radius/md
```
Mark these with `"extractedFrom": "figma-semantic"`.

**Text styles:**
```js
const styles = await figma.getLocalTextStylesAsync();
return styles.map(s => ({
  name: s.name,
  family: s.fontName.family,
  weight: s.fontName.style,
  size: s.fontSize,
  lineHeight: s.lineHeight,
  letterSpacing: s.letterSpacing,
}));
```

---

## Phase 5 — Write tokens/raw.json

```json
{
  "source": "https://www.figma.com/design/aWwqnVauFDa3e2Yawbk3Af/Q---Priority-under-pressure",
  "extractedAt": "<ISO timestamp>",
  "extractionMethod": "figma-variables",
  "modeStrategy": "light-first — light is default, dark is optional toggle",
  "colors": {
    "primitives": {},
    "semantic": {
      "light": {},
      "dark": {}
    }
  },
  "typography": {
    "fontFamilies": ["DM Sans", "DM Mono"],
    "textStyles": []
  },
  "spacing": {},
  "radius": {},
  "shadow": {},
  "components": {}
}
```

---

## Phase 6 — Print summary

Report actual counts found per collection and per group.
Flag any variable that failed to resolve (dangling alias).
Flag any token where light and dark values are identical (may be intentional, but worth noting).
