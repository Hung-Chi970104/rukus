# Rukus — RP2040 Robotics Controller

**A custom two-layer controller brought up and integrated into a working STEM-club RC car.**

[PCB layout](#pcb-layout) · [Schematic](#schematic) · [Demo](#working-hardware) · [Firmware](#firmware) · [Companion radio](https://github.com/Hung-Chi970104/nrf24l01-radio)

## PCB layout

[![Rukus routed PCB with front and back copper, power routing and motor connections](assets/rc_car_pcb.png)](assets/rc_car_pcb.png)

The board integrates the RP2040, a TB6612FNG dual H-bridge, USB-C power/data, and the 12 V, 5 V and 3.3 V power domains. The layout brings the power conversion, digital control and two brushed-motor interfaces onto one board.

## What I built

- Designed the schematic and two-layer PCB around an RP2040 and onboard dual motor driver.
- Integrated buck and buck-boost regulation with USB-C power/data and motor-drive circuitry.
- Brought up the custom controller and integrated its motor interfaces with my nRF24L01 radio board in a working RC car.
- Wrote MicroPython motor-control and radio test code, with demonstrations linked below.

## Working hardware

[Watch the RC-car demonstration](assets/demo.mp4) · [Watch an iteration / debugging clip](assets/iteration_failure.mp4)

https://github.com/user-attachments/assets/a237940f-da78-4733-91b2-979578ba8ca9

https://github.com/user-attachments/assets/f6da3c5c-2f20-4054-9eab-c4b8ad8e2e0a

The wireless module now has its own **[nRF24L01 radio repository](https://github.com/Hung-Chi970104/nrf24l01-radio)**, including the printed antenna, schematic and radio examples. The original [radio folder](radio/) remains available here for existing links.

## Architecture

| Block | Implementation |
|---|---|
| Controller | RP2040 with external clock and SPI flash |
| Motor drive | TB6612FNG dual H-bridge for two brushed DC motors |
| Power conversion | AP62401 buck stage and RT6150 buck-boost stage |
| Power domains | 12 V input/motor domain, 5 V and 3.3 V rails |
| USB | USB-C power and data |
| Wireless interface | SPI connection to the custom nRF24L01 module |

The voltages above describe the design. Maximum board current has not been established by a published load test; a motor-driver pulse rating is not a continuous system rating.

## Schematic

[Open the full schematic PDF](RC%20Car%20Module.pdf) · [Open the KiCad schematic](RC%20Car%20Module.kicad_sch)

[![Rukus schematic: power conversion, RP2040, flash and dual motor driver](assets/rc_car_schematic.svg)](RC%20Car%20Module.pdf)

## 3D views

Original board renders:

| Front | Back |
|---|---|
| ![Original front 3D view](assets/rc_car_3d_front.png) | ![Original back 3D view](assets/rc_car_3d_back.png) |

[Detailed board render](assets/rc_car_module.png) · [PCB source](RC%20Car%20Module.kicad_pcb)

## Firmware

| File | Purpose |
|---|---|
| [rc_motor_driver.py](rc_motor_driver.py) | Motor-driver control |
| [transmitter.py](transmitter.py) | Radio transmit / acknowledgment test |
| [receiver.py](receiver.py) | Radio receive test |
| [joystick_control.py](joystick_control.py) | Joystick input example |

The radio examples import the MicroPython `nrf24l01` driver; install that dependency separately. They are individual bring-up examples, not a packaged vehicle-control application.

## Design and fabrication files

Open [RC Car Module.kicad_pro](RC%20Car%20Module.kicad_pro) in KiCad to explore the design. The [production directory](production/) contains manufacturing exports and previous snapshots.

For assembly, inspect the selected archive, BOM and placement data against the intended board revision before ordering. The schematic and layout are the source for the circuit; the demonstration videos show the assembled system.
