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

## O1 — ESP32 DevKit 30-pin Footprint Strategy

**Status:** Frozen

### Decision

Use a generic 30-pin ESP32 DevKit footprint strategy with documented compatibility limitation.

### Frozen RevA Definition

- Target module type: ESP32 DevKit-style development board.
- Pin count: 30 pins.
- Header strategy: two 1x15 female headers.
- Compatibility scope: limited to DevKit boards matching the selected 30-pin header spacing, pinout and mechanical envelope.
- 30-pin and 38-pin DevKit boards will not be supported on the same RevA PCB.
- Silkscreen must clearly indicate module orientation.
- A visible antenna keepout region should be marked whenever possible.
- Exact module mechanical dimensions and pinout must be confirmed before fabrication.

### Fabrication Warning

Client must confirm the exact ESP32 DevKit mechanical dimensions and pinout before fabrication.

### Justification

Many low-cost ESP32 DevKit boards exist in the market, and they may differ in width, header spacing, pin naming, USB connector position, antenna area and mechanical envelope.

A generic 30-pin DevKit footprint strategy is appropriate for RevA because it keeps the design realistic for freelance-style projects while making the compatibility limitation explicit.

This avoids overpromising universal support and keeps the board scope controlled.

### Impact

This decision allows the architecture to progress while keeping fabrication responsibility clear.

It does not change the schematic, layout or fabrication release status by itself.

### Owner

Governance / hardware architecture.

---

## O2 — MOSFET Gate GPIO

**Status:** Frozen

### Decision

Use ESP32 `GPIO26` as the MOSFET gate control signal for RevA.

### Frozen RevA Definition

- Selected GPIO: `GPIO26`
- Gate net name: `MOSFET_GATE`
- Gate series resistor: `100 Ω`
- Gate pulldown resistor: `100 kΩ`
- Default safe state: MOSFET OFF during boot/reset
- Firmware shall configure GPIO26 as output LOW during initialization before enabling load control.

### Gate Drive Interface

```text
GPIO26 ── 100 Ω ── MOSFET_GATE
                    |
                  100 kΩ
                    |
                   GND
```

### Rationale

GPIO26 is a general-purpose output-capable ESP32 GPIO suitable for driving a low-side N-channel MOSFET gate through a series resistor.

It is not one of the typical ESP32 boot/strapping pins, is not input-only and is not used as UART0 programming/debug by default.

### Pins Intentionally Avoided

- GPIO0, GPIO2, GPIO5, GPIO12, GPIO15: boot/strapping-sensitive pins.
- GPIO34, GPIO35, GPIO36, GPIO39: input-only pins.
- GPIO1, GPIO3: UART0 programming/debug pins.
- GPIO6, GPIO7, GPIO8, GPIO9, GPIO10, GPIO11: usually connected to SPI flash and not suitable for general GPIO use.
- GPIO16, GPIO17: avoid unless the selected module confirms they are free, because some modules may use them for PSRAM or internal functions.
- GPIO4: not selected for RevA; kept available/reserved to avoid unnecessary board-variant ambiguity.

### Compatibility Note

For the generic 30-pin ESP32 DevKit strategy, the exact DevKit pinout must still be confirmed before fabrication.

Client must confirm the exact ESP32 DevKit mechanical dimensions and pinout before fabrication.

### Owner

Governance / embedded hardware.

---

## O3 — Logic-Level N-Channel MOSFET Part Number

**Status:** Frozen

### Decision

Use `AO3400A` as the primary N-channel MOSFET for the RevA low-side output stage.

### Frozen RevA Definition

- Reference designator: `Q1`
- Primary part number: `AO3400A`
- Type: N-channel MOSFET
- Package: SOT-23
- Function: low-side load switch
- Load target: up to 12 V / 1 A
- Gate drive: ESP32 `GPIO26` at 3.3 V
- Gate net: `MOSFET_GATE`
- Switched load net: `LOAD_SW`

### Equivalent Part Requirement

An equivalent MOSFET may be used only if it satisfies:

- N-channel MOSFET.
- `VDS ≥ 30 V`.
- `RDS(on)` specified at `VGS ≤ 2.5 V`.
- Suitable thermal behavior for 1 A load switching.
- Package suitable for RevA assembly and PCB layout.

Preferred wording:

```text
AO3400A or equivalent N-channel MOSFET with VDS ≥ 30 V and RDS(on) specified at VGS ≤ 2.5 V.
```

### Power Dissipation Check

Using the AO3400A worst-case `RDS(on)` at low gate voltage:

```text
P = I² · R
P = 1² · 0.048
P = 48 mW
```

This dissipation is low for the target RevA load of 1 A, assuming reasonable copper area around the MOSFET power pins and operation within the documented load limit.

### Required Support Components

The MOSFET stage shall include:

- `100 Ω` series resistor between GPIO26 and `MOSFET_GATE`.
- `100 kΩ` pulldown resistor from `MOSFET_GATE` to GND.
- Flyback diode for inductive loads.
- Optional/DNP MOSFET output LED footprint.

### Rationale

The AO3400A is appropriate for RevA because it is a compact SOT-23 N-channel MOSFET with sufficient voltage margin for a 12 V load and low `RDS(on)` specified at low gate voltage.

This makes it suitable for direct 3.3 V GPIO-controlled low-side switching in the ESP32 carrier board.

### Avoided Parts

The following MOSFETs are not recommended for this RevA output stage:

- `IRFZ44N`
- `IRF540N`
- `2N7002`
- `BSS138`
- `BS170`

Reason:

These parts are either oversized/older devices intended for higher gate drive, or too small for a professional 1 A load-switching output with comfortable margin.

### Owner

Governance / component selection.

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
| O1 | ESP32 DevKit 30-pin footprint strategy | Frozen | Generic 30-pin DevKit footprint with documented compatibility limitation |
| O2 | MOSFET gate GPIO | Frozen | GPIO26 with 100 Ω gate resistor and 100 kΩ pulldown |
| O3 | Logic-level N-channel MOSFET part number | Frozen | AO3400A or equivalent, VDS ≥ 30 V, RDS(on) specified at VGS ≤ 2.5 V |
| O4 | Load connector type | Frozen | 2-pin 5.08 mm terminal block / KRE |
| O5 | MOSFET output LED strategy | Frozen | Optional/DNP footprint |

---

## Architecture Freeze Requirement

Architecture RevA v0.1 may be marked as Frozen because all tracked RevA open decisions have been resolved.

Current freeze progress:

```text
O1 — Frozen
O2 — Frozen
O3 — Frozen
O4 — Frozen
O5 — Frozen
```

Current architecture state:

```text
Architecture RevA v0.1 — Frozen
KiCad schematic — Released for Draft Schematic
PCB layout — Not released
Fabrication — Not released
```

### Schematic Release Note

The KiCad schematic may now begin as a draft schematic.

Fabrication is still not released.

PCB layout is still not released.

Before fabrication, the project must still complete:

- Full schematic capture.
- ERC review.
- Footprint assignment review.
- ESP32 DevKit mechanical and pinout confirmation.
- PCB layout.
- DRC review.
- Gerber review.