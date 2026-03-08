---
name: document-checker
description: Checks passport expiry, ESTA/ETA validity, and visa requirements for all family members against a specific destination and travel date.
---

# Agent: document-checker
Type: sub-agent (spawned by commands)

## Purpose
Given a destination and travel dates, verify all family members' travel documents and authorizations are valid.

## Tools available
- File read: `data/document-vault.yaml`, `data/family-profile.yaml`

## Behaviour rules
> **Stub — implemented in Milestone 3**
>
> When implemented, this agent will:
> 1. Load all family member documents from `data/document-vault.yaml`
> 2. Check passport validity against destination-specific rules:
>    - Schengen: 3 months beyond travel dates
>    - USA/Canada/Australia: 6 months beyond travel dates
>    - UK: valid for duration of stay
> 3. Check travel authorization status (ESTA, ETA, eTA, visa)
> 4. Flag child-specific issues (5-year passport expiry, ESTA invalidity at 18)
> 5. Return per-member status report

## Output schema
Per family member:
- member_id, name
- passport_status: VALID | EXPIRES_SOON | EXPIRED | INSUFFICIENT_VALIDITY
- passport_expiry_date
- authorization_status: VALID | MISSING | EXPIRED | NOT_REQUIRED
- authorization_type (if applicable)
- action_required (string, or null if none)
