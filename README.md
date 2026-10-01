# Tineo for Claude

Tineo keeps your travel plans in one place: flights, stays, transport, and activities, organized by trip and day. This plugin connects Claude to your Tineo account and adds four guided skills so Claude knows how to work with your trips.

## Use it

1. Add the plugin, then connect the Tineo connector from the plugin's **Connectors** tab and sign in with your Tineo account. A Tineo account is required; you can create one at [tineo.ai](https://tineo.ai).
2. Ask Claude about your travel. The skills cover:
   - **Trip overview**: "What trips do I have coming up?", "Show my Lisbon itinerary", "What's my hotel in Kyoto?"
   - **Plan a trip**: create a trip, add cities, add flights by flight number, hotels, places, and travel companions.
   - **Add a booking**: paste a confirmation email or PDF text; Claude shows a preview before anything is saved.
   - **Day of travel**: flight status, recorded schedule and gate changes, airport conditions, and weather.

You can also ask Claude to change your Tineo display units, date and time formats, flight monitoring, and trip reminder emails.

Claude asks before it saves, replaces, or deletes anything. Editing a saved itinerary changes your Tineo records only; it does not change a reservation with an airline, hotel, or other travel provider.

## Data

The plugin itself runs no code and stores nothing. Its skills are instructions for Claude. The bundled connector sends your requests and the trip details you share to your Tineo account at `https://api.tineo.ai/mcp/claude`, over OAuth, and reads your trips from there. See the [Tineo privacy policy](https://tineo.ai/privacy) and [terms of service](https://tineo.ai/terms).

## Support

Visit [tineo.ai/support](https://tineo.ai/support), or ask Claude to open a Tineo support ticket.
