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
> **Stub — implemented in Milestone 5**
>
> When implemented, this command will:
> 1. Load family profile for accommodation requirements (4+ bedrooms, kitchen, garden)
> 2. Invoke the accommodation-scout agent
> 3. Agent searches Airbnb and Booking.com with family filters
> 4. Filter out apartments and shared spaces — houses/villas only
> 5. Sort by rating, then price
> 6. Return top 3 options with amenities and booking links

## Output format
Per option: name, price per night, total price, key amenities, distance to beach/centre, rating, and booking URL.
