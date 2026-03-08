# Command: research-destination
Slash command: /travel:research-destination

## Purpose
Browse the web and return 2-3 curated destination recommendations based on a rough brief.

## Inputs
- brief (required): Natural language description (e.g., "warm, beaches, July, 10 days, ~€8k")

## Instructions

1. Load `data/family-profile.yaml` to have family constraints available (budget, preferences, home airport, trip types)
2. Load `data/school-calendar.yaml` — if the brief mentions dates, check if they fall during school holidays; if during school term, flag this proactively
3. Load `data/trip-history.yaml` — pass past destinations to the agent to avoid re-suggesting
4. If the brief conflicts with family preferences (e.g., mentions ski but ski is in `trip_types_avoided`), note this upfront before researching
5. Invoke the destination-researcher agent with the brief and family context
6. Present 2-3 destination options using this format per option:

```
## Option [N]: [Destination], [Country] ([duration], [month])

**Why it fits:** [narrative referencing family preferences]

**Rough budget:**
  Flights (6 pax return):     ~€[amount]
  Airbnb (4BR, [N] nights):   ~€[amount]
  Car rental (7-seat, [N]d):  ~€[amount]
  Food + activities:          ~€[amount]
  Total estimate:             ~€[amount]  [vs budget: under/on/over]

**Top activities for kids:** [list]
**Best areas to stay:** [list]
**Weather:** [summary for target month]
**Flight info:** [direct/1-stop, duration, airlines]
```

7. After presenting all options, add:
   - A brief comparison note ("Option 1 is cheapest, Option 2 has the best beaches, Option 3 is closest")
   - A prompt: "Pick a destination and I'll check documents and search for specific flights and accommodation."

## Output format
Per destination: why it fits, rough budget breakdown (flights, accommodation, car, food + activities, total), top activities for kids, best areas to stay.
