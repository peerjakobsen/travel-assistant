---
name: document-vault
description: Passport validity rules, ESTA/ETA/visa logic per destination, and proactive document expiry warnings for Danish citizens travelling as a family.
---

# Document Vault Skill

## Destination Classification

Group destinations to determine which passport validity and authorization rules apply.

### Schengen Area (3-month passport validity rule, no authorization needed)
Spain, Portugal, France, Italy, Greece, Germany, Austria, Netherlands, Belgium, Switzerland, Norway, Sweden, Finland, Czech Republic, Poland, Hungary, Croatia, Iceland, Malta, Cyprus, Luxembourg, Slovenia, Slovakia, Estonia, Latvia, Lithuania, Denmark

### 6-Month Passport Validity Countries
USA, Canada, Australia, Thailand, Indonesia, Mexico, South Africa, Brazil, Turkey, UAE, Singapore, Japan

### Countries Requiring Travel Authorization
- **USA** → ESTA
- **UK** → ETA
- **Canada** → eTA
- **Australia** → ETA (subclass 601)

### All Other Destinations (default)
Apply the 6-month passport validity rule as the safest assumption. Check authorization requirements case-by-case.

---

## Passport Validity Rules

When checking a family member's passport against a destination and travel dates:

| Destination type | Required validity | Measured from |
|---|---|---|
| Schengen Area | 3 months beyond **return date** | Return date |
| USA, Canada, Australia, 6-month countries | 6 months beyond **entry date** | Departure/entry date |
| UK | Valid for duration of stay only | Return date |
| Unknown / other | 6 months beyond **entry date** (safest default) | Departure/entry date |

### Danish Passport Expiry Rules
- **Adult passports** (issued at age 18+): valid for 10 years from issue date
- **Children's passports** (issued under 18): valid for 5 years from issue date
- If a child's passport was issued more than 5 years ago, it is likely expired

### Passport Status Classification
- **VALID**: Passport has sufficient validity for the destination and travel dates
- **EXPIRES_SOON**: Passport is valid but within 3 months of the required validity threshold — renew proactively
- **INSUFFICIENT_VALIDITY**: Passport does not meet the destination's validity requirement — renewal required before travel
- **EXPIRED**: Passport expiry date has already passed
- **UNKNOWN**: Passport expiry date is null or missing — cannot determine status

---

## Travel Authorization Rules for Danish Citizens

| Destination | Authorization | Cost | Validity | Application |
|---|---|---|---|---|
| USA | ESTA | ~$21 | 2 years | esta.cbp.dhs.gov |
| UK | ETA | £10 | — | UK ETA app |
| Canada | eTA | CAD $7 | 5 years | canada.ca/eta |
| Australia | ETA (subclass 601) | — | — | Australian ETA app |
| Schengen zone / most of Europe | None required | — | — | — |

### ESTA-Specific Rules
- Individual application required per person (including children)
- Linked to a specific passport number — renewing a passport invalidates the existing ESTA
- Usually approved instantly, but allow up to 72 hours
- Must be obtained before boarding a flight to the USA

### Authorization Status Classification
- **VALID**: Authorization on record, obtained, and not expired before travel date
- **MISSING**: No authorization on record, or `obtained: false`
- **EXPIRED**: Authorization expiry date is before the travel date
- **NOT_REQUIRED**: Destination does not require authorization for Danish citizens
- **POTENTIALLY_INVALID**: Authorization exists but linked passport has been renewed since it was issued

---

## Proactive Flag Rules

Generate these flags automatically whenever relevant — do not wait for the user to ask.

### Passport Flags
- **Expiring within validity window**: "[Member]'s passport expires [date] — insufficient for [destination] travel (requires [X] months validity beyond [entry/return] date). Renewal needed before booking. Allow 4-6 weeks."
- **Missing expiry date**: "[Member]'s passport expiry date is missing — add via `/travel:update-profile` before a reliable check can be run."
- **Child passport age rule**: "[Member]'s passport was issued [date], over 5 years ago — likely expired under Danish children's passport rules."

### Authorization Flags
- **No authorization on record**: "No [ESTA/ETA/eTA] on record for [Member]. Required for [destination] travel. Apply at [link] — costs [amount], takes ~10 minutes, [processing time note]."
- **Authorization expiring before travel**: "[Member]'s [ESTA/ETA] expires [date], before the travel date. Renew before the trip."
- **Passport renewed since authorization**: "[Member]'s passport was renewed since their [ESTA/ETA] was issued — the authorization is linked to the old passport and is likely invalid. Re-apply required."

### Child-Specific Flags
- **Child turning 18 before travel**: "[Member] turns 18 before [travel_date] — their child ESTA/authorization is invalid. An adult authorization is required."
- **Child passport issued > 5 years ago**: "[Member]'s passport was issued [date] (> 5 years ago). Danish children's passports are valid for 5 years — likely expired."
