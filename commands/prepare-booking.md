# Command: prepare-booking
Slash command: /travel:prepare-booking

## Purpose
Navigate to a booking page, pre-fill all form fields from the family profile and document vault, and stop at payment for user validation.

## Inputs
- type (required): What to book — "flight", "accommodation", or "car"
- url (required): The booking URL to navigate to

## Instructions
> **Stub — implemented in Milestone 6**
>
> When implemented, this command will:
> 1. Open the booking URL in the browser
> 2. Read current form state
> 3. Fill all passenger/guest fields from `data/document-vault.yaml`
> 4. Select extras (seats, baggage) per `data/family-profile.yaml` preferences
> 5. Navigate to the payment page
> 6. **STOP** — do not enter payment details
> 7. Present a clear summary of what's been filled

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
