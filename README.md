# thmanyah-icon

A Claude Code skill that generates icons directly into the [Thmanyah](https://thmanyah.com) Figma icon library — following a locked-down style spec with outline and fill variants.

## Install

```bash
git clone git@github.com:RazanSon/thmanyah-icon-skill.git ~/.claude/skills/thmanyah-icon
```

Restart Claude Code. The `/thmanyah-icon` skill will be available immediately.

## Usage

```
/thmanyah-icon [icon name] in [Category]
```

**Examples:**
```
/thmanyah-icon Trophy in Sport
/thmanyah-icon Wifi in Device / System
/thmanyah-icon Filter in Action
/thmanyah-icon Crown in Sport
/thmanyah-icon Voucher in Misc / Utility
```

If you omit the category, Claude will infer it from the icon's function.

## What it generates

For every icon, two components are created directly in the Figma file:

| Component | Style |
|---|---|
| `[Name]` | Outline — 1.5px stroke, no fill |
| `[Name], Fill` | Fill — solid black, no stroke |

Both are placed inside the correct category frame and follow the Thmanyah icon style spec.

## Style spec

| Property | Value |
|---|---|
| Canvas | 24 × 24 px |
| Padding | 2–3 px from each edge |
| Stroke weight | 1.5 px |
| Stroke cap | Round |
| Outer corners | 3–4 px radius (smooth) |
| Inner corners | 1 px radius (crisp) |

## Available categories

| Category | Figma Frame |
|---|---|
| Navigation | Home, Search, Library, Settings, Bookmark, Menu |
| Arrows | Directional arrows, circle arrows, download, back |
| Action | Plus, Heart, Close, Share, Send, Edit |
| Media Controls | Play, Pause, Volume, Bell, Series, Movies |
| Sport | Goal, Star, Matches, Whistle, Penalty |
| Communication | Phone, Email, Comments |
| User & Account | User, Subscription, Jersey |
| Status / Confirmation | Done, Warning, Info, Alert |
| Device / System | Mobile, TV, Desktop, Cast, AirPlay |
| Misc / Utility | Calendar, Security, Credit card, Grid |

## Requirements

- Claude Code with Figma MCP connected
- Access to the `Thmanyah_icons` Figma file
