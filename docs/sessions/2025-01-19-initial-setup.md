# Session Transcript: Initial Project Setup

**Date:** January 19, 2025 (Sunday)  
**Duration:** ~1.5 hours  
**Participants:** Will + Claude (Opus 4.5)

---

## Session Goals

- Introduce the VR Immersion project to Claude
- Establish project structure and documentation approach
- Create initial context files for AI-first development

---

## Discussion Summary

### 1. Project Introduction

Will shared the grand vision: a fully immersive VR experience on a virtual island with multiple environments (cave, beach, volcanic caldera, forest, dock, abandoned temple). Each location delivers distinct sensory experiences beyond just visuals and audio—including smell, temperature, and wind.

**Key insight established:** The magic is in the *transitions* between environments, not maintaining steady states. A 10-20 second sensory shift is what creates immersion.

### 2. Design Principles Established

Three core principles emerged from discussion:

1. **Transitions matter most** - The sensory shift between environments creates immersion
2. **Orientation contracts** - Design VR world constraints to align with fixed physical hardware (e.g., all benches face east so physical bench can be positioned correctly)
3. **Minimal gradient** - Accept flat physical space; create journey through atmosphere rather than elevation (volcanic caldera instead of climbing a volcano)

### 3. VR Smell Prototype Review

Will shared annotated photo of current prototype. Components identified:
- Arduino with WiFi (HTTP server receiving commands from Unreal Engine)
- 12V air pump (always running)
- 12V electric solenoid valves (3 channels: unscented, campfire, lavender)
- BJT transistors for valve switching
- Handcrafted smell chambers (hard plastic with drilled bulkhead fittings)
- 1/4" ID silicone tubing
- Single output tube to nose

**Key design decision in v1:** Air always flows through unscented bypass when no smell active. This solves back-pressure and contamination issues simultaneously.

### 4. Collaboration Model Discussion

Established division of labor for AI-first development:
- **Will:** Physical assembly, soldering, testing, purchasing, 3D printing
- **Claude:** Code, schematics, CAD designs, documentation, research, BOMs

Will noted he works on projects weekends only (IP considerations with day job).

### 5. Parts Research Framework

Discussed approach for VR Smell v2:
- Goal: Minimize manual fabrication (no drilling, sanding)
- Strategy: Find a "tubing ecosystem" with push-to-connect fittings
- Leverage 3D printer for custom chambers, mounts, enclosures
- Fail-fast: Order small test kit (~$30-50) before committing

**Open questions answered:**
- Channel count: 6 (prototype) → 20 (early versions) → 100+ (long-term "scent printer")
- Form factor: Wearable preferred (stationary creates tethering/tangle issues)

### 6. Pump Specifications

Will provided SparkFun pump link. Specs documented:
- Model: ROB-10398 (12V diaphragm pump)
- Flow rate: 9-15 LPM (sufficient for 6-20 channels)
- Noise: 65 dB (concern for wearable)
- Ports: 1/4" barbs
- Price: $25.50

### 7. Scaling Challenges Identified

Will raised latency scaling problem: as channel count grows, air path length increases, causing delay between valve-on and smell-at-nose. This could break immersion at high channel counts.

**Scaling behavior:** Likely O(√n) or worse depending on manifold geometry.

### 8. Double-Buffered Pneumatics Concept

Will proposed clever mitigation (for future, not initial prototype):

Two parallel outlet paths with branch near nose:
1. **Preload phase:** High-pressure burst through target chamber → DUMP outlet (away from nose)
2. **Delivery phase:** Switch to NOSE outlet at normal pressure

Scented air is pre-staged at branch point before user arrives. Perceived latency: near-zero.

**Analogy:** Like UI double-buffering—render next frame off-screen, swap instantly when ready.

**Constraints identified:**
- Dump must vent away from nose
- Dump can exit anywhere in system (doesn't need to be near branch)
- Dump output must NOT feed back into pump inlet (no scent recycling)

---

## Artifacts Created

### Repository Structure
```
vr-immersion/
├── README.md
├── .gitignore
├── docs/
│   ├── context/
│   │   ├── 000-template.md
│   │   ├── 001-project-overview.md
│   │   ├── 002-vr-smell-system.md
│   │   └── 003-collaboration-and-parts.md
│   └── sessions/
│       └── 2025-01-19-initial-setup.md  (this file)
├── firmware/README.md
├── unreal/README.md
├── hardware/README.md
└── assets/images/
    └── vr-smell-prototype-v1.jpg
```

### Git Commits Made
1. Initial project setup with structure and context files
2. Add collaboration workflow and parts research context
3. Update with channel count and form factor decisions
4. Add pump specs and latency scaling challenges
5. Add double-buffered pneumatics concept

---

## Action Items

### For Next Session (Claude)
- [ ] Web search for push-to-connect pneumatic ecosystems
- [ ] Find valve options with native push-to-connect ports
- [ ] Research small pneumatic manifolds
- [ ] Draft initial BOM for test kit (~$30-50)
- [ ] Design parametric smell chamber in OpenSCAD
- [ ] Research quieter pump alternatives for wearable

### For Will (Between Sessions)
- [ ] Measure scent latency on current prototype (stopwatch: valve-on to smell-at-nose)
- [ ] Consider sourcing preferences (Amazon vs McMaster-Carr vs AliExpress)
- [ ] Optional: Measure pump output using bag displacement method

---

## Key Quotes & Ideas

> "Transitions create immersion—the sensory shift between environments is the magic."

> "Orientation contracts: design the VR world so all benches face east, then your physical bench can be in a known position."

> "Double-buffered pneumatics: like UI double-buffering—prepare the scent off-nose, swap instantly when ready."

> "I want to make sure key context lives in the codebase/context files and not in your memory. It scales better with team."

---

## Notes for Future Sessions

- Will prefers short, focused sessions with thorough documentation
- Context files are the source of truth—always update them
- Session transcripts help track evolution of ideas over time
- Will works weekends only (IP considerations)
- Will's neighbor (mechanic) may join project later—documentation enables this

---

*Next session planned: Parts research and initial BOM*
