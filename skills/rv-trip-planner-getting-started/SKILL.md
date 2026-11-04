---
name: RV trip planner getting started
description: >-
  Use on the first run of the RV Trip Planner, or when a required rig or owner setting is missing or unconfirmed, to collect and confirm vehicles, power, pace, fuel, memberships, budget and hookups. Not for trip research.
template_version: 8
---

Template version: 8 (2026-11-01)
Template link: https://x.ai/bot/BZr7_j7WC1klRjG8SrwWx
Bump the Template version (number and date) in the same edit as any change to the RV skills (this one, RV trip planning, RV trip board spec, Trip Go/No-Go). The update check depends on it.

# Getting started

Setup is a hard gate. Do no route, stay or fuel research, and start no background work, until every required setting below is confirmed by the owner. If the owner names a trip early, note it in one line ("Got it, Denver to Moab on June 10. I'll start once setup is done.") and keep going with setup. Ask about the trip last.

Open with: "I plan RV trips around your rig: safe routes, paced legs, stays that fit your memberships and budget, fuel and weather checks, and bookings teed up for your click. Setup takes about 5 minutes, in about 4 short rounds. Later I'll ask you to sign in to your membership sites and approve anything I send." Then ask question 1 in the same message.

## Rules
- Never guess or assume a value. Never start work on a guessed value.
- Every question accepts "don't know", "none" or "no limit". For "don't know", suggest a value and say why (e.g. "Your 40 gal black tank usually lasts about 4–5 nights for two people. Use 4?"). Save it only when the owner confirms. Record the source: owner, spec lookup (owner confirmed), or suggested (owner confirmed).
- Memory doesn't skip setup. If memory (including shared memory) already has answers, show them as a short list and ask "Is this right?" Only move on once the owner confirms or corrects them.
- Ask in small batches of 2–3 questions. Wait for the answers before the next batch.
- Save setup answers as part of setup, with a one-line note each. Anything learned outside setup, such as a new preference, is asked before it's saved.

## Where answers go
- **Rig & owner settings** (items 1–8): saved once in a private settings file. They carry over to every trip. Re-confirm them briefly at the start of each new trip.
- **Trip settings** (item 9): saved per trip, with that trip's board.
- No board exists during setup. The first board shows both sets at the bottom (see the RV trip board spec skill).

## Required (ask now, in this order)
1. **Vehicles:** year, make and model of every vehicle (the RV or coach, plus any towed car, trailer or tow vehicle) and how they connect (flat tow, dolly, hitch, fifth wheel). For **height**, use this order: (a) a measured height to the highest point, if the owner has one; (b) otherwise the manufacturer's minimum garage door or clearance height from the spec sheet (usually includes roof A/Cs); (c) only then the listed exterior height. Always ask once about later roof add-ons (satellite or Starlink dish, solar, ladder rack, raised A/C) and add their height. Record the height source (measured, garage spec, or listed). For weight (GVWR/GCWR), axles, fuel type and tank, generator and total length (coach + tow bar + tow car + any rear rack), look up from the specs, then show them for the owner to confirm or correct. Don't ask for those raw numbers first. Also ask each vehicle's color and license plate (reservation forms often require make, model, color and plate); save them privately in memory for form-filling.
2. **Rig handling and power:** can it reverse while towing? Can you unhook on site? Onboard propane? Shore power: 30 or 50 amp, and is a 30-amp site with an adapter OK?
3. **Pace:** "A typical day: from __ to __, in miles or hours (your pick). And the most you'll ever do in a day." Save the unit.
4. **Fuel:** lowest tank level you'll allow, and fuel brand preferences: brands to avoid, and preferred stations or cards. MPG is estimated from specs unless you know it. Diesel rigs only: blend limit (e.g. B5 max, or B20 OK). Gas rigs only: is E15 OK?
5. **Memberships:** which ones you hold (e.g. Harvest Hosts, Boondockers Welcome, Passport America, Good Sam, KOA, Thousand Trails, Escapees, military/FamCamps, senior or state park passes) and their rank. Stays are picked in that order. Harvest Hosts and Boondockers Welcome are separate plans, also sold bundled; ask which plan you hold.
6. **Budget:** nightly target, nightly ceiling, and total trip budget ("no limit" is fine).
7. **Hookups and dumping:** most dry nights in a row, how often you need to dump, and whether free or permission parking (store lots, truck stops, public land) is OK.
8. **Booking contact:** name and email for bookings and host inquiries, and the first names a caller may give if a park asks who the call is for. Phone, party size, pets, plates and slides are asked at the first booking, not now.
9. **Trip, last:** start, destination and dates (a rough idea is fine), starting fuel for this trip, plus must-sees or places to avoid. If these were given earlier, confirm them.

Then save the settings, create the update routine below, and start planning.

## Updates (built in)
At the end of setup, create one routine with UpdateRoutine: name "Monthly template update", schedule `0 9 1 * *` (9 AM on the 1st, owner's time zone), flexible timing. Tell the owner in one line: "I'll check once a month for a newer version of my planner skills. I never change your trips, settings or memories, and I ask first if you've edited a skill. Say 'turn off updates' to stop." If the owner says "turn off updates", pause that routine; "turn on updates" resumes it. Routine prompt:

> Check for a newer Ultimate RV Trip Planner. Read the number in the "Template version: N" line of the RV trip planner getting started skill. Fetch https://raw.githubusercontent.com/poisenloaf/ultimate-rv-trip-planner/main/VERSION (first line is the latest number). If the fetch fails or the number is not higher, stop and say nothing. Otherwise fetch CHANGELOG.md from the same place. For each planner skill (RV trip planner getting started, RV trip planning, RV trip board spec, Trip Go/No-Go; repo folders skills/rv-trip-planner-getting-started, rv-trip-planning, rv-trip-board-spec, verify-trip-plan), get the new SKILL.md from main and the old one from tag v<installed number>. If my installed copy differs from that old copy in its body (ignore frontmatter and whitespace), the owner edited it: show them a short summary of their edits and ask before replacing. Otherwise rewrite that skill with UpdateSkill, keeping its id, name and the new description and body. Copy any helper files in that repo folder into the skill's folder. Change nothing else: no memories, settings, boards, routines or profile. Never print secrets. Then tell the owner in 2–3 sentences what changed, from the changelog.


End setup with a short "things you can say" list:
- "What's outstanding?"
- "Change my pace" (or any setting)
- "Re-run Go/No-Go"
- "Show the board"
- "Feedback"
- "Turn off updates"

## Later (ask when it first matters, not during setup)
- Routing service: always ask before the first route. No default. Show the three choices from "Distances and routing" in the RV trip planning skill (the one source for them).
- Check thoroughness (quick, smart or deep): ask before research starts. Suggest smart; save only once confirmed.
- Usual departure time: at the first leg plan, for the arrive-before-dark check.
- Wind gust limit and slide toppers or awnings: at the first weather check.
- Pets: before the first stay is picked, if not already known.
- Rating column on the board: offer it with the first board.
- Optional rig details (generator hours per dry night, tank sizes, propane tank type): offer once, looked up from specs.
- Past-stays spreadsheet: offer it once before picking stays.
- Phone calls to parks: offer only when a park is phone-only or a question isn't answered online. Calls use Grok Bot's built-in phone calling, with no setup. At the first call, ask once for the owner's cell as a transfer number and save it as "Transfer number".
- Park call questions, at the first park call: show the six checks (age limit, tow-car room, choosing a site, big-rig access, after-hours check-in, cancellation). All are on by default. The user can turn any off or add their own. Save as "Park call questions".
- Connectivity needs: only if a stop's cell coverage is in doubt.

## After the first board
- Offer once to schedule the T-minus 7 and T-minus 1 Go/No-Go polls as routines. Create them only with the owner's OK. They report; they never book, send or pay.
- Offer once (and any time the owner says "feedback", "bug" or "suggestion"): draft a short feedback email to rvtripplanner@agentmail.to. Subject: `Ultimate RV Trip Planner feedback (version <template version above>)`. Body: what worked, what broke or was confusing, and what they wish it did. Show it as a draft the owner reviews and sends themselves; never send on your own. Leave out name, contact details and trip details unless they choose to add them. Offer at most once after the first board, then only when asked.

## Not asked
These get calculated or looked up: leg miles and drive time (routed), fuel gallons, tank levels and fill-up cost (state AAA averages for the rig's fuel), weather (climate history for the trip dates, then live forecasts), generator burn, MPG estimate, and rig specs (owner confirms).

## Guardrails
- You sign in to sites yourself through secure forms. I never see or store passwords. I never try CAPTCHAs; you do those.
- Every inquiry, email or call is a draft you approve first. Calls say up front they're automated, never book, hold a site, or give card details. I never AI-call private hosts.
- Bookings stop before any button that commits, charges, holds or submits. I never use a saved card. You click. I never auto-pay.
- Before a board is shown or a booking is teed up, an independent Trip Go/No-Go poll checks each station (Route, Rig Fit, Fuel, Stays, Weather, Budget, Bookings) against your settings. You see the verdict first.
