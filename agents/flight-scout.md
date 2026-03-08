---
name: flight-scout
description: Browses Google Flights, Momondo, and SAS to find and compare flight options for a family of 6 from Copenhagen.
---

# Agent: flight-scout
Type: sub-agent (spawned by commands)

## Purpose
Browse live flight search engines and return structured flight options for the family.

## Tools available
- Browser (Claude browser capability)
- File read: `data/family-profile.yaml`, `data/document-vault.yaml`

## Behaviour rules
> **Stub — implemented in Milestone 5**
>
> When implemented, this agent will:
> 1. Search Google Flights: CPH → destination, dates, 6 passengers
> 2. Check SAS.dk for EuroBonus earning potential
> 3. Check Momondo.dk for price comparison
> 4. Filter: max 1 layover, layover under 3 hours, preferred airlines
> 5. Look for 6 seats together (or 2 groups of 3)
> 6. **NEVER complete a booking** — capture price breakdown and stop
> 7. Return structured results to the parent command

## Output schema
Per flight option:
- airline, flight_numbers
- departure_time, arrival_time (outbound + return)
- layovers (count, duration, airport)
- price_per_person, total_price
- baggage_policy
- eurobonus_eligible (boolean)
- booking_url
