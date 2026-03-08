# Command: update-profile
Slash command: /travel:update-profile

## Purpose
Update any section of the family profile, document vault, or travel preferences.

## Inputs
- section (optional): Which section to update — "members", "preferences", "documents", "loyalty", or "all"
- member (optional): Specific family member to update (by name or ID)

## Instructions

### 1. Read Current Data
Load both data files:
- `data/family-profile.yaml`
- `data/document-vault.yaml`

### 2. Determine Scope
- If the user specified a `section`, focus on that section only
- If the user specified a `member`, show only that member's data across both files
- If neither is specified, show a summary of all sections and ask which to update

### 3. Show Current Values
Display the current values for the selected scope in a readable format (table or structured list). Highlight fields that are `null` or empty — these are the most likely candidates for updates.

### 4. Accept Updates Interactively
- Walk through each field the user wants to update
- For member updates: name, date_of_birth, seat_preference, dietary, notes
- For document updates: document_number, issue_date, expiry_date, notes
- For preference updates: any field in `travel_preferences`
- For loyalty updates: program name, member_id, linked member
- Support adding new members (assign next available child ID or custom ID)
- Support removing members who are no longer part of the travel group

### 5. Write Changes
- Write updated data back to the appropriate YAML file(s)
- Preserve all existing data that wasn't explicitly changed
- Use `null` for fields the user wants to clear (not empty strings)

### 6. Confirm Changes
Show a before/after summary of every field that changed. Example:

```
Changes saved:

  family-profile.yaml:
    members[spouse].name: null → "Anna"
    members[child1].name: null → "Oliver"
    members[child1].date_of_birth: null → "2015-03-22"

  document-vault.yaml:
    documents[peer].document_number: null → "123456789"
    documents[peer].expiry_date: null → "2032-06-15"
```

## Output format
Before/after summary of all changes made, with confirmation that files have been updated.
