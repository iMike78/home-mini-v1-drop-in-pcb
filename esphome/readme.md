# MiciMike ESPHome Firmware

This directory contains the ESPHome firmware configuration for MiciMike.

## Files

```text
esphome/
|-- micimike-factory.yml  # Factory / first-install firmware
|-- micimike-voice.yml    # Full voice assistant firmware
```

## Factory Firmware

`micimike-factory.yml` follows the Home Assistant Voice Preview Edition factory pattern. It is not a tiny bootstrap-only image; it includes the full MiciMike voice assistant configuration and adds the first-install provisioning layer on top.

The factory overlay provides:

- BLE Improv provisioning
- Serial Improv provisioning
- Home Assistant discovery
- Center-button authorization for BLE Improv
- BLE disable after Wi-Fi connection, after a short delay so Improv can report success
- HTTP update entity backed by the GitHub release manifest
- Optional beta firmware update source switch

The fallback Wi-Fi AP is present, but `captive_portal:` and `web_server:` are intentionally not enabled. ESPHome may warn about this during validation; that is expected for this provisioning model.

## BLE Improv Flow

1. Flash the factory firmware over USB.
2. Home Assistant discovers the device via BLE Improv.
3. Home Assistant asks for Wi-Fi credentials.
4. Home Assistant asks for center-button authorization.
5. The device connects to Wi-Fi and is added as an ESPHome device.

The center button is implemented through the MPR121 capacitive touch controller on channel 1.

## Flashing

From the repository root:

```bash
esphome run esphome/micimike-factory.yml
```

To flash a known serial port:

```bash
esphome run esphome/micimike-factory.yml --device COM8
```

## Release Manifest

GitHub releases should provide `manifest.json`, `manifest-beta.json`, and the firmware binary referenced by the manifest.

Example:

```json
{
  "name": "MiciMike Voice Assistant",
  "version": "v0.9.8",
  "builds": [
    {
      "chipFamily": "ESP32-S3",
      "improv": false,
      "parts": [
        {
          "path": "micimike-voice.bin",
          "offset": 0
        }
      ]
    }
  ]
}
```

Expected release assets:

```text
manifest.json
manifest-beta.json
micimike-voice.bin
```

## Notes

- The factory firmware mirrors the Home Assistant Voice PE provisioning behavior as closely as possible.
- The factory firmware already contains the full voice assistant configuration through `micimike-voice.yml`.
- The HTTP update entity can check the GitHub release manifest, but installing an update still depends on the ESPHome/Home Assistant update flow.
- Future firmware updates are not forced automatically; users can update through Home Assistant or ESPHome when a release is available.
