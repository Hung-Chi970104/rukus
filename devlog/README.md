# Build log: Rukus: Pocket Robotics Control Computer

A modular robotics control platform for small mobile robots. It combines an MCU, a motor driver and
radio communication, so you can focus on programming instead of hardware hassle.

**Features**

- Minimal size (45 mm × 45 mm)
- A robust power rail, handling 12 V to 5 V / 3.3 V conversion
- Plenty of computing power from the dual-core RP2040

I posted these as devlogs on [Hack Club Flavortown](https://flavortown.hackclub.com/projects/2312) while building the project: 53 entries and 97 h 9 min of logged work, 27 Dec 2025 to 30 Apr 2026. The text is lightly edited for spelling and clarity; dates are UTC.

| # | Date | Logged | Entry |
|---|---|---|---|
| 1 | 27 Dec 2025 | 1 h 14 min | [Reading the TB6612FNG design](#1--reading-the-tb6612fng-design) |
| 2 | 2 Feb 2026 | 1 h 18 min | [First schematic, from the Pico's](#2--first-schematic-from-the-picos) |
| 3 | 2 Feb 2026 | 1 h 31 min | [Working out the regulator](#3--working-out-the-regulator) |
| 4 | 2 Feb 2026 | 1 h 24 min | [A 12 V to 5 V regulator](#4--a-12-v-to-5-v-regulator) |
| 5 | 3 Feb 2026 | 2 h 2 min | [Power and motor-driver schematic](#5--power-and-motor-driver-schematic) |
| 6 | 4 Feb 2026 | 1 h 32 min | [Schematic complete](#6--schematic-complete) |
| 7 | 4 Feb 2026 | 21 min | [Footprints](#7--footprints) |
| 8 | 5 Feb 2026 | 1 h 43 min | [Rough placement](#8--rough-placement) |
| 9 | 6 Feb 2026 | 2 h 28 min | [Routing, nearly done](#9--routing-nearly-done) |
| 10 | 6 Feb 2026 | 2 h 4 min | [Wishing for four layers](#10--wishing-for-four-layers) |
| 11 | 7 Feb 2026 | 1 h 45 min | [Routing finished](#11--routing-finished) |
| 12 | 7 Feb 2026 | 1 h 53 min | [Bulk capacitor and DRC](#12--bulk-capacitor-and-drc) |
| 13 | 7 Feb 2026 | 2 h 3 min | [Back to the RP2040 datasheet](#13--back-to-the-rp2040-datasheet) |
| 14 | 7 Feb 2026 | 1 h 3 min | [Render and blueprint](#14--render-and-blueprint) |
| 15 | 7 Feb 2026 | 45 min | [README, release and Sketchfab](#15--readme-release-and-sketchfab) |
| 16 | 7 Feb 2026 | 21 min | [Last checks before ordering](#16--last-checks-before-ordering) |
| 17 | 8 Feb 2026 | 1 h 26 min | [Designing the nRF24L01 antenna](#17--designing-the-nrf24l01-antenna) |
| 18 | 13 Feb 2026 | 2 h 3 min | [Fixing the AP62401WU section](#18--fixing-the-ap62401wu-section) |
| 19 | 13 Feb 2026 | 1 h 2 min | [Wrong inductors](#19--wrong-inductors) |
| 20 | 14 Feb 2026 | 2 h 13 min | [4 mm inductors](#20--4-mm-inductors) |
| 21 | 14 Feb 2026 | 1 h 35 min | [Before the radio](#21--before-the-radio) |
| 22 | 14 Feb 2026 | 1 h 14 min | [Radio schematic](#22--radio-schematic) |
| 23 | 14 Feb 2026 | 1 h 2 min | [RF antenna from the datasheet](#23--rf-antenna-from-the-datasheet) |
| 24 | 15 Feb 2026 | 1 h 9 min | [Redrawing it by hand](#24--redrawing-it-by-hand) |
| 25 | 15 Feb 2026 | 58 min | [nRF24L01 module schematic finished](#25--nrf24l01-module-schematic-finished) |
| 26 | 15 Feb 2026 | 1 h 29 min | [Onto the radio layout](#26--onto-the-radio-layout) |
| 27 | 15 Feb 2026 | 1 h 4 min | [Part sizes fixed](#27--part-sizes-fixed) |
| 28 | 15 Feb 2026 | 1 h 6 min | [Radio layout](#28--radio-layout) |
| 29 | 17 Feb 2026 | 1 h 29 min | [Picking every part by hand](#29--picking-every-part-by-hand) |
| 30 | 18 Feb 2026 | 44 min | [Soldering the prototype parts](#30--soldering-the-prototype-parts) |
| 31 | 18 Feb 2026 | 1 h 27 min | [Testing on an Arduino first](#31--testing-on-an-arduino-first) |
| 32 | 20 Feb 2026 | 1 h 11 min | [Transmitter test](#32--transmitter-test) |
| 33 | 20 Feb 2026 | 31 min | [Two boards talking](#33--two-boards-talking) |
| 34 | 28 Feb 2026 | 3 h 7 min | [Prototyping on the dev board](#34--prototyping-on-the-dev-board) |
| 35 | 5 Mar 2026 | 2 h 3 min | [The prototype validates the design](#35--the-prototype-validates-the-design) |
| 36 | 8 Mar 2026 | 1 h 44 min | [Routing the radio PCB](#36--routing-the-radio-pcb) |
| 37 | 8 Mar 2026 | 2 h 32 min | [Radio module ordered](#37--radio-module-ordered) |
| 38 | 14 Mar 2026 | 2 h 28 min | [First attempt: failed](#38--first-attempt-failed) |
| 39 | 15 Mar 2026 | 1 h 32 min | [Design changes after the failure](#39--design-changes-after-the-failure) |
| 40 | 15 Mar 2026 | 1 h 11 min | [Tidying the crystal cluster](#40--tidying-the-crystal-cluster) |
| 41 | 19 Mar 2026 | 3 h 28 min | [Big layout changes](#41--big-layout-changes) |
| 42 | 19 Mar 2026 | 19 min | [Final tweaks before ordering](#42--final-tweaks-before-ordering) |
| 43 | 22 Mar 2026 | 2 h 35 min | [Reflowing the nRF24L01 module](#43--reflowing-the-nrf24l01-module) |
| 44 | 23 Mar 2026 | 1 h 40 min | [The radio works](#44--the-radio-works) |
| 45 | 28 Mar 2026 | 3 h 10 min | [Fixing the RT6154A wiring](#45--fixing-the-rt6154a-wiring) |
| 46 | 28 Mar 2026 | 19 min | [Adding the radio](#46--adding-the-radio) |
| 47 | 2 Apr 2026 | 2 h 15 min | [Ready for the fourth order](#47--ready-for-the-fourth-order) |
| 48 | 6 Apr 2026 | 2 h 16 min | [Signal integrity](#48--signal-integrity) |
| 49 | 19 Apr 2026 | 5 h 37 min | [Reflow attempt 2](#49--reflow-attempt-2) |
| 50 | 26 Apr 2026 | 3 h 42 min | [Reflow attempt 3](#50--reflow-attempt-3) |
| 51 | 29 Apr 2026 | 5 h 7 min | [Reflow attempt 4: Rukus is alive](#51--reflow-attempt-4-rukus-is-alive) |
| 52 | 30 Apr 2026 | 5 h 21 min | [Final testing and small bug fixes](#52--final-testing-and-small-bug-fixes) |
| 53 | 30 Apr 2026 | 1 h 34 min | [Wiring clean-up and a stress-test bug](#53--wiring-clean-up-and-a-stress-test-bug) |

---

## 1 · Reading the TB6612FNG design
<sub>27 Dec 2025 · 1 h 14 min logged</sub>

For the past hour I was reading the TB6612FNG schematic and trying to apply it to my own. The
problems:

- I wanted to power the RP2040 from both USB (5 V) and a battery (12 V).
- Which pins to keep and which to drop, since I wanted it minimal.

<p>
<img src="media/3393-1.png" width="70%">
</p>

---

## 2 · First schematic, from the Pico's
<sub>2 Feb 2026 · 1 h 18 min logged</sub>

I roughly completed the schematic, referring to the Pico's. Things I noticed:

- Use an RT6154A instead of the MCP1700x-330xxTT for more current.
- Add Schottky diodes if an external power supply is needed.
- Connect GPIO24 and GPIO23 to the RT6154A to detect whether the board is on battery or USB, and
  switch between PWM and power-saving mode.

<p>
<img src="media/18773-1.png" width="49%">
<img src="media/18773-2.png" width="49%">
</p>

---

## 3 · Working out the regulator
<sub>2 Feb 2026 · 1 h 31 min logged</sub>

I was figuring out how to wire the RT6150AGQW.

- The Pico schematic uses the RT6150B-33GQW, but apparently no one sells it, or it's too expensive.
- Richtek only shows the minimal design, so I had to work out the maths:
  R1 = R2 × (Vout / Vfb − 1).
- Imported the symbol and footprint for both the RT6150AGQW and the MBR120VLSFT1G.
- Tidied the symbol to keep things organised.

<p>
<img src="media/18796-1.png" width="49%">
<img src="media/18796-2.png" width="49%">
</p>

---

## 4 · A 12 V to 5 V regulator
<sub>2 Feb 2026 · 1 h 24 min logged</sub>

Spent some time finding a regulator to convert 12 V to 5 V, so the battery can also supply the
RP2040 (not directly, of course).

- Learned about and chose the AP62401.
- Created its symbol and footprint from the datasheet:
  https://www.diodes.com/datasheet/download/AP62400.pdf

<p>
<img src="media/19040-1.png" width="49%">
<img src="media/19040-2.png" width="49%">
</p>

---

## 5 · Power and motor-driver schematic
<sub>3 Feb 2026 · 2 h 2 min logged</sub>

Completed the schematic for the AP62401 (12 V to 5 V) and the TB6612FNG. I also just realised that
the TB6612FNG is actually the chip itself. Here's how the power supply works:

```
12V --> AP62401 (12V to 5V) --> RT6154AGQW (5V to 3V3) --> MCU
    --> TB6612FNG --> motors
```

<p>
<img src="media/19076-1.png" width="49%">
<img src="media/19076-2.png" width="49%">
</p>

---

## 6 · Schematic complete
<sub>4 Feb 2026 · 1 h 32 min logged</sub>

Finally completed the schematic. I tried to make the PCB square. For now it only includes the RP2040
and the TB6612FNG motor driver; I'll add the nRF24L01 later.

<p>
<img src="media/19406-1.png" width="70%">
</p>

---

## 7 · Footprints
<sub>4 Feb 2026 · 21 min logged</sub>

Linked the symbols to their footprints.

<p>
<img src="media/19412-1.png" width="70%">
</p>

---

## 8 · Rough placement
<sub>5 Feb 2026 · 1 h 43 min logged</sub>

Roughly positioned all the main components, and changed the number of output pins to make room for
GND and 3V3.

- Motor driver at the front, far from the RP2040.
- 45 mm × 45 mm.

I still need to reorder the pin headers to reduce crossings as much as possible.

<p>
<img src="media/19692-1.png" width="49%">
<img src="media/19692-2.png" width="49%">
</p>

---

## 9 · Routing, nearly done
<sub>6 Feb 2026 · 2 h 28 min logged</sub>

After two and a half hours of work, I've almost finished routing. Trace widths used: 0.18, 0.20,
0.30, 0.50 and 1.00 mm. There's not much to say about it; it's just fun.

<p>
<img src="media/19965-1.png" width="49%">
<img src="media/19965-2.png" width="49%">
</p>

---

## 10 · Wishing for four layers
<sub>6 Feb 2026 · 2 h 4 min logged</sub>

This is getting frustrating; I really should have used a 4-layer board. Two problems now:

- Some GND pins are still unconnected.
- Two pins are far apart, and the MCU's traces block the way between them.

<p>
<img src="media/20081-1.png" width="49%">
<img src="media/20081-2.png" width="49%">
</p>

---

## 11 · Routing finished
<sub>7 Feb 2026 · 1 h 45 min logged</sub>

Finally finished routing.

- Rerouted everything around the two voltage converters (you can see it in my Lapse).
- Reassigned PWMB to GPIO27 to make room for more vias.
- Routed everything and cleaned up the silkscreen.

<p>
<img src="media/20247-1.png" width="32%">
<img src="media/20247-2.png" width="32%">
<img src="media/20247-3.png" width="32%">
</p>

---

## 12 · Bulk capacitor and DRC
<sub>7 Feb 2026 · 1 h 53 min logged</sub>

What I did over the past two hours:

- Added a 100 µF polarised capacitor to absorb 12 V spikes.
- Rerouted the 12 V section, again.
- Edited the RT6154AGQW footprint.
- Cleaned up the DRC errors.

<p>
<img src="media/20270-1.png" width="49%">
<img src="media/20270-2.png" width="49%">
</p>

---

## 13 · Back to the RP2040 datasheet
<sub>7 Feb 2026 · 2 h 3 min logged</sub>

I reread the RP2040 datasheet and learned that it has only two SPI and two I2C engines, with eight
PWM slices.

- Fixed the PWMA and PWMB routing.
- Rerouted the USB-C, after deciding to swap it with the 01×04 pin header.

Everything apart from the USB-C has now been rerouted, and the USB-C has moved to the top-right
corner.

<p>
<img src="media/20413-1.png" width="32%">
<img src="media/20413-2.png" width="32%">
<img src="media/20413-3.png" width="32%">
</p>

---

## 14 · Render and blueprint
<sub>7 Feb 2026 · 1 h 3 min logged</sub>

Rendered my PCB, and made a blueprint-style image (the second image).

<p>
<img src="media/20466-1.png" width="49%">
<img src="media/20466-2.png" width="49%">
</p>

---

## 15 · README, release and Sketchfab
<sub>7 Feb 2026 · 45 min logged</sub>

Updated the [README](https://github.com/Hung-Chi970104/rc_car_module/blob/main/README.md) and added a
[release](https://github.com/Hung-Chi970104/rc_car_module/releases/tag/v1.0.0). I also added a small
[Sketchfab model](https://skfb.ly/pG7D9).

<p>
<img src="media/20484-1.png" width="49%">
<img src="media/20484-2.png" width="49%">
</p>

---

## 16 · Last checks before ordering
<sub>7 Feb 2026 · 21 min logged</sub>

The last devlog before ordering. I double-checked everything, added more vias and ran DRC twice. One
last look at my beautiful PCB.

<p>
<img src="media/20490-1.png" width="70%">
</p>

---

## 17 · Designing the nRF24L01 antenna
<sub>8 Feb 2026 · 1 h 26 min logged</sub>

For the past two hours I was designing my nRF24L01 antenna and working it into the design. I'd hoped
to just import someone else's design, but no luck. Some references I can use:

- AN043
- AN2731
- AN91445

I lean towards the first, and will probably follow it closely.

<p>
<img src="media/20580-1.png" width="70%">
</p>

---

## 18 · Fixing the AP62401WU section
<sub>13 Feb 2026 · 2 h 3 min logged</sub>

I spent a lot of time fixing and rerouting the AP62401WU.

- Added a Schottky diode to stop current leaking back into the AP62401WU.
- Moved the AP62401WU's output capacitors, since they could create a lot of voltage ripple and upset
  the other, more delicate ICs (see the highlighted components).

<p>
<img src="media/21937-1.png" width="70%">
</p>

---

## 19 · Wrong inductors
<sub>13 Feb 2026 · 1 h 2 min logged</sub>

I just noticed I picked the wrong inductors: I accidentally chose 0201 parts, which can't carry
nearly enough current (see the highlighted part).

<p>
<img src="media/22137-1.png" width="70%">
</p>

---

## 20 · 4 mm inductors
<sub>14 Feb 2026 · 2 h 13 min logged</sub>

After two hours of struggle, I finally replaced the two inductors with 4 mm × 4 mm ones (my Lapse
shows how much I struggled).

<p>
<img src="media/22206-1.png" width="70%">
</p>

---

## 21 · Before the radio
<sub>14 Feb 2026 · 1 h 35 min logged</sub>

A few changes before moving on to the nRF24L01 radio:

- Changed the 0603 47 µF capacitors to 1206 so they can handle 25 V spikes.
- Rerouted everything around the two capacitors.
- Double-checked the schematic and cleaned up the layout.

<p>
<img src="media/22222-1.png" width="70%">
</p>

---

## 22 · Radio schematic
<sub>14 Feb 2026 · 1 h 14 min logged</sub>

Spent an hour designing the radio schematic. I'm going to build the radio module first.

<p>
<img src="media/22464-1.png" width="70%">
</p>

---

## 23 · RF antenna from the datasheet
<sub>14 Feb 2026 · 1 h 2 min logged</sub>

Finally copied the RF antenna from the datasheet. It was a bit of a hassle.

<p>
<img src="media/22485-1.png" width="70%">
</p>

---

## 24 · Redrawing it by hand
<sub>15 Feb 2026 · 1 h 9 min logged</sub>

EasyEDA wouldn't export to KiCad for some reason, so I had to draw a new one myself. After some
time, here's the result.

<p>
<img src="media/22501-1.png" width="70%">
</p>

---

## 25 · nRF24L01 module schematic finished
<sub>15 Feb 2026 · 58 min logged</sub>

Finished the schematic for the custom nRF24L01 module.

<p>
<img src="media/22519-1.png" width="70%">
</p>

---

## 26 · Onto the radio layout
<sub>15 Feb 2026 · 1 h 29 min logged</sub>

On to the most exciting part, since radio is very sensitive to any change. We've got this. I also
picked the crystal (YXC X49SD16MSD2SC) from LCSC; let's hope it works.

<p>
<img src="media/22543-1.png" width="70%">
</p>

---

## 27 · Part sizes fixed
<sub>15 Feb 2026 · 1 h 4 min logged</sub>

I mixed up some reference designators and parts; I didn't know I needed to use 0402. Now we can
finally get into PCB layout and routing.

<p>
<img src="media/22708-1.png" width="70%">
</p>

---

## 28 · Radio layout
<sub>15 Feb 2026 · 1 h 6 min logged</sub>

This time the layout wasn't too bad. I spent an hour working it out and connecting everything.

<p>
<img src="media/22725-1.png" width="70%">
</p>

---

## 29 · Picking every part by hand
<sub>17 Feb 2026 · 1 h 29 min logged</sub>

I picked all the parts manually, which took a long time (and look at the price: so expensive). I
also found I had accidentally used 0603 for a 10 µF capacitor, but there's a fix.

<p>
<img src="media/23116-1.png" width="70%">
</p>

---

## 30 · Soldering the prototype parts
<sub>18 Feb 2026 · 44 min logged</sub>

Spent some time soldering all the components for prototyping:

- My RP2040 board (I made it myself)
- nRF24L01
- TB6612

<p>
<img src="media/23398-1.jpg" width="70%">
</p>

---

## 31 · Testing on an Arduino first
<sub>18 Feb 2026 · 1 h 27 min logged</sub>

Tested the circuits on an Arduino UNO with an L298N first. I'll try the RP2040 and TB6612 later.

▶ [Video (9.6 MB)](media/23409-1.mp4)

---

## 32 · Transmitter test
<sub>20 Feb 2026 · 1 h 11 min logged</sub>

Tested the transmitter, and my custom RP2040 dev board along with it: a huge success. Pins:

- CSN: GP12
- MOSI: GP7
- CE: GP11
- SCK: GP6
- MISO: GP4

▶ [Video (13.4 MB)](media/24183-1.mp4)

---

## 33 · Two boards talking
<sub>20 Feb 2026 · 31 min logged</sub>

Huge success: I got two custom RP2040 boards talking to each other over nRF24L01. Next, I'll wire
them to my custom board.

▶ [Video (10.2 MB)](media/24187-1.mp4)

---

## 34 · Prototyping on the dev board
<sub>28 Feb 2026 · 3 h 7 min logged</sub>

This time I prototyped on my RP2040 dev board with a DRV8833, though I had to use an Arduino Uno for
power. I tried to power the L298N from my custom board, but it didn't work, because the current
spikes are huge. Still a big success; next I'll connect the nRF24L01 radio and see how it goes.

▶ [Video (3.1 MB)](media/26678-1.mp4)

---

## 35 · The prototype validates the design
<sub>5 Mar 2026 · 2 h 3 min logged</sub>

Finished prototyping with my custom RP2040 dev board. This basically validates the PCB design, since
it uses exactly the same parts apart from different PWM pins. I haven't added the nRF24L01 to the
design yet, because radio is very unpredictable.

▶ [Video (1.9 MB)](media/28159-1.mp4)

---

## 36 · Routing the radio PCB
<sub>8 Mar 2026 · 1 h 44 min logged</sub>

Spent some time routing my radio PCB, checking DRC and rearranging the layout. I just realised I
shouldn't switch 0402 components to 0603, because it might detune the radio.

▶ [Video (18.7 MB)](media/29355-1.mp4)

---

## 37 · Radio module ordered
<sub>8 Mar 2026 · 2 h 32 min logged</sub>

Finished and ordered my custom radio module.

- Cleaned up DRC (the many clearance warnings are just from the custom antenna).
- Sorted out the BOM.
- Added plenty of vias.
- Removed the solder mask (I wanted to test both with and without it).
- Submitted the order.
- Rendered the custom board.

<p>
<img src="media/29494-1.png" width="70%">
</p>

---

## 38 · First attempt: failed
<sub>14 Mar 2026 · 2 h 28 min logged</sub>

I finally got my boards and gave them a first shot. Populating the board is the most boring part,
and it took almost an hour and a half. After nearly another hour of repairs, it failed badly. Here's
the result (see the picture). These hours were recorded the day before; I only now had time to post.

<p>
<img src="media/31021-1.jpg" width="49%">
<img src="media/31021-2.jpg" width="49%">
</p>

---

## 39 · Design changes after the failure
<sub>15 Mar 2026 · 1 h 32 min logged</sub>

After the failure, in case it was a design issue, I made a few changes:

- Removed RUN by simply crossing it out.
- Replaced the AP62401WU's 10 µF capacitor with a ceramic one.
- Removed PSYNC, since it occupies GPIO23 and I just want it working; this strips out some
  complexity.
- Added a few test pads so I don't blow anything up again while testing.

<p>
<img src="media/31382-1.png" width="70%">
</p>

---

## 40 · Tidying the crystal cluster
<sub>15 Mar 2026 · 1 h 11 min logged</sub>

Spent an hour reorganising the crystal cluster: it looked messy, and the parts were so close together
they were hard to solder. Here's the new result.

<p>
<img src="media/31573-1.png" width="70%">
</p>

---

## 41 · Big layout changes
<sub>19 Mar 2026 · 3 h 28 min logged</sub>

Made a few big changes:

- Replaced the big, expensive inductor for the RT6154AGQW with a 1206 part.
- Fixed many layout issues, such as the inductor and capacitor placement for the buck converters.
- Added 12 V and VBUS copper planes (a new skill).
- Redrew many traces, such as AOUT1/2, much thicker (1 mm).

The whole thing looks much more solid now.

<p>
<img src="media/32683-1.png" width="70%">
</p>

---

## 42 · Final tweaks before ordering
<sub>19 Mar 2026 · 19 min logged</sub>

A few more changes before ordering:

- Rearranged the test pads to reduce clutter.
- Added a 0.1 µF capacitor near the motor-control pins.
- Added more copper planes for VBUS and VSYS.

<p>
<img src="media/32781-1.png" width="70%">
</p>

---

## 43 · Reflowing the nRF24L01 module
<sub>22 Mar 2026 · 2 h 35 min logged</sub>

I finally reflowed my nRF24L01 module. At first it couldn't communicate, because of a bridge between
two pins (I forget which), but I spotted it and cleared it with my Pinecil. I also spent a lot of
time on the layout, then realised the 10 µF input capacitors don't need to be 1206 (a waste of time
and space). Still, the radio module is a big success, though I still need to test whether it can
actually send and receive.

<p>
<img src="media/33642-1.jpg" width="70%">
</p>

---

## 44 · The radio works
<sub>23 Mar 2026 · 1 h 40 min logged</sub>

I can't believe it actually worked. You add your own antenna, put everything together, and it just
works. Check out the video to see it replacing my original setup. Next: combining the nRF24L01 with
the robotics module.

▶ [Video (2.9 MB)](media/34215-1.mp4)

---

## 45 · Fixing the RT6154A wiring
<sub>28 Mar 2026 · 3 h 10 min logged</sub>

Fixed the RT6154A by disconnecting VINA from VIN; it's the same fix as on the RP2040 dev board, so
don't be surprised if they look alike. I also optimised the power routing further, since the
feedback resistors should sit closer to the IC. Let's hope it's rock solid now.

<p>
<img src="media/35563-1.png" width="70%">
</p>

---

## 46 · Adding the radio
<sub>28 Mar 2026 · 19 min logged</sub>

Spent some time adding the nRF24L01 radio, but it looks so patched together, like Frankenstein. I
haven't finished routing it yet, but I will. I'm also planning to order both versions, so I can tell
whether the radio is what's causing problems.

<p>
<img src="media/35621-1.png" width="70%">
</p>

---

## 47 · Ready for the fourth order
<sub>2 Apr 2026 · 2 h 15 min logged</sub>

After lots of trial and error, I'm ready to order the board for the fourth time. What I fixed:

- Changed the capacitors to 1206.
- Connected STBY (an upgrade: you can finally turn the motor driver on and off).
- Added another copper pour for VBUS.

I ran DRC on everything I'm about to order and re-rendered the demo. Let's hope this one works.

<p>
<img src="media/36787-1.png" width="70%">
</p>

---

## 48 · Signal integrity
<sub>6 Apr 2026 · 2 h 16 min logged</sub>

After learning a lot about impedance, I finally worked on signal integrity. This board is messy both
electrically and for signals, because it's a 2-layer board and the signal lines and buck converters
all share the same GND. I played with an impedance calculator and redrew traces in KiCad while
watching Phil's Lab videos, which helped a lot with the USB D+ and D− lines. Next I'll reroute all the
GPIOs, since they'll carry SPI, so their lengths and widths matter.

<p>
<img src="media/38313-1.png" width="70%">
</p>

---

## 49 · Reflow attempt 2
<sub>19 Apr 2026 · 5 h 37 min logged</sub>

After another two weeks of waiting, the new PCBs arrived. There was real progress, but the board
still isn't fully working.

**What works**

- Rather than reflowing the whole board, I played it safe and reflowed only the AP62401, to confirm
  the board can turn 12 V into 5 V and feed it to the next stage (the RT6150B).
- At first I forgot to solder the two 22 µF capacitors, so the output wasn't a stable 5 V. Once I
  populated them, it gave 4.7 V, close enough for the RT6150B.
- Then I populated the whole board: the RT6150B and all the other ICs, including the RP2040, the
  crystal, the TB6612FNG and the USB-C.
- At first the 3V3 indicator didn't come on, until I realised I hadn't cleared the bridges on the
  RT6150 (I almost burnt it). In the end the RT6150 delivered 3.18 V to the RP2040, which should be
  enough for it to run.

**What failed**

- My laptop detected the RP2040 and could apparently transfer data, but the board kept rebooting no
  matter what.
- I reflowed the flash, thinking that might help; it didn't. I replaced the flash with a new one; it
  failed again. I reflowed the crystal to rule out a clock problem; no change. Then I swapped in a
  known-working part, and the problem remained.
- I don't think the RP2040 is the cause, but I reflowed it anyway, just in case; I don't want to
  sacrifice another one.

**Conclusion**
Some parts may have been damaged by my first mistake, when I forgot to clear the bridges on the
RT6150, though I really doubt it: the 3.3 V indicator didn't even light, which suggests any short
happened inside the regulator rather than dumping excess voltage downstream. I'll try again soon.

<p>
<img src="media/42025-1.jpg" width="70%">
</p>

---

## 50 · Reflow attempt 3
<sub>26 Apr 2026 · 3 h 42 min logged</sub>

After the endless boot-looping in attempt 2, I finally got it working, though it was messy.

Since the board had been rebooting non-stop, I suspected the RT6150 had been damaged internally when
I left those bridges on, so I decided to swap the regulator one last time with the hot-air gun. I was
trying to be careful, and a pad peeled off. I stared at it thinking the board was dead, since a
peeled pad usually means the whole PCB is done for. Under the microscope I saw the pad that ripped off
was VINA, a power pin, so I didn't think it could work.

But since I was already there, I soldered the new RT6150 on anyway, hoping the internal connection
between VIN and VINA would save me. I plugged in the USB-C expecting failure, and the 3V3 indicator
came on. I grabbed my multimeter: instead of 3.3 V, the output was exactly 2.35 V, presumably because,
with the VINA pad gone, the power chain is struggling.

Then the crazy part: I plugged it into my laptop, the RP2040 showed up, and it didn't boot-loop. It
seems 2.35 V was just enough for the RP2040 and the flash to communicate. It's hanging on by a
thread, but it transfers data and MicroPython runs.

I'm so relieved; the stress this power delivery has caused me is unreal. Even running at 2.35 V with
one pad gone, the core functionality is there, and I didn't have to sacrifice another RP2040. I'll
probably just tape that part down and never touch it again. (You can see how dim the LED at the
bottom right is.)

<p>
<img src="media/44205-1.jpg" width="70%">
</p>

---

## 51 · Reflow attempt 4: Rukus is alive
<sub>29 Apr 2026 · 5 h 7 min logged</sub>

Five hours straight at my desk today, and it was so worth it. After the jankiness of attempt 3,
hanging on by a thread at 2.35 V with a missing pad, I couldn't live with that in my actual robotics
controller. I needed a fresh, perfect board.

**The reflow.** I took a brand-new PCB, got out the flux and paste, and locked in. I was extremely
careful this time, taking my time with the hot-air gun instead of rushing. It came out cleanly,
honestly the cleanest SMD reflow I've ever done: no solder bridges on the RT6150B, no ripped pads,
nothing. I populated the whole board in one go: the AP62401, RT6150B, RP2040, crystal, flash and the
TB6612FNG motor driver. I plugged in the USB-C, and both the 5 V and 3V3 indicators lit up brightly
straight away. My computer detected the RP2040 instantly, and the MicroPython UF2 loaded first time.
No boot-looping at all. I almost cried.

**Assembly and motors.** With the core finally working, I assembled the rest of the board. Then the
real test: the robotics part. I hooked up my LiPo battery, wired the DC motors to the TB6612FNG
outputs, and wrote a quick MicroPython script to toggle the PWM signals to the motor driver. It
worked flawlessly: the motors spun exactly as programmed, with the power supply taking the battery
voltage and feeding the motors like a champ.

**The diodes, and a strange bug.** The Schottky diodes I added worked perfectly: they let me keep
the board safely powered so I could code and test the motors at the same time without blowing up my
laptop's USB port. The annoying part, which took ages to work out: I couldn't change or upload code
while both the USB-C and the battery were plugged in. Thonny would freeze, and my laptop refused to
save the script to the board. I suspect a voltage conflict, or the power switching interfering with
the RP2040's USB data lines. I spent over an hour debugging and checking the schematic, then let it
go, because it isn't a big deal: I just unplug the battery to flash new code and plug it back in to
run the motors.

I'm so happy. After frying chips, shorting 5 V to GND, endless boot loops and weeks of ripped pads,
Rukus is finally alive and fully functional. Time to build the pocket robot chassis for it.

<p>
<img src="media/44859-1.jpg" width="70%">
</p>

---

## 52 · Final testing and small bug fixes
<sub>30 Apr 2026 · 5 h 21 min logged</sub>

**The I2C screen.** I hooked up SDA and SCL and ran a simple I2C scanner script in MicroPython, and
it returned nothing. I spent almost an hour checking continuity between the RP2040 pads and the pin
headers, convinced I had fried the I2C bus during reflow. It turned out the jumper wires were broken
internally. With new wires, the screen came on and displayed my text perfectly.

**Ultrasonic sensor and servo.** The servo was easy: some simple PWM code and it swept perfectly.
The ultrasonic sensor returned random garbage values. It took another hour to remember that the
sensor's echo pin outputs a 5 V signal while the RP2040 only takes 3.3 V logic; I almost burnt my
MCU again. A voltage divider on the breadboard fixed it, and it measured distances flawlessly.

**The final boss: DC motors on 12 V.** I was sweating for this one. I connected the 12 V supply and
kept my eyes glued to the multimeter on the TB6612FNG outputs, making sure it wasn't somehow putting
12 V onto the 3V3 logic lines. The power chain held up. I wrote a script to ramp up the PWM, and the
motors spun up so hard the board almost flew off my desk.

**Small bugs.** The last two hours went on fixing small bugs. Whenever the motors drew too much
current on startup, the I2C screen would glitch and freeze. Watching the rails dip on the
multimeter, I realised I needed much bigger decoupling capacitors to smooth out the spikes.

My brain is fried from five hours of staring at tiny wires, but everything works: all the GPIOs, the
12 V power delivery, the sensors, everything. I can actually build the robot now.

▶ [Video (8.7 MB)](media/45226-1.mp4)

---

## 53 · Wiring clean-up and a stress-test bug
<sub>30 Apr 2026 · 1 h 34 min logged</sub>

Only an hour and a half logged today, but it felt endless, trying to make this usable rather than a
ticking time bomb of loose jumper wires.

**Ditching the breadboard.** First I made the ultrasonic sensor's voltage divider permanent. I spent
about 45 minutes soldering two tiny SMD resistors inline with the jumper wires, because I didn't want
a bulky perfboard hanging off the robot. Heat-shrinking those tiny joints without melting the wires
was a nightmare, but it looks much cleaner now. I also crimped and soldered proper JST connectors for
the battery and the 12 V DC motors. My fingers are numb from the tiny crimps, but no more alligator
clips accidentally shorting things out.

**The stress test.** With everything hard-wired, I ran a full-system stress test: a MicroPython
script driving the I2C screen, polling the ultrasonic sensor, sweeping the servo and running the DC
motors at full speed, all at once. The ultrasonic sensor immediately started returning garbage
again; it said my hand was 400 cm away when it was right in front of the sensor. After about 20
minutes staring at the serial monitor, I realised it was electrical noise from the motors interfering
with the sensor's echo pin: the motors were spraying EMI across the board. Another 20 minutes to fix
it, by twisting the motor wires tightly together to cancel the noise and adding a small ceramic
capacitor across the motor terminals. Tested again, and it works flawlessly even at full throttle.

Getting everything to behave together under full load is frustrating, but the hardware is finally
rock solid. We're officially ready to mount it on a chassis. I also recorded a video showing how many
times I failed while making Rukus. This is the last devlog.

▶ [Video (10.2 MB)](media/45227-1.mp4)
