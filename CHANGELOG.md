# Changelog

## v9 (2026-11-04)
Lessons learned from a real trip:
- Cheaper nearby check (RV trip planning, Go/No-Go Budget): compare parks in the same town or within about 10 mi of the route; flag one at about half the price that fits; record why a pricier park won.
- Fuel stop sanity (RV trip planning, Go/No-Go Fuel, board spec): no token stops under about 10 gal or 10% of the tank; drop or fill to full; fill in the cheapest-fuel state before long stays and at trip end.
- Site-fit search (RV trip planning): search length-filtered calendars with the coach or trailer length when the toad can park separately; say which length was searched.
- Satellite + site-map check (RV trip planning): show a satellite image and the park map, and let the owner pick the site type before tee-up.
- Vehicle details at setup (Getting started): ask each vehicle's color and license plate too, and save them for reservation forms.

## v8 (2026-11-01)
- Built-in monthly self-update: setup creates a "Monthly template update" routine (9 AM on the 1st). It updates only the 4 planner skills, asks first if you edited one, stays quiet when current, and stops with "turn off updates". Replaces the weekly update-check offer.
- RV trip planning: privacy rule for shared boards; review-reading and stale-sheet pitfalls; 90-day re-check on dated prices; feedback steps now live in getting started.
- Phone calls now use Grok Bot's built-in calling (approval card per call, built-in AI and recording disclosure, optional transfer number, question cards mid-call). The hosted calling service and self-built Twilio caller options are removed.
- Getting started: sharper description; routing choices point to one source in RV trip planning.
- Trip board spec: pinned Leaflet 1.9.x, file:// tile test, privacy rule (sample data on shared boards).
- Trip Go/No-Go: creates decisions.tsv if missing; reports times in the owner's time zone.

## v7 (2026-10-18)
- Getting started: first-run setup of vehicles, power, pace, memberships, budget and hookup needs; feedback offer.
- RV trip planning: paced legs, rig-safe routing, overnight stops ranked by memberships, live availability, fuel/dump/weather, inquiry drafts, booking tee-up that stops before paying; optional automated park-call caller.
- Trip board spec: one HTML board per trip with statuses, colors, map (with tile fallback), day table and settings table.
- Trip Go/No-Go: independent Flight Director poll (Route, Rig Fit, Fuel, Stays, Weather, Budget, Bookings) before a board is shown, before bookings, and at T-7 days.
