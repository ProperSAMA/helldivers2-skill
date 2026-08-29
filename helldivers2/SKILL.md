---
name: "helldivers2"
description: "Query Helldivers 2 live war status, major orders, planets, campaigns, and dispatches via the helldivers2.dev public API."
---

# Helldivers 2 War Status

Query live Helldivers 2 galactic war data via the community API at `api.helldivers2.dev`.

## API Basics

- **Base URL**: `https://api.helldivers2.dev/api/v1`
- **Required headers** (every request):
  - `X-Super-Client: openclaw-lumi`
  - `X-Super-Contact: propersama`
- **Rate limit** (measured 2026-08-29): ~5 requests per 10-second sliding window. On 429, response includes `Retry-After: 10`. Safe sustained rate: **1 request per 2 seconds** (`sleep 2` between calls).
- **Auth**: none beyond the headers above.
- Use `/usr/bin/curl` explicitly (PATH may be incomplete in cron/shell contexts).

## Available Endpoints

| Endpoint | Description |
|----------|-------------|
| `GET /war` | Global war stats: player count, missions won/lost, kills by faction, impact multiplier |
| `GET /assignments` | Current Major Order(s): briefing, tasks, progress, reward, expiration |
| `GET /planets` | All 273 planets: name, sector, biome, hazards, owner, health, player count, statistics |
| `GET /planets/{index}` | Single planet detail (same shape as items in `/planets`) |
| `GET /campaigns` | Active campaigns: planet + faction + player count per active front |
| `GET /dispatches` | In-game news dispatches (Super Earth propaganda style), newest first |

## Quick Reference

### War overview
```bash
/usr/bin/curl -s \
  -H "X-Super-Client: openclaw-lumi" \
  -H "X-Super-Contact: propersama" \
  "https://api.helldivers2.dev/api/v1/war"
```
Key fields: `playerCount`, `statistics.missionsWon`, `statistics.missionsLost`, `statistics.missionSuccessRate`, `statistics.terminidKills`, `statistics.automatonKills`, `statistics.illuminateKills`.

### Current Major Order
```bash
/usr/bin/curl -s \
  -H "X-Super-Client: openclaw-lumi" \
  -H "X-Super-Contact: propersama" \
  "https://api.helldivers2.dev/api/v1/assignments"
```
Returns array (usually 0 or 1 item). Key fields: `briefing`, `progress[]` (per-task), `reward.amount`, `expiration` (ISO 8601).

### All planets (large response, ~273 items)
```bash
/usr/bin/curl -s \
  -H "X-Super-Client: openclaw-lumi" \
  -H "X-Super-Contact: propersama" \
  "https://api.helldivers2.dev/api/v1/planets"
```
Each planet: `index`, `name`, `sector`, `biome.name`, `hazards[].name`, `currentOwner` (Humans/Terminids/Automaton/Illuminate), `health`/`maxHealth`, `statistics.playerCount`, `event` (null or active event), `regions[]` (cities/outposts).

### Active campaigns
```bash
/usr/bin/curl -s \
  -H "X-Super-Client: openclaw-lumi" \
  -H "X-Super-Contact: propersama" \
  "https://api.helldivers2.dev/api/v1/campaigns"
```
Each entry embeds a full `planet` object plus `players` count on that front.

### News dispatches
```bash
/usr/bin/curl -s \
  -H "X-Super-Client: openclaw-lumi" \
  -H "X-Super-Contact: propersama" \
  "https://api.helldivers2.dev/api/v1/dispatches"
```
Each dispatch: `id`, `published` (ISO 8601), `message` (contains `<i=N>` formatting tags — strip or interpret for display).

## Data Notes

- Planet `health` / `maxHealth` → liberation progress: lower health = closer to liberation (for enemy-owned) or defense (for Human-owned under attack).
- `regenPerSecond` — planet health regeneration rate; liberation requires outpacing this.
- `impactMultiplier` from `/war` — global modifier affecting how much each mission contributes.
- Dispatch `<i=1>` = yellow highlight, `<i=3>` = red/alert highlight.
- Faction names: `Humans`, `Terminids`, `Automaton`, `Illuminate`.

## Usage Patterns

- **Quick status**: fetch `/war` + `/assignments` → summarize online count, major order progress, time remaining.
- **Planet lookup**: fetch `/planets`, find by name (case-insensitive match), report owner/health/players.
- **Front lines summary**: fetch `/campaigns`, group by faction, list planets with player counts.
- **News digest**: fetch `/dispatches`, take latest N, strip `<i=N>` tags, present as bulletin.
