---
name: thmanyah-icon
description: >-
  Generate a new icon for the Thmanyah icon library in Figma, following the
  locked-down style spec: 24px canvas, 1.5px stroke, white color, outer-rounded /
  inner-sharp corners, outline + fill variants. Triggers on: add icon, new
  icon, generate icon, thmanyah icon, icon library, create icon, draw icon.
---

# Thmanyah Icon Generator

Generates new icons directly into the Thmanyah Figma icon library using the
`use_figma` tool. When invoked, follow all steps in this skill exactly.

---

## Invocation Format

```
/thmanyah-icon [icon name or description] in [Category]
```

Examples:
- `/thmanyah-icon Trophy in Sport`
- `/thmanyah-icon Wifi in Device / System`
- `/thmanyah-icon Filter in Action`

If no category is given, infer it from the icon's function.

---

## Style Spec (never deviate from these)

| Property | Value |
|---|---|
| Canvas | 24 × 24 px |
| Safe zone (wide icons) | x: 2–22, y: 2–22 (20 × 20 content area) |
| Safe zone (tall icons) | x: 2–22, y: 3–21 (20 × 18 content area) |
| **Color** | **White — `{ r: 1, g: 1, b: 1 }`** (never black) |
| Stroke weight | 1.5 |
| Stroke cap | `ROUND` |
| Stroke join | `MITER` |
| Outer corner radius | 3 px (rect shapes), 4 px (nearly circular) |
| Inner corner radius | 1 px (notches/cutouts), 0 px (tight angles) |
| Outline variant | 1.5px white stroke, no fill inside closed paths |
| Fill variant | Solid white fill on main body, no stroke. Open/structural paths (handles, stems) stay as white strokes |
| Complexity | Simple — max 3 paths per icon, no fine detail |

### The outer-soft / inner-sharp rule
- Any **exterior** boundary → smooth rounded corner (3–4 px)
- Any **interior** notch, joint, or cutout → 1 px radius or hard corner
- Example: a rounded-rect frame with a crisp notch punched into it

### Path types
- **Closed paths** (body shapes) → stroke only in Outline; fill only in Fill
- **Open paths** (handles, arcs, structural lines) → always stroke, in both variants

---

## Figma File Reference

**File key:** `f8WMCkLG2FfBTIyMfWyl4i`

**Category frames (node IDs):**

| Category | Node ID |
|---|---|
| Navigation | `125:62` |
| Arrows | `120:401` |
| Action | `120:406` |
| Media Controls | `120:446` |
| Sport | `120:441` |
| Communication | `125:105` |
| User & Account | `120:451` |
| Status / Confirmation | `120:399` |
| Device / System | `123:57` |
| Misc / Utility | `125:119` |

**Naming convention:** `[Icon Name]` (outline) and `[Icon Name], Fill` — matches existing library (e.g. `Goal` / `Goal, Fill`, `Star` / `Star, Fill`).

---

## Design Reference Library

Before designing any icon, consult **[REFERENCES.md](REFERENCES.md)** — it contains
shape blueprints, path structures, and style notes for every Thmanyah category,
sourced from Lucide, Tabler, Mingcute, and Phosphor.

---

## Step-by-Step Process

### Step 1 — Design the icon mentally

Before writing any code:
1. **Check REFERENCES.md** for the target icon category — use existing path structures as shape blueprints
2. Identify the visual concept and reduce to the simplest shape (1–3 paths maximum)
3. Classify each path: **closed** (body) or **open** (handle/arc/line)
4. Map all key points onto the 24×24 grid, ensuring centering at (12, 12)
5. Note outer corners (3–4 px) vs inner corners (sharp/1 px)

Document reasoning in a comment block at the top of the code.

### Step 2 — Build the SVG path strings

- Use absolute commands only: `M` `L` `C` `Q` `A` `Z`
- `Q cx cy ex ey` for outer rounded corners (control point AT the corner, endpoints ON the edges)
- Keep inner joints as hard `L` turns (0–1 px radius in path)
- Verify centering: leftmost x + rightmost x = 24, topmost y + bottommost y = 24

### Step 3 — Write the Figma Plugin code

Use the template below. Call `use_figma` with your substituted paths.

### Step 4 — Verify with a screenshot

After creation, call `get_screenshot` on both component node IDs. If anything looks off (off-center, wrong weight, wrong color), iterate.

---

## Figma Plugin Code Template

```javascript
// ============================================================
// THMANYAH ICON: [ICON_NAME] → [CATEGORY]
// ============================================================
// Design reasoning:
//   [Describe the shape breakdown and path decisions here]
// ============================================================

const TARGET_FRAME_ID = '[CATEGORY_NODE_ID]';
const ICON_NAME = '[ICON_NAME]';
const STROKE_WEIGHT = 1.5;
const CANVAS = 24;
const WHITE = { r: 1, g: 1, b: 1 };

// ── Your paths ──────────────────────────────────────────────
// Closed body path (used as stroke in Outline, fill in Fill)
const BODY_PATH = '[YOUR_CLOSED_PATH]';

// Open paths — always stroke in both variants (handles, arcs, lines)
// const HANDLE_L = '[YOUR_OPEN_PATH]';
// const HANDLE_R = '[YOUR_OPEN_PATH]';

// ── Helpers ─────────────────────────────────────────────────
async function importPath(pathD) {
  // Transparent rect forces Figma to preserve the full 24×24 coordinate space
  const svg =
    `<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" width="24" height="24">` +
    `<rect width="24" height="24" fill="none"/>` +
    `<path d="${pathD}" fill="none" stroke="none"/>` +
    `</svg>`;
  const group = figma.createNodeFromSvg(svg);
  let vec = null;
  const find = (n) => {
    if (n.type === 'VECTOR') { vec = n; return; }
    if ('children' in n) n.children.forEach(find);
  };
  find(group);
  if (!vec) { group.remove(); return null; }
  figma.currentPage.appendChild(vec);
  group.remove();
  vec.x = 0; vec.y = 0;
  return vec;
}

function asStroke(vec) {
  vec.strokes = [{ type: 'SOLID', color: WHITE, opacity: 1 }];
  vec.fills   = [];
  vec.strokeWeight = STROKE_WEIGHT;
  vec.strokeCap    = 'ROUND';
  vec.strokeJoin   = 'MITER';
}

function asFill(vec) {
  vec.fills   = [{ type: 'SOLID', color: WHITE }];
  vec.strokes = [];
}

function makeFrame(name) {
  const f = figma.createFrame();
  f.name = name;
  f.resize(CANVAS, CANVAS);
  f.fills = [];
  f.clipsContent = true;
  return f;
}

function getNextPosition(frame) {
  if (!frame.children.length) return { x: 80, y: 80 };
  let maxY = 80;
  for (const c of frame.children) if (c.y > maxY) maxY = c.y;
  const lastRow = frame.children.filter(c => Math.abs(c.y - maxY) < 2);
  let maxX = 80;
  for (const c of lastRow) if (c.x > maxX) maxX = c.x;
  const nextX = maxX + 32;
  if (nextX + CANVAS > frame.width - 16) return { x: 80, y: maxY + 32 };
  return { x: nextX, y: maxY };
}

// ── Build variants ───────────────────────────────────────────
async function buildOutline() {
  const frame = makeFrame(ICON_NAME);
  const body = await importPath(BODY_PATH);
  if (body) { asStroke(body); frame.appendChild(body); body.x = 0; body.y = 0; }
  // Uncomment if you have open paths:
  // const lh = await importPath(HANDLE_L);
  // if (lh) { asStroke(lh); frame.appendChild(lh); lh.x = 0; lh.y = 0; }
  return frame;
}

async function buildFill() {
  const frame = makeFrame(`${ICON_NAME}, Fill`);
  const body = await importPath(BODY_PATH);
  if (body) { asFill(body); frame.appendChild(body); body.x = 0; body.y = 0; }
  // Open paths stay as strokes even in fill variant:
  // const lh = await importPath(HANDLE_L);
  // if (lh) { asStroke(lh); frame.appendChild(lh); lh.x = 0; lh.y = 0; }
  return frame;
}

// ── Main ─────────────────────────────────────────────────────
async function run() {
  const frame = await figma.getNodeByIdAsync(TARGET_FRAME_ID);
  if (!frame || frame.type !== 'FRAME') {
    figma.notify(`❌ Frame ${TARGET_FRAME_ID} not found`, { error: true });
    return null;
  }

  const outlineComp = figma.createComponentFromNode(await buildOutline());
  const fillComp    = figma.createComponentFromNode(await buildFill());

  const pos = getNextPosition(frame);
  frame.appendChild(outlineComp);
  outlineComp.x = pos.x;
  outlineComp.y = pos.y;

  frame.appendChild(fillComp);
  fillComp.x = pos.x + 32;
  fillComp.y = pos.y;

  const neededH = pos.y + CANVAS + 16;
  if (neededH > frame.height) frame.resize(frame.width, neededH);

  figma.notify(`✅ "${ICON_NAME}" added!`);
  return { outlineId: outlineComp.id, fillId: fillComp.id };
}

return run();
```

---

## Corner Radius in Code

```javascript
// Rounded rect (use for simple container shapes instead of a path)
const rect = figma.createRectangle();
rect.resize(18, 18);
rect.x = 3; rect.y = 3;
rect.cornerRadius = 3;           // outer corners
rect.fills = [{ type: 'SOLID', color: WHITE }];
```

---

## Worked Example: Trophy Icon

**Category:** Sport (`120:441`)
**Concept:** Cup body + side handles + stem + base

**Design reasoning:**
- Main body = 1 closed path (cup + stem + base), outer top corners r=3, outer base corners r=2, inner joints sharp
- Side handles = 2 open arc paths (extend to x=3 / x=21 for symmetry)
- Outline: body as stroke, handles as stroke
- Fill: body as fill, handles as stroke (open paths stay stroked in fill mode)
- Centered: x-center=12, y-center=(3+21)/2=12 ✓

```
Body path (clockwise):
M 6 6 Q 6 3 9 3 L 15 3 Q 18 3 18 6
L 18 14 L 14 16 L 14 18
L 17 18 L 17 19 Q 17 21 15 21
L 9 21 Q 7 21 7 19
L 7 18 L 10 18 L 10 16 L 6 14 Z

Left handle (open arc):  M 6 7 Q 3 7 3 10 Q 3 13 6 13
Right handle (open arc): M 18 7 Q 21 7 21 10 Q 21 13 18 13
```

---

## Checklist Before Finishing

- [ ] Color is WHITE (`{ r:1, g:1, b:1 }`) — never black
- [ ] Icon fits within 24×24 with 2–3px padding
- [ ] Horizontally and vertically centered at (12, 12)
- [ ] Outline: 1.5px white stroke, no fill on closed paths
- [ ] Fill: solid white on body, white stroke on open/structural paths
- [ ] Outer corners smooth (3–4 px), inner joints crisp (0–1 px)
- [ ] Max 3 paths total
- [ ] Named `[Icon Name]` and `[Icon Name], Fill`
- [ ] Placed in correct category frame
- [ ] Screenshot verified after creation
