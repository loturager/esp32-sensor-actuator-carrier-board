\# Open Decisions — RevA



\## Project: ESP32 Sensor/Actuator Carrier Board



This document tracks open technical decisions for RevA.



The goal is to keep uncertainty visible and controlled before freezing the architecture and starting KiCad schematic capture.



\---



\## Decision Status Legend



| Status | Meaning |

|---|---|

| Open | Decision still needs technical analysis. |

| Proposed | A candidate solution exists but is not approved yet. |

| Approved for Draft | Good enough to start schematic draft, but not fully frozen. |

| Frozen | Final decision for RevA unless a serious issue is found. |

| Deferred | Decision moved to a later revision or later project phase. |



\---



\## O1 — Exact ESP32 DevKit 30-pin Reference



\*\*Status:\*\* Open



\### Question



Which exact ESP32 DevKit 30-pin board should be used as the mechanical and pinout reference for RevA?



\### Why this matters



Different ESP32 DevKit boards may have different:



\- Board width.

\- Header spacing.

\- Pin naming.

\- 5 V input pin location.

\- 3V3 pin location.

\- GND pin locations.

\- Boot/strapping pin exposure.

\- USB connector overhang.

\- Mechanical clearance requirements.



\### Required Output



Before schematic and footprint finalization, document:



\- Reference board name.

\- Pin count.

\- Header spacing.

\- Board width.

\- Pinout source.

\- 5 V input pin.

\- 3V3 output pin.

\- GND pins.

\- Pins to avoid for boot/strapping reasons.



\### Current Direction



Use a common ESP32 DevKit 30-pin development board as the RevA reference.



\### Owner



Technical decision / hardware architecture.



\---



\## O2 — GPIO Selection for MOSFET Gate Control



\*\*Status:\*\* Open



\### Question



Which ESP32 GPIO should drive the low-side MOSFET gate?



\### Why this matters



The MOSFET gate control pin must avoid unsafe ESP32 boot behavior.



Some ESP32 pins are related to:



\- Boot mode selection.

\- Strapping configuration.

\- Flash interface.

\- Input-only functionality.

\- Boot-time logic states.



Choosing the wrong GPIO may cause:



\- Boot failure.

\- Unexpected load activation during reset.

\- Unstable output behavior.

\- Difficult debugging.



\### Required Output



Before schematic finalization, document:



\- Selected GPIO.

\- Reason for selection.

\- Pins intentionally avoided.

\- Boot/reset behavior.

\- Whether a gate pulldown is required.



\### Current Direction



Use a safe output-capable GPIO that does not interfere with ESP32 boot/strapping behavior.



\### Owner



Technical decision / embedded hardware.



\---



\## O3 — Logic-Level N-Channel MOSFET Part Number



\*\*Status:\*\* Open



\### Question



Which N-channel MOSFET should be used for the low-side output stage?



\### Requirements



The MOSFET should support:



\- Load voltage up to 12 V.

\- Load current up to 1 A.

\- Gate drive from ESP32 GPIO at 3.3 V.

\- Low enough RDS(on) at VGS = 2.5 V or 3.3 V.

\- Package suitable for hand assembly or realistic PCB assembly.

\- Availability from common suppliers.



\### Why this matters



Not every MOSFET that is called “logic-level” works well at 3.3 V gate drive.



The selected MOSFET must switch reliably with an ESP32 output pin and avoid excessive heating at the target current.



\### Required Output



Before schematic finalization, document:



\- MOSFET part number.

\- Package.

\- VDS rating.

\- Continuous drain current rating.

\- RDS(on) at VGS = 2.5 V or 3.3 V.

\- Thermal considerations for 1 A load.

\- Availability.



\### Current Direction



Select a common N-channel MOSFET with documented low RDS(on) at 2.5 V or 3.3 V gate drive.



\### Owner



Technical decision / component selection.



\---



\## O4 — Load Connector Type



\*\*Status:\*\* Proposed



\### Question



Which connector should be used for the external load output?



\### Proposed Decision



Use a 2-pin terminal block with 5.08 mm pitch.



\### Suggested Pinout



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

