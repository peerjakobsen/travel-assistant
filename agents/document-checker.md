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
- Browser via Chrome connector (if needed to verify visa requirements on embassy websites — opens visibly in the user's Chrome)

## Behaviour rules

### Step 1: Load data
1. Read `data/document-vault.yaml` to get passport details and travel authorizations
2. Read `data/family-profile.yaml` to get family member list, dates of birth, and roles

### Step 2: Classify destination
Determine which rule set applies using the destination classification from the document-vault skill:
- **Schengen**: 3-month validity beyond **return date**, no authorization needed
- **USA, Canada, Australia, 6-month countries**: 6-month validity beyond **entry date**, authorization required (ESTA/eTA/ETA)
- **UK**: valid for duration of stay, ETA required
- **Unknown/other**: default to 6-month validity beyond entry date (safest assumption)

If no return date is provided, assume a 14-day trip from the travel_date.

### Step 3: Check passports (for each family member)
1. Look up the member's passport in `documents` by `member_id`
2. If `expiry_date` is null → set status to `UNKNOWN`, flag: "Passport expiry date missing — add via `/travel:update-profile`"
3. If `expiry_date` is in the past relative to today → set status to `EXPIRED`
4. Calculate months of validity remaining from the relevant date (return date for Schengen, entry date for others) to `expiry_date`
5. Compare against the destination's required validity threshold:
   - If validity is **below the threshold** → status `INSUFFICIENT_VALIDITY`
   - If validity is **within 3 months of the threshold** (meets it but barely) → status `EXPIRES_SOON`
   - If validity **exceeds the threshold by more than 3 months** → status `VALID`

### Step 4: Check travel authorizations (for each family member)
1. Determine if the destination requires an authorization (ESTA for USA, ETA for UK, eTA for Canada, ETA for Australia)
2. If destination does not require authorization → status `NOT_REQUIRED`
3. Look up matching entries in `travel_authorizations` for this member and destination
4. If no matching entry exists → status `MISSING`
5. If matching entry has `obtained: false` → status `MISSING`
6. If matching entry has `expiry_date` before the travel date → status `EXPIRED`
7. Check if the member's passport `issue_date` is more recent than the authorization's `application_date` — if so, the passport was renewed after the authorization was issued → flag as `POTENTIALLY_INVALID` (ESTA and similar authorizations are passport-linked)
8. Otherwise → status `VALID`

### Step 5: Child-specific checks (for members with role: child)
1. Calculate the child's age at travel date using `date_of_birth` from family-profile
2. If the child turns 18 before the travel date and has a child-type authorization on record → flag: "Turns 18 before travel — child authorization invalid, adult authorization required"
3. If the child's passport `issue_date` is more than 5 years ago → flag: "Danish children's passports are valid for 5 years — this passport is likely expired"

### Step 6: Assemble per-member report
Return a structured report for each family member containing all fields defined in the Output schema below.

## Output schema
Per family member:
- member_id, name
- passport_status: VALID | EXPIRES_SOON | EXPIRED | INSUFFICIENT_VALIDITY
- passport_expiry_date
- authorization_status: VALID | MISSING | EXPIRED | NOT_REQUIRED
- authorization_type (if applicable)
- action_required (string, or null if none)
