# ZMK Firmware Environment Initialization Tutorial

## Preface

`zmk firmware` is the most well-known wireless keyboard firmware in the custom keyboard community, but it has always lacked a "complete" and "clean" environment setup process. Therefore, the moment I first entered `zmk`, I decided to simultaneously "execute" the environment setup and write down this "step-by-step" operation tutorial.

I am personally not an expert in the three major engineering fields of firmware, hardware, or circuit design. Thus, when approaching such a hardcore electronic project, I must definitely follow the official operation tutorials, combined with my own hands-on debugging attempts, to establish the execution `SOP` step by step. This is my "Open Source Spirit."

Of course, I will try my best to write the tutorial as plainly and detailedly as possible. I hope this tutorial text can perfectly integrate the "three major fields" I am familiar with, and deliver it to you, the reader, word for word, reducing some of the disappointment and frustration you might feel towards "open-source custom keyboards."

—After all, the excitement at the moment of lighting up your own keyboard is unparalleled.

(`DreaM117er`, Timestamp: `2026.05.09`)

<br>

## About This Tutorial

This tutorial text is suitable for the "following" audiences:
1. Complete beginners and novices.
2. Users whose computers are experiencing anomalies after following other online tutorials.
3. Users who successfully established a personalized environment following other online tutorials, but cannot find out how to solve encountered errors.
4. Engineers from the three major professional fields.

Operating environment of this tutorial: `Ubuntu`, `Mint`.
This tutorial is attached to the `Vanguard-Keyboard-Outsider` project, therefore it will borrow the design file belonging to that keyboard—`outsider.pdf`

<br>

## Differences Between qmk and zmk

The architecture of `zmk` is completely different from `qmk`—`qmk` uses a "Global Data Polling Mechanism," while `zmk` uses a "Tree-structured Data Encapsulation and Transmission Mechanism." We must understand the differences here:
1. The keyboard main controller "`MCU`" in `qmk` will periodically and sequentially "knock on the door" of the hardware to ask if anyone has been pressed or executed. At the same time, your "`Host`" will also periodically knock on the `MCU` to request data.
2. The `zmk` firmware uses `Zephyr RTOS`, an industrial-grade real-time operating system, to encapsulate your operations on a single keyboard into data and send it to the "`Host`." Therefore, even a split keyboard will be treated as a "single device," which is the same principle as connecting your "Bluetooth headphones."

> The "`Host`" explained here generally refers to the device being "connected or linked" to be "operated or controlled."

`Master`, `Slave`—which is the "primary" and "secondary" relationship of the devices. In `qmk`, you can freely choose to define "Left" or "Right"; in `zmk`, the default is usually based on "which side connects to the computer via Bluetooth" as the `Master`.

This makes it easier for everyone to understand the logic, and you will also more easily understand what the official `zmk` text is writing about.

<br>

## Creating a Local zmk Galaxy

### What is a zmk Galaxy?

1. First, you need to prepare your `GitHub` account and `fork` the official personalized configuration file to your project directory (e.g., `XXX-zmk-config`)
—[https://github.com/zmkfirmware/unified-zmk-config-template](https://github.com/zmkfirmware/unified-zmk-config-template)
2. Then operate the terminal, navigate to your default local `GitHub` repository directory, and execute the `zmk` installation process: [https://zmk.dev/docs/user-setup](https://zmk.dev/docs/user-setup)
3. When the installation process reaches `zmk init`, enter the `repo` URL of the `XXX-zmk-config` GitHub personalized configuration file you just created into `Repository URL`, and the system will help you set up the `config` directory.
4. Next, you can choose to enter `Y` to use an existing `zmk firmware` configuration file to change the `keymap` configuration for the keyboard you bought online.
5. If you choose `N`, execute `zmk cd` in the terminal to navigate to the default `config` folder directory.

Having executed up to here, we have successfully set up the "local" `zmk` galaxy environment. Next, I will let you understand the universe architecture of `zmk`, please continue reading below.

<br>

Currently, our `XXX-zmk-config` architecture diagram looks like this:

- `boards/shields`
    - The internal subfolders are the locations of all keyboards in the local `zmk` galaxy, which will be explained later.
- `config`
    - The folder sets the main rules of the galaxy, such as global variable settings, Bluetooth transmission power, deep sleep mode, etc.
    - The `west.yml` file is used to select which version of the `zmk` firmware you want to use for flashing.
    - `zmk.conf`.
- `zephyr`
    - `module.yml` is responsible for declaring to the `zephyr` compilation system that "this galaxy is an independent module."
- `build.yaml`
    - The command script for `GitHub Actions` (`CI/CD`) to consult, defining "who acts as whose master control, outputting as a `.uf2` firmware file."

<br>

## Prerequisites

### Installing Dependencies

If you are a pure novice wanting to enter `zmk`, you will overlook a lot of software engineering details. This is also what I discovered while writing the text. Here I will summarize them step by step:

1. Execute update in the terminal (different distributions have their own differences):

``` bash
sudo apt update
```

2. Install `Python3` and `pip3`:

``` bash
sudo apt install python3 python3-pip
```

3. Install `west`:

``` bash
pip3 install west
```

After mounting is complete, our XXX-zmk-config folder will look like this:

![dir-tree2](pic/info/dir-tree2.png)

<br>

<br>

### Dependency Error Handling

If executing `pip3 install west` fails, please proceed as follows:

1. Install the virtual environment build tool:

```bash
sudo apt install python3-venv

```

2. Under the `cd` directory, create an isolated environment for `zmk-env`.

```bash
python3 -m venv ~/zmk-env

```

3. Activate the isolated environment, you will notice an extra `(zmk-env)` added to the front of your terminal.

```bash
source ~/zmk-env/bin/activate

```

4. Execute the `pip3 install west` command once more.

> Precaution: Every time you open the terminal and prepare to compile `zmk`, you must first execute `source ~/zmk-env/bin/activate` to activate the environment before you can use `west build`.

<br>

### Flashing Tool Setup

#### west.yml Configuration

I can't think of what to use as a subtitle, being straightforward is better: As mentioned earlier, "the `west.yml` file is used to select which version of the `zmk` firmware you want to use for flashing." I thought about it and still feel this part must be completed *before* "adding a new keyboard," otherwise, after all the firmware files are set up, you will be staring at your terminal waiting for a long time to flash the `uf2` firmware.

1. Execute `zmk cd`, enter the `config` folder, and open the `west.yml` file; it will present an architecture like this:

```yaml
manifest:
  defaults:
    revision: v0.3 # Change v0.3 to main
  remotes:
    - name: zmkfirmware
      url-base: https://github.com/zmkfirmware
    # Additional modules containing boards/shields/custom code can be listed here as well
    # See https://docs.zephyrproject.org/3.2.0/develop/west/manifest.html#projects
  projects:
    - name: zmk
      remote: zmkfirmware
      import: app/west.yml
  self:
    path: config

```

You will find that `revision` is mounted as `v0.3` by default; here we need to change this part to `main`, then save and close it.

2. Then open the terminal, and under the `zmk cd` folder, execute the `west update` command to declare to the `zmk` universe that I want to use `main` to directly flash the firmware.

```bash
zmk cd
west update

```

For the first time executing the `west update` command, just wait slowly for `git` to finish running it.

#### .gitignore Ignore List

After executing `west update`, the `west` manager will directly mount all the dependency packages of `zmk` from the `git` repository into the `XXX-zmk-config` folder, but it will not help you configure which files and folders need to be filtered out. If you forcefully execute `git push`, you will find that the dependency packages are all inside the `update list`, and warnings will pop up because the dependency packages are not files of a few `MB` in size, but several `GB`.

1. First, locate the `zmk cd` main folder, and then add `.gitignore`.

``` bash
zmk cd
touch .gitignore
```

2. Next, open `.gitignore`, copy the data list below into it, then save and close it.

``` .gitignore
# Ignore basic west and zmk
.zmk/
.west/

# Ignore all dependency packages downloaded by west
/zephyr/
/zmk/
/modules/
/optional/

# Ignore garbage and firmware files generated by local compilation
/build/

# Ignore Python virtual environment
/zmk-env/
.venv/
```

3. Execute the upload of the `.gitignore` ignore list.

```
git add .gitignore
git commit -m "add .gitignore in config setting"
git push
```

After the list is configured, executing `git push` will no longer report errors, unless you `compile` a new firmware, only then will there be new files listed in the `git` upload list.

<br>

#### Importing the Zephyr Engine

The function of `.gitignore` has already been mentioned above, so I won't repeat the explanation here.

What about the function of `west update`? Its purpose means "assisting you in moving several `GB` of pure text files (source code) from `GitHub` to your hard drive," but it does not possess the function to convert them into `uf2` firmware. Since `zmk` is an embedded system `OS` running based on `Zephyr RTOS`, we must mount its dependency packages:

* `CMake` / `Ninja`.
* `Python` libraries.
* `Zephyr SDK`.

<br>

1. Ensure your terminal environment is in a "compilable" state, and execute the "install `CMake` / `Ninja`" step:

> Note: `(zmk-env)`, `source ~/zmk-env/bin/activate`

``` bash
sudo apt install -y cmake ninja-build device-tree-compiler wget xz-utils
```

2. Lock onto the `XXX-zmk-config` folder and execute:

``` bash
zmk cd
pip install -r zephyr/scripts/requirements.txt
```

3. Register the `Zephyr` engine path with `CMake`:

``` bash
west zephyr-export
```

4. Install `Zephyr SDK` (`uf2` compiler):

```bash
cd ~
wget https://github.com/zephyrproject-rtos/sdk-ng/releases/download/v0.16.5-1/zephyr-sdk-0.16.5-1_linux-x86_64.tar.xz
tar xvf zephyr-sdk-0.16.5-1_linux-x86_64.tar.xz
cd zephyr-sdk-0.16.5-1
./setup.sh -t arm-zephyr-eabi -c
```

> Caution: After `setup.sh` is executed, it will automatically help you configure the `ARM` compiler, which is specially used to deal with the `nRF52840` chip.

#### Firmware Compilation

5. In this way, the entire `zmk` universe and compilation environment are fully mounted. If you want to create `uf2` firmware later, you must lock the folder to the `zmk/app` subdirectory under the `XXX-zmk-config` directory.

``` bash
zmk cd
cd zmk/app
```

Then execute the `west build` command:

``` bash
west build -p -b nice_nano -- -DSHIELD=<custom_keyboard> -DZMK_CONFIG="[Your XXX-zmk-config folder path]"
```

> Super important: The system will build the `uf2` firmware inside the `~/XXX-zmk-config/zmk/app/build/zephyr` folder, a file named `zmk.uf2`.

<br>

## Tutorial Begins

### A. The Birth of a Planet

After we have pioneered the `zmk` galaxy and completed all the prerequisite settings as mentioned above, this `zmk` galaxy is currently very clean and has nothing in it. We need to give birth to a new "planet" in this galaxy, which also corresponds to the operation methods of points `4-5` in "What is a zmk Galaxy?" above:

* Existing `zmk` universe settings, imported from the cloud to your computer (galaxy) for configuration modification.
* Creating a "new keyboard" from `0` (creating a planet in the galaxy).

Here I will not discuss the part about importing cloud settings; the focus is on the "new keyboard."

<br>

1. Execute `zmk cd`, then execute the following under the directory:

``` bash
cd boards/shields
mkdir <custom_keyboard>
touch Kconfig.shield <custom_keyboard>.overlay <custom_keyboard>.keymap <custom_keyboard>.conf
```

> First create the "planet (`dir`)", "flag (`Kconfig`)", "operating core (`.overlay`)", "functional core (`.keymap`)", and "Bluetooth settings (`.conf`)" files.

2. Then open `Kconfig.shield`, write in the code, and save:

``` markdown
config SHIELD_CUSTOM_KEYBOARD
    def_bool $(shields_list_contains,<custom_keyboard>)
```

The purpose of writing this string of code is: you need to tell your `zmk` universe environment that this `custom_keyboard` keyboard has been born.

> Simply creating the file is useless; if there is nothing inside your `Kconfig.shield` file, how are you going to tell the universe that "this keyboard" exists here?

<br>

### B. Operating Core and Function Setup

After setting the keyboard's "flag," only then will we sequentially create the "operating core" and "functional core." That is, you must first define what method your keyboard uses to drive the `zmk` firmware, and subsequently, build up the keyboard's functions step by step.

Currently, there shouldn't be a need to specifically explain the driving methods, there are only 2 types:

* Development board driven (e.g., `PICO-W` or `SEED XIAO-BLE` series).
* `MOB` driven (`MCU on board`).

Here I will introduce the commonly used development board drive, which uses the `nRF52840` chip development board (including `nice!nano v2`) as an example for introduction. First, we need to prepare a few things:

1. The development board itself.
2. Development board specifications and pinout definition table.
3. Keyboard circuit board schematic (`PDF` file or design drawing).
4. Actual keyboard matrix diagram (`KLE` or design drawing).

<br>

#### Development Board, Specifications, and Pinout Definition Table

<table>
  <tr>
    <td width="50%">
      <img src="pic/info/rp2040.jpg" width="100%" alt="rp2040">
    </td>
    <td width="50%">
      <img src="pic/info/nrf52840.png" width="100%" alt="nRF52840">
    </td>
  </tr>
</table>

For standard development boards using `ProMicro` footprints, assuming you want to develop a circuit board (`PCB`) shared for both wireless and wired use, then you will definitely use this pinout diagram. Here are a few of the most commonly used functions from these two diagrams:

* Cross-referencing the `GPIO` distribution on the front and back.
* Globally cross-referencing what function each `GPIO` has.

> Pay special attention to the positions and magnitude values of "Voltage (`VCC`)" and "Ground (`GND`)". This is extremely important in circuit design; using the wrong design method can damage your development board and circuit board.

> The biggest difference between `nice!nano v2` and `clones` is reflected in the magnitude of "`VCC`". If I remember correctly, the `VCC` output of early `nice!nano` development boards was `4.3V`, not `3V3`.

Let everyone know a few very simple concepts in circuit design:

* If the output voltage is wrong, it will go on strike, or burn out.
* If the wires are not connected correctly, it will go on strike, or burn out.

> Incorrect = strike or burn out.

It's just that simple.

<br>

Although we are not discussing the `RP2040 ProMicro` development board here, I still need to explain to everyone "why" I am bringing out a wired keyboard development board.

1. You need to know the actual physical locations and voltage values of `VCC` and `GND`.
2. Which `GPIO` the identical `I2C` communication channels are located on; for example, the locations of `I2C0` `SDA`/`SCL` are on `GP0-1`.
3. Likewise, which `GPIO`s the `SPI` communication pins are located on. Taking the `SPI` channel will cost more `GPIO`s to process communication data.

> You need to take the `GPIO` pins of the wired development board and cross-reference them with the pins of the `nRF52840 ProMicro` development board to see if there is an "intersection" of functions, and then prioritize extracting those pins for layout design.

The pins and functions of the `nRF52840 ProMicro` and `nice!nano v2` both adopt the same design method (different components used):

* Output voltage `3V3`, `GND` position is the same as the `RP2040 ProMicro`.

> Very important: All full-function pins can output `I2C` communication. Although all full-functions can, it is recommended to prioritize using the `SDA/SCL` pins marked by default on the development board to reduce the trouble of software definition.

> Very important: `RAW` is the interface for the battery's "positive pole".

<br>

#### Circuit Board Schematic and Actual Matrix Diagram

<table>
  <tr>
    <td width="50%">
      <img src="pic/info/layout.png" width="100%" alt="layout">
    </td>
    <td width="50%">
      <img src="pic/info/mcu-ki.png" width="100%" alt="mcu-ki">
    </td>
  </tr>
   <tr>
    <td width="50%">
      <img src="pic/info/matrix-ki.png" width="100%" alt="matrix-ki">
    </td>
    <td width="50%">
      <img src="pic/info/matrix-ki2.png" width="100%" alt="matrix-ki2">
    </td>
  </tr>
</table>

Here, I will first explain two very common misconceptions:

1. Matrix size "does not equal" the actual matrix diagram.
2. Ideal matrix configuration "does not equal" the actual matrix configuration.

Taking the example `Outsider` keyboard, the keyboard matrix size for a standard `Plank` configuration is `4x12`. My design adds an extra floating key (volume encoder `EC-11`), so the total number of keys is `49`.

* The ideal matrix layout diagram will definitely be `4x12`, which is very reasonable, right?
* In reality, I designed an `8x8` matrix, totaling `64` keys, which is easier to design than a `7x7` matrix, and is also convenient for expansion and routing.

This should explain why, right?

<br>

#### Writing the Operating Core and Function Setup 

Whether in `qmk` or `zmk` firmware, the schematic and matrix diagrams are extremely important when writing configurations, because you need to physically cross-reference "your design" to write in the functions it should have, one by one.

1. First, we open `<custom_keyboard>.overlay`. The example for `Outsider` is below; you can modify your keyboard's `.overlay` file according to the comments.

``` c
#include <dt-bindings/zmk/matrix_transform.h>

/ {
    chosen {
        zmk,kscan = &kscan0;
        zmk,matrix_transform = &default_transform;
    };

    /* Development Board Settings */
    kscan0: kscan_0 {
        compatible = "zmk,kscan-gpio-matrix";
        label = "KSCAN";
        diode-direction = "col2row"; /* Set according to the keyboard's diode direction */

        /* Matrix Columns Pin Setup */
        col-gpios
            = <&pro_micro 10 GPIO_ACTIVE_HIGH> /* col0: PB6 */
            , <&pro_micro 16 GPIO_ACTIVE_HIGH> /* col1: PB2 */
            , <&pro_micro 14 GPIO_ACTIVE_HIGH> /* col2: PB3 */
            , <&pro_micro 15 GPIO_ACTIVE_HIGH> /* col3: PB1 */
            , <&pro_micro 18 GPIO_ACTIVE_HIGH> /* col4: PF7 (A0) */
            , <&pro_micro 19 GPIO_ACTIVE_HIGH> /* col5: PF6 (A1) */
            , <&pro_micro 20 GPIO_ACTIVE_HIGH> /* col6: PF5 (A2) */
            , <&pro_micro 21 GPIO_ACTIVE_HIGH> /* col7: PF4 (A3) */
            ;

        /* Matrix Rows Pin Setup */
        row-gpios
            = <&pro_micro 9 (GPIO_ACTIVE_HIGH | GPIO_PULL_DOWN)> /* row0: PB5 */
            , <&pro_micro 7 (GPIO_ACTIVE_HIGH | GPIO_PULL_DOWN)> /* row1: PE6 */
            , <&pro_micro 4 (GPIO_ACTIVE_HIGH | GPIO_PULL_DOWN)> /* row2: PD4 */
            , <&pro_micro 1 (GPIO_ACTIVE_HIGH | GPIO_PULL_DOWN)> /* row3: PD2 */
            , <&pro_micro 8 (GPIO_ACTIVE_HIGH | GPIO_PULL_DOWN)> /* row4: PB4 */
            , <&pro_micro 6 (GPIO_ACTIVE_HIGH | GPIO_PULL_DOWN)> /* row5: PD7 */
            , <&pro_micro 5 (GPIO_ACTIVE_HIGH | GPIO_PULL_DOWN)> /* row6: PC6 */
            , <&pro_micro 0 (GPIO_ACTIVE_HIGH | GPIO_PULL_DOWN)> /* row7: PD3 */
            ;
    };

    /* Matrix Size Setup */
    default_transform: keymap_transform_0 {
        compatible = "zmk,matrix-transform";
        /* Define matrix size */
        columns = <8>;
        rows = <8>;
        map = <
            /* Actual matrix size */
            RC(0,0) RC(4,0) RC(0,1) RC(4,1) RC(0,2) RC(4,2) RC(0,3) RC(4,3) RC(0,4) RC(4,4) RC(0,5) RC(4,5)
            RC(1,0) RC(5,0) RC(1,1) RC(5,1) RC(1,2) RC(5,2) RC(1,3) RC(5,3) RC(1,4) RC(5,4) RC(1,5) RC(5,5)
            RC(2,0) RC(6,0) RC(2,1) RC(6,1) RC(2,2) RC(6,2) RC(2,3) RC(6,3) RC(2,4) RC(6,4) RC(2,5) RC(6,5)
            RC(3,0) RC(7,0) RC(3,1) RC(7,1) RC(3,2) RC(7,2) RC(3,3) RC(7,3) RC(3,4) RC(7,4) RC(3,5) RC(7,5) RC(7,7)
        >;
        /* RC() format explanation: R = Row; C = Col; Combined code = RC(A, B) */
    };
};
```

> This part is setting up your hardware operating core, so you need to set it in the `.overlay` file, defining the core function "flag," "defining matrix size," and "actual matrix size" for every pin on the development board.

Then, after setting up the "operating core," only then will we set up the "functional core"—that is, "what functions" are on each key in your actual matrix.

> Supplementary note: `zmk` has a dedicated pinout definition table for development boards with `ProMicro` footprints. Please refer to the official documentation page here—https://zmk.dev/docs/troubleshooting/hardware-issues

![ProMicro pinout](https://zmk.dev/assets/images/pinout-4ed4b6eb1e452a7be44c3a0143cd5605.png)

<br>

2. Next, open the `.keymap` file. This will also be where you spend the most time, because everyone's ideal key functions are different.

```c
#include <behaviors.dtsi>
#include <dt-bindings/zmk/keys.h>
#include <dt-bindings/zmk/bt.h>
#include <dt-bindings/zmk/pointing.h> // This part is the database for HID Device functions and key operations

/ {
    keymap {
        compatible = "zmk,keymap";

        // Layer 0: Base
        default_layer {
            display-name = "Default Layer";
            bindings = <
                &kp ESC   &kp Q     &kp W     &kp E     &kp R     &kp T      &kp Y     &kp U     &kp I     &kp O     &kp P     &kp BSPC
                &kp LSHFT &kp A     &kp S     &kp D     &kp F     &kp G      &kp H     &kp J     &kp K     &kp L     &kp SEMI  &kp SQT
                &kp LCTRL &kp Z     &kp X     &kp C     &kp V     &kp B      &kp N     &kp M     &kp COMMA &kp DOT   &kp FSLH  &kp RET
                &mkp RCLK &mkp LCLK &mkp MCLK &kp LALT  &mo 2     &kp SPACE  &kp RSHFT &mo 1     &kp RGUI  &kp LBKT  &kp RBKT  &to 2     &kp C_MUTE
            >;
        };

        // Layer 1: Lower
        lower_layer {
            display-name = "Lower Layer";
            bindings = <
                &kp TILDE   &none   &kp EXCL  &kp AT    &kp HASH  &kp EQUAL  &kp F1    &kp F2    &kp F3    &kp F12   &none     &kp DEL
                &kp LA(TAB) &none   &kp DLLR  &kp PRCNT &kp CARET &kp UNDER  &kp F4    &kp F5    &kp F6    &kp F11   &none     &trans
                &trans      &none   &kp AMPS  &kp STAR  &kp LPAR  &kp RPAR   &kp F7    &kp F8    &kp F9    &kp F10   &kp BSLH  &trans
                &trans      &trans  &trans    &kp LALT  &kp LGUI  &kp SPACE  &kp RSHFT &trans    &trans    &trans    &trans    &to 3     &trans
            >;
        };

        // Layer 2: Raise
        raise_layer {
            display-name = "Raise Layer";
            bindings = <
                &kp GRAVE &kp KP_DIVIDE   &kp N1 &kp N2    &kp N3    &kp PLUS   &kp PG_UP &kp HOME  &kp UP    &kp END   &kp C_VOL_UP &kp BSPC
                &kp TAB   &kp KP_MULTIPLY &kp N4 &kp N5    &kp N6    &kp MINUS  &kp PG_DN &kp LEFT  &kp DOWN  &kp RIGHT &kp C_VOL_DN &kp RET
                &kp LCTRL &kp DOT         &kp N7 &kp N8    &kp N9    &kp N0     &kp LBKT  &kp RBKT  &trans    &trans    &kp BSLH     &trans
                &trans    &trans          &trans &sk LALT  &kp LGUI  &kp RET    &kp RSHFT &trans    &trans    &trans    &trans       &to 0     &trans
            >;
        };

        // Layer 3: System/Control
        sys_layer {
            display-name = "System Layer";
            bindings = <
                &none        &none        &none        &kp INS  &kp SLCK &kp PAUSE_BREAK &none   &none   &none &none &kp PSCRN &none
                &kp CAPS     &none        &none        &none    &none    &none           &none   &none   &none   &none   &none     &none
                &none        &none        &none        &none    &none    &none           &none   &none   &none   &none   &none     &none
                &none        &none        &none        &none    &none    &none           &none   &none   &none   &none   &none     &to 0     &none
            >;
        };
    };
};
```

<br>

The `Keymap` above is the final layout code I mixed and matched specifically for the `Outsider` keyboard layout. Below, I will put the `keymap` comparison diagrams in `QMK`. Basically, for the wired keyboard firmware (`QMK`) and wireless keyboard firmware (`ZMK`) parts, you shouldn't have too much difference in designing the `Keymap`.

<table>
  <tr>
    <td width="50%">
      <img src="pic/info/keymap0.png" width="100%" alt="keymap0">
    </td>
    <td width="50%">
      <img src="pic/info/keymap1.png" width="100%" alt="keymap1">
    </td>
  </tr>
   <tr>
    <td width="50%">
      <img src="pic/info/keymap2.png" width="100%" alt="keymap2">
    </td>
    <td width="50%">
      <img src="pic/info/keymap3.png" width="100%" alt="keymap3">
    </td>
  </tr>
</table>

<br>

As for what kind of layout and mixing patterns the entire keyboard should be made into, this is also a very time-consuming area. Assuming you don't have the need for multi-variable key positions, just skip this part.

<table>
      <img src="pic/info/layout2.png" width="100%" alt="layout2">
</table>

<br>

At this point, our local `zmk` folder architecture diagram will look like this:

![](pic/info/dir-tree.png)

- `boards/shields`
    - The internal subfolders are the locations of all keyboards in the local `zmk` galaxy.
        - `Kconfig.shield` is the "flag" of the planet. If the inside is not configured, the planet does not exist.
        - The `.overlay` file explains the operating rules of the planet, such as how the core drives, how the water flows, etc.
        - The `.keymap` file defines what things are on the planet, such as what data will pop out to the computer after a key is pressed.
        - The `.conf` file is used for device connection settings, which will be mentioned later.
- `config`
    - The folder sets the main rules of the galaxy, such as global variable settings, Bluetooth transmission power, deep sleep mode, etc.
    - The `west.yml` file is used to select which version of the `zmk` firmware you want to use for flashing.
    - `zmk.conf`。
- `zephyr`
    - `module.yml` is responsible for declaring to the `zephyr` compilation system that "this galaxy is an independent module."
- `build.yaml`
    - The command script for `GitHub Actions` (`CI/CD`) to consult, defining "who acts as whose master control, outputting as a `.uf2` firmware file."

This way, the `zmk` folder has been comprehensively defined and parsed. Next time you are reading the official `zmk` text, it will be much easier.

> Supplementary note: Regarding the `#include <dt-bindings/zmk/pointing.h>` database, it belongs to the latest version of settings; the old version of `mouse.h` has been integrated in. If you are on the latest version, make sure to use `pointing.h` to make declarations.

<br>

#### Encoder Setup

Earlier we discussed how to set up the matrix part for standard buttons. Because the `Outsider` supports an encoder in its default hardware setup, here I will tell you how to set up the encoder's "Rotation" and "Pulse".

There is also a section regarding the setup of "Pointing Devices." Since the `Outsider`'s hardware also supports this, I will explain it in sequence.

1. First, we open the `.keymap` file, locate the `default_layer` block, and add the "rotation values" for the encoder.

``` c
... Omitted above.

/ {
    keymap {
        compatible = "zmk,keymap";

        // Layer 0: Base
        default_layer {
            display-name = "Default Layer";
            bindings = <
                &kp ESC   &kp Q     &kp W     &kp E     &kp R     &kp T      &kp Y     &kp U     &kp I     &kp O     &kp P     &kp BSPC
                &kp LSHFT &kp A     &kp S     &kp D     &kp F     &kp G      &kp H     &kp J     &kp K     &kp L     &kp SEMI  &kp SQT
                &kp LCTRL &kp Z     &kp X     &kp C     &kp V     &kp B      &kp N     &kp M     &kp COMMA &kp DOT   &kp FSLH  &kp RET
                &mkp RCLK &mkp LCLK &mkp MCLK &kp LALT  &mo 2     &kp SPACE  &kp RSHFT &mo 1     &kp RGUI  &kp LBKT  &kp RBKT  &to 2     &kp C_MUTE
            >;
            // Encoder A, B setup
            sensor-bindings = <&inc_dec_kp C_VOL_UP C_VOL_DN>;
        };

        // Layer 1: Lower

... Omitted below.
```

For a single encoder's rotation values, you need to copy the same `sensor-bindings` into every `Layer`.

If you want "different rotation values for each `Layer`", then you need to insert the corresponding `sensor-bindings` declaration at the bottom of each `Layer`.

```c
... Omitted above.

/ {
    keymap {
        compatible = "zmk,keymap";

        // Layer 0: Base
        default_layer {
            ...
            // Volume control
            sensor-bindings = <&inc_dec_kp C_VOL_UP C_VOL_DN>;
        };

        // Layer 1: Lower
        lower_layer {
            ...
            // Mouse scroll wheel
            sensor-bindings = <&inc_dec_kp mwh SCROLL_UP mwh SCROLL_DOWN>;
        };

        // Layer 2: Raise
        raise_layer {
            ...
            // Switch Next/Previous track
            sensor-bindings = <&inc_dec_kp C_NEXT C_PREV>;
        };
... Omitted below.
```

<br>

3. For the pulse section, we must open the `.overlay` file and add the "encoder" setup to the main definition area:

``` c
/ {
    ...

    /* Matrix Transform */
    default_transform: keymap_transform_0 {
    ...
    };

    /* Define Encoder */
    left_encoder: encoder_left {
        compatible = "alps,ec11";
        a-gpios = <&pro_micro 2 (GPIO_ACTIVE_HIGH | GPIO_PULL_UP)>;
        b-gpios = <&pro_micro 3 (GPIO_ACTIVE_HIGH | GPIO_PULL_UP)>;
        steps = <80>; /* Physically there are 80 pulses per rotation */
        status = "okay";
    };

    /* Encoder Sensor */
    sensors {
        compatible = "zmk,keymap-sensors";
        sensors = <&left_encoder>;
        triggers-per-rotation = <20>; /* Triggers 20 actions per rotation */
    };
};
```

This part is very similar to the `Encoder` setup in `QMK`:

``` c
// config.h

/* Encoders */
#define ENCODER_A_PINS { GP2 }
#define ENCODER_B_PINS { GP3 }
#define ENCODER_RESOLUTION 4

// keymap.c
#if defined(ENCODER_ENABLE) || defined(ENCODER_MAP_ENABLE)
    const uint16_t PROGMEM encoder_map[][NUM_ENCODERS][NUM_DIRECTIONS] = {
        [0] = { ENCODER_CCW_CW(XXXXXXX, XXXXXXX) },
        [1] = { ENCODER_CCW_CW(XXXXXXX, XXXXXXX) },
        [2] = { ENCODER_CCW_CW(XXXXXXX, XXXXXXX) },
        [3] = { ENCODER_CCW_CW(XXXXXXX, XXXXXXX) },
    };
#endif
```

In `ZMK`, we do not need to directly write `ENCODER_RESOLUTION 4` like in `QMK`. Instead, we must tell the system the "total number of pulses per rotation (`steps`)" and the "total number of detents per rotation (`triggers-per-rotation`)". The system will automatically divide the two to obtain the pulse multiplier for this encoder. Taking a standard `EC-11` as an example, it is usually 80 pulses divided by 20 detents, resulting in a multiplier of 4 pulses; if it is a mouse encoder (scroll wheel), both values are usually the same, resulting in a multiplier of 1 pulse.

Therefore, you still need to pull up the "specification sheet" for the mouse encoder, check the pulse value and total number for that encoder, and then fill the correct values into the code:

```c
left_encoder: encoder_left {
        compatible = "alps,ec11";
        ...
        steps = <24>;  /* Physically there are 24 pulses per rotation */
    };

    sensors {
        ...
        triggers-per-rotation = <12>; /* Triggers 12 actions per rotation */
    };
```

<table>
      <img src="pic/info/ec10e.png" width="100%" alt="data-ec10e">
</table>

4. After filling in the pulse values, finally open the `.conf` file, write the encoder's global setup code into the configuration, then save and close it.

``` conf
...
# Encoder setup
CONFIG_EC11=y
CONFIG_EC11_TRIGGER_GLOBAL_THREAD=y              # Allow encoder hardware interrupts to trigger global threads
```

<br>

### C. Introduction to Pointing Devices

For pointing devices, you need to prepare `2` things:
- The driving method of that pointing device.
    - Pointing device specification sheet.
    - The pointing device component itself.
- Your design drawing.
    - o the `IO` pins support the communication method of the pointing device?

> Very important: How much "voltage (`VCC`, `VDD`)" is needed to drive the pointing device?

Common pointing devices include "optical sensors (`sensor`)", "joystick encoders (`joystick`)", and "trackpads". These three cover over `95%` of the operation methods on the market. Here is a brief introduction:

1. Optical sensors are usually composed of a combination of the "component itself" and an "optical lens", neither of which can be omitted. They need to be a certain distance away from the "sensing contact surface" to function properly.
    * Mouse: Component and lens face downwards, sensing the desktop.
    * Trackball: Component and lens do not face downwards, sensing a "standardized" sphere.
2. Joystick encoders, usually called "joysticks" or "trackpoints". The operation method involves the amount of planar movement on the `X` and `Y` axes. More specialized joystick encoder calculations have a vertical amount on the `Z` axis, though this is usually not used in pointing devices.
3. Trackpads are somewhat special. They calculate the `X` and `Y` axes over a planar circuit sensor board area of a certain size. Like joysticks, more specialized trackpad `MCU`s can calculate the height of the `Z` axis, which is the pressure value.
    * Commonly used ones include the `Azoteq` series and the `Cirque` series. These are universal models for `qmk` and `zmk`. Recently, a designer has also designed the `Azoteq` trackpad into `zmk`, and I will learn from him when the time comes.


![lalapadgen2](pic/info/lalapadgen2.jpeg)
- `Lalapad Gen2` by `kot`——[X Link](https://x.com/kot149_/status/2050509588760551813) (Not the original designer).

<br>

#### How to Read the Specification Sheet?

The specification sheet is what I have repeatedly emphasized needs to be consulted "first". From something as small as a "switch" (mechanical switch), to something as complex as the "trackpad" I am about to explain, you must first read the specification sheet before taking any "design" actions:

1. In terms of software design, you need to know the "communication method" and "detection method".
2. In terms of circuit design, you need to know the "voltage", "communication method", "how to wire the communication interface", "where to place the communication interface", "the direction of the communication interface", etc.
3. In terms of hardware design, you need to know "the actual size of this component", "`Z`-axis interference", "tolerances", etc.

Looking at the whole picture together, you will find the hardest low-level content in this specification sheet, such as "`EMI` interference", "communication conversion", "signal detection", etc. This part is called the "debugging mechanism".

Therefore, you don't need to overcomplicate the specification sheet. It's not an "incomprehensible text", but rather about selecting "the portions of content you currently need" to extract and consult.


<br>

The `Outsider` keyboard supports these two trackpads: `Azoteq TPS43` and `Cirque TM040040`. In my design, I placed the `EC-11`, `TPS43`, and `TM040040` all on the same pins.

First, let's see how I designed it:

#### EC-11

<table>
    <td width="50%">
      <img src="pic/info/ec111.png" width="100%" alt="ec111">
    </td>
  </tr>
   <tr>
    <td width="50%">
      <img src="pic/info/ec112.png" width="100%" alt="ec112">
    </td>
  </tr>
</table>

`EC-11`, which is a commonly used encoder. If terms like clockwise, counter-clockwise rotation (`CW`, `CCW`), and switch (`SW`) are too hardcore, don't overcomplicate this component.

Its name is "Encoder with Switch". Just break it down into `2` parts to look at it:

1. Switch: Treat it as a mechanical switch you use on your keyboard.
2. Encoder: Treat it as a switch that triggers after rotating a few detents, and it can be triggered by rotating clockwise or counter-clockwise.

Simple enough?

![ec11](pic/info/matrix-ki2.png)

Since it is divided into these `2` parts, the switch design is very simple. Design it into the matrix keyboard, use it as a switch, add a diode, and connect it to a `col` and `row`, and it's done.

For the encoder part, it is divided into `A`, `B`, and `C` three `pin`s. I said "treat it as a switch", but its design method is viewed independently, so it requires the "most traditional" circuit switch wiring method:

* One end connects to the signal, the other end connects to ground.
* So where is the ground (`GND`)? This requires consulting the specification sheet. It indicates which `pin` among `A`, `B`, and `C` needs to be grounded (I'll secretly tell you here that the ground for `EC-11` is connected at position `C`).

<br>

#### Azoteq TPS43

<table>
  <tr>
    <td width="50%">
      <img src="pic/info/tps2.png" width="100%" alt="tps2">
    </td>
    <td width="50%">
      <img src="pic/info/tps3.png" width="100%" alt="tps3">
    </td>
  </tr>
   <tr>
    <td width="50%">
      <img src="pic/info/tps4.png" width="100%" alt="tps4">
    </td>
    <td width="50%">
      <img src="pic/info/tps5.jpg" width="100%" alt="tps5">
    </td>
  </tr>
</table>

For the `TPS43` part, the following are the portions I extracted:
1. Actual size and actual model:
    - `TPS43-201A-B`.
    - `43.3 x 40.3 mm`.
    - After calculation, the `PCB` thickness is `1.2 mm`, `3M` double-sided tape thickness is `0.35mm`, and the touch contact surface limitation is within `1.2 mm`.
    - `0.5mm FFC` connector thickness is `2 mm`, positioned asymmetrically at a `45`-degree angle.
    - The `FFC` interface distribution diagram shows the "`6th pin`" is "`SDA`".
2. Communication method:
    - `I2C`.
    - Operating voltage within the `1.65 - 3.6V` range.
    - Built-in `4.7k` pull-up resistor.

<br>

#### Cirque TM040040

<table>
  <tr>
    <td width="50%">
      <img src="pic/info/cirque2.png" width="100%" alt="cirque2">
    </td>
    <td width="50%">
      <img src="pic/info/cirque3.png" width="100%" alt="cirque3">
    </td>
  </tr>
   <tr>
    <td width="50%">
      <img src="pic/info/cirque4.png" width="100%" alt="cirque4">
    </td>
    <td width="50%">
      <img src="pic/info/cirque3.jpg" width="100%" alt="cirque5">
    </td>
  </tr>
</table>

For the `TM040040` part:
1. Communication method and physical interface:
    - Default is `SPI`, supports `I2C` after desoldering the `R1` resistor.
    - `FFC 0.5mm 12pin` vertical cable exit.
    - Operating voltage is `3V3`.
    - No pull-up resistor; requires designing a `4.7K` resistor interface yourself.
2. Interface `IO` distribution:
    - `1-5` are `SPI`.
    - `6-8` are `SW`.
    - `9-12` are `I2C`—this is the part we use.
3. Actual physical size and component distribution:
    - Comes with a plastic bowl-shaped touch surface: concave.
    - The `FFC` interface is located slightly below the center, protruding from the bowl edge.
4. True calculated size (`3D` part):
    - The actual positions of the `3` notches on the `PCB`.
    - The length of the non-circular cutout section on the `PCB` is `6.9` mm.
    - The touch surface `Z`-axis interference radius is `23.065` mm.

After numerical calculation from the specification sheet, I designed both the `KiCAD` `footprint` and the physical `3D` model to use as my own reference graphics.

<table>
  <tr>
    <td width="50%">
      <img src="pic/info/cirque-ki.png" width="100%" alt="cirque-ki">
    </td>
    <td width="50%">
      <img src="pic/info/cirque-3d.jpg" width="100%" alt="cirque-3d">
    </td>
  </tr>
</table>

<br>

- `ProMicro` and `nRF52840 ProMicro` (`nice!nano v2`)

<table>
  <tr>
    <td width="50%">
      <img src="pic/info/rp2040.jpg" width="100%" alt="rp2040">
    </td>
    <td width="50%">
      <img src="pic/info/nrf52840.png" width="100%" alt="nRF52840">
    </td>
  </tr>
</table>

That's right, these `2` "pinout definition tables" are here again because they are truly important. I have said the phrase "prioritize using the intersection parts for design," and you can calculate how many "intersections" there are from the places where I extracted from the specification sheet earlier:
1. `I2C`.
2. `I2C` requires `2pin`, which are `SDA` and `SCL`.
3. `A`, `B`, `C`—removing `C` which is the `GND` position, leaves `A` and `B`.
4. An `8x8` matrix, totaling `16 pin`s.
5. The `VCC` for both development boards output `3V3`, which are both within the driving range of `TPS43` and `TM040040`.

After confirming the `I2C` communication method for the `FFC` connector from the specification sheet, defining the distribution direction in the `KiCAD` schematic, and then cross-referencing with the principles of better circuit board routing, I defined `I2C` on the `GP2 (D2)` and `GP3 (D3)` positions of the `RP2040` and `nRF52840 ProMicro` development boards, with the channel being `I2C1`.

Knob `A` and `B` share the `GP2 (D2)` and `GP3 (D3)` pins, therefore cross-processing must be done in the firmware.

> Very important: An easily overlooked rule of `I2C` communication is that its communication requires `2` `4.7K` resistors connected separately to `SDA` and `SCL`, and the other end of the resistors needs to connect to `VCC`, using "electricity" to pull up the potential of the communication channels.

<table>
  <tr>
    <td width="33%">
      <img src="pic/info/cirque.png" width="100%" alt="cirque">
    </td>
    <td width="33%">
      <img src="pic/info/tps.png" width="100%" alt="tps">
    </td>
    <td width="33%">
      <img src="pic/info/i2c.png" width="100%" alt="i2c">
    </td>
  </tr>
</table>

<br>

#### zmk Setup Items

Back to `zmk` here.

> Pay special attention here, the design of the `Outsider` involves "shared pins", therefore when performing pointing device operations, I will completely mask out the "`Encoder`" part.

Operating up to this point, you will find that the `Outsider` has a huge problem—all of its pins are completely stuffed full, with no other expansion space. And the mechanism of the `zmk` firmware will force the "communication protocol" to use `SPI`'s `CS` or `I2C`'s `RDY` to communicate with the trackpad.

You can treat this "communication" process like this: `A` and `B` are next-door neighbors. `A` can walk through his "front door" to `B`'s "back door," but cannot walk from the "front door" to `B`'s "front door"; conversely, the same goes for `B`.

—But `A` can call `B` to wake up and open the door.

- `CS` and `RDY` are that "telephone line."

But unfortunately, in practice, this will use up an extra `IO Pin`. Therefore, I will not refer to the official text's `SPI` communication protocol here; although it is very convenient and can be compiled directly, it is realistically impossible to do.

...Since it's impossible, `zmk` can utilize the mounting of "third-party" low-level firmware to expand your `zmk` universe compilation core database.

<br>

#### Mounting Third-Party Databases

Although `Gemini` mentioned to me that there are many god-tier engineers on the internet developing pointing device firmware for `zmk`, in reality, the only one I found so far belongs to the great `halfdane`. Judging by the timeframe, it was their team's development that gave `zmk` a huge breakthrough about a year ago (in `2025`).

—Here we must thank them: [https://github.com/halfdane/zmk-input-gestures](https://github.com/halfdane/zmk-input-gestures)

First, enter the `GitHub` homepage of the third-party library. Actually, they have taught everyone how to "mount" the third-party library; here I will simplify the process a bit further.

<br>

Here is our current `XXX-zmk-config` folder architecture:

![dir-tree2](pic/info/dir-tree2.png)

1. First we enter the `west` folder:

``` bash
# 指標設備
zmk cd
cd west
```

2. Open `west.yml`, follow the operations of the third-party library, paste the mounting content into the file, and save.

``` yaml
manifest:
  defaults:
    revision: main
  remotes:
    - name: zmkfirmware
      url-base: https://github.com/zmkfirmware
    # Additional modules containing boards/shields/custom code can be listed here as well
    # See https://docs.zephyrproject.org/3.2.0/develop/west/manifest.html#projects

    # Add the GitHub homepage of the third-party library here
    - name: halfdane
      url-base: https://github.com/halfdane/
  projects:
    - name: zmk
      remote: zmkfirmware
      import: app/west.yml
    # Add other modules at the bottom here
    - name: zmk-input-gestures
      remote: halfdane
      revision: main
    # This is the module for absolute mode, containing trackpad drivers and gesture processing
    - name: zmk-input-processors
      remote: halfdane
      revision: main
    # The library below will conflict with the official one, do not mount it for now
    # - name: cirque-input-module 
    #   remote: halfdane
    #   revision: absolute_mode
  self:
    path: config
```

3. Then execute the `west update` command in the terminal, and wait for it to finish mounting.
4. Next, you still need to manually open `.gitignore` and write the third-party mounted library folders into the exclusion list:

``` .gitignore
/zmk-input-gestures/
/zmk-input-processors/
/cirque-input-module/
```

5. Update `.gitignore` onto `GitHub` first, and then you can normally modify key mappings and add new keyboards again.

``` bash
git add .gitignore
git commit -m "add new module in config"
git push
```

<br>

#### Firmware Module Code Setup

> Precautions: Because during the debugging process of the `Outsider` circuit board on the hardware end, it was discovered that the bottom layer of the `zmk` firmware system forcibly requires designers to use:
> 1. `SPI` communication protocol: `CS`.
> 2. `I2C` communication protocol: `RDY`.
> 
> 
> I will carve out a separate major heading for this part at the bottom, and switch to using a breadboard to let everyone know the working principles.

<br>

#### Supplementary Notes

1. What exactly `I2C` and `SPI` are is actually not that important. You just need to look at the "pinout diagram" and cross-reference the text from `qmk` or `zmk` to handle the "device's" communication method. Simply put, just copy what you see in the diagrams; you don't need to know too much profound knowledge.

If I really have to explain it, I don't know what `I2C` and `SPI` are either. I only know they are "communication rules," as well as "how to wire them" and their "preliminary functions."

<br>

2. The only thing to note about `I2C` is the "pull-up resistor". Why is it `4.7K`?

Because this is the optimal solution derived after formula calculations. In the `3V3` world, you can use `2.2K` or `4.7K` to handle a device transmission speed of `400kHz`:
- `4.7K` is used for normal and general situations.
- `2.2K` is used for extreme situations. Unless the operating environment is extremely harsh, you will rarely have the chance to use it.

Simply put, as long as you see it's an `I2C` device driven by `3V3`, choosing `4.7K` without hesitation is the right move.

As for the resistor specifications, in the `3V3` world, actually any `4.7K` will do. If you want to use a `1/2W` sized resistor, I have no objections.

<br>

### D. Device Connection Settings

After the firmware's core functions and pin setups are complete, next we need to configure the "`MCU`'s" connection settings to the "devices":
- Bluetooth communication.
- `USB` communication.

That's right, both wired and wireless need to be set up. After discussing with a friend, I discovered that if the `zmk` firmware is missing just one setting, the keyboard will go on strike.

1. First, return to the `XXX-zmk-config` folder and enter the `boards/shields/<custom_keyboard>` folder directory:

``` bash
zmk cd
cd boards/shields/<custom_keyboard>
```

2. Then open the `.conf` file, write in the connection settings, and save:

``` conf
# 藍牙設定
C# Bluetooth Settings
CONFIG_ZMK_BLE=y
CONFIG_ZMK_KEYBOARD_NAME="<custom_keyboard_BLE>" # Bluetooth broadcast name
CONFIG_BT_CTLR_TX_PWR_PLUS_8=y                   # Boost Bluetooth transmission power
CONFIG_CLOCK_CONTROL_NRF_K32SRC_RC=y             # Use internal RC oscillator
CONFIG_CLOCK_CONTROL_NRF_K32SRC_500PPM=y         # RC oscillator precision setting

# Battery Settings
CONFIG_ZMK_BATTERY_REPORTING=n                   # Disable battery reporting function

# USB Settings
CONFIG_ZMK_USB=y
...
```

If this part is not set up properly, the `MCU` will play dead on you. You won't be able to find the signal using other Bluetooth devices, but the blue light on the connected device will keep flashing.

> Special Note: Many `nice!nano v2` clones on the market may differ from the original in terms of the Bluetooth chip's transmission power. You must consult the "development board schematic" first before adjusting based on `zmk`'s settings. Here, I enabled the `RC` oscillator calibration function, which finally allowed me to successfully receive my own keyboard's signal on my Bluetooth device.

3. Then open the `.keymap` file and set the key functions that control Bluetooth into the `keymap`:

```c
#include <dt-bindings/zmk/outputs.h> // Uses OUT_* series buttons, refer to the official Output Selection documentation
... Omitted above.
        // Layer 3: System/Control
        sys_layer {
            display-name = "System Layer";
            bindings = <
                &bt BT_CLR   &bt BT_SEL 0 &bt BT_SEL 1 &bt BT_SEL 2 &out OUT_BLE &out OUT_USB &none  &none  &kp INS  &kp SLCK &kp PAUSE_BREAK  &kp PSCRN 
                &kp CAPS     &none        &none        &none        &none        &none        &none  &none  &none    &none    &none            &none
                &none        &none        &none        &none        &none        &none        &none  &none  &none    &none    &none            &none
                &none        &none        &none        &none        &none        &none        &none  &none  &none    &none    &none            &to 0      &none
            >;
            // The key functions for Bluetooth settings can be written in by referencing the official zmk documentation.
        };
        ...
```

Bluetooth connected devices can be set up to a maximum of `5` groups. Here I only set up the standard `3` connection groups commonly seen on the market.

<br>

### E. Setting up GitHub Actions

I believe everyone is definitely not unfamiliar with `GitHub Actions`. This feature allows you to perform `CI/CD` tasks for your existing `zmk` keyboard firmware, letting you modify key mappings and compile firmware directly online. This action is essentially handing over the job of "compilation" for the cloud supercomputer to process.

> Note: `CI/CD`: Continuous Integration / Continuous Deployment.

Earlier, we spent an extremely, extremely massive amount of time architecting the `zmk` compilation environment. In this chapter, I will guide everyone through understanding and controlling the setup methods and architectural explanations for these two parts—`GitHub Actions` and the official `ZMK Studio`.

1. First, returning to the local `GitHub` folder, we need to locate the `.github/workflows/build.yaml` file under the `XXX-zmk-config` directory.

Then open it:

``` yaml
name: Build ZMK firmware
on: [push, pull_request, workflow_dispatch]

jobs:
  build:
    uses: zmkfirmware/zmk/.github/workflows/build-user-config.yml@v0.3 # Change to main
```

Then change the `v0.3` part to `main`, and subsequently save it.

> Note: There is a "`.`" in front of the `.github` folder, meaning it belongs to a hidden file or folder.

2. The `ZMK` community has a very mature web-based graphical interface called "[`Keymap Editor`](https://nickcoutsos.github.io/keymap-editor/)".

We need to use it. First, authorize the binding of your `GitHub` account, and then select your `XXX-zmk-config` repository.

<img src="pic/info/kme.png" width="100%" alt="kme">

3. Next, follow the instructions of `Keymap Editor` and enter the main `GitHub` page.

> Special Note: The first time you run `Keymap Editor`, it will install a package in your `GitHub` account. Then, when choosing "settings," point it to the "`XXX-zmk-config`" repository.

4. Then, following the standard process, on the `Keymap Editor` interface, it will tell you that it cannot find the keyboard's "`.keymap`" file. At this point, we need to adjust the state of our local folders:

The current structure of the local folder is like this:

![](pic/info/dir-tree2.png)

<br>

We need to copy the entire `boards` folder, and then paste a copy of it directly under `XXX-zmk-config/config`. Then, move the `<custom_keyboard>.keymap` from inside the `boards/shields/<custom_keyboard>` folder into the `XXX-zmk-config/config` area (see the picture):

![](pic/info/dir-tree3.png)

<br>

5. After fixing the local folder structure, return to the `XXX-zmk-config` directory, open `build.yaml` to correct it, and then save:

``` yaml
# This file generates the GitHub Actions matrix.
# For simple board + shield combinations, add them to the top level board and
# shield arrays, for more control, add individual board + shield combinations
# to the `include` property. You can also use the `cmake-args` property to
# pass flags to the build command, `snippet` to add a Zephyr snippet, and
# `artifact-name` to assign a name to distinguish build outputs from each other:
#
# board: [ "nice_nano" ]
# shield: [ "corne_left", "corne_right" ]
# include:
#   - board: bdn9_rev2
#   - board: nice_nano
#     shield: reviung41
#   - board: nice_nano
#     shield: corne_left
#     snippet: studio-rpc-usb-uart
#     cmake-args: -DCONFIG_ZMK_STUDIO=y
#     artifact-name: corne_left_with_studio
#
---
# Write in the <custom_keyboard> data according to the format here
include:
  - board: nice_nano//zmk # Be sure to write //zmk at the back here to ensure GitHub Actions uses compatibility compilation
    shield: outsider
```

6. Then return to the terminal to operate the commands:

``` bash
zmk cd
git add .
git commit -m "set KME folder&update keymap"
git push
```

7. After uploading the corrections, return to the `Keymap Editor` interface. After refreshing, you will see the compilation starting to run at the top. You can click into it to see how the `GitHub Actions` firmware compilation status is doing.

<table>
  <tr>
    <td width="50%">
      <img src="pic/info/kme2.png" width="100%" alt="kme2">
    </td>
    <td width="50%">
      <img src="pic/info/kme3.png" width="100%" alt="kme3">
    </td>
  </tr>
</table>

<br>

> Note: Abbreviation for `Keymap Editor`.

8. Then, if `GitHub Actions` has finished compiling the firmware but the `KME` interface hasn't refreshed, enter `GitHub Actions` once more, and there will be a link to download the firmware at the bottom.

![](pic/info/kme4.png)

<br>

9. Following this is the familiar firmware flashing part for everyone. Simple enough?

<br>

## Physical Debugging Rules

Although `zmk` is "software" concerning wireless keyboards, this section should be considered my personal logic-sorting rules provided for everyone's reference.

Between circuit design and firmware design, there is a set of rules in the "gray" area, which is the physical debugging listed in the title. This part actually splits into two main headings:
1. Process of Elimination.
2. The `0/1` Rule.

<br>

I believe everyone is definitely not unfamiliar with it; the process of elimination is one of the "scientific experimental methods." Between circuits and firmware, this rule has always been extremely useful.

Formula: Assume `A + B = C` is an absolutely correct truth. Then I can reverse-deduce that either `A`, `B`, or both have problems, which is why I cannot obtain `C` in the result.

To put it plainly, if a key has no response, then I can first estimate:
1. Is there a setup error in the firmware?
2. Is the hardware connected to the wrong position?
3. Is there continuity from switch `A` and `B pin` to the `MCU`?
4. Is there a short circuit in the process?
5. Is the switch broken?

<br>

Then, amidst this kind of "process of elimination," you will find that the second rule is also influencing your behavioral judgments here—

The `0/1` Rule is extremely simple; it is the concept of "if it's there, it's there, if it's not, it's not," which happens to be an absolute truth in hardware and circuit debugging.

We extend further based on the process of elimination above: assuming the firmware has no setup errors and the hardware is not connected to the wrong position, we can pick up a "multimeter," switch the channel to the "continuity test" gear, and we can test:
1. `MCU` to switch `A pin`.
2. `MCU` to switch `B pin`.
3. `MCU` to diode `A pin`.
4. Switch `B pin` to diode `B pin`.
5. Whether switch `A` and `B` respond when pressed to conduct.

<br>

Right? This way you definitely have a way to achieve `100%` debugging, and no matter whether you encounter Bluetooth or even more obscure underlying principles, you also have a way to completely solve your major problems using these two rules.

<br>

## Pointing Devices

> Under construction, please excuse.

<br>

## ZMK Studio

`ZMK` was once criticized by `QMK` players—why can't it be like `VIA`/`VIAL`, where you plug in the cable and can remap keys instantly?

The officials heard this voice and therefore launched `ZMK Studio`—its underlying operational logic is completely different from `VIA`: `VIA` reads and writes a small block of memory inside the keyboard chip; while `ZMK Studio` dynamically modifies the key tree map in the keyboard memory directly through physical communication.

Simply put, as long as it can connect to "your keyboard," regardless of whether it is connected via a "`USB`" cable or "Bluetooth," it can remap keys.

> Warning: This belongs to an extreme knowledge blind spot. The following text explanations all belong to "my personal" pioneering process. If there are any errors during execution, they belong to the "normal scope."

<br>

### Prerequisites

1. First we need to check if `ZMK Studio`'s underlying compilation packages are installed:

``` bash
sudo apt update
sudo apt install protobuf-compiler
pip install protobuf grpcio-tools
```

2. Next we need to enter the `XXX-zmk-config` directory, open the `boards/shields/<custom_keyboard>/<custom_keyboard>.conf` file, and add this startup code:

```conf
# Enable ZMK Studio
CONFIG_ZMK_STUDIO=y
CONFIG_ZMK_STUDIO_LOCKING=n
```

3. Then open `.overlay`, and write in the `ZMK Studio` toggle code:

```c
#include <physical_layouts.dtsi> /* Add Physical Layout database */

/ {
    /* Physical Layout Declaration */
    key_physical_attrs: key_physical_attrs {
        compatible = "zmk,key-physical-attrs";
        #key-cells = <7>; /* Must declare 7 array parameters, otherwise compilation will error */
    };

    chosen {
        zmk,kscan = &kscan0;
        /* Delete or comment out this line of code, so ZMK Studio compilation won't error */
        // zmk,matrix_transform = &default_transform;
        zmk,physical-layout = &physical_layout0;  /* Add this line of code to "Use physical Layout_0" */
    };

    /* Add physical layout node */
    physical_layout0: physical_layout_0 {
        compatible = "zmk,physical-layout";
        display-name = "Default Layout";
        kscan = <&kscan0>;
        transform = <&default_transform>;
        
        /* Here you need to define the physical size and coordinates of each key */
        keys
            = 
            ;
        /* Code coordinate system explained in another section */
    };

    kscan0: kscan_0 {
    ...
    };
... Omitted below.
};
```

<br>

### Coordinate System

The "Physical Coordinate System" is a new calculation system invented by `ZMK` officials to benchmark against `VIA`/`VAIL`. Here we need to first understand its operational rules.

* Physical Coordinates
  * Origin: `(X, Y)` = `(0, 0)`
  * `X` axis: To the right is a "positive" value.
  * `Y` axis: Downwards is a "positive" value.

* Units:
  * `centi-keyunit`: Calculates "key size" in units of `1/100`. For example: `1U` = `100`.
  * `centi-degree`: Calculates "angle" in units of `1/100`. `10` degrees = `1000`.

* `&key_physical_attrs`:
  * `7` calculation values.
  * Inside `physical_layout0: physical_layout_n`, `keys` are used as sub-macros (`n` follows the `layout` value defined in `chosen`).
  * Usage method: `<&key_physical_attrs w h x y r rx ry>`

|Parameter|Description|Unit|
|--|--|--|
|`w`|`Width`|`centi-keyunit`|
|`h`|`Height`|`centi-keyunit`|
|`x`|`X` Coordinate|The `X` coordinate of the key's "top-left corner"|
|`y`|`Y` Coordinate|The `Y` coordinate of the key's "top-left corner"|
|`r`|`Rotation`|`centi-degree`|
|`rx`|`Rotation` `X`|The `X` coordinate of the rotation center point|
|`ry`|`Rotation` `Y`|The `Y` coordinate of the rotation center point|

<br>

Writing up to here, have you noticed something?

> Important: `zmk` officials did not invent any new coordinate system, but rather took the `KLE` calculation method and multiplied everything by `100` to perform operations.

<br>

### KLE Conversion

According to `Gemini`'s auxiliary analysis, regarding this `ZMK Studio` section, there is absolutely no tutorial text available. Even the official documentation only tells you this feature exists and how to set up the `.conf`, etc., but the accompanying source code is not explained at all.

> Note 1: https://zmk.dev/docs/features/studio

> Note 2: https://zmk.dev/docs/development/studio-rpc-protocol

> Note 3: https://zmk.dev/docs/config/studio

After gaining a deeper understanding, let's first create a basic `4x12` `Layout` following the arrangement in the photo, and then adjust it into a standard `Plank` configuration.

<table>
  <tr>
    <td width="50%">
      <img src="pic/info/zmkstudio-default.png" width="100%" alt="default">
    </td>
  </tr>
  <tr>
    <td width="50%">
      <img src="pic/info/zmkstudio-plank.png" width="100%" alt="plank">
    </td>
  </tr>
</table>

<br>

> From here on, the explanation will be paired with the `.json` files in the `Outsider` project folder and the web-based `Keyboard Layout Editor`.
> 1. `zmkstudio-default.json`
> 2. `zmkstudio-plank.json`

<br>

First, we open `KLE`, and then import the `default` layout diagram.

![](pic/info/kle1.png)

Then click on the first key in the top-left corner, and I will tell everyone how to build the code for this part next.

<br>

![](pic/info/kle2.png)

|Parameter|Description|Unit|
|--|--|--|
|`w`|`Width`|`centi-keyunit`|
|`h`|`Height`|`centi-keyunit`|
|`x`|`X` Coordinate|The `X` coordinate of the key's "top-left corner"|
|`y`|`Y` Coordinate|The `Y` coordinate of the key's "top-left corner"|
|`r`|`Rotation`|`centi-degree`|
|`rx`|`Rotation` `X`|The `X` coordinate of the rotation center point|
|`ry`|`Rotation` `Y`|The `Y` coordinate of the rotation center point|

Here we need to process it following the same code-writing rules as in `QMK`:
1. Start from left to right, then top to bottom.
2. The numerical fields in the red border correspond to the table above:
    - `Width`: `w`
    - `Height`: `h`
    - `X`: `x`
    - `Y`: `y`
    - `Rotation`: `r`, `rx`, `ry`

3. `<&key_physical_attrs w h x y r rx ry>`

<br>

From this, we know to first locate the values of the first key in the top-left corner, and then fill in the values from the red box after multiplying by `100` (`x100`):

```c
<&key_physical_attrs 100 100   0   0 0 0 0>
```

<br>

4. Then you can sequentially build up all the keys for the entire keyboard:

```c
/ {
    ...
    /* Physical Layout */
    physical_layout0: physical_layout_0 {
        compatible = "zmk,physical-layout";
        display-name = "Default Layout";
        kscan = <&kscan0>;
        transform = <&default_transform>;
        
        /* X,Y & Units */
        keys
            /* Row0, Row4 - col0-5 */
            = <&key_physical_attrs 100 100    0    0 0 0 0>
            , <&key_physical_attrs 100 100  100    0 0 0 0>
            , <&key_physical_attrs 100 100  200    0 0 0 0>
            , <&key_physical_attrs 100 100  300    0 0 0 0>
            , <&key_physical_attrs 100 100  400    0 0 0 0>
            , <&key_physical_attrs 100 100  500    0 0 0 0>
            , <&key_physical_attrs 100 100  600    0 0 0 0>
            , <&key_physical_attrs 100 100  700    0 0 0 0>
            , <&key_physical_attrs 100 100  800    0 0 0 0>
            , <&key_physical_attrs 100 100  900    0 0 0 0>
            , <&key_physical_attrs 100 100 1000    0 0 0 0>
            , <&key_physical_attrs 100 100 1100    0 0 0 0>
            , <&key_physical_attrs 100 100 1325   50 0 0 0> // EC-11

            /* Row1, Row5 - col0-5 */
            , <&key_physical_attrs 100 100    0  100 0 0 0>
            , <&key_physical_attrs 100 100  100  100 0 0 0>
            , <&key_physical_attrs 100 100  200  100 0 0 0>
            , <&key_physical_attrs 100 100  300  100 0 0 0>
            , <&key_physical_attrs 100 100  400  100 0 0 0>
            , <&key_physical_attrs 100 100  500  100 0 0 0>
            , <&key_physical_attrs 100 100  600  100 0 0 0>
            , <&key_physical_attrs 100 100  700  100 0 0 0>
            , <&key_physical_attrs 100 100  800  100 0 0 0>
            , <&key_physical_attrs 100 100  900  100 0 0 0>
            , <&key_physical_attrs 100 100 1000  100 0 0 0>
            , <&key_physical_attrs 100 100 1100  100 0 0 0>

            /* Row2, Row6 - col0-5 */
            , <&key_physical_attrs 100 100    0  200 0 0 0>
            , <&key_physical_attrs 100 100  100  200 0 0 0>
            , <&key_physical_attrs 100 100  200  200 0 0 0>
            , <&key_physical_attrs 100 100  300  200 0 0 0>
            , <&key_physical_attrs 100 100  400  200 0 0 0>
            , <&key_physical_attrs 100 100  500  200 0 0 0>
            , <&key_physical_attrs 100 100  600  200 0 0 0>
            , <&key_physical_attrs 100 100  700  200 0 0 0>
            , <&key_physical_attrs 100 100  800  200 0 0 0>
            , <&key_physical_attrs 100 100  900  200 0 0 0>
            , <&key_physical_attrs 100 100 1000  200 0 0 0>
            , <&key_physical_attrs 100 100 1100  200 0 0 0>

            /* Row3, Row7 - col0-5 */
            , <&key_physical_attrs 100 100    0  300 0 0 0>
            , <&key_physical_attrs 100 100  100  300 0 0 0>
            , <&key_physical_attrs 100 100  200  300 0 0 0>
            , <&key_physical_attrs 100 100  300  300 0 0 0>
            , <&key_physical_attrs 100 100  400  300 0 0 0>
            , <&key_physical_attrs 100 100  500  300 0 0 0>
            , <&key_physical_attrs 100 100  600  300 0 0 0>
            , <&key_physical_attrs 100 100  700  300 0 0 0>
            , <&key_physical_attrs 100 100  800  300 0 0 0>
            , <&key_physical_attrs 100 100  900  300 0 0 0>
            , <&key_physical_attrs 100 100 1000  300 0 0 0>
            , <&key_physical_attrs 100 100 1100  300 0 0 0>
            ;
    };
};
```

<br>

### Starting ZMK Studio

To be safe, personally, when implementing up to this point, I recommend compiling once to see if there are any errors before continuing to create other `Layout`s.

Because my testing environment is `Linux Mint`, there are very strict restrictions regarding `USB` connections... Most `Linux` systems are like this, while other systems require testing.

<br>

Before compiling, a few setup items need to be addressed. Otherwise, you might successfully compile and flash it, but `ZMK Studio` will absolutely not let you open the official website to connect and modify keys.

1. Return to the terminal. We first need to open the `USB` web read permissions for the `Linux` system:

```bash
sudo chmod 666 /dev/ttyACM0
sudo usermod -aG dialout $USER
```

2. We return to the local `XXX-zmk-config` folder, first open `build.yaml` under the directory, and add the startup code for `ZMK Studio` below the `<custom_keyboard>` configuration file under `include`.

``` yaml
# This file generates the GitHub Actions matrix.
# For simple board + shield combinations, add them to the top level board and
# shield arrays, for more control, add individual board + shield combinations
# to the `include` property. You can also use the `cmake-args` property to
# pass flags to the build command, `snippet` to add a Zephyr snippet, and
# `artifact-name` to assign a name to distinguish build outputs from each other:
#
# board: [ "nice_nano" ]
# shield: [ "corne_left", "corne_right" ]
# include:
#   - board: bdn9_rev2
#   - board: nice_nano
#     shield: reviung41
#   - board: nice_nano
#     shield: corne_left
#     snippet: studio-rpc-usb-uart
#     cmake-args: -DCONFIG_ZMK_STUDIO=y
#     artifact-name: corne_left_with_studio
#
---
include:
  - board: nice_nano//zmk
    shield: outsider
    snippet: studio-rpc-usb-uart # Add this line
```

3. Next, we can flash the firmware, but we need to adjust the compilation command a bit:

``` bash
west build -p -b nice_nano -S studio-rpc-usb-uart -- -DSHIELD=<custom_keyboard> -DZMK_CONFIG="[Your XXX-zmk-config folder path]"
```

4. After successful compilation, flash the firmware into the keyboard. Then conveniently test if the keyboard works normally while connected with the `USB` cable, and then open the official `ZMK` website — https://zmk.dev/
5. There is a `ZMK Studio` button at the top of the official website, click into it.

![](pic/info/kme5.png)

<br>

6. Click the USB button in the center, and you will see a connection option pop up at the top of the web page. If your keyboard appears at the top, then you have succeeded.

<table>
  <tr>
    <td width="50%">
      <img src="pic/info/kme6.png" width="100%" alt="kme6">
    </td>
  </tr>
  <tr>
    <td width="50%">
      <img src="pic/info/kme7.png" width="100%" alt="kme7">
    </td>
  </tr>
</table>

<br>

### Precautions

After flashing and reading `ZMK Studio`, I need to tell everyone the pros and cons of this feature here.

First are the "Pros":
- You can modify key functions without connection restrictions.

> If there are other pros, I will add them later (laughs).

Cons:
- No official tutorial text — this is extremely serious.
  - There are only major `.conf` startup settings.
  - After setting up `.conf`, there are still a bunch of miscellaneous items that need to be set.
- `Physical Layout`:
  - Don't know how to use it.
  - Don't know how to define it.
  - Don't know how to write the code.
  - Don't know what standard the code's coordinate system is based on.

All of the above was deduced through my "reverse analysis". In the beginning, I specifically emphasized that I am "pioneering." This means that the number of `Error`s I personally encountered while writing this text is probably `10` times more, or even greater, than what you might face over several months.

<br>

1. Cannot set up "menu-style" modifications for "partial" `Layout`s like `VIAL`.
    * Freedom is restricted.
    * The code engineering workload is extremely massive.

2. Having too many multi-layout setups will cause MCU memory allocation issues.
    * Deep water zone.

<br>

Being able to pioneer to this extent for everyone is also quite a surprising thing for me. After all, I already knew that `ZMK` itself is an extremely "unfriendly" firmware for novices, but I didn't expect the level of unfriendliness to be beyond my imagination.

If this included tutorial article can allow everyone's keyboards to "smoothly" appear on the list during Bluetooth device searches, then the significance of this is entirely different.

I am `DreaM117er`, an amateur keyboard developer from Taiwan. If there are more opportunities, I will continue to pioneer.

<br>

> As for pointing devices, I will find time to isolate them using a development board for pioneering and debugging later. Please wait for me a while.

