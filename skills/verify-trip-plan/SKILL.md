---
name: Trip Go/No-Go (verify-trip-plan)
description: >-
  Use to run the independent GO / NO-GO / HOLD check of an RV trip board: before
  a board is shown, before a booking or early inquiry, at T-minus 7 days, and
  optionally at T-minus 1. Not for building the plan itself.
template_version: 8
---
# Trip Go/No-Go

Independent launch poll of an RV trip board (see the RV trip planning skill), modeled on mission control polling each station before launch. Run it in a **fresh-context subagent, on a different model than the planner if possible**. That subagent is the **Flight Director**. It gets only: the trip board, its settings (Rig & owner settings and Trip settings), and this checklist. It does not get the planner's reasoning.

Tools the Flight Director needs: web browsing (official park and membership pages, road and weather sources), the routing service from settings, and file access to the board and `decisions.tsv`. It can't talk to the owner. It returns any questions for the owner to the planner, which asks them.

## Rule zero: the owner's parameters are the standard
- Every limit comes from the **settings**: the setup answers plus every rule added or accepted during planning (pace range, unit and hard max, MPG, tank, reserve, generator burn, dry/dump limits, wind limit, budget target/ceiling, memberships and their rank, routing service, total length, height (with source), weight, axles, amps, propane, pets, check thoroughness, and anything new).
- Never use a default number when settings have a value. If a check needs a parameter that is missing, mark the row **UNVERIFIED: missing parameter X** (station answers HOLD) and add the question to the list returned to the planner. Do not guess.
- A leg or stop the owner explicitly accepted outside a limit is PASS (accepted), not FAIL. Note the acceptance source and date.
- Re-check facts live. Board notes are claims, not proof.
- If `decisions.tsv` is missing, create it with the header below. Never rewrite earlier lines.
- `poll_time` carries its zone. Report times to the owner in the owner's zone.

## Poll points (T-minus)
1. **Board review:** before a board is shown to the owner.
2. **Booking tee-up:** before any booking is teed up to the owner's click. If an inquiry or stay request must go out before the board poll, run a narrow Stays + Rig Fit poll for that stop first.
3. **T-minus 7 days:** full re-poll with the first point forecasts.
4. **T-minus 1 (optional):** the night before departure; poll Weather (live forecast, gusts, freezes, storms) and Route/Rig Fit (live road closures, chain controls, construction) for the first days.

The planner offers to schedule points 3 and 4 as routines after the board is approved.

## Station poll
The Flight Director calls each station in turn. Each check row gets PASS / FAIL / UNVERIFIED plus evidence (URL + check date, screenshot path, or routing output).
1. **Route:** per leg, routed miles or drive time (matching the pace unit) against the pace range and hard max. Mileage margin: with a car-only router, add 5% to routed miles and drive time; with a truck router, use them as is. Out-of-range legs pass only if accepted (note source and date). Contiguous route with no missing legs. Steep grades and mountain passes flagged. Arrival before sunset and within each stop's check-in or host hours.
2. **Rig Fit:** the route clears the rig's height (from settings, with its source: measured, garage spec, or listed), weight, total length and axle limits; any clearance within about 6 inches of the rig height is HOLD until checked; tunnel and bridge propane restrictions on the route checked; low bridges, weight-limited and last-mile roads; tight turns and entrances; no spot that forces reversing with a tow car attached.
3. **Fuel:** tank never below the owner's reserve using their MPG, tank and starting fuel; generator burn on dry nights; blend limits (diesel rigs); DEF where needed; each station has big-rig in/out access per the thoroughness setting; fuel brand preferences (avoided brands, preferred stations, accepted card); diesel sold at each diesel stop; price source named.
4. **Stays:** official site length and type fit the total length (with the tow car if no unhook spot); a park with no official length is UNVERIFIED (HOLD, cleared by the park); amps match settings (or adapter OK); availability ("Confirmed" only with a dated source; re-check if older than 14 days; flag empty calendars and worker housing); nights contiguous, no gaps or overlaps; seasons, minimum stays, arrival-hour rules (e.g. Harvest Hosts); pets vs each stop's rules; hookups; dry streaks and dump/water cadence within limits.
5. **Weather:** forecast or climate-history gusts vs the owner's wind limit; freeze risk (hoses, tanks); storms; seasonal and weather closures of parks and roads.
6. **Budget:** nightly + fees match the source; nightly vs target/ceiling; total vs trip budget; memberships applied per park terms and in the owner's ranked order. FAIL rule: an available higher-ranked membership option that is neither shown nor documented as ruled out (with a reason) is a FAIL.
7. **Bookings:** every URL loads and matches the named place; every stop has its official link; every stop's status is from the RV trip board spec status list; every Booked stop has a confirmation (number, email or screenshot); every other stop has a clear next action with an owner.
8. Every other settings row not covered above gets its own row under the closest station.

## Station answers
- **GO:** every row for the station is PASS (accepted counts as PASS).
- **NO-GO:** any row is FAIL. Give the fix.
- **HOLD:** no FAIL, but at least one row can't be verified yet (UNVERIFIED, missing parameter, no forecast yet, no live calendar). Give the reason and **who clears it** (owner, host/park, or a later poll point).
- A station's status is its worst row: NO-GO > HOLD > GO.

## Output
- Verdict line first, one of:
  - **GO for launch** (all stations GO)
  - **NO-GO: <stations>** (any station NO-GO; list the NO-GO stations, then "HOLD: <stations>" if any)
  - **HOLD: <stations>** (no NO-GO, at least one HOLD)
- Then list only the stations that aren't GO, worst first. Per row: check, the setting it was measured against, evidence, fix (NO-GO) or reason and who clears it (HOLD).
- Then the hard-to-check items (what could not be checked and why).
- Then the questions for the owner, for the planner to ask.
- `decisions.tsv` saved beside the board. **Append** one line per check; never overwrite earlier polls. Tab-separated with header:
  `poll_time	poll_kind	station	check	item	setting_measured_against	result	evidence	suggested_fix`
  (`poll_time` is ISO date-time with zone; `poll_kind` is Board review, Booking tee-up, Narrow, T-minus 7 or T-minus 1; `station` is Route, Rig Fit, Fuel, Stays, Weather, Budget or Bookings; `result` is PASS, PASS (accepted), FAIL or UNVERIFIED[: reason].)
- The board's station strip uses the latest poll in `decisions.tsv` (green GO, red NO-GO, amber HOLD, gray not polled), with the verdict and poll date. Mark it "GO for launch <date>" only when every station is GO.

## Loop
- Planner fixes the NO-GO rows; the Flight Director re-polls only those rows (and their stations).
- After 2 failed rounds on the same item, stop and have the planner ask the owner.
- Show the owner a plan only after GO for launch, or with the verdict line and an explicit HOLD list.
- Each new type of failure becomes a new row under its station; each new parameter goes into settings.

Never send, book, or pay.
