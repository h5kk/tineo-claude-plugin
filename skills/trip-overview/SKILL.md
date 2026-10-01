---
name: trip-overview
description: Show or summarize the user's Tineo trips. Use this when the user asks what trips are coming up, wants a trip's itinerary or a single booking's details, or asks for a trip's cost summary, travel stats, or a packing list. Also the starting point for someone new to Tineo who asks what it can do.
---

# Trip overview

Use this skill to answer questions about trips already saved in the user's Tineo account.

## Priorities and guardrails

- Explicit user instructions take priority over the guidelines in this skill. They never override the Tineo server's own authorization checks or confirmation requirements.
- Never show internal IDs or UUIDs (trip, segment, traveler, or import IDs). Refer to items by title, date, place, or route.
- Use YYYY-MM-DD for dates and 24-hour HH:mm for times when passing them to tools.
- Summarize any change and wait for the user to say yes before calling a tool that saves, replaces, or deletes data.
- Never invent flight numbers, times, confirmation codes, prices, or bookings. If a tool returns nothing, say so and ask the user.
- If a tool errors or the request is not supported, say so briefly and offer to open a support ticket. Call `support_ticket_create` only after the user agrees.

## Workflow

1. If the conversation already identifies a Tineo trip, such as one found or discussed earlier, use that trip and skip the lookup. Call `trip_details` with that trip for anything the conversation doesn't already cover, such as a booking's details.
2. Otherwise call `trips_list` with `status` set to `upcoming` (or `ongoing`, `past`, or `all` when the user asks). Use `search` when the user names a destination or trip.
3. If more than one trip matches and the request is about one trip, ask which one.
4. For an itinerary, call `trip_details` and summarize segments in date order, grouped by day.
5. For one booking (a hotel, flight, or activity), call `segment_details` for that item.
6. On request only:
   - Costs: `get_trip_expense_summary`.
   - Travel history and totals: `get_travel_stats`.
   - Packing list: `generate_packing_list`. This saves a checklist to the trip, so confirm first. Pass `replace=true` only when the user explicitly asks to regenerate an existing list, and say that it overwrites unchecked items.

## Output

- Lead with the answer: trip name, dates, and destination, then the relevant items.
- Keep lists short; offer to show more rather than dumping every field.
- For a first-time user with no trips, explain in one or two sentences that they can create a trip, paste a booking confirmation, or add flights by number, and ask what they want to do.
- When a first-time user asks what Tineo can do, also mention in one sentence that they can ask you to change their Tineo display units, date and time formats, flight monitoring, and trip reminder emails.

## Do not

- Do not claim a reservation exists with a provider; Tineo stores the user's own records.
- Do not use this skill to create or edit itinerary items; the plan-a-trip and add-a-booking skills cover those.
