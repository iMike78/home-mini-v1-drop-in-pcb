Changelog for MiciMike drop-in PCB replacement for Google Home Mini (Gen-1):

* https://github.com/Hedda/home-mini-v1-drop-in-pcb/

## Update – v0.9.5: Touch controller migration to MPR121 + new test points

### Touch controller change
The ESP32-S3 built-in touch peripheral has been replaced with an external **MPR121 capacitive touch controller** (I²C, address 0x5A) due to an unresolved ESP-IDF / hardware issue (IDFGH-16448) that caused the touch sensor FSM to lock up, leaving all channels stuck at the maximum raw value.

**What changed**
- **Hardware:** MPR121 added on the I²C bus, powered from the clean +3V3 rail (pre-ferrite). Touch pads connect via ELEC0–ELEC2; IRQ on GPIO2. Unused electrodes tied together to GND through a single 1MΩ resistor.
- **Firmware:** `esp32_touch` component removed; replaced with `mpr121` hub and three `binary_sensor` entries on channels 0/1/2 (volume_down / center / volume_up). Behavior, multi-click handling and LED feedback are unchanged.
- **Freed pins:** ESP32-S3 GPIO3/4 are now general-purpose, not in use.

**Why**

The IDF-level bug is closed by Espressif as "Resolution: NA" — no upstream fix is planned. The MPR121 is a robust, well-supported solution (native ESPHome component) and removes the touch reliability risk ahead of the production run.

### New solderable test points
Following community feedback, the board now exposes large, easy-to-solder test points to support alternative use cases — for users who don't have an original Google Home Mini with its factory daughterboard and FPC connector, or who want to integrate the board into a custom enclosure:

- **+5V and GND power input pads** — allow direct power injection without relying on the FPC connector through 3 mm test points
- **Mute switch pad** — exposes the hardware mute input, which has an on-board pull-up. **Pull this pad to GND to power the microphones**; otherwise the board stays in MUTE state and the mic path is disabled. That's how the hardware mute switch works.

The power pads are sized for hand soldering, so even less experienced users can wire them up reliably.
