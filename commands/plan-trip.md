# Command: plan-trip
Slash command: /travel:plan-trip

## Purpose
Start planning a new trip from scratch. Takes a rough brief and orchestrates the full research workflow — destination options, flights, accommodation, document check, and itinerary.

## Inputs
- brief (required): Natural language description of the trip (e.g., "10 days somewhere warm in July, ~€8k")
- dates (optional): Specific travel dates if already known (YYYY-MM-DD to YYYY-MM-DD)
- destination (optional): Specific destination if already chosen

## Instructions

1. Load `data/family-profile.yaml` for all family preferences and constraints
2. Load `data/school-calendar.yaml` — if dates are provided, verify they fall during school holidays; if not, warn the user
3. **If no destination specified**: invoke `/travel:research-destination` with the brief to get 2-3 options. Present options and wait for user to choose.
4. **If destination is specified**: skip research, move directly to step 5
5. Once destination is chosen, run these in sequence:
   a. Run `/travel:check-documents` for the chosen destination and travel date — report any passport/visa issues that need resolving
   b. Run `/travel:find-flights` for the destination and dates — present top 3 flight options
   c. Run `/travel:find-accommodation` for the destination, dates, and recommended area from the research phase — present top 3 accommodation options
6. Present a trip proposal summary:
   - Destination + dates
   - Document status (all clear or action items)
   - Top flight option + top accommodation option with combined total
   - Comparison against the stated budget (under/on/over)
   - Next steps: "Pick your preferred flight and accommodation, and I'll prepare the bookings with all details pre-filled."

## Output format
A structured trip proposal with destination recommendation, flight options, accommodation options, document status, and estimated total budget.
