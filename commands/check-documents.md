# Command: check-documents
Slash command: /travel:check-documents

## Purpose
Validate passports, visas, and travel authorizations for all family members against a specific destination and travel date.

## Inputs
- destination (required): Country name or code (e.g., "USA", "UK", "Portugal")
- travel_date (required): YYYY-MM-DD — the departure date to check against

## Instructions
> **Stub — implemented in Milestone 3**
>
> When implemented, this command will:
> 1. Load `data/document-vault.yaml` for all family members
> 2. Invoke the document-checker agent
> 3. Check passport validity against destination-specific rules
> 4. Check travel authorization status (ESTA, ETA, eTA, visa)
> 5. Generate per-member status with specific action items

## Output format
```
Document Check — [DESTINATION], [DATE]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[Member]    [Passport status]   [Authorization status]
...

Action items:
1. [Specific action with timeline and link]
```
