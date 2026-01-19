# Collaboration Workflow & Parts Research

> **Context file for AI-assisted development sessions**
> Last updated: January 2025

## Collaboration Model

This project uses an "AI-first" development approach. The human handles physical work; Claude handles cognitive load reduction.

### Division of Labor

| Area | Human | Claude |
|------|-------|--------|
| Physical assembly | ✓ | Assembly guides, step-by-step instructions |
| Soldering/wiring | ✓ | Wiring diagrams, pinout documentation |
| Testing/debugging | ✓ (hands-on) | Interpret results, suggest fixes, debug code |
| Sourcing parts | ✓ (purchasing) | Research, BOMs with links, ecosystem analysis |
| Pneumatic fitting/sealing | ✓ | Document techniques, design printable solutions |
| 3D printing | ✓ (run printer) | OpenSCAD/CAD designs, STL generation |
| Arduino firmware | Flashing | Full code development |
| Unreal Engine | Editor work, placement | Blueprint logic, C++, HTTP integration, guides |
| 3D modeling | Complex organic sculpting | Procedural generation scripts, Blender Python |
| Documentation | Review/feedback | Write and maintain all docs |

### Session Rhythm

- **Weekends only** (IP considerations with day job)
- **Goal**: Make meaningful progress each week
- **Approach**: Document thoroughly so context isn't lost between sessions

### Context File Discipline

Before each session:
1. Claude reads relevant context files
2. Human provides any updates since last session
3. Agree on scope for the session

After each session:
1. Update context files with decisions made
2. Document any new learnings
3. List clear next steps

---

## Parts Research: VR Smell v2

### Goals for Parts Selection

1. **Minimize manual fabrication** - No drilling holes, sanding edges, improvised seals
2. **Fail-fast approach** - Test small before committing to full build
3. **Time over cost** - Willing to pay more for parts that "just work"
4. **Leverage 3D printer** - Custom chambers, mounts, enclosures

### Key Decision: Tubing Ecosystem

The tubing choice cascades into everything else (valves, fittings, chambers). Need to pick an ecosystem, not just a tube.

**Candidates to research:**

| Ecosystem | Tubing | Pros | Cons | Research needed |
|-----------|--------|------|------|-----------------|
| 6mm OD metric | Polyurethane | Industry standard, huge selection of push-to-connect fittings | May need metric sources | Valve availability, fitting costs |
| 4mm OD metric | Polyurethane | Smaller, lighter | May be too restrictive for airflow? | Flow rate adequacy |
| 1/4" OD imperial | Polyurethane or silicone | Common in US, familiar | Fewer push-to-connect options? | Fitting ecosystem comparison |

**Key insight**: Pneumatic fittings spec by **OD (outer diameter)**, not ID. Push-to-connect fittings grip the outside of the tube.

### Fitting Strategy: Push-to-Connect

**Current state (v1):** Barbed fittings, hose clamps, friction fits → Leak-prone, labor intensive

**Target state (v2):** Push-to-connect everywhere → Click in, airtight, removable, no tools

Research needed:
- [ ] Push-to-connect fittings for chosen tubing size
- [ ] Bulkhead push-to-connect fittings (for chamber walls)
- [ ] T-splitters and manifolds with push-to-connect
- [ ] Panel-mount options

### Valve Selection Criteria

Current: 12V solenoid valves (working fine electrically)

Questions for v2:
- [ ] Are there valves with native push-to-connect ports?
- [ ] Manifold-style valve banks (multiple valves, one unit)?
- [ ] Response time requirements (PWM for future magnitude control?)
- [ ] Noise level?

### Smell Chamber Design (3D Printed)

**Concept:** Replace drilled plastic containers with purpose-built printed chambers

Design requirements:
- Integrated push-to-connect bulkhead ports (or standard bulkhead threads)
- O-ring grooves for airtight seal on lid
- Easy to open for reloading scent medium
- Parametric (adjustable size for different scent volumes)
- Print-in-place or minimal assembly

**Tool:** OpenSCAD (code-based, parametric, version-controllable)

### Current Pump Specifications

**SparkFun Vacuum Pump - 12V (ROB-10398)**

| Spec | Value | Notes |
|------|-------|-------|
| Type | Diaphragm | Oil-free, good for intermittent use |
| Voltage | 12V | |
| Power | 12W | |
| Flow Rate | 9-15 LPM | Sufficient for 6-20 channels |
| Vacuum | >350 mmHg | |
| Working Pressure | -70 to 220 kPa | |
| Noise | 65 dB | Conversation-level; may be issue for wearable |
| Lifespan | 1000 hours | ~500 days at 2hr/day |
| Ports | 1/4" barbs | Suggests 1/4" tubing ecosystem |
| Price | $25.50 | |

**Assessment:**
- Flow rate adequate for current channel targets (6 → 20)
- Noise is a concern for wearable form factor—may need quieter alternative
- 1/4" barbs suggest staying in 1/4" tubing ecosystem
- Lifespan acceptable for prototyping; production may need upgrade

---

## Scaling Challenges

### Latency Scaling Problem

As channel count increases, **scent delivery latency increases** due to longer air paths.

**The physics:**
- Air travels: pump → valve → chamber → merge manifold → output tube → nose
- More channels = more branching, longer/more complex merge points
- Total path length scales with number of channels

**Scaling behavior:**
- Best case (perfect binary tree): O(log n)
- Realistic physical layouts: likely O(√n) or worse
- Linear manifold (chambers in a row): O(n) for furthest chamber

**Why it matters:**
- If latency grows significantly, user sees the campfire before smelling it
- Sensory mismatch breaks immersion
- Violates "transitions are the magic" principle—timing is critical

**Example concern:**
- At 6 channels: ~200ms latency (acceptable)
- At 100 channels: ~800ms+ latency (immersion-breaking?)
- These numbers are hypothetical—actual measurement needed

**Potential mitigations:**
1. **Higher flow rate / pressure** - Faster transit, but more noise and power
2. **Shorter, fatter tubing** - Less resistance
3. **Decentralized architecture** - Multiple small manifolds (e.g., 10 channels each) instead of one large one
4. **Pre-pressurized chambers** - "Spritz" approach: chamber holds pressurized scented air, valve releases burst
5. **Predictive triggering** - Software fires scent slightly *before* user reaches zone (based on trajectory)
6. **Parallel output paths** - Multiple tubes to nose area, each serving a subset of scents

**Research needed:**
- [ ] Measure actual latency on current 3-channel prototype
- [ ] Model latency vs. channel count for different manifold geometries
- [ ] Investigate "spritz" vs. continuous flow architectures

---

### Future Concept: Double-Buffered Pneumatics

> *"Like double-buffering for scented air—prepare invisibly, swap instantly."*

**The problem:** As channel count grows, latency from valve-on to scent-at-nose increases due to longer air paths.

**The concept:** Two parallel outlet paths with a branch point near the nose:

```
                                         ┌─→ [NOSE outlet]
[Pump] → [Valves] → [Chambers] → ────────┤
                                         └─→ [DUMP outlet] (away from nose)
```

**Delivery sequence:**
1. **Preload phase** (predictive, as user approaches zone):
   - High-pressure burst through target scent chamber
   - Routes to DUMP outlet (vents away from user)
   - Scented air fills tubing up to branch point
   - User perceives nothing

2. **Delivery phase** (when user enters zone):
   - Switch to NOSE outlet at normal pressure
   - Scented air already staged inches from nose
   - Perceived latency: near-zero

**Key design points:**
- Dump outlet must vent *away* from nose (not just filtered)
- Dump tubing can loop back and exit anywhere (e.g., near pump/valve assembly)
- Dump output must NOT feed back into pump inlet (no scent recycling)
- High-pressure preload is brief and within safe ranges
- Enables predictive triggering based on user trajectory

**Analogy:** Like UI double-buffering, where the next frame is rendered off-screen and swapped in when ready. Here, the next scent is "rendered" (staged in tubing) off-nose and "swapped in" (outlet switch) when needed.

**Benefits:**
- Decouples transport time from delivery time
- Works regardless of manifold complexity
- Preload can happen during user's approach (seconds of lead time)
- Maintains immersion even at high channel counts

**Status:** Future R&D concept—not for initial prototype

### Open Questions for Research Session

1. **Airflow requirements**: How much CFM/LPM actually needed to deliver scent? Does current pump provide enough for 6-10 channels?
   - *Action*: Measure current pump output using bag displacement method (see below)

2. **Channel count**: Design for how many scents?
   - **ANSWERED**: Start with 6 channels for prototype
   - Scale to 20 ± for early post-prototype versions
   - Long-term vision: 100+ channels ("scent printer" capability)
   - *Implication*: Need modular, compact solutions. Valve manifolds preferred over individual valves. Size-per-channel is critical metric.

3. **Form factor**: 
   - **ANSWERED**: Wearable preferred
   - Stationary creates tethering, tangle, latency, and presence-breaking hose
   - Wearable challenges: weight, noise near ears, heat, refilling
   - *Possible stepping stone*: Pump mounted on walking pad frame with short coiled hose

4. **Sourcing preferences**:
   - Amazon (fast, easy returns)?
   - McMaster-Carr (industrial quality, good specs)?
   - AliExpress (cheap, slow)?
   - Specialty pneumatic suppliers?

### How to Measure Pump Output (LPM)

Simple bag displacement method:
1. Get a plastic bag (gallon ziploc ≈ 3.8 liters) or balloon
2. Empty it completely
3. Connect pump output to bag opening
4. Time how long to fill
5. Calculate: `Volume (L) / Time (seconds) × 60 = LPM`

Example: 3.8L bag fills in 10 seconds → 3.8/10 × 60 = **22.8 LPM**

For more precision: submerge tube in water bucket, measure displaced water volume over time.

---

## Next Steps

### For Next Session (Parts Research)

- [ ] Claude: Web search for push-to-connect pneumatic ecosystems
- [ ] Claude: Find example valve options with native push-to-connect
- [ ] Claude: Research small pneumatic manifolds
- [ ] Claude: Draft initial BOM for "test kit" (~$30-50 to validate ecosystem)
- [ ] Claude: Design parametric smell chamber in OpenSCAD (ready to print once tubing size decided)
- [ ] Claude: Research quieter pump alternatives for wearable form factor

### For Human (Between Sessions, If Time)

- [ ] Measure current pump output (LPM) using bag method—see instructions above
- [ ] Measure scent latency on current prototype (time from valve-on to smell-at-nose)
- [ ] Consider sourcing preferences (Amazon vs McMaster-Carr vs AliExpress)

---

## Future Collaborators

*Section reserved for when neighbor joins the project*

---

*See also: [002-vr-smell-system.md](002-vr-smell-system.md) for v1 prototype details*
