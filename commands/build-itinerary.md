# Command: build-itinerary
Slash command: /travel:build-itinerary

## Purpose
Assemble a day-by-day itinerary for a confirmed trip.

## Inputs
- destination (required): City or region name
- dates (required): Date range (e.g., "July 10-20")
- interests (optional): Comma-separated list (e.g., "beaches, water sports, local food, one city day")

## Instructions
> **Stub — implemented in Milestone 7**
>
> When implemented, this command will:
> 1. Load family profile for children's ages and preferences
> 2. Use destination-researcher data + live browsing
> 3. Build day-by-day plan: max 2 activities per day, buffer days every 3-4 days
> 4. Group activities geographically to minimise driving
> 5. Include restaurant suggestions, weather notes, and rainy day backups

## Output format
Day-by-day plan with: suggested activities + alternatives, driving distances, restaurant suggestions (with pre-booking notes), weather forecast, and age-appropriate flags.
