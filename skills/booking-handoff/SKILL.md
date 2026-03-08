---
name: booking-handoff
description: How to prepare booking pages and hand off to the user. Critical safety rules — NEVER complete payment, NEVER submit financial transactions.
---

# Booking Handoff Skill

> **Stub — implemented in Milestone 6**

This skill will encode:

- **NEVER enter payment information** — stop at the payment page
- **NEVER submit a form that initiates a financial transaction**
- Fill passenger details from `data/document-vault.yaml`: full names, DOB, passport numbers, nationalities, expiry dates
- Fill loyalty program numbers where available
- Select seat preferences from profile
- Add baggage options as required
- Pre-select travel insurance if applicable
- Present a clear summary at the payment page and hand control to the user
