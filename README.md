# MiciMike drop-in PCB replacement for Google Home Mini (Gen-1)

[MiciMike Home Mini](https://github.com/iMike78/home-mini-v1-drop-in-pcb) is a drop-in PCB replacement for the ["Google Home Mini" (Google's first-generation smart speaker hardware with a Micro-USB charging port)](https://en.wikipedia.org/wiki/Google_Nest_(smart_speakers)), but based on ESP32 and XMOS microcontrollers for running open-source firmware.

<img src="pics/MiciMike_Home_Mini_v1_PCB_Prototype_1.png" width="1000">

Tip! If you are instead looking for a similar drop-in PCB replacement for the "Google Nest Mini" (Google's second-generation smart speaker with a barrel connector charging port) then check out the sister-project at https://github.com/iMike78/nest-mini-drop-in-pcb

Both of these are fully open-source hardware projects, taking some concep inspiration from the [Onju Voice](https://github.com/justLV/onju-voice) however aiming to follow [Open Home Foundation's open voice assistants standard with Home Assistant Voice Preview Edition as reference](https://www.home-assistant.io/blog/2024/12/19/voice-preview-edition-the-era-of-open-voice/) for PCB designs and specifications.

# Project scope

The goal of this project/repository and its sister-project (which are both similar to the concept in [Onju Voice](https://github.com/justLV/onju-voice) but under a fully open-source hardware license) is to design drop-in replacement PCBs (Printed Circuit Board) with hardware schematics following open voice assistants standard from the Open Home Foundation so that anyone can make or order from a one-stop PCB manufacturer as a custom drop-in replacement PCB for the Google Nest Mini (2nd Gen) to convert it into open-source voice assistant hardware.

These projects is primarly targeting people looking to repurpose their old Google Home Mini (and Google Nest Mini) smart speakers into open-source hardware for [Voice Control of Home Assistant](https://www.home-assistant.io/voice_control/) and/or media player speaker output for [Music Assistant](https://www.music-assistant.io), (the hardware can however probably also be used with other applications as well with other firmware as it is based on the popular Espressif ESP32 platform).

The hardware design will (similar to [Home Assistant Voice Preview Edition](https://www.home-assistant.io/blog/2024/12/19/voice-preview-edition-the-era-of-open-voice/)) integrate an ESP32-S3 microcontroller for WiFi, BLE, and [onboard wake-word detection](https://www.home-assistant.io/voice_control/about_wake_word/) (using the no-code [ESPHome firmware](https://esphome.io/) firmware) + an XMOS xCORE XU316 chip for advanced audio processing (with custom firmware for microphone cleanup offloading for better voice recognition capabilities by using using locally running algorithms for Noise Suppression, Acoustic Echo Cancellation, Interference Cancellation, and Automatic Gain Control).

Again, functionality-wise it is made to be mostly hardware comatible with the [Home Assistant Voice Preview Edition (a.k.a. Home Assistant Voice PE](https://www.home-assistant.io/blog/2024/12/19/voice-preview-edition-the-era-of-open-voice/) reference design (which has been released as open-source hardware design from Open Home Foundation in coolaboration with Nabu Casa). The main difference will be due to very tight constraints defined by the Google Home Mini and Google Nest Mini enclosures and its other comonents, (the design of the replacement PCBs will therefore be limited by the same type of physical capacity inputs as the original hardware from Google).

As such the scope for these projects is not to develop new features/functions for the ESPHome and XMOS firmware, so if you want that then you instead need to turn to the firmware development of the Home Assistant Voice Preview Edition as well as to the upstream ESPHome repositories on GitHub:

- https://github.com/esphome/home-assistant-voice-pe
  - https://github.com/esphome/esphome
    - https://github.com/esphome/voice-kit-xmos-firmware

## Request for collaboration and contributions

If you are a non-developer and would like to show your support me working on these projects please consider [buying me a coffee via Ko-fi](https://ko-fi.com/imike78) ☕ 

If you have experience with board-level electrical design / working with PCB design andlayout in CAD software (epecially with PCB routing and ground pouring in KiCad, or optimizing noise-sensitive digital+analog layouts), then **your help would be highly appreciated**! Please feel free to open a new issue and add comments to existing issues, or start a new discussion in this repository to suggestions/requests/input/feedback.

For reference, you can find some more background information about the original concept/idea see and post to Home Assistant related community discussion in this Home Assistant community forum thread:

- https://community.home-assistant.io/t/any-news-on-alternative-to-onju-voice-pcb-repacement-design-for-google-nest-home-mini-speakers-with-added-xmos-chip-to-match-official-home-assistant-voice-preview-edition-reference-hardware/860001/

### Current status

- ✅ Schematic completed
- ✅ Component placement done
- ✅ Routing
- ✅ Ground pour, shielding strategy, and EMI considerations
- ✅ Test run of the first test batch
- ✅ Ready for manufacturing

## Tools used

- 🛠️ KiCad 9
- 🧰 SnapEDA / LCSC for footprint sourcing

## Known hardware specifications

- 4-layer PCB
- ESP32-S3R8 bare chip (ESP32-S3 for WiFi, BLE, and onboard wake-word detection) with 16MB flash
- XMOS XU316-1024-QF60B-C24 (XMOS XU316 xCORE DSP audio processing) with 4MB flash
- Dual I²S buses (to allow I2S interfaces at the same time, i.e. simultaneous audio output and audio input)
- AIC3204 DAC and TPA6211 PA for speaker output
- 2x MEMS microphones (dual T3902 with 68mm inter-mic spacing)
- 4x SK6812 RGB LEDs

## References

### Home Assistant Voice Preview Edition resources including PCB design files
- https://www.home-assistant.io/blog/2024/12/19/voice-preview-edition-the-era-of-open-voice/
  - https://voice-pe.home-assistant.io/resources/
    - https://support.nabucasa.com/hc/en-us/categories/24451727188125-Home-Assistant-Voice-Preview-Edition
      - https://support.nabucasa.com/hc/en-us/sections/24980381645213-Resources

#### ESPHome firmware for Home Assistant Voice PE (which also use the same ESP32-S3 + XMOS XU316 combination):

- https://github.com/esphome/home-assistant-voice-pe
  - https://esphome.github.io/home-assistant-voice-pe/
- https://voice-pe.home-assistant.io/

### XMOS xCORE DSP (XU316-1024-QF60B-C32) MCU IC chip

- https://www.xmos.com/download/XU316-1024-QF60B-xcore.ai-Datasheet(3).pdf
- https://www.xmos.com/software-tools/
  - https://www.xmos.com/develop/xcore-voice
  - https://www.xmos.com/usb-multichannel-audio/
  - https://www.xmos.com/xcore-ai
 
#### XMOS firmware from the ESPHome prokect for the Home Assistant Voice Preview Edition hardware:

- https://github.com/esphome/voice-kit-xmos-firmware
  - https://github.com/esphome/xmos_fwk_rtos
  - https://github.com/esphome/xmos_fwk_io

## License

This project is licensed under the [CERN Open Hardware License Version 2 - Strongly Reciprocal (CERN-OHL-S v2)]
Any modified version of this hardware must also be distributed under the same license.


