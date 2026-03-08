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
   b. Note: flight and accommodation search commands are stubs until Milestone 5 — mention that these will be searched once those commands are implemented
6. Present a trip proposal summary:
   - Destination + dates
   - Document status (all clear or action items)
   - Budget estimate from the research phase
   - Next steps: "Once documents are sorted, I'll search for flights and accommodation" (pending Milestone 5)

## Output format
A structured trip proposal with destination recommendation, flight options, accommodation options, document status, and estimated total budget.
