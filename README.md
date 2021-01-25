See also the [project work log](https://bitbuilt.net/forums/index.php?threads/wiirdflex.3365/)

Features
---------

The final board designs here will include two flexible PCBs designed to be mounted on top (CPU/GPU side) and bottom (AVE side) of the Wii.  These will provide the following features:

* Power supplies (top)
* Temperature sensor (top)
* External communication ports
  * USB ports (2x, top)
  * SD card (top)
  * Gamecube controllers (4x, bottom)
  * Gamecube memory cards (2x, bottom)
  * Extra GPIO (6x, bottom)
* Wii component relocations
  * Bluetooth module (top, external)
  * WiFi module (bottom, external)
  * NAND (bottom, on flex)
  * MX (bottom, external)
  * U10 (bottom, on flex)
  * AVE (bottom, external), including:
    * I2C, I2S, digital video and "mode" pin
    * optional pull resistors on flex for mode control
* Other (pins available for these things)
  * U-AMP/wiiHUD compatibility
  * RVL-DD compatibility
  

Connection Strategies
----------------------

Top Side:
  TODO: the following is a general idea of the order of operations, but parts of some tasks may interleave with others.  There may also be other, better, orders to follow (I'm thinking of an alternate strategy using low-temp solder paste).  Reorganize into an actual instruction list.
  
  TODO: any extra alignment aid? 3d printable pin jig for mounting holes maybe?

  0. Prep:
    a. TODO: other prep work (install bottom flex, perform U10/NAND/MX/AVE relocations, etc. and test) 
    b. Trim Wii
    c. Desolder bulk caps (TODO: references), label and set aside, or ensure you have replacements ready (TODO: suggest replacement options)
    d. Desolder other components (TODO: list which ones) and discard
    e. If using bluetooth and rev 0 flex, cut out bluetooth trace access window on flex
  
  1. Power
    a. solder bulk caps to flex, ensuring good coat (thin but even) of solder on bottom side (before doing USB)
    b. prepare thermal sensing solution by following step 4a
    c. prepare USB by following steps 2[a-b]
    d. prepare BT by following steps 3[a-e]
    e. prepare SD by fololwing steps 5[a-b]
    f. align flex and solder bulk caps
    g. complete remaining steps of sections 2-5

  2. USB
    a. solder magnet wire into each via, leaving a bit of length
    b. thread magnet wires through corresponding holes on flex
    c. align flex and solder down
    d. trim wires flush

  3. Bluetooth:
    a. cut data traces
    b. shave data traces
    c. solder magnet wire to data traces and sense via
    d. thread through flex
    e. place and align flex
    f. solder wires to flex
    g. trim wires

  4. Thermal sensing
    a. solder components to flex
    b. peel adhesive, align whole flex and adhere to Wii
  
  5. SD
    a. solder magnet wire into SD_CLK via
    b. thread wire through flex
    c. align flex and solder down
    d. trim wire flush
    e. solder termination resistors (TODO: references)

Bottom side:
  TODO: this section is a work in progress. it currently is just a general outline of the planned connections from a couple different perspectives.  pin counts are estimates.
  
  - A/V [relocation]: 19 pins
    - I2S audio (4 pins)
      - MCLK, WS, SD, SCK
    - digital video
      - V[0:7], "54"
    - I2C (wiiHUD/RVL-DD)
      - SDA, SCL
    - SPI ([RVL-DD](https://bitbuilt.net/forums/index.php?threads/rvl-dd-documentation.3951/))
      - CS, CK, DI
    - M(mode)
    - TODO: check for others needed by AVE but not listed here
  - MX [relocation]: 9 pins
  - NAND [relocation]: (17 pins, but not offboard)
  - Fan control: 1 pin
  - GC memory cards: 12 pins
  - GC controllers: 4 pins
  - Reset: 1 pin (shared with MX? not sure)
  - Extra test points: 6 pins
  - WiFi: 6 pins

  External connections:
    - main ZIF connection for portables [27 signal + ?? power/GND]
      - Power (3.3v, 1.8v, GND) [??]
        - this is to provide power from Wii to support circuitry to avoid need for extra routing from the power management system, NOT to power the Wii itself
      - Digital video [9]
      - I2S [4]
      - AVE I2C [2]
      - TP223 (Headphone detect in [wiiHUD connection diagram](https://bitbuilt.net/forums/index.php?threads/gboy-rev3.2959/#lg=thread-2959&slide=1)) [1]
      - SPI CS for RVL-DD [1]
      - MX [9], including 2 pins also used for RVL-DD SPI
      - P1 Controller [1]
    - DLC ZIF connection (some of these probably are dupes of others used elsewhere) [29 signal + ?? power/GND]
      - Fan control [1]
      - Reset [1]
      - GC mem cards [12]
      - Extra test points [5]
      - WiFi [6]
      - Video mode [1]
      - P2-4 controllers [3]
    - additional components:
      - pull resistors for "mode" pin
      - voltage detector (U10) with decoupling cap(s)
      - source termination resistor arrays as needed (digital video, i2s, etc)
      - other pull resistors as needed (reset, I2C, GC controllers, mem cards, etc)
      - any other components removed from Wii
    - ZIF pin ordering preferences? a few ideas, probably contradicting each other a bit:
      - probably match wii ordering for AVE pins
      - i2c near bottom-right (in Altium layout orientation) for ease of transition across to PMS board
      - i2s also near same, since audio codec will be in back case
