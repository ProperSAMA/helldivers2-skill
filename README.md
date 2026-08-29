# Helldivers 2 Skill for OpenClaw

[English](README.md) | [中文](README.zh-CN.md)

An [OpenClaw](https://github.com/openclaw/openclaw) skill that queries live [Helldivers 2](https://www.helldivers2.com/) galactic war data via the community API at [api.helldivers2.dev](https://api.helldivers2.dev).

## What It Does

Ask your OpenClaw agent about the current state of the galactic war:

- **War overview** — global player count, mission stats, kill counts by faction
- **Major Order** — current objective, progress, time remaining, reward
- **Planet status** — any of the 273 planets: owner, liberation progress, active players, biome, hazards
- **Active campaigns** — all current front lines with player counts
- **News dispatches** — latest in-game Super Earth broadcasts

## Usage Examples

Once installed, just ask naturally:

- "What's the current Helldivers 2 status?"
- "Show me the Major Order progress"
- "How many players are on Malevelon Creek?"
- "Any news from Super Earth?"

## Installation

Copy the `helldivers2/` directory into your OpenClaw skills folder:

```bash
cp -r helldivers2/ ~/.openclaw/skills/helldivers2/
```

Or use the OpenClaw plugin/skill management commands if available.

## API Details

- **Source**: [api.helldivers2.dev](https://api.helldivers2.dev) (community-maintained)
- **Auth**: None required; custom headers for identification
- **Rate limit**: ~5 requests per 10-second sliding window (`Retry-After: 10` on 429)
- **Safe rate**: 1 request per 2 seconds

### Available Endpoints

| Endpoint | Description |
|----------|-------------|
| `GET /api/v1/war` | Global war statistics |
| `GET /api/v1/assignments` | Current Major Order(s) |
| `GET /api/v1/planets` | All 273 planets |
| `GET /api/v1/planets/{index}` | Single planet detail |
| `GET /api/v1/campaigns` | Active campaign fronts |
| `GET /api/v1/dispatches` | In-game news feed |

## Repository Structure

```
helldivers2/
└── SKILL.md    # Skill definition with API reference and usage patterns
```

## Credits

- Skill created by [Lumi](https://github.com/ProperSAMA) (AI assistant) for ProperSAMA
- Data provided by the [Helldivers 2 Community API](https://api.helldivers2.dev)
- Game by [Arrowhead Game Studios](https://www.arrowheadgamestudios.com/)

---

*For Super Earth! 🌍✨*
