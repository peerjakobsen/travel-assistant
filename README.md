# Family Travel Assistant

![Version](https://img.shields.io/badge/version-1.0.0-blue)
![License](https://img.shields.io/badge/license-MIT-green)
![Platform](https://img.shields.io/badge/platform-Claude%20Cowork-7C3AED)
![Plugin Type](https://img.shields.io/badge/type-Cowork%20Plugin-orange)
![Status](https://img.shields.io/badge/status-stable-brightgreen)
![Family Size](https://img.shields.io/badge/family-6%20members-teal)

A personal travel concierge built as a [Claude Cowork](https://www.anthropic.com) plugin. It knows your family — passports, preferences, loyalty programs, school holidays, past trips — and handles everything from destination research to booking preparation.

You say **"10 days somewhere warm in February, ~€8k"** and it comes back with curated options, ready to book.

---

## How It Works

The plugin combines persistent family data with Claude's browser capabilities to automate the tedious parts of travel planning:

1. **You describe the trip** — dates, vibe, budget
2. **It researches** — destinations, flights, accommodation via live browser search
3. **It compares** — structured tables with prices, ratings, and recommendations
4. **It prepares the booking** — pre-fills passenger details, selects seats, applies loyalty numbers
5. **You pay** — the plugin always stops at the payment page

> **Safety boundary:** The plugin never enters payment information or clicks pay. You always complete the final step yourself.

## Commands

| Command | What It Does |
|---------|-------------|
| `/travel:plan-trip` | Full trip planning from a brief — orchestrates destination research, document checks, flights, and accommodation |
| `/travel:research-destination` | Curate 2–3 destination options with cost breakdowns and family suitability |
| `/travel:find-flights` | Search Google Flights, SAS.dk, and Momondo — returns a comparison table |
| `/travel:find-accommodation` | Search Airbnb and Booking.com for villas and houses matching family requirements |
| `/travel:check-documents` | Validate passports and travel authorizations for every family member |
| `/travel:build-itinerary` | Day-by-day plan with activities, restaurants, and rainy day backups |
| `/travel:prepare-booking` | Navigate to booking page, pre-fill all details, stop at payment |
| `/travel:trip-brief` | Generate a printable pre-departure document with all trip details |
| `/travel:update-profile` | Update family members, preferences, or passport data |

## Skills

Skills are domain knowledge that Claude loads automatically — they shape how it reasons about travel decisions.

- **Family Profile** — loads family data at session start, calculates children's ages dynamically, flags profile gaps
- **Document Vault** — passport validity rules by destination (Schengen/USA/UK/other), authorization tracking (ESTA/ETA/eTA), child-specific checks
- **Flight Research** — search priority (Google Flights → SAS.dk → Momondo), filtering rules, seat strategy, EuroBonus optimization
- **Accommodation Research** — platform priority (Airbnb → Booking.com), minimum requirements (4+ bedrooms, full kitchen, private garden), evaluation criteria
- **Itinerary Builder** — pacing rules (max 2 activities/day, buffer days every 3–4 days), age-appropriate filtering, geographic grouping
- **Booking Handoff** — form-filling strategy by booking type, payment boundary enforcement

## Agents

Browser-driven sub-agents that open Chrome and search the web in real time:

- **Flight Scout** — searches multiple flight sites, filters by family constraints, returns top 3 options
- **Accommodation Scout** — searches Airbnb and Booking.com, evaluates properties against family requirements
- **Destination Researcher** — browses travel sites, blogs, and tourism boards to curate destination options
- **Document Checker** — validates passport validity and travel authorizations per destination rules

## Data

All family data lives in `data/` as YAML files:

| File | Contents |
|------|----------|
| `family-profile.yaml` | Family members, travel preferences, budget ranges, loyalty programs |
| `document-vault.yaml` | Passport numbers, expiry dates, ESTA/ETA status per member |
| `trip-history.yaml` | Past and upcoming trips with ratings and notes |
| `school-calendar.yaml` | Danish school holidays by year |
| `scheduled-tasks.yaml` | Recurring tasks — passport checks, pre-trip reminders, calendar updates |

## Installation

### Via Cowork Marketplace

```
/marketplace install family-travel-assistant
```

### Manual Install

1. Download `family-travel-assistant.zip` from the [latest release](https://github.com/peerjakobsen/travel-assistant/releases/latest)
2. Extract into your Claude Code plugins directory
3. Restart Claude Code

### From Source

```bash
git clone https://github.com/peerjakobsen/travel-assistant.git
```

## Project Structure

```
travel-assistant/
├── .claude-plugin/
│   ├── plugin.json          # Plugin manifest
│   └── marketplace.json     # Marketplace metadata
├── commands/                # 9 slash commands
├── skills/                  # 6 domain knowledge skills
├── agents/                  # 4 browser-driven sub-agents
├── data/                    # Family data (YAML)
└── .github/workflows/
    └── release.yml          # Automated GitHub releases
```

## Known Limitations

- **Payment requires manual action** — enforced by both plugin instructions and the Chrome extension's built-in safety rules
- **Login state is inherited** — uses your existing Chrome session. Log into SAS.dk, Airbnb, etc. before starting
- **CAPTCHAs** — some sites will pause and require manual solving in Chrome
- **Real-time pricing** — flight and accommodation prices change constantly. Always re-verify before paying
- **Chrome connector required** — browser tasks only run while Chrome is open with the Cowork connector enabled

## License

[MIT](LICENSE)
