# Testing Checklist — Family Travel Assistant

Manual validation checklist for testing the plugin in Claude Cowork.

---

## Full Flow Test

Run an end-to-end trip planning flow to verify all commands work together.

- [ ] `/travel:plan-trip` — provide a brief like "10 days somewhere warm in July, ~€8k"
- [ ] Verify family profile and document vault are loaded automatically (no re-asking preferences)
- [ ] `/travel:research-destination` — confirm 2-3 options returned with budget estimates
- [ ] `/travel:find-flights` — confirm comparison table with real flight data
- [ ] `/travel:find-accommodation` — confirm comparison table with real listings
- [ ] `/travel:check-documents` — confirm per-member document status report
- [ ] `/travel:prepare-booking` — confirm form pre-fill and payment stop
- [ ] `/travel:build-itinerary` — confirm day-by-day plan respects family rules
- [ ] `/travel:trip-brief` — confirm complete pre-departure document generated

---

## Document Intelligence

Test the document checker logic for correctness.

- [ ] ESTA required for USA — all 6 members flagged if missing
- [ ] ETA required for UK — all members flagged if missing
- [ ] eTA required for Canada — all members flagged if missing
- [ ] ETA required for Australia — all members flagged if missing
- [ ] No authorization needed for Schengen destinations
- [ ] Expired passport flagged with renewal action item
- [ ] Passport expiring within 6 months flagged for USA/Canada/Australia
- [ ] Passport expiring within 3 months flagged for Schengen
- [ ] Child turning 18 before travel date — child ESTA flagged as invalid, adult ESTA required
- [ ] Missing passport data (`null` fields) — flagged as incomplete, not treated as valid
- [ ] ESTA linked to specific passport — passport renewal invalidates ESTA warning

---

## Profile Management

Test the update workflow and data persistence.

- [ ] `/travel:update-profile` — update a family member's name
- [ ] `/travel:update-profile` — update a travel preference (e.g., budget)
- [ ] `/travel:update-profile` — add a loyalty program
- [ ] Verify changes written to YAML file correctly
- [ ] Start a new session — confirm updated values are loaded
- [ ] Profile gaps flagged proactively (missing passport expiry, null fields)

---

## Booking Handoff

Test on each target booking site.

### Google Flights
- [ ] Navigate to booking page from flight search
- [ ] Passenger details pre-filled from document vault
- [ ] Stops at payment page — does NOT enter payment info

### SAS.dk
- [ ] Navigate to booking page
- [ ] Passenger details pre-filled
- [ ] EuroBonus number applied to Peer's booking
- [ ] Seat preferences applied (aisle for Peer, window for others)
- [ ] Stops at payment page

### Airbnb
- [ ] Navigate to booking page from accommodation search
- [ ] Guest count and dates filled
- [ ] Stops at payment page

### Booking.com
- [ ] Navigate to booking page
- [ ] Guest details pre-filled
- [ ] Stops at payment page

### Rentalcars.com
- [ ] Navigate to booking page
- [ ] Driver details pre-filled
- [ ] Vehicle class matches preference (minivan/large SUV, 7+ seats)
- [ ] Stops at payment page

---

## Edge Cases

- [ ] Null passport data — document checker handles gracefully, flags as incomplete
- [ ] Travel during school term — warning issued about non-holiday travel
- [ ] Avoided trip type requested (e.g., ski) — reminder shown from profile
- [ ] Budget significantly over/under typical range — budget mismatch flagged
- [ ] Destination with no direct flights from CPH — layover rules applied correctly
- [ ] Family member added/removed — profile update handles correctly
- [ ] Trip with status `cancelled` in history — not treated as upcoming

---

## Known Limitations Verification

Confirm these limitations are handled gracefully.

- [ ] CAPTCHA encountered during browsing — execution pauses, user prompted to solve manually
- [ ] Site requires login — Claude reports login needed, user logs in, then retries
- [ ] Payment boundary — confirm Chrome extension blocks payment submission independently
- [ ] Price changed between research and booking — user warned to re-verify
- [ ] Chrome connector disconnected — clear error message, instructions to re-enable

---

## Data File Integrity

- [ ] `data/family-profile.yaml` — valid YAML, all members present, seat preferences set
- [ ] `data/document-vault.yaml` — valid YAML, all members have passport entries
- [ ] `data/trip-history.yaml` — valid YAML, schema matches header comment
- [ ] `data/school-calendar.yaml` — valid YAML, both school years have Whitsun entries
- [ ] `data/scheduled-tasks.yaml` — valid YAML, all 3 scheduled tasks defined
- [ ] `plugin.json` — valid JSON, all skills/commands/agents referenced
