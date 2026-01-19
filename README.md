# VR Immersion Project

A hardware/software system for creating fully immersive virtual reality experiences through multi-sensory stimulation: smell, temperature, wind, and physical interactions.

## Vision

Transport users to a virtual island where they can explore diverse environments—a sea cave, volcanic caldera, forest, dock, and abandoned temple—with each location delivering distinct sensory experiences that go beyond visuals and audio.

## Project Status

**Current Phase:** VR Smell prototype (functional)

## Repository Structure

```
vr-immersion/
├── docs/
│   └── context/          # AI-first context files for development sessions
├── firmware/             # Arduino/embedded code
├── unreal/               # Unreal Engine project/plugins
├── hardware/
│   ├── schematics/       # Electrical diagrams
│   ├── bom/              # Bills of materials
│   └── cad/              # 3D models, enclosure designs
└── assets/
    └── images/           # Reference photos, diagrams
```

## Documentation

- [Project Context](docs/context/001-project-overview.md) - Grand vision, design principles, current state
- [VR Smell System](docs/context/002-vr-smell-system.md) - Current prototype details

## Core Design Principles

1. **Transitions matter most** - The sensory shift between environments creates immersion
2. **Orientation contracts** - Design the virtual world to align with fixed physical hardware
3. **Minimal gradient** - Accept flat physical space; create journey through atmosphere

## Technology Stack

- **VR Hardware:** Meta Quest 3
- **Game Engine:** Unreal Engine
- **Microcontroller:** Arduino with WiFi
- **Communication:** HTTP REST API (evaluating serial alternatives)

## License

TBD
