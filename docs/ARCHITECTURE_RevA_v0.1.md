# ARCHITECTURE_RevA_v0.1.md

## Project: ESP32 Sensor/Actuator Carrier Board

**Revision:** RevA v0.1  
**Status:** Frozen  
**Previous phase:** Requirements RevA v0.1 frozen  
**Current phase:** Architecture frozen / schematic draft released  
**Next phase:** KiCad schematic capture  

---

## 1. Board Overview

The ESP32 Sensor/Actuator Carrier Board is a two-layer PCB designed as a reusable hardware platform for ESP32-based sensing, communication and low-power actuator control applications.

The board targets light industrial, IoT prototyping and embedded control use cases where an ESP32 DevKit module needs external power input, regulated 5 V distribution, 3.3 V logic access, sensor connectors and a protected low-side output stage.

The board provides:

- External VIN input from 7 V to 12 V.
- Basic input protection.
- 5 V generation through a module-based adjustable buck converter stage.
- 3.3 V logic rail supplied by the ESP32 DevKit.
- I2C, UART and GPIO expansion connectors.
- One low-side N-channel MOSFET output for driving an external load up to 12 V / 1 A.
- Power indication LED.
- Test points for debugging, validation and measurement.
- Mounting holes for mechanical integration.
- Ground planes on both PCB layers.

This revision prioritizes clear architecture, manufacturability, safe prototyping practices and portfolio-quality documentation over maximum circuit complexity.

---

## 2. Textual Block Diagram

```text
[External VIN 7–12 V]
        |
        v
[Input Connector]
        |
        v
[Input Protection]
  - Fuse / PTC footprint
  - Reverse polarity protection
  - Input bulk capacitor
        |
        +------------------------------+
        |                              |
        v                              v
[5 V Buck Converter Block]       [Load Supply Path]
  Protected VIN -> 5 V             Protected VIN -> LOAD+
        |                              |
        v                              |
[5 V Rail]                            |
        |                              |
        v                              |
[ESP32 DevKit 30-pin]                 |
        |                              |
        +--> [3V3 Rail]                |
        |                              |
        +--> [I2C Connector]           |
        +--> [UART Connector]          |
        +--> [GPIO Connector]          |
        |                              |
        +--> [GPIO26 MOSFET Gate Control]
                       |              |
                       v              |
              [AO3400A Low-side MOSFET] <-----+
                       |
                       v
                     [GND]

Additional support blocks:

- Power LED
- Optional MOSFET output LED / DNP
- Test points
- Mounting holes
- Top and bottom GND planes
```

---

## 3. VIN Input Block

### Function

The VIN input block receives external DC power for the carrier board and for the load path.

### Requirements

- Input voltage range: 7 V to 12 V DC.
- Expected input current: up to 1 A.
- Connector must be suitable for repeated prototyping use.
- Input must feed both:
  - Buck converter input.
  - External load supply path.

### Initial Implementation Direction

Recommended connector:

- 2-pin terminal block.
- Suggested pitch: 5.08 mm.

Suggested pins:

```text
1 — VIN+
2 — GND
```

### Design Notes

VIN is not directly connected to ESP32 logic pins.

VIN is only used as the raw input supply for:

- The input protection stage.
- The buck converter.
- The external load path.

---

## 4. Input Protection Block

### Function

The input protection block protects the board against common user and prototyping mistakes.

### Protection Elements

The RevA protection block should include:

- Fuse or resettable PTC footprint.
- Reverse polarity protection.
- Input bulk capacitor.
- Ceramic decoupling capacitor near the buck module input.
- Clear polarity marking on silkscreen.

### Recommended Architecture

```text
VIN Connector
     |
     v
Fuse / PTC Footprint
     |
     v
Reverse Polarity Protection
     |
     v
Protected VIN Rail
```

### Design Notes

The exact reverse polarity method is still open and may be selected during schematic design.

Acceptable RevA options include:

- Series diode for simplicity.
- Schottky diode if voltage drop and thermal behavior remain acceptable.
- P-channel MOSFET ideal-diode style protection for lower voltage drop.

For RevA, the protection should be simple, understandable, easy to validate and easy to explain in portfolio documentation.

---

## 5. 5 V Buck Converter Block

### Function

The 5 V buck converter block generates a regulated 5 V rail from the protected VIN input.

### Requirements

- Input: protected VIN, 7 V to 12 V.
- Output: 5 V.
- Current capability: compatible with board requirements and ESP32 DevKit input needs.
- Implementation: module-based adjustable buck converter stage.

### Initial Implementation Direction

Use a ready-made adjustable buck module footprint or header interface.

The buck module should expose at least:

- VIN+
- VIN-
- VOUT+
- VOUT-

### Output Rail

The buck output creates the board-level 5 V rail.

```text
Protected VIN -> Buck Module -> +5V Rail
```

### Design Notes

The 5 V rail is intended to feed the ESP32 DevKit 5 V / VIN input pin, depending on the selected DevKit pinout.

The buck converter must be adjusted and verified to output 5 V before inserting the ESP32 DevKit.

A test point for 5 V is mandatory.

---

## 6. ESP32 DevKit 30-pin Block

### Function

Host the ESP32 DevKit module and expose its power and signal pins to the carrier board.

The ESP32 DevKit acts as the main controller of the carrier board.

It provides:

- Processing.
- GPIO control.
- I2C interface.
- UART interface.
- MOSFET gate control signal through GPIO26.
- 3.3 V logic rail output for external low-current logic use.

### Frozen Footprint Strategy

RevA uses a generic 30-pin ESP32 DevKit footprint strategy with documented compatibility limitation.

Frozen definition:

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

### Design Notes

The ESP32 DevKit family has many low-cost variants with different mechanical dimensions, header spacing, pin naming and USB connector placement.

For RevA, the board will not attempt to support every ESP32 DevKit variant available in the market.

This strategy keeps the PCB scope controlled and makes the compatibility limitation explicit before fabrication.

### Interfaces

The ESP32 DevKit block connects to:

- 5 V rail input.
- 3V3 logic rail output.
- GND.
- I2C connector.
- UART connector.
- GPIO connector.
- MOSFET gate control signal through GPIO26.

---

## 7. 5 V, 3V3 and GND Rails

### 7.1 5 V Rail

#### Source

The 5 V rail is generated by the buck converter module.

#### Loads

The 5 V rail may supply:

- ESP32 DevKit 5 V input.
- External connector pins, if explicitly marked as 5 V.
- Optional future 5 V peripherals.

#### Requirements

- Must have a test point.
- Must be clearly labeled on silkscreen.
- Must not be confused with the 3V3 logic rail.

---

### 7.2 3V3 Rail

#### Source

The 3V3 rail is supplied by the ESP32 DevKit onboard regulator.

#### Loads

The 3V3 rail may supply:

- I2C pull-ups.
- Low-current external sensors.
- UART logic reference.
- GPIO connector logic reference.

#### Requirements

- Must have a test point.
- Must be clearly marked as 3V3.
- Must not be used as a high-current sensor supply without checking the ESP32 DevKit regulator capability.

---

### 7.3 GND

#### Function

GND is the common reference for:

- Input supply.
- Buck converter.
- ESP32 DevKit.
- Sensors.
- UART.
- GPIO.
- MOSFET source.
- Load return path.

#### Layout Requirements

- GND plane on both layers.
- Avoid unnecessary splits in GND.
- Keep MOSFET/load current return paths short and controlled.
- Avoid routing sensitive signals through high-current return paths when possible.

---

## 8. I2C, UART and GPIO Connectors

### 8.1 I2C Connector

#### Function

Provide a standard sensor expansion interface.

#### Suggested Pins

```text
1 — 3V3
2 — GND
3 — SDA
4 — SCL
```

#### Requirements

- I2C logic level: 3.3 V.
- Pull-ups to 3V3 should be optional.
- Pull-ups may be implemented as DNP or jumper-selectable depending on schematic complexity.

#### Design Notes

Many I2C sensor modules already include pull-up resistors.

For this reason, onboard pull-ups should not be mandatory unless needed.

---

### 8.2 UART Connector

#### Function

Provide a 3.3 V UART expansion/debug interface.

#### Suggested Pins

```text
1 — 3V3 or 5V, to be decided
2 — GND
3 — TX from ESP32
4 — RX to ESP32
```

#### Requirements

- UART logic level: 3.3 V.
- Connector must be clearly labeled to avoid TX/RX confusion.
- No RS-232 voltage levels are supported.

#### Design Notes

The UART connector is for logic-level serial communication only.

---

### 8.3 GPIO Connector

#### Function

Expose selected ESP32 GPIO pins for general prototyping.

#### Requirements

GPIOs must avoid unsafe boot/strapping pins when possible.

The connector must include at least:

- GND.
- 3V3 reference.
- Selected GPIO signals.

#### Design Notes

GPIO selection for the expansion connector depends on the final ESP32 DevKit pinout.

The MOSFET gate GPIO is already frozen separately as GPIO26.

---

## 9. Low-side MOSFET Output

### Function

Drive an external load using an N-channel MOSFET in low-side configuration.

### Requirements

- Load voltage: up to 12 V.
- Load current: up to 1 A.
- MOSFET type: N-channel logic-level.
- Primary MOSFET part number: AO3400A.
- MOSFET package: SOT-23.
- Gate drive: ESP32 GPIO26 at 3.3 V.
- Flyback diode required for inductive loads.
- Load connector required.

### Architecture

```text
LOAD+
  |
  v
External Load
  |
  v
AO3400A MOSFET Drain

AO3400A MOSFET Source
  |
  v
GND
```

The ESP32 controls the MOSFET gate through GPIO26 and a gate resistor.

### MOSFET Selection

The RevA low-side output MOSFET is frozen as `AO3400A`.

Frozen MOSFET definition:

- Reference designator: `Q1`
- Primary part number: `AO3400A`
- Type: N-channel MOSFET
- Package: SOT-23
- Function: low-side load switch
- Load target: up to 12 V / 1 A
- Gate drive: ESP32 `GPIO26` at 3.3 V
- Gate net: `MOSFET_GATE`
- Switched load net: `LOAD_SW`

Equivalent part requirement:

```text
AO3400A or equivalent N-channel MOSFET with VDS ≥ 30 V and RDS(on) specified at VGS ≤ 2.5 V.
```

Power dissipation check for 1 A load:

```text
P = I² · R
P = 1² · 0.048
P = 48 mW
```

This dissipation is low for the RevA 1 A target load, assuming reasonable copper area around the MOSFET power pins and operation within the documented load limit.

### MOSFET Gate Control

The low-side MOSFET output is controlled by ESP32 `GPIO26`.

The gate drive interface shall include:

- `100 Ω` series resistor between GPIO26 and the MOSFET gate.
- `100 kΩ` pulldown resistor from gate to GND.
- Gate net named `MOSFET_GATE`.
- Switched load net named `LOAD_SW`.

The pulldown resistor ensures that the MOSFET remains OFF during ESP32 boot/reset or while the GPIO is still high-impedance.

Firmware shall configure GPIO26 as output LOW during initialization before enabling load control.

### Gate Drive Interface

```text
GPIO26 ── 100 Ω ── MOSFET_GATE
                    |
                  100 kΩ
                    |
                   GND
```

### Required MOSFET Support Components

The MOSFET output stage shall include:

- `100 Ω` series resistor between GPIO26 and `MOSFET_GATE`.
- `100 kΩ` pulldown resistor from `MOSFET_GATE` to GND.
- Flyback diode for inductive loads.
- Optional/DNP MOSFET output LED footprint.

### Load Connector

The load connector is frozen for RevA as a 2-pin terminal block / KRE connector with 5.08 mm pitch.

Frozen connector definition:

- Connector type: terminal block / KRE.
- Number of pins: 2.
- Pitch: 5.08 mm.
- Intended use: external load connection.
- Electrical target: up to 12 V / 1 A.
- Required current rating: at least 1 A.
- Preferred current rating: above 1 A for margin.

Suggested schematic pinout:

```text
1 — LOAD+
2 — LOAD_SW
```

Where:

- `LOAD+` is connected to protected VIN or a dedicated load supply path.
- `LOAD_SW` is the switched low-side node connected to the MOSFET drain.

Recommended silkscreen label:

```text
LOAD+
LOAD-
```

The schematic should preferably use `LOAD_SW` instead of `LOAD-` for the switched node to make the circuit behavior clearer during review and debugging.

---

## 10. Test Points

### Function

Provide easy access to important electrical nodes for measurement, debugging and validation.

### Mandatory Test Points

The following test points should be included in RevA:

| Test Point | Signal | Purpose |
|---|---|---|
| TP_VIN | Protected VIN | Verify input supply after protection |
| TP_5V | 5 V | Verify buck output |
| TP_3V3 | 3.3 V | Verify ESP32 logic rail |
| TP_GND | GND | Scope/multimeter reference |
| TP_GATE | MOSFET_GATE | Verify GPIO26 gate control signal |
| TP_LOAD | MOSFET drain / LOAD_SW | Verify switched load node |

### Design Notes

Test points should be accessible after assembly and clearly labeled on silkscreen.

---

## 11. LEDs

### 11.1 Power LED

#### Function

Indicate that the board 5 V rail is present.

#### Suggested Connection

```text
5V -> Resistor -> LED -> GND
```

Alternative connection to 3V3 may be considered if lower brightness or logic rail indication is preferred.

#### Requirement

Power LED is mandatory for RevA.

---

### 11.2 MOSFET Output LED

#### Function

Indicate MOSFET output activation for debug and validation.

#### Status

Frozen as optional / DNP for RevA.

#### Frozen RevA Definition

- Output LED footprint: included.
- Default population status: DNP.
- Required for electrical operation: no.
- Purpose: visual debug and validation aid.
- BOM status: listed as DNP unless intentionally populated.
- Final connection point: to be defined during schematic implementation.

#### Design Notes

The output LED can be useful for debugging and demonstration, but it must not interfere with the load behavior.

During schematic capture, the LED circuit must be implemented so that:

- It does not affect MOSFET gate behavior.
- It does not significantly load the switched output node.
- It remains clearly documented as optional/DNP.

---

## 12. Mounting Holes

### Function

Provide mechanical mounting points for enclosure or bench use.

### Requirements

- 4 mounting holes.
- Mechanically symmetrical where possible.
- Keep copper clearance around holes if they are non-plated.
- If plated mounting holes are used, decide whether they connect to GND.

### Initial Direction

Use 4 mounting holes near PCB corners.

Mounting hole diameter and keepout must be selected before layout.

---

## 13. Block Interfaces

### Interface Table

| From Block | To Block | Signals / Rails | Notes |
|---|---|---|---|
| VIN connector | Input protection | VIN, GND | Raw external input |
| Input protection | Buck module | Protected VIN, GND | Feeds 5 V converter |
| Input protection | Load connector | Protected VIN / LOAD+ | Supplies external load |
| Buck module | 5 V rail | +5V, GND | Board 5 V generation |
| 5 V rail | ESP32 DevKit | 5V, GND | Powers DevKit |
| ESP32 DevKit | 3V3 rail | 3V3, GND | Logic rail from DevKit |
| ESP32 DevKit | I2C connector | SDA, SCL, 3V3, GND | 3.3 V logic |
| ESP32 DevKit | UART connector | TX, RX, GND, optional VCC | 3.3 V logic only |
| ESP32 DevKit | GPIO connector | GPIO signals, 3V3, GND | General expansion |
| ESP32 DevKit | MOSFET output | GPIO26, MOSFET_GATE, GND | Gate driven at 3.3 V |
| MOSFET output | Load connector | LOAD_SW, LOAD+ | Low-side switching |
| Power rails | Test points | VIN, 5V, 3V3, GND | Debug access |
| MOSFET output | Test points | MOSFET_GATE, LOAD_SW | Output validation |

---

## 14. Frozen Decisions

All RevA architecture-level open decisions have been resolved.

| ID | Decision | Status | Frozen Direction |
|---|---|---|---|
| O1 | ESP32 DevKit 30-pin footprint strategy | Frozen | Generic 30-pin DevKit footprint with documented compatibility limitation |
| O2 | MOSFET gate GPIO | Frozen | GPIO26 with 100 Ω gate resistor and 100 kΩ pulldown |
| O3 | Exact MOSFET part number | Frozen | AO3400A or equivalent, VDS ≥ 30 V, RDS(on) specified at VGS ≤ 2.5 V |
| O4 | Load connector type | Frozen | 2-pin 5.08 mm terminal block / KRE |
| O5 | MOSFET output LED | Frozen | Include footprint as optional/DNP |

---

## 15. Criteria to Release KiCad Schematic Capture

The criteria to release KiCad schematic capture have been satisfied for draft schematic work.

### Release Status

```text
KiCad schematic — Released for Draft Schematic
PCB layout — Not released
Fabrication — Not released
```

### Satisfied Conditions

- Requirements RevA v0.1 are frozen.
- Architecture RevA v0.1 is frozen.
- Block list is accepted.
- Power flow is clear:
  - VIN -> Protection -> Buck -> 5 V -> ESP32.
  - VIN -> Load path -> MOSFET -> GND.
- ESP32 DevKit 30-pin strategy is frozen as a generic footprint with documented compatibility limitation.
- MOSFET output topology is frozen as low-side N-channel.
- MOSFET gate GPIO is frozen as GPIO26.
- MOSFET part number is frozen as AO3400A or equivalent.
- Load connector type is frozen.
- Test point list is accepted.
- Power LED is mandatory.
- MOSFET output LED is frozen as optional/DNP.

### Important Limitation

This release applies only to draft schematic capture.

It does not release:

- PCB layout.
- Fabrication.
- Gerber files.
- PCB purchase.
- Physical validation.

Before fabrication, the project must still complete:

- Full schematic capture.
- ERC review.
- Footprint assignment review.
- ESP32 DevKit mechanical and pinout confirmation.
- PCB layout.
- DRC review.
- Gerber review.

---

## 16. Architecture Status

Current architecture status:

| Item | Status |
|---|---|
| Architecture RevA v0.1 | Frozen |
| Ready for schematic draft | Yes |
| Ready for PCB layout | No |
| Ready for fabrication | No |
| KiCad schematic | Released for Draft Schematic |
| PCB layout | Not released |
| Fabrication | Not released |

Freeze review result:

| Decision | Status |
|---|---|
| O1 — ESP32 DevKit 30-pin footprint strategy | Frozen |
| O2 — MOSFET gate GPIO | Frozen |
| O3 — Exact MOSFET part number | Frozen |
| O4 — Load connector type | Frozen |
| O5 — MOSFET output LED | Frozen |

Architecture RevA v0.1 is frozen for schematic capture.

Any future change to O1–O5 must be treated as a revision change or documented engineering change.

---

## 17. Next Actions

The architecture phase is complete.

Next project phase:

```text
Phase 2 — KiCad Schematic Capture
```

Immediate schematic tasks:

- Create the KiCad project structure.
- Add ESP32 DevKit 30-pin headers.
- Add VIN input connector.
- Add input protection block.
- Add adjustable buck module interface.
- Add 5 V, 3V3 and GND rails.
- Add I2C, UART and GPIO connectors.
- Add AO3400A low-side MOSFET output.
- Add gate resistor and gate pulldown.
- Add flyback diode for inductive loads.
- Add power LED.
- Add optional/DNP MOSFET output LED footprint.
- Add required test points.
- Run ERC.
- Submit schematic for technical review before PCB layout.

Fabrication remains blocked until schematic, footprint, ERC, layout and DRC reviews are complete.