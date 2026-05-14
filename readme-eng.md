# Vanguard Keyboard Outsider

![info](pic/outsider/info.jpg)

<br>

## Preface

The "Vanguard Series" is a branch point I personally developed based on the "Explorer No.3" architecture. This is because, in the design of Explorer No.3, I "modularized" the keyboard's MCU area, allowing its trackpad and `ProMicro` MCU module to be completely reused in subsequent works.

- Why is this keyboard number two?

The codename for Vanguard No.1 is `Mono`, and its Chinese codename is "Twin Sisters". Considering that people were eagerly asking for the development of zmk firmware, I started another keyboard first to try and research zmk, taking the opportunity to create a wireless version of the firmware. Once successful, I will go back to complete the development of "No.1".

——Of course, there is also the development of the "Explorer No.3" `v2` and its wireless firmware version.

Because it is based on the "Explorer Series", although Vanguard is somewhat restricted to developing in the "modular" area, I will not abandon the degree of freedom of the "Explorer". Conversely, this freedom actually caused some confusion for me during development, which was a minor accident.

`Plank` has always been the progenitor of ortholinear layouts that I really like, and No.`3` is also a keyboard developed based on `Plank`. My idea is very simple:

——Starting from the most primitive, there will definitely be better creativity.

<br>

- Auxiliary references:
    - `about-zmk-XXX` tutorial text.
    - `QMK Firmware`: https://qmk.fm/
    - `VIAL`: https://get.vial.today/
    - `ZMK Firmware`: https://zmk.dev/
    - `KLE`: https://www.keyboard-layout-editor.com/#/
    - `nRF Connect SDK`:
        - `Android` Link: https://play.google.com/store/apps/details?id=no.nordicsemi.android.mcp&pcampaignid=web_share
        - PC Version: https://www.nordicsemi.com/Products/Development-tools/nrf-connect-for-desktop/download
        - Used for initial Bluetooth signal capture and debugging.
    - `FreeCAD`.

<br>

## Hardware Design

### Degrees of Freedom and Limitations

The main reason I designed the `Outsider` into this unique appearance is the retention of "degrees of freedom". So where are its degrees of freedom? The answer is very simple——

——It can be used by flipping it "arbitrarily".

And it is an extended design based on Explorer No.3, which means its MCU area is inherited. The bill of materials will specify which components directly use the design files of "Explorer No.3".

<br>

Tai Chi theory has a core concept that "things turn into their opposites when they reach the extreme". The same applies to any design. This keyboard provides the ultimate degree of freedom. Its "current" limitations are listed below:
1. Supports soldering.
2. Does not support `Gateron Low Profile 2.0`, aka `GLP2.0`.
3. The trackpad only supports the `QMK` wired version.

<br>

The parts regarding degrees of freedom based on the "limitations" are listed below:
1. Supports `MX` footprint "switches", not limited to "low-profile switches".
2. Supports `GLP3.0` low-profile switches.
3. Supports `Choc` footprint "switches".
4. Supports flipped assembly of all PCB and case components.
5. Supports `zmk` encoder wireless version.
6. Supports `qmk` `Azoteq TPS43` and `Cirque TM040040` trackpads and encoder wired version.
7. Supports multi-layout soldering adjustments.
8. Supports physical board-splitting assembly for use.

<br>

### Circuit Design

#### Core PCB

<table>
  <tr>
    <td width="100%">
      <img src="pic/outsider/A.png" width="100%" alt="A">
    </td>
  </tr>
  <tr>
    <td width="100%">
      <img src="pic/outsider/B.png" width="100%" alt="B">
    </td>
  </tr>
</table>

<br>

#### Positioning Plate, MCU Cover Plate, and Bottom Plate

<table>
  <tr>
    <td width="50%">
      <img src="pic/outsider/B1.png" width="100%" alt="B1">
    </td>
    <td width="50%">
      <img src="pic/outsider/M.png" width="100%" alt="M">
    </td>
  </tr>
  <tr>
    <td width="50%">
      <img src="pic/outsider/P1.png" width="100%" alt="P1">
    </td>
    <td width="50%">
      <img src="pic/outsider/P2.png" width="100%" alt="P2">
    </td>
  </tr>
</table>

<br>

#### MCU and Modules

<table>
  <tr>
    <td width="50%">
      <img src="pic/outsider/T1.png" width="100%" alt="T1">
    </td>
    <td width="50%">
      <img src="pic/outsider/T2.png" width="100%" alt="T2">
    </td>
  </tr>
  <tr>
    <td width="50%">
      <img src="pic/outsider/T3.png" width="100%" alt="T3">
    </td>
    <td width="50%">
      <img src="pic/outsider/T4.png" width="100%" alt="T4">
    </td>
  </tr>
</table>

<br>

#### 3D Printed Parts

<table>
  <tr>
    <td width="50%">
      <img src="pic/outsider/T5.png" width="100%" alt="T5">
    </td>
    <td width="50%">
      <img src="pic/outsider/T6.png" width="100%" alt="T6">
    </td>
  </tr>
</table>

<br>

## Materials Used

The materials used part is relatively simple. `Outsider` does not have excessive `3D` designs and consistently retains the "sandwich" structure.

||Name|Quantity|Quantity||
|--|--|--|--|--|
|⚠️ **PCB**|**Layout**|**4x10**|**4x12**|**Remarks**|
||Main PCB|1|1|Thickness `1.2mm`|
||Bottom Plate|1|1|Thickness `1.6mm`|
||`4x8` Positioning Plate|1|1|**See notes below**|
||`4x2` Positioning Plate|1|2|**See notes below**|
|`TM040040`|Module `A`|1|1|**No.3 Component**|
|`TM040040`|Module `B`|1|1|**No.3 Component**|
|`TM040040`|Module `C`|1|1|**No.3 Component**|
|`TPS43`|Module `D`|1|1|**No.3 Component**|
|⚠️ **MCU**|**Footprint**||||
|`ATMega32U4`|`ProMicro`|1|1|**Basic Wired Version**|
|`RP2040`|`ProMicro`|1|1|**Wired Trackpad**|
|`nRF52840`|`ProMicro`|1|1|**Wireless Version (Supports Encoder)**|
|⚠️ **Electronic Components**|**Name/Specification**||||
|`1N4148`|`SOD-123`|40-41|48-49||
|`EC-11`/`EC-12`|`L10mm` `A4.5-5.0mm`|1|1|**Optional**|
|Tactile Switch|`6x3.5x4.3mm` `2 Pin`|1|1||
|Tactile Switch|`SKRK` `2mm`|1|1|**Optional**|
|Slide Switch|`MSKT-12D14`|1|1|**Optional**|
|Under `501735`|`3.7V` Li-Po Battery|1|1|**Optional, placed above MCU**|
|-|`3.7V` Li-Po Battery Level Display Module|1|1|**Optional**|
|`PH2.0` `THT` Male/Female Connector|`JST PH S2B-PH-K`|1|1|**Optional**|
|💡 **TM040040**|||||
|**Name**|**Specification**||||
|Trackpad|-|1|1|**Optional**|
|Resistor|`1/4W` `4.7K`|2|2||
|FFC Cable|`0.5x5mm` `FFC 12pin` `AA`|1|1||
|FFC Connector|`0.5mm` `FFC 12pin`|1|1|**Any flip cover type**|
|Screw|`M2x3`|20|24||
|Screw|`M2x6`|4|4||
|Standoff|`M2x5` `ø3.05-4mm`|6|8|**Low-profile switch height**|
|Standoff|`M2x6` `ø3.05-4mm`|4|4|**Recommended height**|
|Standoff|`M2x7` `ø3.05-4mm`|2|2|**Wired MCU**|
|Standoff|`M2x10` `ø3.05-4mm`|2|2|**Wireless MCU**|
|💡 **TPS43**|||||
|Trackpad|-|1|1|**Optional**|
|FFC Cable|`0.5x5mm` `FFC 6pin` `AA`|1|1||
|FFC Connector|`0.5mm` `FFC 6pin`|1|1|**Any flip cover type**|
|Screw|`M2x3`|20|24||
|Screw|`M2x6`|4|4||
|Standoff|`M2x5` `ø3.05-4mm`|6|8|-|
|Standoff|`M2x6` `ø3.05-4mm`|4|4||
|Standoff|`M2x8` `ø3.05-4mm`|4|4|**Recommended height**|
|Standoff|`M2x7` `ø3.05-4mm`|2|2|**Wired MCU**|
|Standoff|`M2x10` `ø3.05-4mm`|2|2|**Wireless MCU**|
|📌 **3D Printed Parts**|||||
|`TPS43`|Touch contact surface|1|1||
|`EC-11`|`ø39mm`, `L4.5mm` Encoder cap|1|1||
|📌 **Others**|||||
|Self-adhesive `Poron` foam|Thickness `2mm`|-|-||
|Self-adhesive silicone feet|`ø8mm` `0.5-1mm`|4|4||

<br>

## Precautions

### Components

- If you want to install the `EC-11` encoder, please choose the module that supports `TM040040`.

- The above standoff specifications are primarily for "low-profile switch" configurations; if using standard `MX` switches, the height specifications must be completely adjusted to at least `M2x7mm`.

- The positioning plate for standard `MX` switches uses a thickness of `1.6mm`; low-profile switches uniformly use a thickness of `1.2mm`.

- Prepare "at least" `40` or `48` switches or more; choose the type according to your own preference.

<br>

### Assembly

- Soldering iron operation involves high heat; be sure to pay attention to electrical and fire safety.

- Rosin and solder are highly toxic substances; be sure to ensure environmental ventilation.

- It is recommended to wear gloves, masks, and safety goggles during operation to ensure safety.

- Operation technique recommendations:
  - Soldering iron: constant temperature control between `250`-`330` degrees Celsius.
    - `K`.
    - `BC`.
  - Solder: `183` degrees Celsius.
  - Wires:
    - Specification at least `24`-`28` `AWG`.
    - Colors must clearly distinguish at least `2` colors.

- Operation tools:
  - Crimping pliers:
    - Need to be able to handle `PH2.0` specifications.
    - Prepare some extra metal terminals as spares.
    - Adjust the pressure latch first before crimping.
  - Wire strippers:
    - Used for the Li-Po battery and its battery level display module.

- Trackpad options:
  - `TM040040`:
    - Requires soldering `4.7K` resistors.
    - Requires removing the `R1` resistor on the trackpad yourself.
    - Tutorial references:
      - [40mm Cirque GlidePoint Circle Trackpad Module DIY Kit for Split Mechanical Keyboard](https://shop.beekeeb.com/product/40mm-cirque-glidepoint-circle-trackpad-module-diy-kit-for-split-mechanical-keyboard/)
      - [GlidePoint Cirque Trackpad TM040040 TM035035](https://keycapsss.com/keyboard-parts/parts/211/glidepoint-cirque-trackpad-tm040040-tm035035)
  - `TPS43`: No need to solder resistors.

<br>

![](pic/guide/m1.png)

- Regarding jumpers:
  - You only need to solder one side of the PCB.
  - `PWR`:
    - Always choose the `3V3` power supply position based on the `MCU`.
    - Usually, `3V3` is located at the `P1` position.
  - `VCC`:
    - Use jumper: solder it when assembling the `TM040040` trackpad.
    - Do not solder jumper: you can replace `TM040040` with `TPS43` without desoldering the `4.7K` resistors.

<br>

<table>
  <tr>
    <td width="50%">
      <img src="pic/guide/m2.jpg" width="100%" alt="m2">
    </td>
    <td width="50%">
      <img src="pic/guide/m3.jpg" width="100%" alt="m3">
    </td>
  </tr>
</table>

- Regarding `MCU`:
  - Install according to the `MCU`'s pinout diagram.
  - Refer to the image above and use the direction with "`2` `GND`s" for reading, which makes it less likely to install backwards.

<br>

### Icon Identification

<table>
  <tr>
    <td width="40%">
      <p>Diodes are directional.</p>
      <p>Note the marking line at the front of the diode.</p>
    </td>
    <td width="30%">
      <img src="pic/guide/d1.png" width="100%" alt="d1">
    </td>
    <td width="30%">
      <img src="pic/guide/d2.png" width="100%" alt="d2">
    </td>
  </tr>
  <tr>
    <td width="40%">
      <p>Resistors are not directional.</p>
      <p>Please install the resistors on the same side as the "MCU".</p>
    </td>
    <td width="30%">
      <img src="pic/guide/r1.png" width="100%" alt="r1">
    </td>
    <td width="30%">
      <img src="pic/guide/r2.png" width="100%" alt="r2">
    </td>
  </tr>
</table>

<br>

## Assembly Method

After reviewing the aforementioned specifications and entering the physical assembly stage, please pay special attention to the following challenging parts:
- Soldering technique:
  - Temperature control.
  - `3`-second rule.
  - `0.5mm` `FFC` connector.
- Debugging technique:
  - `0/1` rule.
  - Anti-short circuit rule.

<br>

<table>
  <tr>
    <td width="38%">
      <img src="pic/info/layout2.png" width="100%" alt="layout2">
    </td>
    <td width="62%">
      <img src="pic/info/layout3.png" width="100%" alt="layout3">
    </td>
  </tr>
</table>

<br>

Take out the PCB, and then think for a moment:
  - What should the layout look like?
  - Is the keyboard wired or wirelessly driven?
  - What devices can I equip?

<img src="pic/guide/g0.jpg" width="100%" alt="g0">

<br>

Once you have finished thinking about the above questions, you can begin the formal assembly.

> Note: Previously, I would incline towards asking everyone to flash the firmware into the MCU first, but here I must make a minor correction.

<br>

### Common Parts

> Note: In some photos, wired and wireless versions are used interchangeably, but it will not affect the installation.

Based on your ideal layout, first solder the diodes.

<img src="pic/guide/g1.jpg" width="100%" alt="g1">

<br>

Then solder all the peripheral components you will need:
  - `EC-11` or trackpad's `FFC connector`, `1/4W 4.7K resistors`.
  - Jumpers.
  - `6x3.5x4.3mm` `2 Pin` tactile switch.
  - Wireless version:
    - `MSKT-12D14` slide switch.
    - `SKRK` `2mm` tactile switch.
    - `PH2.0` female connector.

<img src="pic/guide/g2.jpg" width="100%" alt="g2">

<br>

Decide whether to flash the firmware first or solder it first based on the `MCU` you chose.
- `RP2040`:
  - Most versions perform the flashing action first.
  - `Sea-Picro` can be soldered and fixed first before flashing the firmware.
  - The method for flashing the firmware is generally "holding the `BOOT` button", plugging in the device, and then flashing the `uf2` firmware into it.
- `nRF52840`:
  - Solder and fix it first before flashing the firmware.
  - After connecting the device, trigger the `RST` button "`2` times" consecutively, and then flash the `uf2` firmware into it.

<img src="pic/guide/g3.jpg" width="100%" alt="g3">

<br>

After the `MCU` is soldered and fixed, and the firmware is flashed in, do not rush to solder the switches in the next step. You need to do a few things:
1. Confirm if your keyboard matrix is working, which means whether all your keys trigger.
  - `VIAL`'s `Matrix Tester`.
  - `ZMK Studio` — the `about-zmk-XXX` tutorial text will explain this.
  - Third-party key tester: https://hwtest.in/keyboard
2. Confirm if your modules are driving normally.
  - `EC-11` encoder.
  - Pointing device firmware operation for the trackpad.

If all the above is fine, and you are installing a trackpad, remember to unplug the FFC cable first before soldering the switches; if it is the `EC-11` encoder, just solder the switches directly.

<img src="pic/guide/g4.jpg" width="100%" alt="g4">
<img src="pic/guide/g5.jpg" width="100%" alt="g5">

> Note: The `EC-11` can be bare-tested directly without soldering, which is the operation shown in my photo illustration.

<br>

### Trackpad

#### TPS43

For the `TPS43` part, first you need to prepare the "touch surface" and "Module `D` (frame)". First, peel off the `3M` tape on the `TPS43` and combine it with the touch surface to fix it in place.

<img src="pic/guide/g6.jpg" width="100%" alt="g6">
<img src="pic/guide/g7.jpg" width="100%" alt="g7">

<br>

After installing the standoffs on the entire module, connect the FFC cable, and then fix the MCU cover plate together with the module. After that, you can install the bottom plate to finish up.

<img src="pic/guide/g8.jpg" width="100%" alt="g8">
<img src="pic/guide/g9.jpg" width="100%" alt="g9">

<br>

#### TM040040

For the `TM040040` part, you need to first assemble the main body with "Modules `A`, `B`, and `C`", and then install the standoffs to complete the entire module.

<img src="pic/guide/g10.jpg" width="100%" alt="g10">
<img src="pic/guide/g11.jpg" width="100%" alt="g11">
<img src="pic/guide/g12.jpg" width="100%" alt="g12">

<br>

For the installation method of the entire `TM040040` module, you must first connect the `5mm` FFC cable and then pass it through the hole.

<img src="pic/guide/g13.jpg" width="100%" alt="g13">

> The final completed installation diagram will be exactly in an "`S`" shape, just like in the "tutorial".

![](https://camo.githubusercontent.com/b15feb1fb0ef73124fbc38e109d9254808041f45607b13747b30ec43c3a2b845/68747470733a2f2f6265656b6565622e636f6d2f747261636b7061642f736964652e6a7067)

<br>

### Regarding the Battery

From here, I will explain some very important matters regarding the wireless version:

#### Li-Po Battery

The specification is marked as a `501735` `3.7V` polymer Li-Po battery. Its size limitation is based on being able to fit in the area above the `ProMicro` MCU.

<img src="pic/guide/g14.jpg" width="100%" alt="g14">

> Precautions: Your Li-Po battery will usually have insulation treatment like this after unboxing. After removing the insulation protection, pay special attention not to let these `2` "positive and negative" wires touch each other. This will cause your battery to purely discharge on the wires, ultimately leading to a fire. Be extremely careful when handling it.

<br>

First, you need to crimp the `PH2.0` metal terminals. Again, ensure that the "positive and negative poles" do not touch each other. Then take a multimeter, switch it to the "`20V`" setting, and directly test the "positive and negative poles" to check:
1. Whether the terminals are successfully crimped.
2. Whether the battery has power.

<img src="pic/guide/g15.jpg" width="100%" alt="g15">

<br>

After confirming there are no issues, electricity is conducting, and the battery has power, you can install the male connector.

<img src="pic/guide/g16.jpg" width="100%" alt="g16">

<br>

#### Li-Po Battery Level Display Module

Due to the lack of spare pins to implement an `LCD` battery level display, `Outsider` can only "settle for the next best thing" and use a hardware-operated display module. This little thing is very cheap to buy in online stores, easy to buy, and won't be out of stock.

<img src="pic/guide/g17.jpg" width="100%" alt="g17">
<img src="pic/guide/g18.jpg" width="100%" alt="g18">

<br>

The assembly method is very simple. There are "`+`" and "`-`" markings on the top of the display module. Solder easily identifiable wires to them respectively, and then connect them to the corresponding interfaces on the side of the PCB.

> Operation and testing method:
> 1. The slide switch must be turned on first, this is very important.
> 2. Only then can you press the tactile switch on the side. At this time, the battery level on the display module will light up.

<table>
  <tr>
    <td width="50%">
      <img src="pic/guide/g19.jpg" width="100%" alt="g19">
    </td>
    <td width="50%">
      <img src="pic/guide/g20.jpg" width="100%" alt="g20">
    </td>
  </tr>
</table>

<br>

Finally, after fixing the battery and the display module with adhesive, the installation is complete.

<img src="pic/guide/g21.jpg" width="100%" alt="g21">

<br>

#### Bottom Plate and Finishing Up

The final step is to install the standoffs on the bottom plate, and then fix the bottom plate together with the main keyboard body.

Then put on the keycaps, and you're done.

<table>
  <tr>
    <td width="50%">
      <img src="pic/guide/e1.jpg" width="100%" alt="e1">
    </td>
    <td width="50%">
      <img src="pic/guide/e2.jpg" width="100%" alt="e2">
    </td>
  </tr>
  <tr>
    <td width="50%">
      <img src="pic/guide/e3.jpg" width="100%" alt="e3">
    </td>
    <td width="50%">
      <img src="pic/outsider/info.jpg" width="100%" alt="info">
    </td>
  </tr>
</table>

<br>

## References

- Main Design Tools:
  - `Google Gemini` Assistance.
  - `KiCAD v9.0.0`.
  - `FreeCAD`.
- References:
  - `Rotray Encoder` `EC10E`.
  - `Rotray Encoder` `EC11`.
  - `Cirque TM040040`.
  - `TPS43`-`TPS65-201A-B`.
  - `ProMicro nrf52840`.
  - `RP2040 ProMicro`.
