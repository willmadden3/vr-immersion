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

### Pump Considerations

Current: 12V air pump (adequate)

Questions:
- [ ] Noise level acceptable?
- [ ] Flow rate sufficient for multi-channel?
- [ ] Quieter alternatives?

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

### For Human (Between Sessions, If Time)

- [ ] Measure current pump output (LPM) using bag method—see instructions above
- [ ] Consider sourcing preferences (Amazon vs McMaster-Carr vs AliExpress)

---

## Future Collaborators

*Section reserved for when neighbor joins the project*

---

*See also: [002-vr-smell-system.md](002-vr-smell-system.md) for v1 prototype details*
