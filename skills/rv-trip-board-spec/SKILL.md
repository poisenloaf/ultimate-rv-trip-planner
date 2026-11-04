---
name: RV trip board spec
description: >-
  Use when building or updating an RV trip board HTML page: layout, statuses,
  colors, map, Go/No-Go strip, day table and settings table. Not for planning
  steps or Go/No-Go checks.
template_version: 9
---
## Trip board page spec

This skill is the only board spec. The RV trip planning skill points here. Follow it exactly.

**File.** One self-contained HTML file per trip. Leaflet CSS and JS go inline. Simplify route lines so the page loads fast. Load nothing from outside except map tiles. Pin the Leaflet version you inline (1.9.x) and note it in an HTML comment.

**Map tiles.** Use `https://tile.openstreetmap.org/{z}/{x}/{y}.png` (no `{s}` subdomains). Show the attribution "© OpenStreetMap contributors" with a link to https://www.openstreetmap.org/copyright. Keep use light, per the OSM tile policy. Tiles can be blocked when the page opens from a local `file://` path or in some preview panes, because no Referer is sent. Test the board that way. Fallback: if tiles fail, still draw the route lines, stops and legend on a plain background, and show a small note: "Map tiles didn't load. Route and stops are still shown." Before you show a board, open it once from `file://` and confirm that tiles load or the fallback note shows. A user may also supply their own tile provider and key (store it like other keys; never print it).

**Privacy.** A board shared outside the owner (screenshots, posts, template demos) uses sample stops and dates, never real stays, dates or locations of the travelers.

**Layout:** header, map, Go/No-Go strip, day table, sources footnote, Settings. Full width; fits about 1000px with no hidden columns. Bg `#f7f6f2`, cards `#fff`, lines `#e4e2dc`, text `#1f2933`, muted `#6b7280`, accent `#2f6fb5`. System font, 15px/1.45. Table 13px, sub text 11.5px.

**Header:** h1 19px `<Start> → <Destination>`. Under it, muted: `<Dates> · RV trip board`. On the right, stats (bold number over a small uppercase label): Road miles, Nights, Fuel (N gal), Camping, `≈ $X` Est. total.

## Stop status list

One list, used everywhere: the table pill, the map dot, the legend, popups, the status dashboard and Go/No-Go. Status says where the stop stands. The next action is separate (the blue `→` line).

| Status | Meaning | CSS class | Map color |
|---|---|---|---|
| Start / Destination | Trip start or end point | `p-fixed` | `#7a7f87` |
| Pending | Options still being checked; no pick yet | `p-pending` | `#d98a1c` |
| Picked · not booked | Chosen stop, nothing sent or booked yet | `p-picked` | `#2f6fb5` |
| Requested | Inquiry or stay request sent; waiting for a reply | `p-req` | `#7b5ea7` |
| Phone-only | Needs a call to reserve | `p-phone` | `#a0522d` |
| Ready to pay | Teed up to the stop point; the user clicks | `p-ready` | `#138a8a` |
| Booked | Confirmed, with a confirmation number, email or screenshot | `p-booked` | `#2f8f4e` |

Membership stops use the same statuses. They get a badge, not their own status.

**Map** (56vh; scroll zoom only after a click):
- Legs: white casing under a color. `#2f5d9a` in range, `#e0a800` out of range but accepted, `#c0392b` out of range and not accepted.
- Mid-leg chip: white, 11px, `N mi` (or `N h` if pace is in hours), plus ` ⚠` when out of range. Amber chip is `#8a6700` on `#fff6d6`. Red chip has `#c0392b` text.
- Stops: 26px circles, white border, labeled `S`, `1…n`, `F`, colored by the status list above.
- ⛽ fuel icons. Backups are smaller, at 55% opacity.
- Popups: a stop shows label · night, name, town, status, hookups and rating (if on). Fuel shows brand, exit, backup and a maps link.
- Legend (bottom left): a dot for each status used on this board, the 3 line colors, and ⛽. A ⤢ "Show whole route" button sits top right.
- Clicking a row highlights it, flies to the stop and opens its popup.

**Go/No-Go strip:** a bold verdict (NO-GO red, HOLD amber, `GO for launch` green, `Not polled` gray). Then pills `Name <small>GO|HOLD|NO-GO|—</small>` for Route, Rig Fit, Fuel, Stays, Weather, Budget, Bookings. On the right, muted: `<Poll kind> <date time zone> · red NO-GO · amber HOLD · green GO`. Built from the latest poll in `decisions.tsv`.

**Day table** (rounded card, sticky header, zebra rows). Column widths: `#` 32px · Night 64px · Overnight · alternates (no set width; it takes the leftover space, about 200px at 1000px) · Miles 64px (r) · Price 72px (r) · Rating 90px · Hookups / site 11% · Status · notes · next 19% · Fuel 16%. When the Rating setting is off, omit the Rating column; Overnight gets the space.
- **#**: 24px circle in the status color.
- **Night**: `<Mon D>` or `<Mon D–D>`, bold.
- **Overnight**: bold `<Stop name>` plus badges (`HH`, other memberships, `Dry camp`). Then a muted `<Town, ST>` line, a `🔗 <Link> · <Link>` line and `↳ Alt: <Alt name> $X` lines.
- **Miles**: the leg, then sub `N total`. Out of range: `N ⚠`, with sub `over|under, accepted` (amber) or `not approved` (red). If pace is in hours, also show drive time.
- **Price**: bold `$X.XX`, `Free` or `TBD`, plus a sub note. Start and end rows show `—`.
- **Rating** (only if on): gold stars filled to the score, the score, and the source as sub text.
- **Hookups / site**: hookups, amps (30 or 50), site type and length, with `UNVERIFIED` when the park lists no length.
- **Status**: one pill from the status list. Below it, a sub line for availability as of a date, `📝 <note>`, and a blue `→ <next action>`.
- **Fuel**: on a fill day, `⛽ <Brand> · fill N gal × $X = $Y (<price source>)`, then sub `mi N · arrive N gal [⚠ low] · <exit> · map` and `Backup: <name>`. Otherwise, sub `tank ≈ N gal`. No token fuel rows (under about 10 gal): drop the stop or show a fill-to-full.
- Dry rows have a 3px `#b9a27a` left bar on the Night cell.
- **Sub-row under each day** (full width, 12px):
  - `☁ Weather`: `hi N° / lo N° · freeze N% of nights · gust ≥<user's wind limit> mph N% of days`, a flag (red if it has ⚠), and the source.
  - `⛰ Route`: steep grades, mountain passes, propane restrictions, and arrival time vs sunset and check-in hours, only when flagged.
  - `★ Must-sees`: linked sights joined by ` · `, each with a note and an optional green `<length> OK` chip.
- **Totals row** (`#eef1f6`, bold): `Trip total · N nights` · miles · `$X camping` · `Grand total ≈ $Y` · `$X fuel · N gal bought · arrive ≈ N gal left`.

**Settings** (bottom of the board): tables with the columns Setting / Value / Source / When set, and gray group rows. Source is one of: owner, spec lookup (owner confirmed), suggested (owner confirmed), or planning rule (accepted).
1. Rig & owner settings: Rig, Power, Fuel, Generator, Camping, Budget, Pace, Wind, Memberships & booking, Calling.
2. Trip settings: dates, start fuel, waypoints, avoidances, accepted exceptions.
3. Calculated & planning notes: Fuel, Route, Booking, Weather.
4. Display: rating column on/off, thoroughness. These are display and research choices, not rig limits.

**≤760px:** rows become cards, each cell gets an uppercase label, the `#` circle moves top right, empty cells hide, and the map is 50vh. **Print:** landscape, map 95mm, 9.5px table, no popups or zoom controls, exact colors.

```css
.pill,.st{display:inline-block;padding:1px 7px;border-radius:999px;font-size:12px;font-weight:600;border:1px solid;white-space:nowrap}
.p-booked,.st-go{color:#1f6b39;background:#e6f4ea;border-color:#bfe0c9}
.p-ready{color:#0f6b6b;background:#e3f4f4;border-color:#b5dede}
.p-picked{color:#1f528a;background:#e7effa;border-color:#c3d6ef}
.p-req{color:#5b3f8a;background:#f0eaf8;border-color:#d6c7ec}
.p-phone{color:#7a3b17;background:#f8ebe2;border-color:#e6c6b0}
.p-pending,.st-hold{color:#8a5208;background:#fdf1df;border-color:#f1d3a3}
.p-fixed,.st-np{color:#4b5563;background:#f0f0ee;border-color:#dcdcd8}
.st-nogo{color:#9b2c20;background:#fbe6e3;border-color:#efbcb5}
.badge{font-size:10.5px;font-weight:700;padding:1px 6px;border-radius:4px;margin-left:4px}
.badge.hh{background:#6d4c9f;color:#fff}
.badge.dry{background:#f3ead9;color:#7a5a20;border:1px solid #e3d2ae}
.mk{display:inline-flex;align-items:center;justify-content:center;width:24px;height:24px;border-radius:50%;color:#fff;font:700 12px sans-serif}
table{width:100%;table-layout:fixed;border-collapse:separate;border-spacing:0}
th,td{padding:6px;vertical-align:top;border-bottom:1px solid #e4e2dc;overflow-wrap:anywhere}
thead th{position:sticky;top:0;background:#f1efe9;font-size:11px;text-transform:uppercase;color:#4b5563}
tbody tr.z{background:#faf9f6}tfoot td{background:#eef1f6;font-weight:650}
.sub{font-size:11.5px;color:#6b7280}.warn{color:#c0392b;font-weight:650}.amb{color:#b38600}
.nx{color:#1f528a;font-size:12px}.s-fg{color:#d9a21b}.s-bg{color:#ddd8cc}
```
