# Command: find-accommodation
Slash command: /travel:find-accommodation

## Purpose
Search and compare accommodation options for the family.

## Inputs
- destination (required): City or region name
- checkin (required): YYYY-MM-DD
- checkout (required): YYYY-MM-DD
- area (optional): Preferred neighbourhood or area

## Instructions

1. Load `data/family-profile.yaml` for: accommodation requirements (4+ bedrooms, full kitchen, private garden/terrace), accommodation style (Airbnb preferred), car rental needs
2. Invoke the accommodation-scout agent with: destination, check-in/check-out dates, area preference (if provided), family requirements
3. Present the top 3 accommodation options in this format:

```
## Accommodation Options: [Destination] ([checkin] → [checkout])

### Option 1: [Property Name] ⭐ [rating] ([review_count] reviews)
**Why this one:** [brief note referencing family preferences]
- **Type:** [house/villa]
- **Bedrooms:** [N] | **Bathrooms:** [N]
- **Price:** €[nightly] /night → **€[total] total**
- **Key amenities:** [pool, garden, kitchen, parking, etc.]
- **Location:** [area] — [distance] to beach, [distance] to centre
- **Cancellation:** [policy]
- **Book:** [url]

### Option 2: ...
### Option 3: ...

### Car Rental
- **Vehicle:** [type] ([N]-seat)
- **Rate:** €[daily]/day → **€[total] total** ([N] days)
- **Supplier:** [name]
- **Pickup:** [destination airport]
```

4. Add a brief comparison note after the options
5. Prompt: "Pick accommodation and I'll prepare the booking with all guest details pre-filled."

## Output format
Per option: name, price per night, total price, key amenities, distance to beach/centre, rating, and booking URL.
