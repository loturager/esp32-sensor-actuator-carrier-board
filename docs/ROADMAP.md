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

**Status:** In progress / Ready for Freeze Review after cleanup

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
- Architecture status changes from Draft / Ready for Freeze Review to Frozen.

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
- Test I2C, UART and GPIO connectors.
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
```

The next technical milestone is:

```text
Architecture RevA v0.1 Freeze
```

The next GitHub milestone is:

```text
RevA Bootstrap
```