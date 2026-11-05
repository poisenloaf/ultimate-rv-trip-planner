# Ultimate RV Trip Planner

A [Grok Bot](https://x.ai/bot/BZr7_j7WC1klRjG8SrwWx) template for RVers who plan multi-day trips. It learns your rig and preferences once, then builds paced legs, picks overnight stops that fit your memberships and budget, checks fuel, dump and weather, and tees up bookings for your click. You stay in control of every send, call and payment.

Also listed on [Bot Shelf](https://botshelf.dev/templates/c36c9d961413/).

## What it does

**First-run setup (about 5 minutes).** It asks for each vehicle’s year, make and model, looks up height, weight, length and fuel specs, and has you confirm them. It also collects power, pace, fuel reserve, memberships (ranked), budget, hookups and dump limits, and a booking contact. Setup is a hard gate: no trip research starts until those answers are confirmed.

**Route planning around your rig.** Legs stay inside your daily miles or hours. Routes respect height, weight, length and propane limits. Motorhomes with a tow car are treated as no-reverse unless you can unhook on site. Grades, passes and late arrivals are flagged.

**Overnight stops by your rules.** Stays are ranked by your memberships first, then free or permission parking (if you allow it), public parks, and private parks within your nightly ceiling. It checks live calendars where it can, drafts short host inquiries for you to approve, and can place park phone calls with Grok Bot’s built-in calling (your OK each time; never books or gives card details). Private hosts are never AI-called.

**Fuel, dump and weather.** Fuel stops keep you above your reserve, skip token fills, and respect blend limits and brand preferences. Dump cadence and dry-night limits are respected. Weather uses climate history far out, then live forecasts closer to departure.

**Trip board.** One HTML board per trip: map, day table, prices, hookups, fuel, weather, status pills and your settings. Statuses run from Pending through Booked.

**Independent Go/No-Go check.** Before a board is shown, before bookings, and optionally at T-minus 7 and T-minus 1, a fresh “Flight Director” poll checks Route, Rig Fit, Fuel, Stays, Weather, Budget and Bookings against your settings. You see the verdict first.

**Bookings up to the pay screen.** It fills forms and stops before any button that commits, charges, holds or submits. One booking at a time. You click. Optional Google Sheets export only with your OK.

**Monthly self-update.** Bots from this template can check once a month for newer planner skills and update only those four skills (never your trips, settings or memories). Say “turn off updates” to stop.

## What it never does

- Never auto-pays or uses a saved card
- Never sends an email, inquiry or review without your approval of that item
- Never places or retries a phone call without your OK in the latest message
- Never books, holds a site, or gives card details on a call
- Never AI-calls private hosts (Harvest Hosts, Boondockers Welcome, home or personal numbers)
- Never stores or sees your passwords; you sign in through secure forms, and you solve CAPTCHAs
- Never starts planning on a guessed setting

## How to get started

1. Open the template: [x.ai/bot/BZr7_j7WC1klRjG8SrwWx](https://x.ai/bot/BZr7_j7WC1klRjG8SrwWx)
2. Create a bot from it (or import the skills into an existing bot).
3. Say hello. Setup runs first: about four short rounds of questions about your rig and preferences. You can name a trip early; planning waits until setup is done.
4. After setup, pick a routing service, choose check thoroughness, and the bot builds the board. Approve drafts and click any final pay or commit button yourself.

Useful things you can say later: “What’s outstanding?”, “Change my pace”, “Re-run Go/No-Go”, “Show the board”, “Feedback”, “Turn off updates”.

## What’s in this repo

Source of truth for the four public template skills. Each `SKILL.md` has `template_version` in its frontmatter.

| Folder | Skill | What it covers |
|---|---|---|
| `skills/rv-trip-planner-getting-started/` | RV trip planner getting started | First-run setup, settings, monthly update routine, guardrails |
| `skills/rv-trip-planning/` | RV trip planning | Legs, routing, stays, fuel/dump/weather, inquiries, calls, booking tee-up |
| `skills/rv-trip-board-spec/` | RV trip board spec | HTML board layout, map, statuses, colors, settings table |
| `skills/verify-trip-plan/` | Trip Go/No-Go | Independent Flight Director poll before show, booking and T-minus checks |

Also:

- `VERSION` — current template version (line 1) and release date (line 2)
- `CHANGELOG.md` — what changed in each version

Published share-card version 10 maps to template **v9** in this repo.

## Monthly update check

Bots made from the template can run a routine at 9 AM on the 1st of each month (owner’s time zone). It compares the “Template version: N” line in the installed getting-started skill with `VERSION` here. If this repo is newer, it updates only these four skills (plus any helper files in their folders). It never changes the owner’s memories, boards, routines or other skills. If the owner edited a skill, it asks first. If current, it stays quiet. The owner can say “turn off updates”.

Bots from v7 or earlier don’t have this routine; re-import the template or ask the bot to add the “Monthly template update” routine from its getting-started skill.

## Feedback

Draft feedback goes to **rvtripplanner@agentmail.to**. The bot can draft a short note for you to review and send yourself. Leave out name, contact and trip details unless you choose to add them.
