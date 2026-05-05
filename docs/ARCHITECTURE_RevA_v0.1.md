\# ARCHITECTURE\_RevA\_v0.1.md



\## Project: ESP32 Sensor/Actuator Carrier Board



\*\*Revision:\*\* RevA v0.1  

\*\*Status:\*\* Architecture Draft  

\*\*Previous phase:\*\* Requirements RevA v0.1 frozen  

\*\*Current phase:\*\* Block Architecture  

\*\*Next phase:\*\* KiCad schematic capture  



\---



\# 1. Board Overview



The ESP32 Sensor/Actuator Carrier Board is a two-layer PCB designed as a reusable hardware platform for ESP32-based sensing, communication and low-power actuator control applications.



The board targets light industrial, IoT prototyping and embedded control use cases where an ESP32 DevKit module needs external power input, regulated 5 V distribution, 3.3 V logic access, sensor connectors and a protected low-side output stage.



The board provides:



\- External VIN input from 7 V to 12 V.

\- Basic input protection for safer prototyping.

\- 5 V generation through a module-based adjustable buck converter stage.

\- 3.3 V logic rail supplied by the ESP32 DevKit.

\- I2C, UART and GPIO expansion connectors.

\- One low-side N-channel MOSFET output for driving an external load up to 12 V / 1 A.

\- Power indication LED.

\- Test points for debugging, validation and measurement.

\- Mounting holes for mechanical integration.

\- Ground planes on both PCB layers.



This revision prioritizes clear architecture, manufacturability, safe prototyping practices and portfolio-quality documentation over maximum circuit complexity.

