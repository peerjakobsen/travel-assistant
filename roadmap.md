# Family Travel Assistant — Claude Cowork Plugin
## Project Roadmap & Implementation Guide for Claude Code

---

## What We're Building

A personal travel concierge plugin for Claude Cowork that acts like a wealthy family's dedicated travel assistant. It knows your family intimately — passports, preferences, loyalty programs, past trips, seat preferences, school holidays — and uses Claude's browser capabilities to research, compare, and prepare everything for booking. You validate and pay. The plugin handles everything else.

**Core philosophy:** The plugin should feel like a trusted assistant that never needs to be briefed. You say "10 days somewhere warm in February, ~€8k" and it comes back with curated options, ready to book.

---

## Plugin Architecture Overview

### Official Plugin Structure (follow exactly)

```
family-travel-assistant/
├── .claude-plugin/
│   ├── plugin.json          # Manifest — name, version, description, entrypoints
│   └── marketplace.json     # Optional: marketplace metadata for sharing
├── .mcp.json                # MCP server connections (if any external APIs)
├── skills/                  # Auto-invoked domain knowledge (SKILL.md files)
│   ├── family-profile/
│   │   └── SKILL.md         # Teaches Claude about the family; loaded automatically
│   ├── flight-research/
│   │   └── SKILL.md         # How to search and evaluate flights for a family
│   ├── accommodation-research/
│   │   └── SKILL.md         # Airbnb vs hotel criteria, filtering for family of 6
│   ├── document-vault/
│   │   └── SKILL.md         # Passport rules, ESTA/ETA/visa logic per destination
│   ├── itinerary-builder/
│   │   └── SKILL.md         # Day-by-day planning patterns for family travel
│   └── booking-handoff/
│       └── SKILL.md         # How to prepare booking pages and hand off to user
├── commands/                # Explicit slash commands the user invokes
│   ├── plan-trip.md         # /travel:plan-trip — start a new trip from scratch
│   ├── check-documents.md   # /travel:check-documents — validate passports/visas for a destination
│   ├── research-destination.md  # /travel:research-destination — browse and summarise a destination
│   ├── find-flights.md      # /travel:find-flights — search and compare flights
│   ├── find-accommodation.md # /travel:find-accommodation — search and compare places to stay
│   ├── build-itinerary.md   # /travel:build-itinerary — assemble a day-by-day plan
│   ├── prepare-booking.md   # /travel:prepare-booking — navigate to booking page, pre-fill form
│   ├── trip-brief.md        # /travel:trip-brief — generate pre-departure document
│   └── update-profile.md    # /travel:update-profile — update family profile data
├── agents/                  # Sub-agents for parallel/complex research tasks
│   ├── flight-scout.md      # Browses Google Flights, Momondo, SAS for a family of 6
│   ├── accommodation-scout.md  # Browses Airbnb, Booking.com with family filters
│   ├── destination-researcher.md  # Browses travel blogs, TripAdvisor, local guides
│   └── document-checker.md  # Checks passport expiry, ESTA/ETA validity, visa requirements
├── data/
│   ├── family-profile.yaml  # Persistent family data: members, preferences, loyalty programs
│   ├── document-vault.yaml  # Passport numbers, expiry dates, travel authorizations
│   ├── trip-history.yaml    # Past trips with ratings and notes
│   └── school-calendar.yaml # Danish school holidays by year
└── CLAUDE.md                # Root context: how this plugin works, what files to read first
```

### Key Structural Rules

- **Skills** (`skills/`) are automatically invoked by Claude when contextually relevant. No user action needed. Use them for domain knowledge, heuristics, and decision frameworks.
- **Commands** (`commands/`) are explicit slash commands (`/travel:plan-trip`). Use them for structured workflows with clear inputs.
- **Agents** (`agents/`) are sub-agents Claude can spawn to parallelize research. Great for "search flights AND accommodation simultaneously."
- **`plugin.json`** is the manifest. It must reference all entrypoints.
- **`CLAUDE.md`** at the root gives Claude immediate orientation when the plugin loads.
- **`data/`** contains persistent YAML files that store the family profile. These are read at the start of every session and updated when the user runs `/travel:update-profile`.

---

## Milestone 1: Plugin Scaffold & Manifest

**Goal:** A valid, installable plugin with correct structure before any real functionality.

### Tasks

1. Create `plugin.json` with correct schema:
```json
{
  "name": "family-travel-assistant",
  "version": "1.0.0",
  "description": "Personal travel concierge for the family. Plans trips, researches flights and accommodation, checks travel documents, and prepares bookings — all pre-loaded with family preferences and passport data.",
  "skills": [
    "skills/family-profile",
    "skills/flight-research",
    "skills/accommodation-research",
    "skills/document-vault",
    "skills/itinerary-builder",
    "skills/booking-handoff"
  ],
  "commands": [
    "commands/plan-trip.md",
    "commands/check-documents.md",
    "commands/research-destination.md",
    "commands/find-flights.md",
    "commands/find-accommodation.md",
    "commands/build-itinerary.md",
    "commands/prepare-booking.md",
    "commands/trip-brief.md",
    "commands/update-profile.md"
  ],
  "agents": [
    "agents/flight-scout.md",
    "agents/accommodation-scout.md",
    "agents/destination-researcher.md",
    "agents/document-checker.md"
  ]
}
```

2. Create `CLAUDE.md` at root — this is what Claude reads first. It should explain:
   - What this plugin is for
   - Where to find the family profile and how to use it
   - The booking handoff rule (never complete payment — always stop at payment page)
   - Tone: concise, decisive, like a trusted assistant not a chatbot

3. Create all directory placeholders with empty SKILL.md stubs.

4. Test install locally:
```bash
claude plugin install --plugin-dir ./family-travel-assistant
```

**Acceptance criteria:** Plugin appears in `/plugin list`, slash commands are visible with `/`.

---

## Milestone 2: Family Profile System

**Goal:** The plugin's memory. Everything that makes it feel like it knows your family.

### `data/family-profile.yaml` schema

```yaml
family:
  primary_contact:
    name: "Peer [Lastname]"
    email: ""
    phone: ""

  members:
    - id: peer
      name: "Peer"
      role: parent
      date_of_birth: ""
      nationality: Danish
      seat_preference: aisle
      dietary: none
      notes: "Prefers direct flights. Window seat for kids."

    - id: spouse
      name: "[Wife's name]"
      role: parent
      date_of_birth: ""
      nationality: Danish
      seat_preference: window
      dietary: none

    - id: child1
      name: ""
      role: child
      date_of_birth: ""
      nationality: Danish
      age_at_travel: null   # calculated dynamically

    # ... repeat for all 4 children

  travel_preferences:
    home_airport: CPH
    preferred_airlines:
      - SAS
      - Norwegian
      - Lufthansa
    max_layovers: 1
    max_layover_duration_hours: 3
    accommodation_style: airbnb_preferred   # airbnb_preferred | hotel_preferred | either
    accommodation_requirements:
      - private_garden_or_terrace
      - full_kitchen
      - minimum_bedrooms: 4
    car_rental:
      required: true
      minimum_seats: 7   # family of 6 + luggage
      preferred_class: minivan_or_large_suv
    budget_per_trip_eur:
      low: 5000
      typical: 8000
      splurge: 15000
    trip_types_enjoyed:
      - beach
      - city_with_culture
      - nature
    trip_types_avoided:
      - ski  # example
    past_destinations_loved: []
    past_destinations_avoid: []

  loyalty_programs:
    - program: SAS EuroBonus
      member_id: ""
      member: peer
    # add more as needed

  school_calendar_country: Denmark
  typical_trip_durations_days: [7, 10, 14]
```

### `data/document-vault.yaml` schema

```yaml
documents:
  - member_id: peer
    document_type: passport
    document_number: ""         # filled by user via /travel:update-profile
    nationality: Danish
    issue_date: ""
    expiry_date: ""
    issuing_country: Denmark
    notes: ""

  travel_authorizations:
    - member_id: peer
      type: ESTA                # ESTA | ETA_UK | eTA_Canada | visa
      destination: USA
      obtained: false
      application_date: null
      expiry_date: null
      linked_passport: peer
      notes: "Valid 2 years from approval. Tied to specific passport."
```

### `skills/family-profile/SKILL.md` instructions

The skill should instruct Claude to:
- Always load `data/family-profile.yaml` and `data/document-vault.yaml` at session start
- Never ask the user to repeat preference information that's already in the profile
- Dynamically calculate children's ages at time of travel for pricing and activity filtering
- Flag profile gaps (missing passport expiry, etc.) proactively

### `commands/update-profile.md`

Slash command that walks the user through updating any section of the profile. Should:
- Read current value before proposing changes
- Write changes back to the YAML file
- Confirm changes made in a clean summary

**Acceptance criteria:** Running `/travel:update-profile` lets you update family members, preferences, and document vault. Profile persists between sessions.

---

## Milestone 3: Document Intelligence

**Goal:** The plugin proactively knows if your family can legally travel to a destination, what they need, and flags issues before they become problems.

### `skills/document-vault/SKILL.md` rules

Encode the following logic as skill instructions:

**Passport validity rules by destination:**
- Schengen Area (most of Europe): 3 months validity beyond travel dates
- USA, Canada, Australia: 6 months validity beyond travel dates
- UK: valid for duration of stay only (as of 2024)
- Always check the youngest family member — children's passports expire in 5 years (Denmark), adults 10 years

**Travel authorizations by destination:**
- USA: ESTA required for Danish citizens. Cost ~$21. Valid 2 years. Apply at esta.cbp.dhs.gov. Each member needs individual ESTA. ESTA is linked to specific passport — renewing passport invalidates ESTA.
- UK: ETA required for Danish citizens since 2024. Cost £10. Apply via UK ETA app.
- Canada: eTA required. Cost CAD $7. Valid 5 years.
- Australia: ETA (subclass 601) required. Apply via Australian ETA app.
- No visa/authorization needed: Schengen zone countries, most of Europe for Danish citizens.

**Proactive flags the skill should generate:**
- "Child X's passport expires in 8 months — insufficient for USA travel (requires 6 months). Renewal needed before booking."
- "No ESTA on record for [member]. This is required for USA travel. Apply at esta.cbp.dhs.gov — takes ~10 minutes, costs $21, usually approved instantly."
- "ESTA for Peer expires in 3 months. Renew before the trip."
- "Child X turns 18 before travel — their child ESTA is invalid. Adult ESTA required."

### `agents/document-checker.md`

Sub-agent that, given a destination and travel dates:
1. Checks all family member passports against destination validity requirements
2. Checks existing travel authorizations against expiry dates
3. Returns a structured report: READY | NEEDS_RENEWAL | NEEDS_APPLICATION per member

### `commands/check-documents.md`

Slash command: `/travel:check-documents destination=[USA] travel_date=[2026-07-15]`

Output format:
```
Document Check — USA, July 15 2026
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Peer        ✅ Passport valid   ⚠️  ESTA not on record
[Wife]      ✅ Passport valid   ⚠️  ESTA not on record  
Child 1     ✅ Passport valid   ✅ ESTA valid to 2027-03
Child 2     ⚠️  Passport expires Aug 2026 — renew before travel
Child 3     ✅ Passport valid   ⚠️  ESTA not on record
Child 4     ✅ Passport valid   ⚠️  ESTA not on record

Action items:
1. Renew Child 2's passport — allow 4-6 weeks
2. Apply for ESTA for Peer, [Wife], Child 3, Child 4
   → esta.cbp.dhs.gov | ~$21 each | ~10 min | usually instant
```

**Acceptance criteria:** Running `/travel:check-documents` for any destination produces an accurate per-member report with specific action items.

---

## Milestone 4: Destination Research Agent

**Goal:** Given a rough brief ("warm, family-friendly, July, €8k"), the plugin browses the web and returns 2-3 curated destination recommendations.

### `agents/destination-researcher.md`

This agent uses Claude's browser capability. It should:

1. Search Google for "best family destinations [month] [region/vibe]"
2. Browse 2-3 travel sites (e.g., Lonely Planet, Condé Nast Traveler family travel sections, TripAdvisor family guides)
3. Filter results by: flight time from CPH, family-friendliness, weather in target month, budget fit
4. For each shortlisted destination, browse to find: typical flight cost CPH→destination for 6, Airbnb availability and typical price for 4-bedroom, top family activities, car rental availability

**Output format per destination:**
```
## Option 1: Algarve, Portugal (10 days, July)

Why it fits: 3h direct from CPH on SAS/TAP, warm and dry in July, 
excellent beaches suitable for kids, good Airbnb availability, 
well under budget.

Rough budget:
  Flights (6 pax return):     ~€2,400
  Airbnb (4BR, 10 nights):    ~€2,800
  Car rental (7-seat, 10d):   ~€400
  Food + activities:          ~€1,500
  Total estimate:             ~€7,100

Top activities for kids: [list]
Best areas to stay: Lagos, Carvoeiro, Vilamoura
```

### `commands/research-destination.md`

Slash command: `/travel:research-destination brief="warm, beaches, July, 10 days, ~€8k"`

Should invoke the destination-researcher agent and present 2-3 options cleanly.

**Acceptance criteria:** Running the command produces at least 2 well-structured destination options with real browsed data, cost estimates, and activity suggestions.

---

## Milestone 5: Flight & Accommodation Research

**Goal:** Once a destination is chosen, find the best flights and accommodation for a family of 6.

### `agents/flight-scout.md`

Browser agent that searches for flights. Instructions:
- Search Google Flights first for an overview: CPH → [destination], dates, 6 passengers
- Then check SAS (sas.dk) specifically for EuroBonus earning potential
- Also check momondo.dk for price comparison
- Filter by: max 1 layover, layover under 3 hours, preferred airlines where possible
- Look for seat availability together (6 adjacent or in 2 groups of 3)
- For each viable option, capture: airline, flight numbers, departure/arrival times, price per person, total price, baggage policy, booking URL

**Never complete a booking.** Navigate to the booking confirmation page, capture the final price breakdown, then stop and report back.

### `agents/accommodation-scout.md`

Browser agent that searches Airbnb and Booking.com:
- Airbnb: search with filters: 4+ bedrooms, dates, 6 guests, target area
- Sort by: rating first, then price
- Filter out: apartments (want houses/villas), shared spaces
- Booking.com: search family-friendly villas/houses as comparison
- For each candidate: capture name, price per night, total price, key amenities, distance to beach/centre, rating, booking URL

### `commands/find-flights.md`

Slash command: `/travel:find-flights destination=[Faro] outbound=[2026-07-10] return=[2026-07-20]`

Invokes flight-scout agent. Returns top 3 options in a comparison table.

### `commands/find-accommodation.md`

Slash command: `/travel:find-accommodation destination=[Algarve] checkin=[2026-07-10] checkout=[2026-07-20] area=[Lagos]`

Invokes accommodation-scout agent. Returns top 3 options with photos (if possible), amenities, and direct booking links.

**Acceptance criteria:** Both commands return real, current data from live browsing, formatted as clean comparison tables with booking links.

---

## Milestone 6: Booking Handoff

**Goal:** Navigate to the booking page, pre-fill all form fields from the family profile and document vault, stop at payment, present a summary for user validation.

### `skills/booking-handoff/SKILL.md`

Critical rules for this skill:
- **NEVER enter payment information.** Stop at the payment page.
- **NEVER submit a form that initiates a financial transaction.** The user must click the final confirm/pay button themselves.
- Fill in passenger details from `data/document-vault.yaml`: full names (as on passport), dates of birth, passport numbers, nationalities, expiry dates
- Fill in loyalty program numbers where available
- Select seat preferences from profile (aisle for Peer, window for others)
- Add any baggage options required
- Pre-select travel insurance if applicable

### `commands/prepare-booking.md`

Slash command: `/travel:prepare-booking type=[flight] url=[booking_url]`

Workflow:
1. Open the booking URL
2. Read current form state
3. Fill all passenger fields from document vault
4. Select extras (seats, baggage) per preferences
5. Navigate to payment page
6. **STOP** — do not enter payment details
7. Present summary:
```
Booking Ready for Your Review
━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Flight: SAS SK571 CPH→FAO, 10 July 2026
Return: SAS SK572 FAO→CPH, 20 July 2026

Passengers: [list of 6 with seat assignments]
Extras: 6x checked bags (23kg)
EuroBonus: Applied to Peer's booking

Total: €2,847.60

⏸  I've stopped at the payment page. 
   Review the booking, then enter your card details to complete.
   URL: [current page URL]
```

**Acceptance criteria:** Running `/travel:prepare-booking` navigates to a real booking page, fills passenger details automatically, and stops cleanly at payment with a clear summary.

---

## Milestone 7: Itinerary Builder & Trip Brief

**Goal:** Turn a confirmed trip into a useful day-by-day plan and a pre-departure document the whole family can use.

### `skills/itinerary-builder/SKILL.md`

Rules for building family itineraries:
- Don't over-schedule — maximum 2 structured activities per day with kids
- Always include a buffer/beach/free day every 3-4 days
- Check opening days/hours for attractions (many close Monday or Tuesday)
- Note which activities need pre-booking (popular restaurants, theme parks, skip-the-line tickets)
- Group activities geographically to minimise driving
- Include 1-2 rainy day backup options
- Flag age-appropriate activities by child age

### `commands/build-itinerary.md`

Slash command: `/travel:build-itinerary destination=[Algarve] dates=[July 10-20] interests=[beaches, water sports, local food, one city day]`

Uses destination-researcher data + browsing to build a day-by-day plan with:
- Suggested activity per day + alternatives
- Driving distances between bases
- Restaurant suggestions (1 per day, family-friendly, pre-booking note)
- Weather notes for the period

### `commands/trip-brief.md`

Slash command: `/travel:trip-brief` (uses current trip context)

Generates a clean markdown document with:
- Trip overview (dates, destination, family members)
- All confirmation numbers and booking references
- Accommodation address + check-in instructions
- Day-by-day itinerary
- Car rental pickup details
- Emergency contacts (local hospital, embassy, travel insurance)
- Packing list tailored to destination + activities + children's ages
- What to do at home before leaving (arrange pet care, mail hold, etc.)

Outputs as a PDF or markdown file the family can reference during the trip.

**Acceptance criteria:** `/travel:trip-brief` produces a complete, well-formatted trip document ready to print or share.

---

## Milestone 8: Scheduled Tasks & Proactive Alerts

**Goal:** The plugin works for you even when you're not actively planning a trip.

### Scheduled tasks to implement

Using Cowork's `/schedule` feature, set up:

**Monthly passport expiry check:**
- Task: "Check all family passports — flag any expiring within 12 months"
- Schedule: First Monday of each month
- Output: Slack or email notification (if connectors configured) or Cowork alert

**Pre-trip document reminder (30 days before travel):**
- If a trip is in `trip-history.yaml` with status `confirmed`, run document-checker 30 days before
- Flag any missing ESTAs, ETAs, or near-expiry passports

**Danish school holiday calendar update:**
- Task: Update `data/school-calendar.yaml` with the next year's school holidays
- Schedule: August 1st each year
- Browses uvm.dk (Danish Ministry of Education) for the official calendar

### Implementation notes

Scheduled tasks in Cowork are defined with `/schedule` in a Cowork task. Store the task definitions in `data/scheduled-tasks.yaml` as reference so Claude Code can scaffold them.

---

## Milestone 9: Polish & Real-World Testing

**Goal:** The plugin works reliably on real booking sites, handles edge cases gracefully, and feels like a trusted assistant rather than a prototype.

### Testing checklist

- [ ] Full trip planning flow: brief → destination options → flights → accommodation → document check → booking handoff → itinerary → trip brief
- [ ] ESTA/ETA logic correct for all family members
- [ ] Profile update roundtrip (update YAML, reload in next session)
- [ ] Booking handoff on: Google Flights, SAS.dk, Airbnb, Booking.com, Rentalcars.com
- [ ] Document checker correctly flags: expired passport, missing ESTA, child ESTA expiry at 18
- [ ] Itinerary respects "no more than 2 structured activities per day"
- [ ] Trip brief generates correctly with all confirmation data

### Known limitations to document

- Payment always requires manual user action — this is intentional and by design
- Booking sites may require login — the plugin cannot handle passwords; user must log in first
- CAPTCHA and bot detection on some sites will require manual user action
- Flight prices and availability change in real time — always re-verify before booking
- Airbnb availability windows can be short — act quickly on good finds

---

## File Creation Order for Claude Code

When implementing, build in this sequence to avoid dependency issues:

1. Plugin scaffold + `plugin.json` + `CLAUDE.md`
2. `data/` YAML schemas (empty but correctly structured)
3. `skills/family-profile/SKILL.md` (everything depends on this)
4. `skills/document-vault/SKILL.md`
5. `commands/update-profile.md` (so you can populate real data immediately)
6. `commands/check-documents.md` + `agents/document-checker.md`
7. `agents/destination-researcher.md` + `commands/research-destination.md`
8. `agents/flight-scout.md` + `commands/find-flights.md`
9. `agents/accommodation-scout.md` + `commands/find-accommodation.md`
10. `skills/booking-handoff/SKILL.md` + `commands/prepare-booking.md`
11. `skills/itinerary-builder/SKILL.md` + `commands/build-itinerary.md`
12. `commands/trip-brief.md`
13. Scheduled tasks
14. End-to-end testing and polish

---

## Key Technical Conventions

### SKILL.md frontmatter format
```markdown
---
name: flight-research
description: Best practices for researching and comparing flights for a family of 6 from Copenhagen. Covers Google Flights, SAS, Momondo, layover rules, seat selection, and EuroBonus optimization.
---

[skill instructions follow here as plain prose or structured markdown]
```

### Command file format
```markdown
# Command: find-flights
Slash command: /travel:find-flights

## Purpose
Search and compare flight options for the family.

## Inputs
- destination (required): IATA code or city name
- outbound (required): YYYY-MM-DD
- return (required): YYYY-MM-DD

## Instructions
[step-by-step instructions Claude follows when this command runs]

## Output format
[exact output template]
```

### Agent file format
```markdown
# Agent: flight-scout
Type: sub-agent (spawned by commands)

## Purpose
Browse live flight search engines and return structured flight options.

## Tools available
- Browser (Claude Chrome plugin)
- File read: data/family-profile.yaml, data/document-vault.yaml

## Behaviour rules
[explicit instructions, including "STOP before payment"]

## Output schema
[structured output the parent command expects back]
```

### Data file conventions
- All personal data in `data/` as YAML
- Never hardcode personal data in skill or command files
- Use `null` for fields not yet filled in (don't use empty strings)
- Dates always in ISO format: YYYY-MM-DD
- Currency amounts always include unit: `8000 EUR`

---

## Reference Links

- Official plugin docs: https://code.claude.com/docs/en/plugins
- Official plugin reference: https://code.claude.com/docs/en/plugins-reference
- Anthropic open-source plugins (study these for patterns): https://github.com/anthropics/knowledge-work-plugins
- Cowork scheduled tasks: https://support.claude.com/en/articles/13345190-get-started-with-cowork
- ESTA application (for document vault): https://esta.cbp.dhs.gov
- UK ETA application: https://www.gov.uk/guidance/apply-for-an-electronic-travel-authorisation-eta
- Danish school holidays: https://www.uvm.dk/folkeskolen/skoleaaret/ferier-og-fridage
