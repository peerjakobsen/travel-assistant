# Command: prepare-booking
Slash command: /travel:prepare-booking

## Purpose
Navigate to a booking page, pre-fill all form fields from the family profile and document vault, and stop at payment for user validation.

## Inputs
- type (required): What to book — "flight", "accommodation", or "car"
- url (required): The booking URL to navigate to

## Instructions

1. Load `data/family-profile.yaml` for: family members (names, DOBs, seat preferences), loyalty programs (SAS EuroBonus member ID), contact details (email, phone), car rental preferences
2. Load `data/document-vault.yaml` for: passport details per member (full legal name, document number, nationality, expiry date, issuing country)
3. Open the provided booking URL in the browser via Chrome connector
4. Read the current form state — identify what fields are present and what needs filling
5. Fill form fields based on booking type:
   - **Flight:** Enter all 6 passenger details (names as on passport, DOBs, nationalities, passport numbers), apply EuroBonus number to Peer's booking, select seat preferences (aisle for Peer, window for spouse and children), add 6x checked bags if not included
   - **Accommodation:** Enter primary guest name (Peer), email, phone, number of guests (6), any special requests (parking, early check-in)
   - **Car:** Enter driver details (Peer), select 7-seat vehicle, add second driver (spouse) if available, confirm pickup/return location
6. Review all filled fields for accuracy — verify names match passport exactly, dates are correct, extras are applied
7. Navigate forward through the booking flow toward the payment page
8. **STOP at the payment page** — do not enter any payment information, do not click any pay/confirm button
9. Capture: the current page URL, the total price shown, a breakdown of what's included
10. Present the booking summary:

```
Booking Ready for Your Review
━━━━━━━━━━━━━━━━━━━━━━━━━━━━

**[Flight/Accommodation/Car] Booking**
[Specific booking details — route, dates, property name, vehicle type]

**Passengers/Guests:**
[List each person with relevant filled details]

**Extras:**
[Seats selected, baggage, insurance, add-ons]

**Loyalty:**
[EuroBonus applied / not applicable]

**Total: €[amount]**

⏸  I've stopped at the payment page.
   Review the details above, then enter your card details to complete.
   URL: [current page URL]
```

11. If any fields could not be filled (unexpected form layout, missing data in profile), list them clearly so the user can fill them manually before paying

## Output format
```
Booking Ready for Your Review
━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[Booking details: flight/hotel/car info]
Passengers/Guests: [list with relevant details]
Extras: [selected add-ons]
Total: [price]

⏸  I've stopped at the payment page.
   Review the booking, then enter your card details to complete.
   URL: [current page URL]
```

## Browser Session Notes

- **Login state is shared:** The Chrome connector uses the user's existing Chrome session. If you're already logged into SAS.dk, Airbnb, or other booking sites, Claude will use that session — loyalty pricing and saved details apply automatically.
- **CAPTCHAs:** If a CAPTCHA appears during the booking flow, execution pauses. Solve it manually in Chrome, then Claude continues.
- **Payment enforcement:** The Chrome extension's built-in prohibited actions prevent submitting payment forms. This is a platform-level safety boundary, not just a plugin instruction.
