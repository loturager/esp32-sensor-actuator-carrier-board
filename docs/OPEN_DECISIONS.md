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

**Status:** Frozen

### Decision

Use a 2-pin terminal block / KRE connector with 5.08 mm pitch for the external load output.

### Frozen RevA Definition

- Connector type: terminal block / KRE.
- Number of pins: 2.
- Pitch: 5.08 mm.
- Intended use: external load connection.
- Electrical target: up to 12 V / 1 A.
- Required current rating: at least 1 A.
- Preferred current rating: above 1 A for margin.
- Footprint strategy: use a common 2-pin 5.08 mm terminal block footprint or compatible equivalent.

### Pinout

```text
1 — LOAD+
2 — LOAD_SW
```

### Net Meaning

- `LOAD+` is connected to the protected VIN/load supply path.
- `LOAD_SW` is the switched low-side node connected to the MOSFET drain.

### Silkscreen Recommendation

The connector may be labeled on silkscreen as:

```text
LOAD+
LOAD-
```

However, the schematic net name for the switched node should preferably be `LOAD_SW` to make the MOSFET switching function clear.

### Justification

A 2-pin 5.08 mm terminal block is common, inexpensive, robust, easy to assemble and suitable for external wiring in simple industrial/IoT prototyping applications.

This connector style is appropriate for the RevA target load of up to 12 V / 1 A and keeps the board easy to understand, test and document.

### Impact

This decision does not depend on the exact ESP32 DevKit model, MOSFET GPIO or MOSFET part number.

### Owner

Governance / hardware architecture.

---

## O5 — MOSFET Output LED Population Strategy

**Status:** Frozen

### Decision

Include a MOSFET output indicator LED footprint as optional / DNP.

### Frozen RevA Definition

- Output LED footprint: included.
- Default population status: DNP.
- Required for electrical operation: no.
- Purpose: debug, validation and visual indication of output activity.
- BOM status: listed as DNP unless intentionally populated.
- Final connection point: to be defined during schematic implementation so that it does not interfere with the load behavior.

### Justification

The MOSFET output LED improves testability and makes bench validation easier, especially when demonstrating the board as a portfolio project.

Keeping the LED as optional/DNP avoids forcing extra current consumption or unintended interaction with the external load path.

This gives RevA a professional debug feature without making it mandatory for the core electrical function.

### Impact

This decision does not block schematic planning.

During schematic capture, the LED circuit must be implemented so that:

- It does not affect MOSFET gate behavior.
- It does not significantly load the switched output node.
- It is clearly marked as optional/DNP.
- It is documented in the BOM as DNP by default.

### Owner

Governance / hardware architecture.

---

## Summary Table

| ID | Decision | Status | Current Direction |
|---|---|---|---|
| O1 | Exact ESP32 DevKit 30-pin reference | Open | Select common 30-pin DevKit reference |
| O2 | GPIO for MOSFET gate control | Open | Select safe output GPIO |
| O3 | Logic-level N-channel MOSFET part number | Open | Select MOSFET with low RDS(on) at 3.3 V |
| O4 | Load connector type | Frozen | 2-pin 5.08 mm terminal block / KRE |
| O5 | MOSFET output LED strategy | Frozen | Optional/DNP footprint |

---

## Architecture Freeze Requirement

Architecture RevA v0.1 should not be marked as Frozen until:

- O1 is resolved or approved for draft schematic.
- O2 is resolved or approved for draft schematic.
- O3 is resolved or approved for draft schematic.
- O4 is frozen. ✅
- O5 is frozen. ✅

Current freeze progress:

```text
O4 — Frozen
O5 — Frozen
O1 — Open
O2 — Open
O3 — Open
```

Current architecture state:

```text
Architecture RevA v0.1 — Ready for Freeze Review
KiCad schematic — Not released
```