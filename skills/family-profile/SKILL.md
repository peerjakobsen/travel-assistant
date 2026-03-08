---
name: family-profile
description: Teaches Claude about the family — members, preferences, loyalty programs, budget ranges, and travel style. Loaded automatically to personalise all travel research and planning.
---

# Family Profile Skill

## Session Start

At the beginning of every session, load these two files:
- `data/family-profile.yaml` — family members, preferences, loyalty programs, budget
- `data/document-vault.yaml` — passports, travel authorizations

Parse both files and keep their contents available for all subsequent interactions. Do not ask the user to provide information that already exists in these files.

## Using the Profile

### Member Information
- Reference family members by their `id` or `name` from the profile
- There are 6 members: 2 parents + 4 children
- Use `nationality`, `seat_preference`, `dietary`, and `notes` when researching or booking

### Age Calculation
- Children's `age_at_travel` fields are calculated dynamically — never store a static age
- To calculate: subtract `date_of_birth` from the travel departure date
- Use calculated age for: airline pricing (infant/child/adult thresholds), activity suitability, hotel policies, car seat requirements

### Travel Defaults
- Always use `home_airport: CPH` as the departure point unless told otherwise
- Default to `preferred_airlines` when searching flights
- Use `budget_per_trip_eur.typical` as the default budget unless the user specifies otherwise
- Apply `accommodation_requirements` (4 bedrooms, kitchen, garden/terrace) and `accommodation_style` when searching stays
- Car rental: always include a 7-seat vehicle unless the user says otherwise
- Respect `max_layovers` and `max_layover_duration_hours` when evaluating flight options

### Loyalty Programs
- Reference loyalty program details when comparing flight options (e.g., SAS EuroBonus earning potential)
- Include member IDs when preparing bookings

### School Calendar
- Load `data/school-calendar.yaml` when planning trip dates
- Only suggest travel during school holidays or weekends unless the user explicitly says they're flexible on school days
- When the user says "February break" or "winter holiday", look up the exact dates from the calendar

## Proactive Gap Flagging

At session start and before any trip planning, check for and flag:
- Members with `null` for `date_of_birth` — needed for age-based pricing
- Members with `null` for `name` — needed for bookings
- Passports with `null` for `expiry_date` — needed for document checks
- Passports with `null` for `document_number` — needed for booking
- Missing or expired travel authorizations for common destinations (USA, UK, Canada)
- Empty `loyalty_programs.member_id` — useful but not blocking

Present gaps as a concise checklist, not a wall of text. Example:
> **Profile gaps to fill via `/travel:update-profile`:**
> - Spouse name missing
> - Children 1-4: names and dates of birth missing
> - All passports: numbers and expiry dates missing
