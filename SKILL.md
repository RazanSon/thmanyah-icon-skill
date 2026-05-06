---
name: thmanyah-icon
description: >-
  Generate a new icon for the Thmanyah icon library in Figma, following the
  locked-down style spec: 24px canvas, 1.5px stroke, outer-rounded /
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
| Stroke weight | 1.5 |
| Stroke cap | `ROUND` |
| Stroke join | `MITER` |
| Outer corner radius | 3 px (rect shapes), 4 px (nearly circular) |
| Inner corner radius | 1 px (notches/cutouts), 0 px (tight angles) |
| Fill variant | Same shape, no stroke, solid fill (`#000000` / currentColor) |
| Outline variant | Stroke only, no fill inside closed paths |
| Complexity | Simple — max 2–3 paths per icon, no fine detail |

### The outer-soft / inner-sharp rule
- Any **exterior** boundary → smooth rounded corner (3–4 px)
- Any **interior** notch, joint, or cutout → 1 px radius or hard corner
- Example: a rounded-rect frame with a crisp notch punched into it

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

---

## Step-by-Step Process

### Step 1 — Design the icon mentally

Before writing any code:
1. Identify the visual concept (what does this icon represent?)
2. Reduce it to the simplest possible shape (1–3 paths maximum)
3. Map all key points onto the 24×24 grid
4. Note which corners should be outer-rounded (3–4 px) vs inner-sharp (1 px)
5. State which paths will appear in Outline only vs both variants

Document your design thinking in a short comment block at the top of the code.

### Step 2 — Build the SVG path string

Express the icon as one or more SVG path `d` strings using absolute coordinates
on a 24×24 grid. Guidelines:
- `M` `L` `C` `Q` `A` `Z` only (no relative commands)
- Use `Q` (quadratic bezier) for outer rounded corners — control point at the
  corner, endpoints on the edge: `Q cx cy ex ey`
- For Figma corner radius on vector vertices, set it in the vectorNetwork
  rather than encoding it in the path curve where possible

### Step 3 — Write the Figma Plugin code

Call `use_figma` with the JavaScript below, substituting your actual paths and
target frame ID. See the template in the next section.

### Step 4 — Verify with a screenshot

After committing, call `get_screenshot` on the newly created component node ID
to confirm the icon looks correct. If not, iterate.

---

## Figma Plugin Code Template

```javascript
// ============================================================
// THMANYAH ICON GENERATOR
// Icon: [ICON_NAME]
// Category: [CATEGORY]
// ============================================================

const FILE_KEY = 'f8WMCkLG2FfBTIyMfWyl4i';
const TARGET_FRAME_ID = '[CATEGORY_NODE_ID]'; // e.g. '125:62'
const ICON_NAME = '[ICON_NAME]'; // e.g. 'Trophy'

// Style constants
const CANVAS = 24;
const STROKE_WEIGHT = 1.5;
const OUTER_RADIUS = 3;
const INNER_RADIUS = 1;

// ── Helper: create a 24×24 icon frame ───────────────────────
function makeIconFrame(name) {
  const frame = figma.createFrame();
  frame.name = name;
  frame.resize(CANVAS, CANVAS);
  frame.fills = [];
  frame.clipsContent = true;
  return frame;
}

// ── Helper: create a vector node from SVG path data ─────────
async function makeVector(svgPath, opts = {}) {
  const node = figma.createNodeFromSvg(`<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"><path d="${svgPath}" /></svg>`);
  // Flatten the imported group to get the vector
  const vec = node.children[0];
  if (vec.type === 'VECTOR') {
    if (opts.stroke) {
      vec.strokes = [{ type: 'SOLID', color: { r: 0, g: 0, b: 0 }, opacity: 1 }];
      vec.strokeWeight = STROKE_WEIGHT;
      vec.strokeCap = 'ROUND';
      vec.strokeJoin = 'MITER';
      vec.fills = opts.filled ? [{ type: 'SOLID', color: { r: 0, g: 0, b: 0 } }] : [];
    }
    if (opts.filled && !opts.stroke) {
      vec.fills = [{ type: 'SOLID', color: { r: 0, g: 0, b: 0 } }];
      vec.strokes = [];
    }
  }
  node.remove(); // detach from page
  return vec;
}

// ── Build OUTLINE variant ───────────────────────────────────
async function buildOutline() {
  const frame = makeIconFrame(`${ICON_NAME} – Outline`);

  // TODO: Replace with your actual SVG path(s)
  // Example for a simple rounded rect:
  const PATH_OUTLINE = 'M5 3 H19 Q21 3 21 5 V19 Q21 21 19 21 H5 Q3 21 3 19 V5 Q3 3 5 3 Z';

  const vec = await makeVector(PATH_OUTLINE, { stroke: true, filled: false });
  frame.appendChild(vec);
  return frame;
}

// ── Build FILL variant ──────────────────────────────────────
async function buildFill() {
  const frame = makeIconFrame(`${ICON_NAME} – Fill`);

  // TODO: Same shape as outline but as a filled path
  const PATH_FILL = 'M5 3 H19 Q21 3 21 5 V19 Q21 21 19 21 H5 Q3 21 3 19 V5 Q3 3 5 3 Z';

  const vec = await makeVector(PATH_FILL, { stroke: false, filled: true });
  frame.appendChild(vec);
  return frame;
}

// ── Assemble component set ──────────────────────────────────
async function run() {
  // 1. Find the target category frame
  const targetFrame = await figma.getNodeByIdAsync(TARGET_FRAME_ID);
  if (!targetFrame || targetFrame.type !== 'FRAME') {
    figma.notify(`❌ Target frame ${TARGET_FRAME_ID} not found`, { error: true });
    return;
  }

  // 2. Build the two variant frames
  const outlineFrame = await buildOutline();
  const fillFrame = await buildFill();

  // 3. Promote to components
  const outlineComp = figma.createComponentFromNode(outlineFrame);
  const fillComp = figma.createComponentFromNode(fillFrame);

  // 4. Set variant properties
  outlineComp.name = `Variant=Outline`;
  fillComp.name = `Variant=Fill`;

  // 5. Combine into a component set
  const compSet = figma.combineAsVariants([outlineComp, fillComp], figma.currentPage);
  compSet.name = ICON_NAME;

  // 6. Place inside the category frame
  // Position after the last existing icon in the frame
  const lastChild = targetFrame.children[targetFrame.children.length - 1];
  const xPos = lastChild ? lastChild.x + CANVAS + 8 : 8;
  const yPos = lastChild ? lastChild.y : 8;
  compSet.x = targetFrame.absoluteTransform[0][2] + xPos - targetFrame.absoluteTransform[0][2];
  compSet.y = targetFrame.absoluteTransform[1][2] + yPos - targetFrame.absoluteTransform[1][2];

  targetFrame.appendChild(compSet);

  figma.notify(`✅ Icon "${ICON_NAME}" added to ${targetFrame.name}`);
  return compSet.id;
}

run();
```

---

## Corner Radius Reference

When you can't encode radii in the path curve, use Figma's corner radius API
on rectangle nodes instead of a full vector path:

```javascript
// Rounded rect with outer-smooth / inner-sharp
const rect = figma.createRectangle();
rect.resize(18, 18);
rect.x = 3; rect.y = 3;
rect.cornerRadius = 3; // outer
// For per-corner: rect.topLeftRadius / topRightRadius / etc.
rect.fills = [{ type: 'SOLID', color: { r:0, g:0, b:0 } }];
```

For a vector with specific per-vertex radii, set `cornerRadius` on each vertex
in the `vectorNetwork`:

```javascript
vec.vectorNetwork = {
  vertices: [
    { x: 3,  y: 3,  cornerRadius: 3 }, // outer corner → 3px
    { x: 21, y: 3,  cornerRadius: 3 },
    { x: 21, y: 21, cornerRadius: 3 },
    { x: 3,  y: 21, cornerRadius: 3 },
    // inner notch vertex:
    { x: 12, y: 8,  cornerRadius: 1 }, // inner → 1px
  ],
  segments: [ /* ... */ ],
  regions: [ /* ... */ ]
};
```

---

## Worked Example: "Filter" Icon

**Category:** Action (`120:406`)
**Concept:** A funnel shape — wide at top, narrow at bottom

**Design reasoning:**
- Wide trapezoid top (y=4, x=3–21), narrows to (y=14, x=8–16)
- Stem from (8,14) to (16,14) down to (10,20)–(14,20)
- Outer top corners: 3px radius
- Inner corners where funnel meets stem: 1px
- Both variants use the same path

```
Outline path:
M 5 4 Q 3 4 3 6 L 8 14 L 8 20 Q 8 21 9 21 L 15 21 Q 16 21 16 20 L 16 14 L 21 6 Q 21 4 19 4 Z
```

**Implementation:** Plug this path into both `PATH_OUTLINE` and `PATH_FILL` in the
template. For Fill: `filled: true`, no stroke. For Outline: `stroke: true`, no fill.

---

## Checklist Before Committing

- [ ] Icon fits entirely within 24×24 with 2–3px padding on each side
- [ ] Outline variant has 1.5px stroke, no fill in closed areas
- [ ] Fill variant has solid fill, no stroke
- [ ] Outer corners feel smooth (3–4px radius)
- [ ] Inner joints feel crisp (0–1px radius)
- [ ] Icon is simple — max 2–3 paths, recognizable at small sizes
- [ ] Component is named `[Icon Name]` with `Variant = Outline / Fill`
- [ ] Component set is placed inside the correct category frame
- [ ] Screenshot taken and confirmed after creation
