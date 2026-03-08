---
name: booking-handoff
description: How to prepare booking pages and hand off to the user. Critical safety rules — NEVER complete payment, NEVER submit financial transactions.
---

# Booking Handoff Skill

> **Stub — implemented in Milestone 6**

This skill will encode:

- **NEVER enter payment information** — stop at the payment page
- **NEVER submit a form that initiates a financial transaction**
- Payment pages require manual user action — this is enforced by the Chrome extension's built-in prohibited actions, not just this plugin's instructions
- Fill passenger details from `data/document-vault.yaml`: full names, DOB, passport numbers, nationalities, expiry dates
- Fill loyalty program numbers where available
- Select seat preferences from profile
- Add baggage options as required
- Pre-select travel insurance if applicable
- Present a clear summary at the payment page and hand control to the user

### Chrome Session & Login State

- The Chrome connector shares the user's existing Chrome session. If the user is already logged into SAS.dk, Airbnb, Booking.com, or other travel sites, Claude uses that session automatically — loyalty numbers, saved payment methods, and member pricing are applied.
- **CAPTCHAs** will pause execution and require the user to solve them manually in Chrome before the agent can continue.
- **Payment pages** always require manual user action. The Chrome extension's built-in safety layer prevents submitting payment forms, independent of this plugin's own rules.
