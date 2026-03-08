# Command: plan-trip
Slash command: /travel:plan-trip

## Purpose
Start planning a new trip from scratch. Takes a rough brief and orchestrates the full research workflow — destination options, flights, accommodation, document check, and itinerary.

## Inputs
- brief (required): Natural language description of the trip (e.g., "10 days somewhere warm in July, ~€8k")
- dates (optional): Specific travel dates if already known (YYYY-MM-DD to YYYY-MM-DD)
- destination (optional): Specific destination if already chosen

## Instructions
> **Stub — implemented in Milestone 4+**
>
> When implemented, this command will:
> 1. Load the family profile for preferences and constraints
> 2. If no destination given, invoke destination-researcher agent with the brief
> 3. Present 2-3 destination options for user selection
> 4. Run document check for chosen destination
> 5. Search flights and accommodation in parallel
> 6. Present a complete trip proposal with costs

## Output format
A structured trip proposal with destination recommendation, flight options, accommodation options, document status, and estimated total budget.
