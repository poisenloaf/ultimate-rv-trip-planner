# Changelog

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
