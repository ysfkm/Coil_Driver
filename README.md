# Coil Driver

## Summary
- Board hosting three independent bi-directional coil drivers (up to 3A each) off a shared +15V supply, using an H-bridge per channel for direction control.
- Setpoints for each channel's analog current control loop are set digitally over a backplane bus (address + data bits → DAC → analog loop); 4-layer 163mm x 100mm PCB.

## Specs
- **Input:** Digital Bus from backplane
  - A0~A7 as addresses
  - D9~D14 as data bits
- **Supply:** An external +15V with 10A rating and +15V from backplane
- **Output:** three bi-directional current up to 3A
- **Description:** Three separate coil drivers each capable of outputting 3A using the same +15V supply to output current. Utilizes digital interface from the backplane to set the setpoints for the analog current control loops separately and uses H-bridge to achieve bi-directional capability.
- **PCB:** 163mm x 100mm / 4 Layer

> **⚠ Note:** When setting a setpoint via the SPI lines, make sure the DIR bits are all driven to the right value since all DIR bits would be updated at the same time, not just the targeted channel for setpoint adjustment.

> **⚠ Note:** For the jumper selecting +15V power supply, make sure either ALL FOUR middle pins are connected or the LEFT AND RIGHT pins are connected, NEVER BOTH.

## Voltage / Nets

| Voltage / Nets | Purpose |
|---|---|
| +15V_EXT | External supply for driving large currents |
| +15V_BP | Backplane supply for regulating voltages and driving small currents |
| +15V | Jumper implemented to choose between +15V_EXT and +15V_BP<br>Drives main op-amp for current output<br>Drives MOSFETs to control H-bridge switching |
| +6V | Regulated from +15_BP, used to regulate +5VA |
| +5VA | Drives DAC and sensing op-amp. Used to generate VREF for DAC |
| +5V | Backplane supply to regulate +3V3 |
| +3V3 | Backplane supply for digital decoder |
| +3V3A | Regulated from +15V_BP, drives DAC |

## Modules

### Root page
- Backplane connector with digital bus, supply, and ground
- This board's own address switch
- Digital decoder latching the data bits when address from A0~A7 matches

### Power Supply page
- Several ferrite beads to connect different ground domains

### DAC
- Takes the three SPI lines and output 0~3V setpoint
- Has four channels, three used for each analog control loop
- Own regulator to precisely generate reference voltage (3V)

### Analog Control pages
All three are basically identical and have the following:
- Main op-amp driving the current
- Setpoint at main op-amp
- Compensator at main op-amp
- Current sense feedback network and voltage amplifier
- H-bridge
- MOSFET gate driver that drives the H-bridge based on DIR bit
