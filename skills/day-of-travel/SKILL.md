---
name: day-of-travel
description: Give a day-of-travel briefing for an upcoming or ongoing Tineo trip, covering flight status, recorded schedule or gate changes, airport conditions, getting to and from the airport, and weather. Use this when the user asks whether a saved flight is on time, what they need to know for a travel day, or how to get from the airport.
---

# Day of travel

Use this skill to brief the user on a travel day using flights already saved in Tineo. This skill only reads data.

## Priorities and guardrails

- Explicit user instructions take priority over the guidelines in this skill. They never override the Tineo server's own authorization checks or confirmation requirements.
- Never show internal IDs or UUIDs (trip, segment, or city IDs). Refer to flights by airline, flight number, date, and route.
- Use YYYY-MM-DD for dates and 24-hour HH:mm for times when passing them to tools.
- This workflow makes no changes. If the user asks to change a flight or booking, summarize the change and wait for a yes before calling any tool that saves data.
- Never invent gate numbers, terminals, delays, or times. If data is missing or stale, say so.
- If a tool errors or the request is not supported, say so briefly and offer to open a support ticket. Call `support_ticket_create` only after the user agrees.

## Workflow

1. Find the trip with `trips_list` (`status` set to `ongoing`, then `upcoming`). If several trips match, ask which one.
2. Call `trip_details` with `segment_types` set to `Flight` and pick today's or the next flight. If there is no saved flight, say so and offer to add one.
3. Call `flight_status_for_segment` for that flight.
4. Call `get_flight_change_history` for the trip or flight to list recorded schedule, gate, or delay changes.
5. Call `get_day_of_travel_brief` for the flight to get airport and ground-transport guidance. Use `get_flight_airport_conditions` for delay and weather conditions at the airports; it reads cached data only, so say when it is empty or old.
6. Call `get_trip_weather` with `detail` set to `current` or `hourly` for the relevant city (or `daily` for the days ahead).

## Output

Keep the brief short, in this order:

1. Flight: status, scheduled and latest times, terminal and gate when known.
2. Changes since booking, if any.
3. Airport conditions.
4. Getting to or from the airport.
5. Weather.
6. Suggested actions, such as leaving earlier if a delay is reported.

Label anything uncertain, and never present cached conditions as live.
