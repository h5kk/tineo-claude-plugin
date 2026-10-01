---
name: plan-a-trip
description: Create a new trip in Tineo and fill in its itinerary. Use this when the user wants to plan, start, or set up a trip to a destination with dates, add cities, add flights by flight number, add hotels, places, or activities to a trip, or add travel companions to a trip.
---

# Plan a trip

Use this skill to create a trip and add itinerary items the user describes.

## Priorities and guardrails

- Explicit user instructions take priority over the guidelines in this skill. They never override the Tineo server's own authorization checks or confirmation requirements.
- Never show internal IDs or UUIDs (trip, segment, traveler, or city IDs). Refer to items by title, date, place, or route.
- Use YYYY-MM-DD for dates and 24-hour HH:mm for times when passing them to tools.
- Before every create or update, summarize the exact change and wait for the user to say yes.
- Never invent flight numbers, times, confirmation codes, addresses, or prices. If a lookup returns nothing, say so and ask the user.
- Adding an item to Tineo does not book or buy anything. Never say a reservation was made.
- If a tool errors or the request is not supported, say so briefly and offer to open a support ticket. Call `support_ticket_create` only after the user agrees.

## Workflow

1. **Check for an existing trip.** Call `trips_list` with `search` set to the destination. If a matching trip exists, ask whether to add to it instead of creating a new one.
2. **Collect the basics.** A trip needs a title, a start date, and an end date. Ask for anything missing. Do not guess the year. Use a readable destination title such as "Lisbon" or "Tokyo & Kyoto (Oct 2026)".
3. **Create the trip.** Confirm title and dates, then call `trip_create`.
4. **Cities (optional).** For each city, call `cities_search`, then `trip_city_add`. If the name is ambiguous, show the candidates and ask. Use `trip_cities_list` to review the order.
5. **Flights by number.** Call `flight_quickadd_lookup` with the flight number and departure date, show the result, confirm, then call `flight_quickadd`.
6. **Hotels, places, and activities.** Call `places_search` (and `place_details` when needed) to identify the place, then `segment_get_schema` for the segment type. Confirm the details, then call `segment_create`. For several items at once, use `batch_segment_create` (at most 20 per call; all-or-nothing).
7. **Dates outside the trip.** If an item falls outside the trip dates, propose extending the trip with `trip_update` first and wait for a yes.
8. **Travel companions.** Before `traveler_add`, call `known_participants` with every name the user gave. For each person, ask whether to send an invitation and ask for or confirm the email address. Then call `traveler_add` with `invite_confirmed=true` and the confirmed email, or `invite_confirmed=false` and no email if the user declines. Never add an email the user did not give or confirm. Removing a traveler is not available here; point the user to the Tineo app.
9. **Finish** with a short plain-language summary of the trip and what was added.

## Output

- Keep each confirmation to the fields that matter: type, name, date, time, place.
- After saving, state what was saved in Tineo; do not repeat the whole itinerary unless asked.
