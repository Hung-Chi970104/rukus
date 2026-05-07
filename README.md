# Rukus: Pocket Robotics Control Computer
Welcome to Rukus! The most powerful and versatile pocket-size robotic control computer that can handle up 18V and 3.2A of peak current. It also features a powerful dual-core RP2040 MCU, allowing users to run heavy computing and even TinyML!

![rendered_image](assets/rc_car_module.png)


## Demo Video
https://github.com/user-attachments/assets/a237940f-da78-4733-91b2-979578ba8ca9

https://github.com/user-attachments/assets/f6da3c5c-2f20-4054-9eab-c4b8ad8e2e0a

## Components
- RP2040
- 12MHz Crystal (ABM8-272-T3)
- 4MB Flash Storage (W25Q16JVUXIQ)
- Buck-Boost Switching Regulator 5V to 3V3 (RT6150BGQW)
- Buck-Boost Switching Regulator 12V to 5V (AP64201)
- USB-C (TYPE-C-31-M-12)
- Motor Driver (TB6612FNG)

## PCBs
- 2 layers
- Single-sided component placement
- 2.6mm board thickness
- HASL (with lead) surface finish
- 1oz outer copper weight

### PCB Schematic
![schematic](assets/rc_car_schematic.svg)

### PCB Layouts
![pcb](assets/RC%20Car%20Module-B_Cu.svg)
![pcb](assets/RC%20Car%20Module-F_Cu.svg)
![pcb](assets/rc_car_pcb.png)

### 3D Model
![3d_front](assets/rc_car_3d_front.png)
![3d_back](assets/rc_car_3d_back.png)

# Fabrication and Assembly Instructions
To manufacture and assemble this board via JLCPCB, follow these steps:
- Download the project .zip file from the repository.
- Upload the .zip file to the JLCPCB ordering portal.
- Select the PCBA (PCB Assembly) option and proceed to the next page.
- Upload the provided BOM.csv (Bill of Materials) and position.csv (CPL/Pick and Place) files.
- Verify the component selection alignments and submit your payment.