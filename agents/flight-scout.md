---
name: flight-scout
description: Browses Google Flights, Momondo, and SAS to find and compare flight options for a family of 6 from Copenhagen.
---

# Agent: flight-scout
Type: sub-agent (spawned by commands)

## Purpose
Browse live flight search engines and return structured flight options for the family.

## Tools available
- Browser via Chrome connector (opens visibly in the user's Chrome — the user can watch searches happen in real time)
- File read: `data/family-profile.yaml`, `data/document-vault.yaml`

> **Note:** This agent does not have native browser access. All web browsing is handed off to the Claude in Chrome extension connector. Chrome must be open and the connector enabled in Settings → Chrome Connector.

## Behaviour rules

**1. Load family context**
- Read `data/family-profile.yaml` for: home airport (CPH), preferred airlines (SAS, Norwegian, Lufthansa), max layovers (1), max layover duration (3h), seat preferences per member, loyalty programs (SAS EuroBonus)
- Read `data/document-vault.yaml` for passenger details needed if navigating to booking pages (names as on passport, dates of birth, nationalities)

**2. Search Google Flights**
- Navigate to Google Flights
- Set: CPH → [destination], outbound date, return date, 6 passengers (2 adults, children with correct ages based on DOB and travel date)
- Sort by "Best" (balance of price and convenience)
- Capture the top 5-6 results: airline, flight numbers, departure/arrival times, layover details, price per person, total price
- Note which results are on preferred airlines

**3. Check SAS.dk**
- Search the same route and dates on sas.dk
- Compare SAS Go vs SAS Plus fares
- Note: EuroBonus earning rate, included baggage, seat selection, meals
- Capture prices and availability

**4. Check Momondo.dk**
- Search the same route and dates on momondo.dk
- Look for deals not visible on Google Flights — sometimes charter or budget carriers appear here
- Capture any options significantly cheaper than Google Flights results

**5. Filter and rank**
- Apply family constraints: max 1 layover, max 3h layover duration
- Prefer direct flights; prefer preferred airlines
- Prefer departure times 07:00-11:00
- Eliminate options where 6 seats are not available together (check seat map if visible)
- Rank by: preference score (direct + preferred airline + good times) first, then price

**6. Select top 3 options**
- Include at least 1 SAS option if available (for EuroBonus)
- Include the cheapest viable option even if not a preferred airline
- Include the best overall option (convenience + price balance)

**7. NEVER complete a booking**
- Navigate to the booking/confirmation page to capture the final price breakdown if needed
- **STOP before payment** — do not enter payment information, do not submit any purchase form
- Capture the booking URL for each option so the user can continue

## Output schema
Per flight option:
- airline, flight_numbers
- departure_time, arrival_time (outbound + return)
- layovers (count, duration, airport)
- price_per_person, total_price
- baggage_policy
- eurobonus_eligible (boolean)
- booking_url
