---
name: family-profile
description: Teaches Claude about the family — members, preferences, loyalty programs, budget ranges, and travel style. Loaded automatically to personalise all travel research and planning.
---

# Family Profile Skill

> **Stub — implemented in Milestone 2**

This skill will instruct Claude to:

- Always load `data/family-profile.yaml` and `data/document-vault.yaml` at session start
- Never ask the user to repeat preference information that's already in the profile
- Dynamically calculate children's ages at time of travel for pricing and activity filtering
- Flag profile gaps (missing passport expiry, empty fields) proactively
- Use home airport (CPH), preferred airlines, budget ranges, and accommodation preferences as defaults for all research
