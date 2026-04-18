# Technical Specifications

## Core Components

| Component              |                                  |
|------------------------|----------------------------------|
| MCU                    | ESP32-D0WD-V3                    |
| Flash                  | Winbond SPI 128MBit W25Q128JVP   |
| RAM                    | APMemory 64MBit APS6404L-3SQR-SN |
| Ethernet PHY           | SMSC LAN8720A                    |
| PoE 802.3af Controller | TI TPS23753                      |
| eFuse                  | TI TPS259520DSGR                 |

![ModuleMap](assets/map.png)

### Flash Memory

Any standard, pin-compatible SPI NOR flash in a WSON-8 6x5 mm package can be used. Common options are the 32MBit and 64MBit chips from the same line of Winbond flash memories.

### PSRAM

Installing a PSRAM chip is optional, the module will work just fine without the PSRAM IC populated. While any pin-compatible PSRAM chip can be installed, any amount over 32Mbit cannot be addressed by the ESP32 MCU.

### eFuse

- The digital power domain (MCU power supply input) is protected by a latch-off type eFuse. It disconnects supply at V<sub>in</sub> > 3.8V or I<sub>in</sub> > 1.04A.
- The module exposes a test pad for measuring the supply current drawn by the module's digital domain components:
  - I<sub>in</sub> = 1.81 x V<sub>test</sub>
  - V<sub>test</sub> is measured against digital ground

## Goldfinger Connector

The module exposes several signals of its components through a goldfinger connector:

### GPIOs

| GPIO | free | limited | input-only |
|------|------|---------|------------|
| 2    |      | X       |            |
| 4    |      | X       |            |
| 5    |      | X       |            |
| 12   |      | X       |            |
| 13   | X    |         |            |
| 14   | X    |         |            |
| 15   |      | X       |            |
| 18   | X    |         |            |
| 23   | X    |         |            |
| 32   | X    |         |            |
| 33   | X    |         |            |
| 34   |      |         | X          |
| 35   |      |         | X          |
| 36   |      |         | X          |
| 37   |      |         | X          |
| 38   |      |         | X          |
| 39   |      |         | X          |

GPIOs marked as "limited" have a boot-strapping function and/or a fixed pull-up/pull-down in place.

3 output-capable GPIOs have to be assigned for Ethernet PHY control.

### Ethernet Interface

- Analog (2 differential data pairs, 2x center taps power input)
- 2 Indicator LEDs (Link/Activity, Speed)
- Bob-Smith-Termination
- Digital interface (Enable/Reset, MDC, MDIO)

### Power Supply / Conversion

- High Voltage (PoE domain)
  - External power adapter input, 36V < V<sub>in</sub> < 54V
  - Direct PoE power output for additional DC/DC converters (maximum power draw limited to 802.3af level)
- Low Voltage (Isolated domain)
  - DC/DC converter V<sub>out</sub> = 5V (3.3V optional)
  - I<sub>out,max</sub> = 1.4A (5V) or 2.1A (3.3V)
  - 7W max. output
- Digital domain
  - MCU power supply input (external 3.3V supply required)
  - I<sub>supply,avg</sub> < 500mA (soft limit)
  - I<sub>supply,lim</sub> = 1.04A (latch-off limit)

### Programming Interface

- ESP32 UART
- MCU Reset
- GPIO0 (boot strapping only, no direct connection)

## Physical

- Standard Mini-PCIe goldfinger connector, 52 pins
- FR4 1.00 mm base material, 6 layers
- Size: 45 x 30 x 14.5 mm
- Weight: 12g

