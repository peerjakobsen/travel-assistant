# Command: trip-brief
Slash command: /travel:trip-brief

## Purpose
Generate a comprehensive pre-departure document the whole family can reference during the trip.

## Inputs
- None — uses the current trip context from the active planning session

## Instructions
1. Gather trip context from conversation history: destination, travel dates, confirmed flights (airline, flight numbers, booking ref), confirmed accommodation (name, address, check-in time, booking ref), car rental details, itinerary (from `/travel:build-itinerary` output)
2. If any details are missing from conversation context, ask the user: "I need a few details to complete the trip brief: [list missing items]"
3. Load `data/family-profile.yaml` for: all member names, contact details, loyalty programs
4. Load `data/document-vault.yaml` for: passport numbers and expiry dates
5. Research emergency contacts via Chrome connector: nearest hospital to accommodation area, Danish embassy/consulate, local emergency number, travel insurance claim number (ask user if not known)
6. Generate packing list tailored to: destination climate, planned activities, children's ages, trip duration. Include documents section (passports, ESTA/ETA, booking confirmations, insurance, driving licences)
7. Generate pre-departure checklist: pet care, hold mail, set light timers, thermostat, notify bank, check roaming, download offline maps, charge devices
8. Assemble and present the trip brief
9. Note: "Copy this to a note app, print it, or share with the family. Designed to be useful offline."

## Output format

```
# Trip Brief: [Destination], [Country]
## [Departure date] — [Return date] ([N] nights)

---

## Family
| Member | Role | Passport | Notes |
|--------|------|----------|-------|
| [Name] | Parent | [number], expires [date] | [seat pref, dietary] |

---

## Flights

### Outbound — [Date]
- **Flight:** [Airline] [Flight#]
- **Route:** CPH → [Destination airport]
- **Departs:** [time] → **Arrives:** [time]
- **Booking ref:** [reference]
- **Seats:** [assignments]
- **Baggage:** [details]

### Return — [Date]
[Same format]

---

## Accommodation
- **Property:** [Name]
- **Address:** [Full address]
- **Check-in:** [Date] from [time] | **Check-out:** [Date] by [time]
- **Booking ref:** [reference]
- **Contact:** [host phone/email]
- **Parking:** [details]

---

## Car Rental
- **Company:** [name]
- **Vehicle:** [type] ([N]-seat)
- **Pickup:** [location], [date] [time]
- **Return:** [location], [date] [time]
- **Booking ref:** [reference]

---

## Day-by-Day Itinerary
[Insert from /travel:build-itinerary output]

---

## Pre-Booking Confirmations
- [ ] [Activity/restaurant] — [date], [time], ref: [number]

---

## Emergency Contacts
- **Local emergency:** [number]
- **Nearest hospital:** [name], [address], [phone]
- **Danish embassy:** [name], [address], [phone]
- **Travel insurance:** [company], claim line: [number], policy: [number]

---

## Packing List

### Documents (DO NOT FORGET)
- [ ] Passports (all 6)
- [ ] [ESTA/ETA confirmations if applicable]
- [ ] Booking confirmations (flights, accommodation, car)
- [ ] Travel insurance card
- [ ] Driving licences (both parents)

### Clothing
[Tailored to destination weather and activities]

### Kids-Specific
[Age-appropriate items]

### Electronics
- [ ] Chargers + power bank
- [ ] Travel adapter ([plug type])
- [ ] Offline maps ([destination])

---

## Before You Leave Home
- [ ] Pet care arranged
- [ ] Mail/packages held
- [ ] Light timers set
- [ ] Thermostat adjusted
- [ ] Bank notified of travel
- [ ] Phone roaming checked
- [ ] Offline maps downloaded
- [ ] Devices charged

---

*Generated [date] by Family Travel Assistant*
```
