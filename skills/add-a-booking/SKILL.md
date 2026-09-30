---
name: add-a-booking
description: Turn pasted booking confirmation, itinerary email, or PDF text into Tineo trip items, after the user reviews a preview. Use this when the user pastes or describes a confirmation for a flight, hotel, car rental, train, or event and wants it in a trip, asks how to forward booking emails to Tineo, or asks why an email import failed.
---

# Add a booking

Use this skill to add bookings to a trip from text the user provides, with a preview before anything is saved.

## Priorities and guardrails

- Explicit user instructions take priority over the guidelines in this skill. They never override the Tineo server's own authorization checks or confirmation requirements.
- Never show internal IDs or UUIDs (trip, segment, or import IDs). Refer to items by title, date, place, or route.
- Use YYYY-MM-DD for dates and 24-hour HH:mm for times when passing them to tools.
- Nothing is saved until the user confirms the preview.
- Never invent flight numbers, times, confirmation codes, or prices, and do not create items the preview did not produce. If a detail is missing, ask the user.
- Do not repeat the raw pasted text back to the user.
- If a tool errors or the request is not supported, say so briefly and offer to open a support ticket. Call `support_ticket_create` only after the user agrees.

## Workflow: pasted confirmation

1. Call `import_parse_text_preview` with the pasted text. This saves nothing.
2. Show the proposed items in plain language: type, name, dates, times, place, and confirmation code when present.
3. Choose the target trip. Call `trips_list` (use `search` with the destination) and ask the user to pick a trip, or propose creating one with `trip_create` after confirming its title and dates.
4. If any item falls outside the trip dates, propose extending the trip with `trip_update` and wait for a yes.
5. Ask the user to confirm the items. Then save them with `batch_segment_create` (at most 20 per call; all-or-nothing), using `segment_get_schema` for field names when needed. For a single item, `segment_create` is fine.
6. Report what was saved. Adding a booking to Tineo does not change the reservation with the provider.

## Workflow: forwarding emails

1. For "how do I forward booking emails", call `forwarding_address_get_or_create` with `operation=get` and share the address.
2. Only if the user explicitly asks to create a personal address, call it with `operation=create` and `confirmed=true`. A requested friendly name needs a paid plan; if unavailable, explain that briefly without promoting an upgrade.

## Workflow: failed import

1. Call `diagnose_failed_import` (with the trip when the user names one).
2. Relay the failed step, the likely reason, and the suggested next actions in plain language.
