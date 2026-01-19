# Project Overview - VR Immersion System

> **Context file for AI-assisted development sessions**
> Last updated: January 2025

## Grand Vision

Create hardware enabling a totally immersive VR experience. The user puts on a headset and is transported to a far-away island with multiple explorable environments:

### The Island Environments

**Cave Entrance (Starting Point)**
- User opens their eyes at cave mouth
- Sunlight visible ahead
- Walking toward light triggers transition to beach

**Beach/Ocean Area**
- Smell: Ocean/salt air
- Feel: Warmth of sun on skin, wind in hair
- Sound: Waves, seabirds catching fish
- Hub connecting to other areas

**Volcanic Caldera** (redesigned from "climb the volcano")
- Flat basin surrounded by ridges (avoids gradient problem)
- Smell: Sulfur from vents
- Feel: Radiant heat from ground/lava pools
- Visual: Orange glow from lava at edges

**Cave Interior**
- Smell: Damp stone, earth
- Feel: Cool air, no wind, no sun warmth
- Sound: Echoes, dripping water
- Transition from warm→cool is key sensory moment

**Forest**
- Smell: Woody aroma, flowers when approached
- Feel: Filtered sun (less warmth), reduced wind
- Hazard: Purple mushrooms (gameplay element TBD)

**Dock**
- Physical bench facing east (orientation contract)
- Kayak boarding from west (orientation contract)
- Enables seated experiences with physical props

**Abandoned Temple**
- TBD - not yet designed

### Kayak Experience

Physical hardware solution:
- All kayaks board from the west in VR world
- Physical kayak seat positioned at known location relative to walking pad
- Magnetic resistance paddles (rowing machine concept) drop down or are grabbed from dock
- Solves "infinite walking space" problem by transitioning to seated experience

## Core Design Principles

### 1. Transitions Create Immersion

The magic is not in maintaining a steady state, but in the **sensory shift** between environments:
- Bright/warm/ocean-smell → dark/cool/damp-smell
- The user's brain fills in the rest when transitions are convincing
- A 10-20 second transition experience is the tractable problem, not whole-room climate control

### 2. Orientation Contracts

Design the VR world with constraints that align physical hardware:
- All benches face east → physical bench faces east
- All kayaks board from west → physical seat at known position
- User perceives design choice ("nice sunrise view"), not constraint

### 3. Minimal Gradient Design

**The unsolved problem:** Simulating walking uphill/downhill without tilting platforms.

**The solution:** Design the VR world to be mostly flat:
- Volcanic caldera (walk into, not up)
- Cave entrance is gentle threshold, not steep ramp
- Journey comes from atmosphere changes, not elevation
- Small visual gradients acceptable; physical gradient not simulated

### 4. Micro-Climate Zones

Temperature control is tractable when scoped correctly:
- User occupies ~2m × 2m × 2m volume
- Displacement ventilation replaces air mass gently (not blasting wind)
- Pre-conditioned air sources (cool reservoir, heated ducts)
- Gravity-assisted cooling from above (cold air descends naturally)
- Radiant infrared for sun/volcano warmth (heats skin, not air)

## Known Technical Challenges

### Solved / In Progress
- [x] Scent delivery (VR Smell prototype working)
- [ ] Multi-scent without cross-contamination (partially solved with unscented purge path)
- [ ] Wind simulation (fans, straightforward)
- [ ] Radiant heat (IR panels, straightforward)

### Future R&D
- [ ] Scent magnitude control (PWM? proportional valves? multiple chambers?)
- [ ] Cooling for cave environment (displacement ventilation + pre-cooled air)
- [ ] Kayak physical handoff from walking pad
- [ ] Paddle resistance mechanism

### Accepted Limitations
- No gradient simulation (design around it)
- User is a "ghost" (hands pass through objects)
- No swimming simulation
- No water/splash sensation (for now)

## Origin Story

> "Took a friend to LA. Wanted to try VR while there... but all of the VR was 'Fight the dragon' or 'Kill the zombies'. Basically things dudes would be interested in. Didn't think my friend wanted to do a 6h flight only to fight the dragon for 30m after."

This led to pursuing VR experiences focused on **presence and immersion** rather than combat/action—maybe a normal but different world, maybe a fantastical mind-bending world. That's up to the world-maker.

---

*See also: [002-vr-smell-system.md](002-vr-smell-system.md) for current prototype details*
