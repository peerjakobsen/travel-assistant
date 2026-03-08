# Command: find-flights
Slash command: /travel:find-flights

## Purpose
Search and compare flight options for the family.

## Inputs
- destination (required): IATA code or city name
- outbound (required): YYYY-MM-DD
- return (required): YYYY-MM-DD

## Instructions

1. Load `data/family-profile.yaml` for: passenger count (6), preferred airlines (SAS, Norwegian, Lufthansa), max layovers (1), max layover duration (3h), seat preferences, loyalty programs (SAS EuroBonus)
2. Calculate children's ages at travel date from their DOB in the profile — this affects pricing (infant/child/adult fares)
3. Invoke the flight-scout agent with: destination, outbound date, return date, passenger details, family constraints
4. Present the top 3 flight options in this format:

```
## Flight Options: CPH → [Destination] ([outbound] → [return])

| | Option 1 | Option 2 | Option 3 |
|---|---|---|---|
| **Airline** | [name] | [name] | [name] |
| **Outbound** | [flight#] [dep]-[arr] | ... | ... |
| **Return** | [flight#] [dep]-[arr] | ... | ... |
| **Stops** | Direct / 1 stop ([airport], [duration]) | ... | ... |
| **Price/person** | €[amount] | €[amount] | €[amount] |
| **Total (6 pax)** | €[amount] | €[amount] | €[amount] |
| **Baggage** | [included/extra] | ... | ... |
| **EuroBonus** | [yes/no + earning note] | ... | ... |
| **Book** | [url] | [url] | [url] |

**Recommendation:** [brief note on which option offers best value and why]
```

5. Flag any issues: if no direct flights exist, if prices are unusually high for the route, if only 1-2 options met criteria
6. Prompt: "Pick a flight option and I'll prepare the booking with all passenger details pre-filled."

## Output format
Comparison table with: airline, flight numbers, departure/arrival times, layovers, price per person, total price, baggage included, EuroBonus earning, and booking URL.
