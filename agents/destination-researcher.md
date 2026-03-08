---
name: destination-researcher
description: Browses travel blogs, TripAdvisor, and local guides to research and compare family-friendly destinations based on a rough brief.
---

# Agent: destination-researcher
Type: sub-agent (spawned by commands)

## Purpose
Research and compare travel destinations suitable for a family of 6, given a rough brief describing preferences, dates, and budget.

## Tools available
- Browser via Chrome connector (opens visibly in the user's Chrome — the user can watch searches happen in real time)
- File read: `data/family-profile.yaml`

> **Note:** This agent does not have native browser access. All web browsing is handed off to the Claude in Chrome extension connector. Chrome must be open and the connector enabled in Settings → Chrome Connector.

## Behaviour rules
> **Stub — implemented in Milestone 4**
>
> When implemented, this agent will:
> 1. Search Google for "best family destinations [month] [region/vibe]"
> 2. Browse 2-3 travel sites (Lonely Planet, Condé Nast Traveler, TripAdvisor family guides)
> 3. Filter by: flight time from CPH, family-friendliness, weather in target month, budget fit
> 4. For each shortlisted destination, browse for: typical flight cost (6 pax), Airbnb prices (4BR), top family activities, car rental availability
> 5. Return 2-3 curated options with rough budgets

## Output schema
Per destination:
- destination_name, country
- why_it_fits (brief summary)
- flight_estimate (6 pax return)
- accommodation_estimate (per night and total)
- car_rental_estimate
- food_and_activities_estimate
- total_estimate
- top_activities (list)
- best_areas_to_stay (list)
- weather_summary
