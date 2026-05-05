# ESP32 Sensor/Actuator Carrier Board

A professionally documented KiCad PCB project for an ESP32 DevKit 30-pin module, designed for sensor interfacing, serial communication, external power input and low-side actuator control.

This project demonstrates a complete early-stage hardware design workflow, including requirements control, block architecture, open decision tracking, documentation structure and preparation for schematic capture.

---

## Overview

The ESP32 Sensor/Actuator Carrier Board is a two-layer PCB intended to support ESP32-based prototyping in light industrial, IoT and embedded control applications.

The board provides a reusable hardware platform for connecting sensors, serial interfaces and small actuator loads to an ESP32 DevKit module.

The design includes:

- External VIN input from 7 V to 12 V.
- Basic input protection.
- 5 V generation using an adjustable buck converter stage.
- 3.3 V logic rail supplied by the ESP32 DevKit.
- I2C, UART and GPIO expansion connectors.
- One low-side N-channel MOSFET output for external load control.
- Power indication LED.
- Test points for measurement and debugging.
- Two-layer PCB structure with top and bottom GND planes.

---

## Project Goals

The main goals of this project are:

- Create a clean and manufacturable ESP32 carrier board.
- Practice professional PCB project organization using KiCad.
- Document the design process in a way that is understandable to clients, reviewers and other engineers.
- Demonstrate requirements control before schematic capture.
- Track open technical decisions instead of hiding uncertainty.
- Prepare a portfolio-quality hardware project suitable for freelance PCB design presentation.

---

## Key Features

- ESP32 DevKit 30-pin carrier board.
- 7 V to 12 V external DC input.
- Protected VIN rail.
- Adjustable buck converter interface for 5 V generation.
- 3.3 V logic distribution from the ESP32 DevKit.
- I2C connector for sensor modules.
- UART connector for serial communication/debugging.
- GPIO expansion connector.
- Low-side MOSFET output for loads up to 12 V / 1 A.
- Flyback diode support for inductive loads.
- Power LED.
- Optional MOSFET output LED footprint.
- Test points for VIN, 5 V, 3V3, GND, MOSFET gate and load node.
- Four mounting holes.
- Two-layer PCB with GND planes on both layers.

---

## Hardware Architecture

The board follows this simplified power and control flow:

```text
External VIN 7–12 V
        |
        v
Input Protection
        |
        +----------------------+
        |                      |
        v                      v
Buck Converter             Load Supply Path
        |                      |
        v                      v
5 V Rail                 External Load
        |                      |
        v                      v
ESP32 DevKit        Low-side N-MOSFET Output
        |
        +--> 3V3 Rail
        +--> I2C Connector
        +--> UART Connector
        +--> GPIO Connector
        +--> MOSFET Gate Control

The architecture is currently documented before schematic capture to keep the design controlled, reviewable and traceable.

Documentation

Main documentation files:

docs/ARCHITECTURE_RevA_v0.1.md
 — Block architecture draft.
docs/ROADMAP.md
 — Project development roadmap.
docs/OPEN_DECISIONS.md
 — Open technical decisions for RevA.
Current Status
Item	Status
Requirements RevA v0.1	Frozen
Block Architecture RevA v0.1	Draft
Portfolio README	In progress
Open decisions tracking	In progress
KiCad schematic	Not started
PCB layout	Not started
Fabrication outputs	Not started
Validation	Not started

The project is currently in the architecture and documentation phase. KiCad schematic capture will begin after the main RevA open decisions are resolved or explicitly approved for draft schematic work.

Open Decisions

The main open decisions before architecture freeze are:

O1 — Exact ESP32 DevKit 30-pin reference.
O2 — GPIO selection for MOSFET gate control.
O3 — Logic-level N-channel MOSFET part number.
O4 — Load connector type.
O5 — MOSFET output LED population strategy.

These decisions are tracked in:

docs/OPEN_DECISIONS.md
Skills Demonstrated

This project demonstrates:

PCB project organization.
Requirements-to-architecture workflow.
Hardware block definition.
Power distribution planning.
ESP32-based embedded hardware design.
Low-side MOSFET load switching.
Basic input protection planning.
Connector and interface planning.
Test point planning.
KiCad-oriented documentation structure.
Git and GitHub project workflow.
Engineering decision tracking.
Portfolio-oriented technical communication.
Repository Structure
.
├── README.md
├── docs/
│   ├── ARCHITECTURE_RevA_v0.1.md
│   ├── ROADMAP.md
│   └── OPEN_DECISIONS.md
├── hardware/
│   └── kicad/
├── firmware/
├── software/
├── production/
└── validation/
Next Steps

Planned next steps:

Resolve the exact ESP32 DevKit 30-pin reference.
Select a safe GPIO for MOSFET gate control.
Select a candidate 3.3 V logic-level N-channel MOSFET.
Confirm the load connector type.
Define the MOSFET output LED population strategy.
Freeze the RevA block architecture.
Start KiCad schematic capture.
Perform schematic review.
Start PCB layout.
Generate fabrication outputs.
Document assembly and validation results.
Design Intent

This project is not intended to be a certified industrial controller.

It is a professionally documented RevA prototype carrier board intended to demonstrate practical PCB architecture, embedded hardware design workflow, KiCad project organization and design-for-manufacturing awareness.

The design focuses on realistic constraints commonly found in freelance PCB projects: limited board complexity, clear interfaces, low-cost assembly, basic protection, testability and clean documentation.


Salve e feche.

---

# 2. docs/ROADMAP.md

Abra:

```bash
notepad docs/ROADMAP.md

Cole:

# Project Roadmap

## Project: ESP32 Sensor/Actuator Carrier Board

**Revision:** RevA  
**Current stage:** Architecture and portfolio documentation  
**Goal:** Create a professional KiCad-based PCB portfolio project with clear documentation, controlled scope and realistic hardware design decisions.

---

## Phase 0 — Requirements

**Status:** Completed / Frozen

### Goals

- Define the purpose of the board.
- Define the main functional requirements.
- Establish the RevA scope.
- Avoid unnecessary feature creep before architecture.

### Outputs

- Requirements RevA v0.1 frozen.
- Initial project scope defined.
- Main board features selected.

### Result

The project scope is clear enough to start block architecture.

---

## Phase 1 — Block Architecture

**Status:** In progress

### Goals

- Define the main hardware blocks.
- Define power flow.
- Define interfaces between blocks.
- Identify open technical decisions.
- Prepare the project for schematic capture.

### Outputs

- `ARCHITECTURE_RevA_v0.1.md`
- `OPEN_DECISIONS.md`
- Updated README with architecture summary.

### Exit Criteria

This phase is complete when:

- ESP32 DevKit reference is selected or approved for draft schematic.
- MOSFET gate GPIO is selected.
- Candidate MOSFET is selected.
- Load connector is confirmed.
- MOSFET output LED strategy is defined.
- Architecture status changes from Draft to Frozen.

---

## Phase 2 — Schematic Capture

**Status:** Not started

### Goals

- Create the electrical schematic in KiCad.
- Implement VIN input and protection.
- Implement buck converter interface.
- Add ESP32 DevKit headers.
- Add I2C, UART and GPIO connectors.
- Add low-side MOSFET output.
- Add LEDs and test points.

### Outputs

- KiCad schematic files.
- Initial ERC report.
- Schematic review notes.

### Exit Criteria

This phase is complete when:

- ERC issues are resolved or justified.
- Power nets are clearly named.
- Connectors are correctly labeled.
- MOSFET output topology is verified.
- Test points are included.
- Schematic is approved for PCB layout.

---

## Phase 3 — PCB Layout

**Status:** Not started

### Goals

- Create a manufacturable two-layer PCB layout.
- Place connectors and mounting holes logically.
- Route power and signal nets.
- Add GND planes on both layers.
- Keep load current paths controlled.
- Maintain clear silkscreen labeling.

### Outputs

- KiCad PCB layout.
- DRC report.
- PCB screenshots for review.
- Layout review notes.

### Exit Criteria

This phase is complete when:

- DRC issues are resolved or justified.
- Critical power paths are reviewed.
- GND strategy is reviewed.
- Connector placement is acceptable.
- Silkscreen is clear.
- Mounting holes are correctly placed.

---

## Phase 4 — Fabrication Outputs

**Status:** Not started

### Goals

- Generate manufacturing files.
- Prepare outputs suitable for PCB fabrication.
- Organize production files clearly.

### Outputs

- Gerber files.
- Drill files.
- BOM.
- Position files, if applicable.
- Fabrication notes.

### Exit Criteria

This phase is complete when:

- Fabrication outputs are generated.
- Files are organized under `production/`.
- Gerber viewer inspection is completed.
- Board dimensions and layers are verified.

---

## Phase 5 — Assembly

**Status:** Not started

### Goals

- Assemble the prototype board.
- Verify component placement.
- Check soldering and mechanical fit.
- Prepare the board for electrical validation.

### Outputs

- Assembly notes.
- Photos of assembled board.
- Known assembly issues, if any.

### Exit Criteria

This phase is complete when:

- Board is visually inspected.
- Power input is checked before inserting the ESP32.
- Buck output is adjusted to 5 V.
- No obvious assembly faults remain.

---

## Phase 6 — Validation

**Status:** Not started

### Goals

- Validate the main electrical functions.
- Confirm power rails.
- Test ESP32 power-up.
- Test I2C/UART/GPIO connectors.
- Test MOSFET output switching.
- Document measurement results.

### Outputs

- Validation checklist.
- Measurement results.
- Test photos or screenshots.
- Known limitations.

### Exit Criteria

This phase is complete when:

- VIN protection path is verified.
- 5 V rail is verified.
- 3V3 rail is verified.
- ESP32 powers correctly.
- MOSFET output switches correctly.
- Test points are usable.
- Any limitations are documented.

---

## Phase 7 — Portfolio Release

**Status:** Not started

### Goals

- Prepare the project for public portfolio presentation.
- Improve README and documentation.
- Add images, diagrams and final notes.
- Present the project as a complete engineering workflow.

### Outputs

- Final README.
- Architecture documentation.
- KiCad screenshots.
- PCB renders.
- Fabrication outputs, if suitable for public release.
- Validation summary.

### Exit Criteria

This phase is complete when:

- The GitHub repository is clean and understandable.
- Documentation is consistent.
- Project status is clearly stated.
- Images and diagrams are included.
- The project can be shared with clients or recruiters.

---

## Current Roadmap Position

The project is currently between:

```text
Phase 1 — Block Architecture
Phase 7 — Early Portfolio Documentation

The next technical milestone is:

Architecture RevA v0.1 Freeze

The next GitHub milestone is:

RevA Bootstrap

Salve e feche.

---

# 3. docs/OPEN_DECISIONS.md

Abra:

```bash
notepad docs/OPEN_DECISIONS.md

Cole:

# Open Decisions — RevA

## Project: ESP32 Sensor/Actuator Carrier Board

This document tracks open technical decisions for RevA.

The goal is to keep uncertainty visible and controlled before freezing the architecture and starting KiCad schematic capture.

---

## Decision Status Legend

| Status | Meaning |
|---|---|
| Open | Decision still needs technical analysis. |
| Proposed | A candidate solution exists but is not approved yet. |
| Approved for Draft | Good enough to start schematic draft, but not fully frozen. |
| Frozen | Final decision for RevA unless a serious issue is found. |
| Deferred | Decision moved to a later revision or later project phase. |

---

## O1 — Exact ESP32 DevKit 30-pin Reference

**Status:** Open

### Question

Which exact ESP32 DevKit 30-pin board should be used as the mechanical and pinout reference for RevA?

### Why this matters

Different ESP32 DevKit boards may have different:

- Board width.
- Header spacing.
- Pin naming.
- 5 V input pin location.
- 3V3 pin location.
- GND pin locations.
- Boot/strapping pin exposure.
- USB connector overhang.
- Mechanical clearance requirements.

### Required Output

Before schematic and footprint finalization, document:

- Reference board name.
- Pin count.
- Header spacing.
- Board width.
- Pinout source.
- 5 V input pin.
- 3V3 output pin.
- GND pins.
- Pins to avoid for boot/strapping reasons.

### Current Direction

Use a common ESP32 DevKit 30-pin development board as the RevA reference.

### Owner

Technical decision / hardware architecture.

---

## O2 — GPIO Selection for MOSFET Gate Control

**Status:** Open

### Question

Which ESP32 GPIO should drive the low-side MOSFET gate?

### Why this matters

The MOSFET gate control pin must avoid unsafe ESP32 boot behavior.

Some ESP32 pins are related to:

- Boot mode selection.
- Strapping configuration.
- Flash interface.
- Input-only functionality.
- Boot-time logic states.

Choosing the wrong GPIO may cause:

- Boot failure.
- Unexpected load activation during reset.
- Unstable output behavior.
- Difficult debugging.

### Required Output

Before schematic finalization, document:

- Selected GPIO.
- Reason for selection.
- Pins intentionally avoided.
- Boot/reset behavior.
- Whether a gate pulldown is required.

### Current Direction

Use a safe output-capable GPIO that does not interfere with ESP32 boot/strapping behavior.

### Owner

Technical decision / embedded hardware.

---

## O3 — Logic-Level N-Channel MOSFET Part Number

**Status:** Open

### Question

Which N-channel MOSFET should be used for the low-side output stage?

### Requirements

The MOSFET should support:

- Load voltage up to 12 V.
- Load current up to 1 A.
- Gate drive from ESP32 GPIO at 3.3 V.
- Low enough RDS(on) at VGS = 2.5 V or 3.3 V.
- Package suitable for hand assembly or realistic PCB assembly.
- Availability from common suppliers.

### Why this matters

Not every MOSFET that is called “logic-level” works well at 3.3 V gate drive.

The selected MOSFET must switch reliably with an ESP32 output pin and avoid excessive heating at the target current.

### Required Output

Before schematic finalization, document:

- MOSFET part number.
- Package.
- VDS rating.
- Continuous drain current rating.
- RDS(on) at VGS = 2.5 V or 3.3 V.
- Thermal considerations for 1 A load.
- Availability.

### Current Direction

Select a common N-channel MOSFET with documented low RDS(on) at 2.5 V or 3.3 V gate drive.

### Owner

Technical decision / component selection.

---

## O4 — Load Connector Type

**Status:** Proposed

### Question

Which connector should be used for the external load output?

### Proposed Decision

Use a 2-pin terminal block with 5.08 mm pitch.

### Suggested Pinout

```text
1 — LOAD+
2 — LOAD-

Where:

LOAD+ is connected to the protected VIN/load supply path.
LOAD- is switched by the MOSFET drain.
Why this matters

The load connector must be suitable for repeated prototyping and external wiring.

A 5.08 mm terminal block is easy to use, easy to understand and suitable for low-voltage loads in a prototype board.

Required Output

To freeze this decision, confirm:

Connector type.
Pitch.
Pinout.
Current rating.
Footprint family.
Current Direction

2-pin 5.08 mm terminal block.

Owner

Governance / hardware architecture.

O5 — MOSFET Output LED Population Strategy

Status: Proposed

Question

Should the MOSFET output LED be populated by default or included only as an optional/DNP footprint?

Proposed Decision

Include the MOSFET output LED footprint as optional/DNP.

Why this matters

An output LED can help during debugging, but it should not interfere with the load behavior or make the output stage harder to interpret.

Keeping the LED as optional/DNP gives flexibility without forcing it into the RevA behavior.

Required Output

To freeze this decision, confirm:

LED included or removed.
Default population status.
Whether it appears in BOM as DNP.
Connection point.
Current Direction

Include footprint, default DNP.

Owner

Governance / hardware architecture.

Summary Table
ID	Decision	Status	Current Direction
O1	Exact ESP32 DevKit 30-pin reference	Open	Select common 30-pin DevKit reference
O2	GPIO for MOSFET gate control	Open	Select safe output GPIO
O3	Logic-level N-MOSFET part number	Open	Select MOSFET with low RDS(on) at 3.3 V
O4	Load connector type	Proposed	2-pin 5.08 mm terminal block
O5	MOSFET output LED strategy	Proposed	Optional/DNP footprint
Architecture Freeze Requirement

Architecture RevA v0.1 should not be marked as Frozen until:

O1 is resolved or approved for draft schematic.
O2 is resolved or approved for draft schematic.
O3 is resolved or approved for draft schematic.
O4 is frozen.
O5 is frozen.

Until then, the architecture remains:

Architecture RevA v0.1 — Draft