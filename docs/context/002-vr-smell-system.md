# VR Smell System - Technical Context

> **Context file for AI-assisted development sessions**
> Last updated: January 2025

## System Overview

Delivers scented air to user's nose based on their position in VR world. Unreal Engine trigger boxes send HTTP requests to Arduino, which controls electric air valves routing airflow through scent chambers.

## Architecture

```
┌─────────────────┐     HTTP/REST      ┌──────────────────┐
│  Meta Quest 3   │◄──────────────────►│  Arduino + WiFi  │
│  (Unreal Engine)│   "lavender:on"    │  (Web Server)    │
└─────────────────┘                    └────────┬─────────┘
                                                │ GPIO
                                                ▼
                                       ┌────────────────┐
                                       │  Transistors   │
                                       │  (BJT array)   │
                                       └────────┬───────┘
                                                │ 12V switched
                                                ▼
┌─────────┐    ┌─────────────────────────────────────────────────┐
│ 12V Air │    │              Valve Manifold                     │
│  Pump   │───►│  ┌─────────┐  ┌─────────┐  ┌─────────┐         │
│(always  │    │  │ Valve 1 │  │ Valve 2 │  │ Valve 3 │         │
│  on)    │    │  │Unscented│  │Campfire │  │Lavender │         │
└─────────┘    │  └────┬────┘  └────┬────┘  └────┬────┘         │
               └───────┼───────────┼───────────┼────────────────┘
                       │           │           │
                       ▼           ▼           ▼
               ┌───────────┐ ┌───────────┐ ┌───────────┐
               │  Bypass   │ │  Chamber  │ │  Chamber  │
               │ (no oil)  │ │ (campfire │ │ (lavender │
               │           │ │   oil)    │ │   oil)    │
               └─────┬─────┘ └─────┬─────┘ └─────┬─────┘
                     │             │             │
                     └──────────┬──┴─────────────┘
                                │
                                ▼
                        ┌──────────────┐
                        │ Output Tube  │
                        │ (to nose)    │
                        └──────────────┘
```

## Current Hardware

### Components (as of January 2025)

| Component | Specification | Notes |
|-----------|--------------|-------|
| Microcontroller | Arduino with WiFi | Running as HTTP server |
| Air Pump | 12V | Always running when system active |
| Valves | 12V Electric Air Valves (×3) | Solenoid, normally closed |
| Switching | BJT transistors | Replaced motor driver for simplicity |
| Power | 9V battery pack | For logic; valves need 12V |
| Tubing | 1/4" ID silicone | Flexible, food-safe |
| Chambers | Hard plastic containers | Drilled + bulkhead fittings for airtight seal |
| Diffusion medium | Paper towel with essential oil | Simple, replaceable |

### Scent Channels

1. **Unscented (bypass)** - Always used when no smell active; prevents contamination and provides continuous airflow
2. **Campfire** - Smoky/woody essential oil blend
3. **Lavender** - Floral essential oil

## Key Design Decisions

### Always-Flowing Air
The pump runs continuously, with airflow routed through the unscented bypass when no smell is active. This solves two problems:
- No back-pressure buildup from closed valves
- Continuous purging prevents scent contamination between triggers

### Single Output Line
All channels merge to one tube at the nose. Cross-contamination is managed by the unscented purge path, not by separate delivery tubes.

### One Smell at a Time (Current Limitation)
Current valve arrangement only supports binary: one scent OR unscented. Future work needed for:
- Simultaneous scents (ocean + flowers)
- Scent magnitude/intensity control

### Airtight Seals Critical
> "A tiny leak can mean a smell that isn't supposed to be present ruins the scene."

All connections must be airtight. Bulkhead fittings through drilled plastic require careful deburring and sealing.

## Communication Protocol

### Current: HTTP REST

```
POST /smell HTTP/1.1
Content-Type: application/json

{"scent": "lavender", "state": "on"}
```

```
POST /smell HTTP/1.1
Content-Type: application/json

{"scent": "lavender", "state": "off"}
```

### Considered Alternative: Serial

HTTP adds overhead and failure modes. Serial (USB or Bluetooth) may be simpler and lower-latency. TBD whether to migrate.

## Unreal Engine Integration

Trigger boxes placed around scent sources in the level:
- When player enters trigger box → send "on" request
- When player exits trigger box → send "off" request
- Overlapping triggers need careful handling (future work)

## Future Improvements

### Magnitude Control (Unsolved)

How to control scent intensity, not just on/off?

**Options considered:**
1. **PWM on valves** - Rapid cycling; may be too slow at small scale, could pulse rather than blend
2. **Proportional valves** - Analog control, more expensive and complex
3. **Bypass mixing** - Two paths per scent (through chamber / bypass), control ratio
4. **Multiple chambers per scent** - "Light campfire" vs "heavy campfire" as separate channels
5. **Variable output distance** - Adjust how much reaches nose

**Current decision:** Keep binary on/off for v1; note as R&D item.

### Additional Scent Channels

The island needs more smells:
- Ocean/salt air
- Sulfur (volcano)
- Damp earth/stone (cave)
- Forest/pine
- Flowers (multiple types?)

Scaling requires:
- More valves
- Larger manifold
- Possibly multiple output tubes or zones

### Battery Power

Currently Arduino powered by laptop USB. Full mobile solution needs:
- Battery for Arduino
- Battery for 12V pump and valves (or voltage conversion)

---

## Reference Image

See: `assets/images/vr-smell-prototype-v1.jpg`

Annotated photo showing:
- Arduino placement
- Battery pack
- Valve array
- Smell chambers with bulkhead fittings
- Tubing routing
- Output to nose position

---

*See also: [001-project-overview.md](001-project-overview.md) for grand vision and design principles*
