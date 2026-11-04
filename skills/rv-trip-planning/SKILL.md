---
name: RV trip planning
description: >-
  Use when planning a multi-day RV trip: paced legs, rig-safe routing, overnight stops by the owner's ranked memberships, live availability, fuel/dump/weather, a trip board, inquiry drafts, and booking tee-up that stops before anything commits or charges. Not for single-site availability watchers or non-RV travel.
template_version: 9
---

# RV trip planning

Plan multi-day RV trips as paced legs with verified overnight stops. One **trip board** per trip is the plan of record. Do the research; tee up the work. The human only signs in (logins/2FA), solves any CAPTCHA, approves drafts, clicks the final commit or pay button, or places phone-only reservations.

Not for: single-site availability watchers or non-RV travel.

## Glossary

- **Toad / tow car:** the car towed behind a motorhome. This template says "tow car".
- **Total length:** coach + tow bar + tow car (+ any rear rack or bike carrier).
- **Stop point:** the last screen before any button that commits, charges, holds or submits.
- **Go/No-Go poll:** an independent check of the board by a fresh subagent (the "Flight Director").
- **T-minus 7 / T-minus 1:** 7 days and 1 day before departure.
- **Dry night:** a night with no hookups.
- **HOLD:** can't be verified yet; names who clears it.

## Settings

Setup is a hard gate. Before any planning or background work, check the settings. If a required setting is missing or not yet confirmed by the owner, run the RV trip planner getting started skill first, even if the owner already named a trip. Values found in memory must be shown and confirmed, not silently used. Never invent a value. Never use a built-in default for a value the owner owns. Never start work on a guess (e.g. an assumed hours-per-day pace).

Accepted answers: a value, "none", "no limit", or "don't know". For "don't know", offer a suggested value and say why. Save it only when the owner confirms it. Record the source as "suggested, confirmed".

Settings live in two places:
1. **Rig & owner settings** (persist across trips). Saved in a private settings file during setup, with a one-line note per answer. They include: rig specs (total length, height with source, weight, axles, propane, reverse/unhook), shore power (30 or 50 amp, adapter OK or not), generator, fuel (tank, MPG, reserve, blend limits, fuel brand preferences), pace range and hard max with unit, budget (nightly target/ceiling, trip total), ranked memberships, dry/dump limits, wind limit, pets, connectivity, booking contact, park call questions, calling method, routing service, thoroughness and display choices.
   - Optional, if the owner wants: generator hours per dry night, fresh/gray/black tank sizes, propane tank type and size (built-in tanks need a drive-up refill, not an exchange). Look these up from specs; the owner confirms.
2. **Trip settings** (per trip). Saved with that trip's board. They include: dates, start and destination, starting fuel, waypoints, avoidances, and every rule or exception accepted during planning.

Each row has its source and date. The board shows both sets at the bottom (see the RV trip board spec skill). They drive all calculations and the Go/No-Go poll. When a new parameter comes up, add it to the right set and to setup. Ask before saving anything learned outside setup.

Calculated or looked up, not asked: leg miles and drive time, fuel gallons and cost, tank levels, weather, generator burn (from the generator's spec sheet if the owner doesn't know it), towing MPG estimate if the owner has none.

## Site-fit rules by rig type

- Motorhome with a flat-towed car: cannot reverse while towing. Require pull-throughs or drive-around access. A site sized for the coach alone is OK only if the car can be unhooked on site.
- Dolly or trailered car: treat as reverse-limited unless the user says otherwise. Count total length.
- Travel trailer, fifth wheel, toy hauler: can back in. Size to combined length if the tow vehicle stays hitched, else trailer length plus parking.
- Tall rigs and fifth wheels: check height for routes, trees and overhangs using the settings height and its source (measured, garage spec, or listed). Any clearance within about 6 inches of the rig height is HOLD until checked. Note slide-out clearance on tight pads.
- Use total length for sites, driveways and host length caps.
- Shore power: pick sites with the owner's amp service (30 or 50). Use a lower-amp site only if the owner said an adapter is OK.
- Tow-car placement: only ask about room for the tow car when a site is both narrow and short. A site at least as long as the total length from settings is fine. A wide site with room beside the coach, slides out, is fine.
- Official site length rules. The park's official site or listing decides fit. If it lists no length, mark the stop **UNVERIFIED** (HOLD) and clear it with a call question or the park's reply. Reviews never override an official length. But a review that mentions tight sites, low branches or a hard entrance triggers a visual check and a call question.

## Inputs

Ask only for what's missing:
1. Start date and departure location (required)
2. Destination (required); arrival date optional, else derive from legs
3. Starting fuel for this trip
4. Fixed waypoints, must-sees, hard avoidances

## Trip board

Follow the **RV trip board spec** skill exactly for the page, layout, colors, columns and the stop status list. Don't copy the spec here.

Each day row holds:

| Field | Notes |
|---|---|
| Day / dates | Stay nights |
| Miles | Leg miles / cumulative and drive time (routed, see Distances) |
| Overnight | Primary + up to 2 alternates, with name, city/state, membership badge |
| Price | Per night and stay total incl. taxes and fees; note host spend expectations |
| Rating | If on: 1–5 stars from the last 12 months, newer and big-rig reviews weighted more; Harvest Hosts uses the host page rating; "too few recent" when thin |
| Hookups / site | Full / partial / dry, amps, site type and official length (or UNVERIFIED) |
| Fuel | Station, gallons, tank estimate after; cost only on fill-up rows |
| Weather | Freeze, snow, wind, storm flags (forecast or climate history) |
| Route flags | Steep grades, passes, propane restrictions, late arrival |
| Must-sees | Rig-accessible only |
| Links + source age | Official/reservation link, membership listing link when public, request forms, call sheets; check date |
| Status + next action | One status from the RV trip board spec status list, plus the next action |

Freshness: flag listing facts (lengths, rules, prices) over 12 months old. Re-check availability older than 14 days.

Leg colors use the pace range (from–to) and hard max. In range: normal. Outside the range but accepted by the user: amber. Outside the range and not yet accepted: red. Don't re-flag accepted legs. Never go over the hard max without asking.

Optional Google Sheets export once settled. Mirror the layout of the past-stays sheet if one was shared. Confirm before writing.

## Distances and routing

Use a real router, never straight-line guesses. Always ask which routing service to use before the first route, and save it. Show the choices:
1. **OSRM** (no signup): car-only, so height, weight and propane are ignored. The public demo server is for light use only (about 1 request per second, no heavy use). Cache results.
2. **OpenRouteService**: free signup, no card. Truck profile with height and weight. Daily limits apply.
3. **HERE**: truck routing with height, weight, length and axles. Needs an account and a card on file, even on the free plan. Has no spending cap. Free-tier limits as of 2026-10; re-check terms if that is older than 90 days.

Mileage margin (one rule, used everywhere): with a car-only router (OSRM), add 5% to routed miles and drive time, and flag legs that leave the interstate. With a truck router, use the routed numbers as is.

Pace: if the owner's pace is in miles, compare leg miles. If it's in hours, compare routed drive time (plus the margin), not miles. Add stop time only if the owner wants it.

Separate checks on every route, whatever the router:
- **Propane:** check tunnel and bridge propane restrictions on the route. Rules vary (bans, valves off, cylinder limits). Note the source and date. Don't avoid whole areas on a guess.
- **Grades and passes:** flag mountain passes and long steep grades (posted grade signs, or about 6% or more). Note them on the board so the driver can plan gears and engine braking.
- **Arrival:** estimate arrival from the usual departure time and drive time. Flag any stop reached after sunset, or outside its check-in or host hours.

Store any API key the same way as every other key (see Secrets). Cap calls and cache results.

## Check thoroughness

Quick, smart or deep. Ask the owner before research starts. Suggest smart, and save only once confirmed. Smart: do costly visual checks (satellite, Street View) only for risky stops: non-truck-stop fuel, parks with tight-site reviews or no official length, and places new to the user. Known truck stops and past stays get quick text checks. Quick: text checks only. Deep: visual checks everywhere.

## Overnight priority

Pick stays in the owner's ranked membership order from settings. Within and after that:
1. Membership stays in rank order (e.g. home hosts, discount parks, winery/farm hosts)
2. Free or permission parking with recent confirmation (call-ahead flags), if the owner allows it
3. State, county, city and regional parks (often the cheapest big-rig sites with hookups)
4. Private RV parks and resorts within the nightly ceiling

Up to 3 options per night. One strong verified option beats three stale listings. Research order per night: membership directories → public parks → private parks.

- **Cheaper nearby check:** before teeing up any paid park, list the other parks in the same town or within about 10 mi of the route, with prices. If one costs about half as much or less and fits the rig, show it first or flag it (e.g. a $75 park when a $35 park in the same town had space). Never quietly pick the pricier one. Record why a paid park won over a cheaper one.

Home and street hosts (driveways, street parking): check the city's posted oversize-vehicle parking rules (some ban large vehicles on posted streets overnight) and the host's rules. Line up a backup.

## Privacy

Boards, screenshots and reviews shared outside the owner never show the travelers' real stays, dates or locations unless the owner says so for that item. Use sample data for demos.

## Past stays

If the user shared a past-stays sheet, read all tabs, cell notes and comments. Reuse places they liked: dates, site number, notes. Suggest the same site when it still fits.

## Membership and availability

- Use logged-in browser sessions. Many public calendars need no login: check those first and ask for a login only at booking.
- When a site is signed out, open its login page and send the user a secure sign-in form. Credentials go straight into the page; never see, store or echo them. After a fill, don't reload the login page (it clears fields); click submit on the current page.
- Respect each site's terms and rate limits. Prefer official APIs and public calendars. Space out requests.
- **Never attempt a CAPTCHA.** On a CAPTCHA, stop and hand that step to the user.
- Never store passwords.
- Harvest Hosts: use Plan-a-Trip / route filters (distance to route, total length, generator). National map pins are not route results. Don't click review buttons while reading. Harvest Hosts and Boondockers Welcome share one website and app but are separate plans (sold alone or bundled, e.g. HH + Boondockers Welcome or All Access, as of 2026-10). Check which plan the owner holds; don't assume one includes the other.
- Passport America: read each park's terms (percent, days, max nights, season, promo code, drive-up only). Dates are on the park's site or phone.
- Supported sites (read-only checks): Harvest Hosts, Boondockers Welcome, Passport America, Recreation.gov, ReserveAmerica and state park systems, county/regional reservation portals (set the max RV length filter; a "no sites available" result means sold out), KOA, Good Sam, Thousand Trails, Firefly/Campspot/Newbook/RoverPass engines, Hipcamp, and park sites. Add sites as they are proven.
- Verification order: online calendar → form or email inquiry → phone.
- Red flags: months of empty calendars, or reviews mentioning conversion to worker housing or cancelled reservations. Find alternatives.
- Never claim availability without a dated source.

## Inquiries and stay requests

- Draft short (2–3 sentences: dates, total length, arrival window, thanks) from the owner's booking name and email. Show recipient, subject and body. Never send without the owner's confirmation.
- Draft inquiries after the board's Go/No-Go poll. If one must go out sooner (a window is opening), run a narrow Stays + Rig Fit poll for that stop first.
- If a request window isn't open yet, offer a scheduled routine that sends the approved text when it opens. Before it sends, re-confirm with the owner if the dates, rig or plan changed since approval. Tell the owner when it goes out. Log every contact on the board.

## Phone calls (optional)

Use calls for phone-only parks, or when the website and listings can't answer a question. Calls use Grok Bot's built-in phone calling. When it can't be used (see limits below), make a call sheet for the owner.

Rules for every AI call:
- Only call business numbers listed on the park's own official site. Before each call, confirm: "Is this a listed business line?"
- **Never AI-call Harvest Hosts, Boondockers Welcome or other private hosts**, or any private home or personal cell. Use the platform's messaging, or give the owner a call sheet.
- Call only during the park's local office hours. Watch time zones.
- At most one retry per park per day.
- Before **each** call, get the owner's OK in their latest message. Never place or retry a call on your own initiative, from a routine, or because of something said earlier.
- Availability and price questions only. Never book, hold a site, or give card details.
- Log the extracted answers, the outcome and the time on the board. The call card keeps its own recording for 30 days; don't save audio anywhere else.
- Recording and calling laws vary by state; this is not legal advice.

### Built-in calling (Grok Bot)
No setup and no extra accounts. On the first call the owner accepts Grok Bot's calling terms.
- Limits set by Grok Bot: US owners calling US numbers only (not Washington or Illinois), up to 10 calls in 24 hours and 4 to the same number, 85 minutes per call. Outside these, make a call sheet.
- Transfer number: at the first call, ask once for the owner's own cell and save it in settings as "Transfer number". With it, the owner can take over a live call, and the voice agent can bring them in when the office needs them. Never ask the voice agent to transfer for anything the owner can answer in chat.
- Before each call, tell the owner in short bullets: park, number, what the call should get, the facts it will share (rig, dates, first names), and the transfer number. Don't read the script back. Say the voice agent may send them a question card during the call. Then place the call. The owner taps Call on the approval card; nothing is dialed before that. The card can't be edited; to change a call, place a new one.
- Script: under 2,000 characters, written to the voice agent as two labeled lines, `Objective:` (one sentence) and `Additional context:` (rig length and type, tow car, total length, dates, the questions to ask, the limits: no booking, no holds, no card). The voice agent knows only what the script says.
- The voice agent opens on its own: it says it is a virtual assistant calling for the owner on a recorded line, then states the purpose. Don't script the disclosure, and never ask it to hide that it is an AI or to pretend to be the owner.
- Tell it to wait until the office finishes its greeting before it asks the first question. (A test showed the first question can get cut off if it starts too soon.)
- During the call, if it needs a detail that isn't in the script, it shows the owner a question card and waits about 60 seconds.
- When the call ends, read the transcript. Report the answers in plain words and update the board. If the call failed or hit voicemail, offer one retry and place it only if the owner says yes.

### Call style
- Research the park's website and listings first. Never ask what is posted there.
- Wait for the office to speak first (a "hello" or the park's name).
- After the built-in disclosure, ask one clear opening question that names the rig, tow car and dates. Model: "I'm checking to see if you have availability for a <length>-foot <rig type> and a <tow car> for <dates>?"
- Speak in the third person about the travelers. Never pretend to be them.
- If asked who it's for, give the travelers' first names from settings. If asked again whether it's an AI, say yes.
- If they object to an automated call, apologize, thank them and end the call.
- Wait silently while they look it up. Don't fill the silence.
- If no: thank them and end the call.
- If yes: answer their questions. Then ask the price by site type, and ask about the owner's discount memberships (only the ones in settings).
- Ask the park call questions below only if they aren't posted.
- Never say anything that sounds like accepting a site, even if the office offers to book or hold one.
- Close with a neutral line, such as: "Thanks, that's helpful. They're still working out other details on the trip, so they'll call back when they're ready to make the reservation."
- Voicemail: hang up by default. If the owner chose to leave messages, leave one only after a clear beep or a "leave a message" prompt. The message gives the dates, rig and the callback number from settings.

### Always-check items for any park (website first, then ask)
All on by default. The user can turn any item off or add their own at the first park call; save the choice as "Park call questions". Even for items turned off, ask anyway when pre-call research raises a concern (e.g. posted age limit, short or unlisted sites, tight-site reviews, tricky entrance). Look up enabled items on the park's site and listings before any call. Put answers you find in the call facts. Ask on the call only what isn't posted, and only after they confirm availability:
1. RV age limit ("10-year rule"): give the rig's model year.
2. Tow car: does it fit on the site, or is there overflow parking? Use the tow-car placement rule (ask only when the site is both narrow and short).
3. Choosing a site: can the user pick one? If yes, ask for the easiest pull-through for the rig.
4. Big-rig access: are GPS directions OK, or is there a better way in? Any tight turns or low branches?
5. After-hours check-in: how does it work if the office is closed?
6. Cancellation policy.

## Secrets

Store every key the same way: routing services, tile providers and any other API keys. Use the bot's secrets form (masked). Never put keys in chat, memory text, board files or logs. Never print them.

## Fuel, dump, weather

- Fuel plan: start from the trip's starting fuel. Never go below the owner's reserve. Use their MPG and tank. Add generator burn on dry nights. Most built-in generators shut off at about 1/4 tank, so generator use rarely empties the tank, but it does cut usable range. Respect blend limits and the fuel brand preferences.
- Fuel brand preferences (settings): brands to avoid, and preferred stations or cards (e.g. a preferred station chain or a card with fuel rewards). Skip stations that won't take the owner's preferred card if they say so. For diesel rigs, check each station sells diesel and has room for the rig.
- Diesel rigs: plan DEF for 2010 and newer diesel engines. Blend limits: #2 diesel may legally contain up to 5% biodiesel (B5). Some engines and generators dislike higher blends such as B20, so record the owner's limit as "B5 max", "B20 OK", etc.
- Fuel cost: only on fill-up rows. Each fill = (tank size − gallons on arrival) × that state's AAA average for the rig's fuel (diesel average for diesel rigs). The board names the price source (AAA state average, or the station's GasBuddy price with its date). Other rows show tank level only. Show a trip total of gallons and cost.
- Fuel stop sanity: no token stops. If a stop buys under about 10 gal (or under about 10% of the tank), drop it if the arrival reserve stays acceptable, else make it a fill-to-full. Before a long stay or at the trip's end, fill up in the cheapest-diesel (or cheapest-fuel) state on the route.
- Station vetting: read GasBuddy notes, amenities and recent reviews for diesel blend and big-rig access. Note the GasBuddy price with its date.
- Visual check (per thoroughness; most important at warehouse clubs and regular stations): satellite view for a drive-through path with no reversing, wide turns and no steep driveway; Street View for canopy height and pump labels (e.g. a B20 pump label). Note the imagery date; ignore old prices in imagery.
- Dump/water: never exceed the owner's max dry nights or dump cadence. Insert dump stops as needed.
- Weather before forecasts exist: use climate history for the actual trip dates, per stop: share of days with gusts at or above the owner's wind limit, freeze nights, snow and storm risk, plus the seasonal outlook. Use it to choose between parks when booking far ahead. Replace with live forecasts at T-minus 7 and T-minus 1.
- Flags: freeze → hoses and tanks; snow/ice → southern bias; wind over the limit → exposed stops, toppers and awnings, tall-rig risk.

## Price traps

Never apply a discount the owner doesn't hold (only memberships in settings). Include taxes, resort and card fees. Quote the official rate page with its date.

## Reservation tee-up

- **Stop point:** stop before any button that commits, charges, holds or submits a request, whatever its label ("Reserve", "Confirm", "Complete", "Request", "Book"). Examples: Thousand Trails member reservations and Harvest Hosts stay requests have no pay screen, so their confirm or request button is the commitment. A review page with a saved card can charge on "Reserve".
- **Never select or use a saved card** or saved payment method. The owner picks payment.
- **One booking at a time,** while the owner is present. Carts and holds expire, so never batch live carts. Prepare the form data for every stop in advance, then tee up each one in turn. Tell the owner how long the cart is held, if the site says.
- **Site-fit search:** when a park's online calendar filters by rig length, search with the coach's or trailer's own length (not the whole train) if the owner can unhook and park the toad separately; otherwise shorter sites that fit stay hidden. Use the whole-train length when a pull-through is needed. Tell the owner which length you searched.
- **Satellite + site map:** before picking a site type, pull a satellite image of the park (e.g. the Esri World Imagery export endpoint, no key needed) and the park's site map if published. Check pad surface, row layout, pull-through vs back-in and tight turns. Show them to the owner and let them choose the site type or range before tee-up. Don't assume the XL or gravel type is better.
- Wait until the route is settled, unless the owner wants one now or a window is closing.
- **Booking details:** at the first tee-up, collect what forms need and wasn't asked at setup: phone number, party size (adults/kids), pets, license plates, number of slides, and an address if the form needs it. Store them privately. Confirm them with the owner before each use.
- Request forms: fill and stop before submit unless approved.
- Phone-only: call sheet (number, hours, dates, site type, rig details, questions). Never pretend a phone hold happened.
- Never auto-pay. Never send, submit or book without explicit confirmation for that item. Confirm before any spend, email or calendar write.

## Status dashboard

On request ("what's outstanding?"), show one row per night: date | overnight | city, state | leg / cumulative miles | hookups | status (from the RV trip board spec status list) | next action. Re-check availability older than 14 days.

## After the trip: reviews

Offer a short review per stay (Harvest Hosts, Campendium, Google, etc.). First ask what to include (highlights, issues, tips). Don't name private hosts in public reviews by default; ask before including any name. Draft, show the text and the target site, and post only after approval of each review.

## Feedback

Follow "After the first board" in the RV trip planner getting started skill (draft only; the owner sends).

## Pipeline

1. Setup gate: Rig & owner settings complete and confirmed (RV trip planner getting started skill); trip asked last
2. Ask the routing service; lock route and leg boundaries with routed miles and drive time
3. Route checks: propane restrictions, grades and passes, arrival times
4. Mine past stays
5. Candidates per night (ranked memberships + rig fit)
6. Live availability from public calendars and listings (read-only)
7. Fuel, dump and weather plan
8. Trip Go/No-Go poll (Trip Go/No-Go skill) in a fresh subagent; fix NO-GO rows
9. Show board + map with the verdict line first; get approval
10. Offer to schedule the T-minus 7 and T-minus 1 Go/No-Go polls as routines (owner's OK). Each routine runs the poll and reports. It never books, sends or pays.
11. Draft inquiries, stay requests and call scripts; send or call only with approval
12. Re-poll, then tee up bookings one at a time to the stop point
13. Optional Sheets export
14. Close finished research tabs; keep sign-in tabs and the board

## Pitfalls

- Stale free-camping reports
- Host length caps and business-hours arrival
- Assumed park discount terms; weekend/holiday exclusions
- Reversing with a car attached
- Low bridges, weight-limited roads, propane-restricted tunnels and bridges; clearances within ~6 inches of rig height left unchecked
- Steep grades and passes not flagged; arriving after dark
- Car-router miles for a big rig without the margin
- Parks with no official site length treated as fine
- Parks quietly converted to worker housing
- Reloading a filled login page
- Clicking review buttons (Helpful etc.) while reading listings
- Treating "longest rig reported" in reviews as the official length
- Stale calendar banners in a past-stays sheet
- Batching live carts that expire

## Done checklist

- [ ] Every day has miles, stop, price, rating (if on), hookups, weather, fuel, availability, links, next action
- [ ] Legs within the pace range, or accepted by the owner
- [ ] Route checked for propane restrictions, grades and passes, and arrival before dark and within check-in hours
- [ ] Fuel never below reserve; dump cadence respected
- [ ] Stays follow the ranked memberships; only held discounts applied
- [ ] Live checks or inquiries logged with dates
- [ ] Settings complete and current
- [ ] Go/No-Go poll run in a fresh subagent; verdict shown first (GO for launch, or NO-GO/HOLD with stations); station strip on the board. Re-poll before booking tee-up. T-minus 7 and T-minus 1 polls offered as routines
- [ ] Tee-ups stopped at the stop point, one at a time, no saved card
- [ ] Sheets export only if requested and confirmed
