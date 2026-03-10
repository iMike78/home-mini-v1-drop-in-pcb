# MiciMike Factory Firmware Structure

## Overview

The factory firmware structure enables:
1. Minimal factory firmware pre-installed at manufacturing
2. Initial setup via BLE Improv for WiFi configuration
3. Automatic upgrade to full firmware after WiFi connection

## File Structure

```
esphome/
├── micimike-factory.yml          # Factory firmware (manufacturing)
└── micimike-voice.yml            # Full firmware (your current config)
```

## Factory Firmware (micimike-factory.yml)

### Main Components:

1. **Package Import**: Loads the full firmware configuration
   ```yaml
   packages:
     micimike_voice: !include micimike-voice.yml
   ```

2. **BLE Improv Provisioning**: 
   - Uses touch center button for authentication
   - BLE connection for WiFi setup
   
3. **HTTP OTA Update**:
   - Downloads manifest from GitHub releases
   - Automatically updates to full firmware

4. **Beta Firmware Switch**:
   - Enables beta version testing
   - Switches manifest URL between production/beta

## Key Differences from HA VPE:

### 1. Center Button Authorizer
HA VPE: `authorizer: center_button` (physical button)
MiciMike: `authorizer: center_button` (touch button - ESP32-S3 GPIO3)

The touch button works the same as physical:
```yaml
binary_sensor:
  - platform: esp32_touch
    id: center_button
    threshold: 5000
    pin: GPIO3
```

### 2. LED Control
The factory firmware also supports the `control_leds` script:
- During BLE provisioning: `improv_ble_in_progress = true`
- During initialization: `init_in_progress = true`
- After WiFi connection: normal LED operation

## Installation Process

### 1. Flash Factory Firmware (USB)
```bash
esphome run micimike-factory.yml
```

### 2. BLE Improv Provisioning
- Device starts in AP mode
- ESPHome app or Home Assistant discovers it
- Touch center button for authentication
- WiFi configuration

### 3. Automatic Update
- 5 second delay after WiFi connection
- BLE disable
- HTTP OTA downloads manifest
- Automatic update to full firmware
- Reboot with full functionality

## Manifest.json Structure

GitHub releases must contain a manifest.json:

```json
{
  "name": "MiciMike Voice Assistant",
  "version": "v0.9.0",
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

## GitHub Releases Structure

```
Releases/
├── v0.9.0/
│   ├── manifest.json           # Production manifest
│   ├── manifest-beta.json      # Beta manifest
│   └── micimike-voice.bin      # Full firmware binary
```

## Benefits

1. **Simple Manufacturing**: Only factory firmware needs to be flashed
2. **OTA Updates**: Users receive full version via WiFi
3. **Beta Channel**: Optional beta firmware testing
4. **No USB Dependency**: After initial flash, everything is OTA
5. **Version Tracking**: GitHub releases-based version management

## Notes

- Touch center button works as authentication just like physical button
- All full configuration functionality is preserved
- Factory firmware contains only bootstrap functions
- LED control works in both versions
- Future Firmware updates are not automatic. Users must manually check for new releases and update via ESPHome Dashboard.
