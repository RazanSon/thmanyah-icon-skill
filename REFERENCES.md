# Thmanyah Icon — Design Reference Library

Shape references from open-source icon libraries. Use these to understand the
**visual structure** of each icon concept, then redraw in Thmanyah style:
absolute coordinates, white color, 1.5px stroke, outer-rounded / inner-sharp.

> **How to read these:** Paths use relative commands (lowercase). Treat them as
> shape blueprints, not direct copies. The key info is the **structure** (how
> many paths, open vs closed, which corners round).

---

## Navigation

### Home
```
# Lucide — 2 paths: outer house shape (closed) + door rect (closed)
house: M3 10a2 2 0 0 1 .709-1.528l7-6a2 2... (closed pentagon + roof)
door:  M15 21v-8a1 1 0 0 0-1-1h-4...

# Tabler — 2 paths: roof lines (open) + door (closed)
roof: M5 12H3l9-9l9 9h-2  M5 12v7a2 2...
door: M9 21v-6a2 2 0 0 1 2-2h2a2 2...

→ Thmanyah approach: 1 closed path (house silhouette) + 1 open door notch
  Outer roof peak: r=2  |  Eaves corners: r=2  |  Door inner: sharp
```

### Search
```
# Lucide — circle (closed) + handle line (open)
# Tabler — M3 10a7 7 0 1 0 14 0a7 7 0 1 0-14 0  m18 11l-6-6

→ Thmanyah: circle (r=full, ~8px radius centered at ~9,9) + diagonal handle
  to bottom-right. 2 paths: circle stroke + handle open line.
```

### Bookmark
```
# Lucide — 1 closed path, pointed bottom, rounded top
# Tabler  — M18 7v14l-6-4l-6 4V7a4 4 0 0 1 4-4h4a4 4 0 0 1 4 4

→ Thmanyah: 1 closed path. Top corners outer r=3, pointed bottom v-notch
  inner sharp. Width ~10px, height ~16px, centered.
```

### Menu (Hamburger)
```
# Lucide — 3 open horizontal lines: y=5, y=12, y=19
# Tabler  — 2 lines: y=8, y=16

→ Thmanyah: 3 open lines. Round caps (ROUND stroke cap already set).
  Lines from x=3 to x=21. y positions: 7, 12, 17.
```

### Settings (Gear)
```
# Lucide — 1 complex closed path (cog outline) + 1 inner circle
→ Thmanyah: outer gear ring (closed, 8 teeth or smooth with notches) +
  inner circle (closed). Keep very simple — 6-point gear or just a circle
  with 3 lines radiating out.
```

---

## Arrows

### Chevrons (directional, no stem)
```
# Lucide
right: M9 18l6-6l-6-6
left:  M15 18l-6-6l6-6
up:    M18 15l-6-6l-6 6
down:  M6 9l6 6l6-6

→ Thmanyah: open V-shape, round caps. Width ~8px, height ~8px.
  Center at (12,12). Stroke only, no fill.
```

### Arrows (with stem)
```
# Lucide
right: M5 12h14  M12 5l7 7l-7 7
left:  M19 12H5  M12 5l-7 7l7 7
up:    M12 19V5  M5 12l7-7l7 7
down:  M12 5v14  M5 12l7 7l7-7

→ Thmanyah: 2 open paths — horizontal/vertical stem + arrowhead V.
  Or 1 closed path with filled arrowhead for the filled variant.
```

### Circle Arrows (arrow inside circle)
```
→ Thmanyah: 2 paths — outer circle (closed stroke) + inner chevron (open)
  Circle: center (12,12), radius 9. Chevron centered inside.
```

### Download
```
# Lucide download: arrow pointing down + horizontal base line
→ Thmanyah: open path — vertical line + arrowhead down + base tray shape
```

---

## Actions

### Plus / Add
```
# Lucide: M5 12h14  M12 5v14  (2 open lines, crosshair)
→ Thmanyah: 2 open paths. Horizontal + vertical, centered at (12,12).
  Each line from 3 to 21. Round caps.
```

### Heart
```
# Lucide — 1 closed complex curve path
→ Thmanyah: 1 closed path. Two top lobes (outer rounded) meeting at bottom
  sharp point. Key points: top-left lobe peak ~(5,8), top-right ~(19,8),
  center dip ~(12,10), bottom tip ~(12,21).
  Simplified: M 5 9 Q 3 5 7 4 Q 12 3 12 10 Q 12 3 17 4 Q 21 5 19 9 L 12 21 Z
```

### Close / X
```
# Lucide: M18 6L6 18  M6 6l12 12  (2 diagonal open lines)
# Mingcute: M12 13.414l5.657 5.657...  (2 diagonals)
→ Thmanyah: 2 open lines crossing at (12,12). From (4,4)→(20,20) and (20,4)→(4,20).
```

### Send
```
# Lucide — paper plane, 1 complex path + 1 fold line
# Mingcute — triangle-like shape pointing top-right
→ Thmanyah: paper plane silhouette. 1 closed path (triangle body) + 1 open
  fold line inside. Or just the filled plane shape for simplicity.
```

### Share
```
# Lucide share-2: M8.59 13.51l6.83 3.98  M15.42 2.53l-6.82 3.98
  + 3 circles at nodes
→ Thmanyah: 3 small circles (dots) + 2 connecting open lines.
  Dots at: top-right (19,4), bottom-right (19,20), left (4,12).
```

### Filter (Funnel)
```
# Lucide: M22 3H2l8 9.46V19l4 2v-8.54z  (1 closed funnel path)
# Mingcute: trapezoid top + narrowing body + handle at bottom-right
→ Thmanyah: 1 closed path. Wide top (x=3–21, y=3), narrows down.
  Outer top corners r=2, inner where it narrows sharp.
  Body: M3 4 Q3 3 5 3 L19 3 Q21 3 21 4 L16 11 L16 19 L8 19 L8 11 Z
```

### Edit / Pencil
```
# Lucide edit-3: diagonal rectangle + tip triangle
→ Thmanyah: 1 closed path (parallelogram body) + 1 small triangle tip.
  Or 1 combined closed path. Body at ~45° angle.
```

### Trash
```
# Lucide: lid (open line) + bin body (closed) + 2 inner lines (open)
→ Thmanyah: 2 paths — bin body (closed rounded rect with opening) +
  handle bar (open line across top). Outer corners r=3, inner sharp.
```

---

## Media Controls

### Play
```
# Lucide: M5 5a2 2 0 0 1 3.008-1.728l11.997 6.998...  (closed triangle)
→ Thmanyah: 1 closed triangle path, right-pointing.
  Vertices: left ~(5,4), left ~(5,20), right ~(20,12).
  Outer left corners r=2, right tip r=1.
```

### Pause
```
→ Thmanyah: 2 closed vertical rects.
  Left bar:  x=6–10,  y=4–20, outer corners r=2
  Right bar: x=14–18, y=4–20, outer corners r=2
```

### Volume
```
# Lucide: speaker shape (closed) + arc lines (open)
→ Thmanyah: 1 closed speaker shape (left side flat, right side pentagon)
  + 1–2 open arc paths for sound waves on the right.
```

### Bell / Notification
```
# Lucide: dome shape (closed) + clapper dot + handle arc
→ Thmanyah: 1 closed dome path (rounded top, flat bottom with notch)
  + 1 small open arc at top (handle) + 1 small circle at bottom (clapper).
  3 paths total.
```

### Skip Forward / Back
```
# Lucide skip-forward: triangle (closed) + vertical bar (closed/open)
→ Thmanyah: 1 closed triangle + 1 closed/open vertical bar.
  Bar at right edge x=18–20 for skip-forward.
```

### Rewind / Rotate
```
# Lucide rotate-ccw: arc circle + arrow pointer
→ Thmanyah: 3/4 circle arc (open) + small arrowhead at arc end.
  2 open paths.
```

### Mic
```
# Lucide: M12 19v3  M7 13v2a5 5 0 0 0 10 0v-2 + capsule body
→ Thmanyah: 1 closed capsule (rounded rect, tall, outer r=full)
  + 1 open arc at bottom + 1 open stem line going down.
```

---

## Sport

### Trophy ✓ (already built)
```
Body: M6 6 Q6 3 9 3 L15 3 Q18 3 18 6 L18 14 L14 16 L14 18
      L17 18 L17 19 Q17 21 15 21 L9 21 Q7 21 7 19 L7 18 L10 18 L10 16 L6 14 Z
Left handle (open):  M6 7 Q3 7 3 10 Q3 13 6 13
Right handle (open): M18 7 Q21 7 21 10 Q21 13 18 13
```

### Crown ✓ (already built)
```
M3 17 Q3 19 5 19 L19 19 Q21 19 21 17 L21 8 Q21 6 19 6
L16 13 L13 5 Q12 4 11 5 L8 13 L5 6 Q3 6 3 8 Z
```

### Star
```
# Lucide/Tabler: 5-point star with inner pentagon
→ Thmanyah: 1 closed 5-point star path.
  Outer points r=1, inner concave corners sharp.
  Center (12,12), outer radius ~9, inner radius ~4.
  Top point: (12,3), then clockwise 72° intervals.
```

### Medal / Award
```
# Lucide award: circle on top + ribbon below
# Tabler medal: circle + ribbon strands
→ Thmanyah: 1 closed circle (top, center x=12, y=9, r=6)
  + 2 open ribbon strands going down (left and right, meeting at bottom point).
```

### Flag
```
# Lucide: M4 22V4... wavy flag + pole line
# Tabler: M5 5a5 5... wavy shape + vertical stem
→ Thmanyah: 1 open vertical pole line (x=4, y=3–21)
  + 1 closed flag body (right side of pole, outer corners r=2, right side curves).
```

### Stadium / Pitch
```
→ Thmanyah: 1 closed oval/stadium outline (outer r=full ellipse)
  + 1 inner center circle (open or closed stroke)
  + 1 open center line (horizontal).
```

### Whistle
```
→ Thmanyah: 1 closed whistle body (rounded rect at angle)
  + 1 open mouthpiece line + 1 small open circle (air hole).
```

---

## Communication

### Phone
```
# Lucide: 1 complex closed path (handset shape)
# Mingcute: similar rounded handset
→ Thmanyah: 1 closed path. Handset silhouette:
  Earpiece top-right (rounded r=3), body diagonal, mouthpiece bottom-left (r=3).
  Keep very simple — rectangle rotated 45° with 2 rounded ends.
```

### Mail / Email
```
# Lucide: envelope rect (closed) + V flap (open)
# Mingcute: M20 4a2 2... rect + diagonal V line inside
→ Thmanyah: 1 closed rounded rect (outer r=3)
  + 1 open V path inside (flap). Width x=3–21, height y=5–19.
```

### Comment / Message
```
# Lucide message-square: rounded rect + tail
→ Thmanyah: 1 closed bubble shape (rounded rect with bottom-left tail notch).
  Outer corners r=3, tail inner corner sharp.
```

---

## User & Account

### User (Person)
```
# Lucide: circle head + shoulder arc
→ Thmanyah: 1 closed circle (head, center x=12, y=8, r=4)
  + 1 open arc (shoulders, from x=4,y=21 curving up and over).
  2 paths.
```

### User Circle (Avatar)
```
→ Thmanyah: 1 outer circle (closed, r=10) + inner person shape (head circle
  + shoulder arc as above but smaller, fitting inside the outer circle).
```

---

## Status / Confirmation

### Check / Done
```
# Lucide: M20 6L9 17l-5-5  (open checkmark, 2 segments)
→ Thmanyah: 1 open path. Left segment shorter (5,13)→(9,17),
  right segment longer (9,17)→(20,6). Round caps.
```

### Check Circle
```
→ Thmanyah: 1 closed circle + 1 open checkmark inside (smaller).
  Circle r=9, centered at (12,12). Check inside proportionally.
```

### Alert / Warning (Triangle)
```
# Lucide: M21.73 18l-8-14a2 2 0 0 0-3.48 0l-8 14...
→ Thmanyah: 1 closed triangle path. Outer corners r=2.
  Top tip: (12,4), bottom-left: (3,20), bottom-right: (21,20).
  + 1 open exclamation: vertical line (12,9)→(12,14) + dot at (12,17).
```

### Info Circle
```
→ Thmanyah: 1 closed circle + open "i": dot at (12,8) + line (12,11)→(12,18).
```

### Block / Stop
```
→ Thmanyah: 1 closed circle + 1 open diagonal line across it.
```

---

## Device / System

### Mobile / Smartphone
```
# Lucide smartphone: rounded rect body + home indicator dot
→ Thmanyah: 1 closed rounded rect (outer r=3, portrait orientation).
  x=7–17, y=2–22. + 1 open short line or dot at bottom (home bar).
```

### TV / Television
```
# Lucide tv: screen rect + stand triangle + legs
→ Thmanyah: 1 closed rounded screen rect (x=2–22, y=3–17, r=3)
  + 1 open stand line or small rect at bottom center.
```

### Desktop / Monitor
```
# Lucide monitor: screen (closed rounded rect) + stand line + base line
→ Thmanyah: 1 closed screen (x=2–22, y=3–17, r=3)
  + 1 open vertical stand (x=12, y=17–20)
  + 1 open horizontal base (x=8–16, y=20).
```

### Cast
```
# Lucide: M2 8V6a2 2... screen outline (partial, open) + wifi arcs
→ Thmanyah: 1 open bottom-left L-shape (screen corner)
  + 2–3 open concentric quarter-circle arcs radiating from bottom-left.
```

### Wifi
```
# Lucide: M12 20h.01  + 3 concentric arcs + dot
→ Thmanyah: 1 dot at (12,20) + 3 open arcs (small, medium, large).
  Each arc is a quarter-circle centered at (12,22).
  Outer arc: r=8, mid: r=5, inner: r=2.5.
```

### AirPlay
```
→ Thmanyah: triangle pointing up (filled/stroked) + screen rectangle below.
```

---

## Misc / Utility

### Calendar
```
# Lucide: rect body (closed) + date rows (open lines) + 2 tick marks at top
→ Thmanyah: 1 closed rounded rect (outer r=3, x=3–21, y=4–21)
  + 2 open tick lines at top (x=8, x=16, y=2–6)
  + 1 open horizontal divider line inside (y=10).
```

### Shield
```
# Lucide: M20 13c0 5-3.5 7.5-7.66 8.95...  (closed shield silhouette)
# Mingcute: similar rounded shield with flat top, pointed bottom
→ Thmanyah: 1 closed path. Flat top (x=4–20, y=3), curved sides,
  pointed bottom at (12,21). Top corners outer r=3.
```

### Grid
```
# Lucide grid-3x3: M3 9h18  M3 15h18  M9 3v18  M15 3v18  (4 open lines)
→ Thmanyah: 4 open lines forming a 3×3 grid. Round caps on ends.
```

### Credit Card
```
# Lucide: rounded rect (closed) + 2 horizontal lines (open) + stripe line
→ Thmanyah: 1 closed rounded rect (x=2–22, y=5–19, r=3)
  + 1 open horizontal stripe (y=9, full width)
  + 1 small open rect or 2 small dots (chip, left side).
```

### Security / Lock
```
→ Thmanyah: 1 closed padlock body (rounded rect, x=5–19, y=11–21, r=3)
  + 1 open arc handle above (from x=8 over the top to x=16, peak at y=5).
```

### Voucher / Coupon
```
→ Thmanyah: 1 closed rounded rect with notched circles on left and right edges
  (like a ticket). Outer r=3, notches are semicircles punched in, inner sharp.
```

---

## How to Adapt Library Paths to Thmanyah Style

1. **Scale to 24×24 grid** — library paths are usually on a 24×24 viewBox already
2. **Convert relative → absolute** — replace `m/l/c/q/a/z` with `M/L/C/Q/A/Z`
3. **Apply Thmanyah corner rules:**
   - Exterior corners: replace sharp `L` turns with `Q` curves (r=3 outer)
   - Interior corners: keep as hard `L` (sharp, 0–1px)
4. **Simplify** — if a library uses 3+ paths, see if you can achieve the same with 1–2
5. **Color** — always `WHITE { r:1, g:1, b:1 }`, never black
6. **Check centering** — verify leftmost_x + rightmost_x = 24 and topmost_y + bottommost_y ≈ 24

---

## Best Reference Libraries by Style Match

| Library | Style | Best for |
|---|---|---|
| **Lucide** | Clean, minimal, stroke | Most categories — great baseline shapes |
| **Tabler** | Clean, slightly bolder | Arrows, Navigation, Actions |
| **Mingcute** | Rounded, filled modern | Sport, Communication, Status |
| **Phosphor** | Versatile, many weights | Any — good shape variety |
| **Iconoir** | Sharp-modern | Device, System icons |
| **Feather** | Ultra-minimal | Simple utility icons |
