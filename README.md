# MAI – Mobs.Anomalies.Interaction

## Version 2.0

An attempt to make NPCs interact with anomalies realistically - they will try to avoid them and can take damage from them.

---

## What Does This Mod Do?

For a long time in S.T.A.L.K.E.R. Anomaly, NPCs completely ignored anomalies - they walked straight through them and took no damage. MAI fixes this by:

1. **Pathfinding Restrictions** - NPCs detect nearby anomalies and avoid walking through them
2. **Damage System** - NPCs can take damage from anomalies based on their rank and movement state
3. **Armor Protection** - NPC outfit protection values affect anomaly damage taken

---

## Features

### Rank-Based Behavior

Higher ranked NPCs are better at detecting and avoiding anomalies:

| Rank | Miss Chance | Behavior |
|------|-------------|----------|
| Novice | 100% | Often walks into anomalies, always takes damage |
| Trainee | 75% | Frequently misses anomalies |
| Experienced | 50% | Average detection |
| Professional | 25% | Good at avoiding anomalies |
| Veteran+ | 0% | Always detects anomalies, immune to damage |

### Movement State Multipliers

NPC awareness depends on what they're doing:

| State | Multiplier | Effect |
|-------|------------|--------|
| Standing | 0.3x | Very aware, rarely misses |
| Walking | 0.6x | Observant |
| Running | 1.2x | Might miss some |
| Combat | 3.0x | Distracted, often misses |

### Community-Specific Behavior

| Community | Evades? | Takes Damage? | Reason |
|-----------|---------|---------------|--------|
| **Sin (greh)** | No | No | Children of the Zone |
| **Monolith** | Yes | No | Protected by the Monolith |
| **Zombified** | No | Yes | Mindless, walk straight in |
| **Companions** | No | No | Protected |
| **Traders** | No | No | Protected |
| **Everyone else** | Yes | By roll | Normal behavior |

### Outfit-Based Damage Protection

Damage is modified by NPC's equipped outfit protection values:

- **Electric anomalies** → shock_protection
- **Fire/thermal anomalies** → burn_protection  
- **Chemical anomalies** → chemical_burn_protection
- **Gravity anomalies** → strike_protection
- **Radiation anomalies** → radiation_protection
- **Psy anomalies** → telepatic_protection

Example: An exoskeleton with 0.7 shock protection reduces electric anomaly damage by 70%.

---

## Installation

Install via Mod Organizer 2. Load order doesn't matter, unless it conflicts with other files *(it should not)*.

---

## Compatibility

### Should be Compatible With:
- **Arrival / DAO** - Cache rebuild is delayed after surge to wait for anomaly respawning
- **New Levels** - Underground locations supported
- **Dynamic NPC Armor Visuals** - Armor protection reads from actual equipped outfit
- **New faction mods** - Unknown factions use default stalker settings
- **New rank mods** - Unknown ranks use novice settings

### Not Compatible With:
- Other mods that enable NPC anomaly damage (would conflict)

---

## MCM Settings

### General
- **Enable Mod** - Toggle the entire mod on/off
- **Debug Mode** - Enable detailed logging and death notifications

### Detection
- **Detection Radius** - How far NPCs can detect anomalies (default: 30m)
- **Max Restrictions Buffer** - Maximum pathfinding restriction string length
- **Cache Rebuild Delay** - Seconds to wait after surge before rebuilding cache (for Arrival compatibility, default: 2.5s)

### Damage
- **Outfit-based Damage** - Use armor protection values for damage calculation
- **Zero Miss Chance Immunity** - NPCs with 0% miss chance (veterans+) are immune to damage

### Movement Multipliers
- Standing, Walking, Running, Combat multipliers (adjustable)

### Rank Miss Chances
- Individual miss chance for each rank (0-100%)

### Map Marker Cleanup
- One-time cleanup of old "anomaly_disabled" map markers from previous MAI versions

---

## Performance

v2.0 has been completely rewritten for better performance:

- **Lightweight anomaly cache** - Rebuilt only after surge/psy-storm or level change
- **Global update loop** - Single 2-second interval loop for all NPCs
- **No per-NPC sessions** - Removed subscribe/unsubscribe overhead
- **Throttled updates** - Each NPC updated at most once per second

With 100 online NPCs, expect minimal performance impact *(improved from v1.0)*.

---

## Debug Mode

When enabled, debug mode provides:

- Console logging with `[MAI]` prefix for all actions
- Categories: `[EVENT]`, `[CACHE]`, `[CONFIG]`, `[RESTRICT]`, `[DETECT]`, `[DAMAGE]`, `[IGNORE]`
- In-game news notifications when NPCs are killed by anomalies

Example death notification:
```
[MAI DEBUG] sim_default_stalker_030721 (novice stalker) killed by zone_mine_electric while running
```

---

## Credits

- **Original anomaly evade concept** - From ogse_anomaly_evader.script in OLR 3.0
- **Important NPC list** - From NPC Die For Real mod by TheMrDemonized
- [**Original MAI mod**](https://www.moddb.com/addons/demonized-exes-mobsanomaliesinteraction-v10beta) bt allc0r3

---

## Changelog

### 2.0 – December 2024

**Complete Rewrite**

Architecture:
- Removed per-NPC session tracking (subscribe/unsubscribe system)
- Implemented lightweight anomaly cache with smart invalidation
- Single global update loop (2s interval) instead of per-NPC timers
- Cache rebuilds only on surge/psy-storm end or level change

New Features:
- Outfit-based damage protection system (reads actual outfit config values)
- Movement state affects both detection and damage chance
- Configurable rank miss chances via MCM
- Community-specific behavior (greh, monolith, zombified)
- Debug news messages for anomaly deaths
- Map marker cleanup utility for old MAI versions
- Cache rebuild delay for Arrival mod compatibility

Performance:
- Significantly reduced CPU usage
- No more memory leaks from session tracking
- Throttled per-NPC updates

Compatibility:
- Full Arrival/DAO support with delayed cache rebuild
- Works with any modded outfits automatically
- Proper engine-level damage enable/disable via set_enable_anomalies_damage()

### 1.01_Beta – 27.10.2024

- Basic support for NPC visual changes and damage based on visual
- NPCs don't take damage while finishing off wounded
- Added anomalies without radius in Red Forest to exceptions
- Re-subscription of online NPCs after player sleep

### 1.00_Beta – 25.10.2024

- Initial release
