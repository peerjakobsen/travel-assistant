---
name: booking-handoff
description: How to prepare booking pages and hand off to the user. Critical safety rules — NEVER complete payment, NEVER submit financial transactions.
---

# Booking Handoff Skill

## Safety Boundary (non-negotiable)

- **NEVER enter payment information** — credit card numbers, CVV, billing address
- **NEVER submit a form that initiates a financial transaction**
- **NEVER click "Pay", "Complete purchase", "Confirm and pay", or any equivalent button**
- The user always clicks the final pay button themselves
- This is enforced at two levels: plugin instructions AND Chrome extension platform safety

## Form-Filling Strategy by Booking Type

### Flights

- Load passenger details from `data/document-vault.yaml`: full legal names (as on passport), dates of birth, passport numbers, nationalities, expiry dates
- Map each family member to a passenger slot (2 adults + children with correct ages at travel date)
- Apply Peer's SAS EuroBonus number from `data/family-profile.yaml` loyalty_programs
- Select seat preferences: aisle for Peer, window for spouse, window for children where possible
- Request 6 checked bags (23kg each) if not included in fare
- If SAS: select SAS Go or SAS Plus as previously recommended

### Accommodation

- Fill guest name (primary contact: Peer) and email/phone from `data/family-profile.yaml`
- Enter number of guests: 6 (2 adults + 4 children)
- Note any special requests: early check-in if arriving by morning flight, parking for 7-seat vehicle

### Car Rental

- Fill driver name (Peer) and details from document vault
- Select 7-seat vehicle (minivan or large SUV)
- Add additional driver (spouse) if option available
- Note: pickup at destination airport, return same location

## Handling Site-Specific Patterns

- Login state is shared from user's Chrome session — loyalty pricing and saved details apply automatically
- If a CAPTCHA appears, pause and notify the user to solve it manually in Chrome
- If the site requires login, notify the user to log in first, then retry
- If form fields don't match expected layout, fill what's possible and list unfilled fields in the summary

## Summary Presentation

- Always present a structured summary before handing off
- Include: what was booked, who is on the booking, what extras were selected, total price, and the current page URL
- Clearly state that the user must enter payment details and click pay

## Chrome Session Notes

- The Chrome connector shares the user's existing Chrome session (cookies, login state)
- If logged into SAS.dk, Airbnb, Booking.com — loyalty pricing and saved methods apply automatically
- CAPTCHAs pause execution and require manual user intervention
- Payment form submission is blocked at the platform level by Chrome extension safety
