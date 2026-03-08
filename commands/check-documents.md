# Command: check-documents
Slash command: /travel:check-documents

## Purpose
Validate passports, visas, and travel authorizations for all family members against a specific destination and travel date.

## Inputs
- destination (required): Country name or code (e.g., "USA", "UK", "Portugal")
- travel_date (required): YYYY-MM-DD — the departure date to check against

## Instructions

### 1. Load data
Read `data/document-vault.yaml` and `data/family-profile.yaml` to get all family members, their passports, and existing travel authorizations.

### 2. Classify destination
Determine which rule set applies to the destination:
- **Schengen Area** (Spain, Portugal, France, Italy, Greece, Germany, Austria, Netherlands, Belgium, Switzerland, Norway, Sweden, Finland, Czech Republic, Poland, Hungary, Croatia, etc.): 3-month passport validity beyond return date, no authorization needed
- **USA**: 6-month passport validity beyond entry date, ESTA required
- **Canada**: 6-month passport validity beyond entry date, eTA required
- **Australia**: 6-month passport validity beyond entry date, ETA required
- **UK**: passport valid for duration of stay, ETA required
- **Other/unknown**: default to 6-month passport validity beyond entry date (safest assumption), check authorization requirements case-by-case

If no return date is provided, assume a 14-day trip from the travel_date.

### 3. Run document checks
Execute the document-checker agent logic for all family members:
- Check each member's passport validity against the destination's requirements
- Check each member's travel authorization status for the destination
- Run child-specific checks (age at travel, 5-year passport rule, turning-18 flag)

### 4. Present results as a formatted table
Display one row per family member with status icons:
- ✅ = VALID or NOT_REQUIRED
- ⚠️ = EXPIRES_SOON, POTENTIALLY_INVALID, or UNKNOWN
- ❌ = EXPIRED, INSUFFICIENT_VALIDITY, or MISSING

Format:
```
Document Check — [DESTINATION], [formatted date]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[Name]       [Passport status icon + detail]   [Authorization status icon + detail]
...
```

### 5. List action items sorted by urgency
Below the table, list numbered action items in this priority order:

**Priority 1 — Passport issues (longest lead time: 4-6 weeks)**
- Expired or insufficient-validity passports first
- Include: member name, current expiry date, what's needed, timeline ("allow 4-6 weeks")

**Priority 2 — Missing authorizations (shorter lead time)**
- Missing or expired authorizations
- Include: member name, authorization type, application link, cost, typical processing time

**Priority 3 — Warnings (informational)**
- Expiring-soon passports or authorizations
- Potentially invalid authorizations (passport renewed since issuance)
- Child turning 18 before travel

### 6. All-clear summary
If every family member has valid passport and valid (or not-required) authorization status, display:
```
✅ All clear — everyone is ready to travel to [DESTINATION].
```

### 7. Missing data notice
If any passport fields are null or missing (expiry_date, document_number, etc.), add a note at the end:
```
⚠️ Incomplete data: [list of members with missing fields]. Update via /travel:update-profile for a complete check.
```

## Output format
```
Document Check — [DESTINATION], [DATE]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[Member]    [Passport status]   [Authorization status]
...

Action items:
1. [Specific action with timeline and link]
```
