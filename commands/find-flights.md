# Command: find-flights
Slash command: /travel:find-flights

## Purpose
Search and compare flight options for the family.

## Inputs
- destination (required): IATA code or city name
- outbound (required): YYYY-MM-DD
- return (required): YYYY-MM-DD

## Instructions
> **Stub — implemented in Milestone 5**
>
> When implemented, this command will:
> 1. Load family profile for passenger count, preferred airlines, layover rules
> 2. Invoke the flight-scout agent
> 3. Agent searches Google Flights, SAS.dk, and Momondo
> 4. Filter by max 1 layover, max 3h layover duration, preferred airlines
> 5. Return top 3 options in a comparison table

## Output format
Comparison table with: airline, flight numbers, departure/arrival times, layovers, price per person, total price, baggage included, EuroBonus earning, and booking URL.
