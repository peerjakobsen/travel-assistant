# Command: update-profile
Slash command: /travel:update-profile

## Purpose
Update any section of the family profile, document vault, or travel preferences.

## Inputs
- section (optional): Which section to update — "members", "preferences", "documents", "loyalty", or "all"
- member (optional): Specific family member to update (by name or ID)

## Instructions
> **Stub — implemented in Milestone 2**
>
> When implemented, this command will:
> 1. Read the current profile from `data/family-profile.yaml` and `data/document-vault.yaml`
> 2. Show current values for the requested section
> 3. Walk the user through updates interactively
> 4. Write changes back to the YAML files
> 5. Confirm changes in a clean summary

## Output format
Before/after summary of all changes made, with confirmation that files have been updated.
