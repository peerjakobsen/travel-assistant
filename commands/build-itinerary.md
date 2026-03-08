# Command: build-itinerary
Slash command: /travel:build-itinerary

## Purpose
Assemble a day-by-day itinerary for a confirmed trip.

## Inputs
- destination (required): City or region name
- dates (required): Date range (e.g., "July 10-20")
- interests (optional): Comma-separated list (e.g., "beaches, water sports, local food, one city day")

## Instructions
1. Load `data/family-profile.yaml` for: children's DOBs (to calculate ages at travel), trip types enjoyed, car rental status
2. Load `data/trip-history.yaml` for: past trips to this destination (reference what they loved, restaurant/activity notes)
3. Calculate each child's age at the first day of the `dates` range
4. Research destination activities via Chrome connector:
   - Search: "[destination] best family activities [month]", "[destination] things to do with kids"
   - Browse 2-3 sources (TripAdvisor family section, Lonely Planet, local tourism board)
   - Build list of 15-20 candidate activities: name, type, area/zone, duration, age suitability, cost, opening hours/days, pre-booking needed
5. Research restaurants via Chrome connector:
   - Search: "[destination] best family restaurants", "[destination] restaurants with kids [area]"
   - Build list of 8-12 candidates: name, area, cuisine, price level, family notes, reservation needed
6. Check weather: search "[destination] weather [month] [year]" — note average highs/lows, rain probability, patterns
7. Build the day-by-day plan applying itinerary-builder skill rules:
   - Day 1 (arrival): settle in, local orientation, easy nearby dinner
   - Last day (departure): pack, check out, drive to airport
   - For each day: morning activity, afternoon activity (or free time), restaurant suggestion, driving distances
   - Assign rainy-day backups to outdoor activity days
   - Group activities by geographic zone per day
   - Flag pre-booking items with "BOOK AHEAD" tag
8. Present the itinerary and prompt: "Want a complete trip brief? Run `/travel:trip-brief`."

## Output format

```
## Day-by-Day Itinerary: [Destination] ([date range])

**Children's ages at travel:** [Child1] ([age]), [Child2] ([age]), ...
**Weather:** [summary] | **Car:** [vehicle type]

---

### Day 1 — [Date] [Day of week] | ARRIVAL DAY
**Morning:** Arrive [airport], pick up rental car
**Afternoon:** Settle in, supermarket run
**Dinner:** [Restaurant] — [cuisine], [area] ([reservation note])
**Drive:** [airport] → accommodation: ~[X] min

---

### Day [N] — [Date] [Day of week] | [ZONE/AREA]
**Morning:** [Activity] — [brief description]
  _Duration: ~[X]h | Ages: [suitable for] | Cost: ~€[X]/family_
  _[Pre-booking note if applicable]_
**Afternoon:** [Activity or "Free time / pool / beach"]
**Alternative:** [Different activity in same area]
**Rainy day swap:** [Indoor backup]
**Dinner:** [Restaurant] — [cuisine], [area] ([reservation note])
**Driving:** [X] min from accommodation

---

### Day [N] — [Date] [Day of week] | BUFFER DAY
**No planned activities** — beach/pool day, explore neighbourhood
**If restless:** [Low-key optional activity nearby]
**Dinner:** Self-catering / [casual local spot]

---

### Day [N] — [Date] [Day of week] | DEPARTURE DAY
**Morning:** Pack up, check out by [time]
**Drive:** Accommodation → [airport]: ~[X] min

---

## Pre-Booking Checklist
- [ ] [Activity/restaurant] — book by [date], via [url]

## Rainy Day Options
1. [Indoor activity] — [location], [hours], [cost]
2. [Indoor activity] — [location], [hours], [cost]
```
