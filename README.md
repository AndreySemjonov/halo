![Halo_v1](assets/Halo_v1.JPG)

# HALO Custom Fork

This repository is a personal fork of the original [HALO project](https://github.com/yashmulgaonkar/halo).

The upstream project supports multiple HALO hardware variants and a broader feature set. This fork focuses on one real, working TFT build and reshapes the firmware around the hardware that is actually installed:

- LILYGO T-Display S3 Long
- SCD41
- SEN54 / SEN55-compatible ESPHome integration
- BME280
- WS2812 LEDs
- no MiCS-4514 gas board

The goal of this fork is not to preserve every upstream option. The goal is to keep the stock HALO look, simplify the firmware for the installed hardware, and add practical quality-of-life improvements for daily use.

---

## What This Fork Changes

Compared with the upstream repository, this fork adds a custom TFT firmware variant with:

- MiCS-4514 gas support removed from the active firmware path
- BME280 support restored and forced to address `0x76`
- vendored ESPHome RMT compatibility component for local reproducible builds
- pressure display and pressure weather-trend logic
- selectable and auto-switching history graphs
- multi-page touch UI
- improved WS2812 behavior for a small LED count
- grouped Home Assistant configuration entities

The full change list is documented in [CHANGES_FROM_UPSTREAM.md](CHANGES_FROM_UPSTREAM.md).

---

## Firmware Layout In This Fork

The active custom firmware lives under:

- [TFT_LCD/T-Display-Long/V1/Firmware/ESPHome/Halo-v1.yaml](TFT_LCD/T-Display-Long/V1/Firmware/ESPHome/Halo-v1.yaml)
- [TFT_LCD/T-Display-Long/V1/Firmware/ESPHome/Halo-v1-Core-local.yaml](TFT_LCD/T-Display-Long/V1/Firmware/ESPHome/Halo-v1-Core-local.yaml)

Supporting local build files:

- [TFT_LCD/T-Display-Long/V1/Firmware/ESPHome/WSL-Build.md](TFT_LCD/T-Display-Long/V1/Firmware/ESPHome/WSL-Build.md)
- [TFT_LCD/T-Display-Long/V1/Firmware/ESPHome/requirements-wsl.txt](TFT_LCD/T-Display-Long/V1/Firmware/ESPHome/requirements-wsl.txt)
- [TFT_LCD/T-Display-Long/V1/Firmware/ESPHome/external_components/esphome_rmt_compat](TFT_LCD/T-Display-Long/V1/Firmware/ESPHome/external_components/esphome_rmt_compat)

---

## Current Hardware Target

### Installed in this build

- LILYGO T-Display S3 Long
- SCD41
- SEN54
- BME280
- HALO MainBoard V1.1
- HALO Breakout V1.0

### Not installed in this build

- MiCS-4514 / SEN0377 gas board

### LED setup

- WS2812
- intended for the first `1-4` LEDs or a short external strip
- not intended for the stock full-bright 11-LED ring behavior

---

## Main Firmware Features In This Fork

### Sensors

- SCD41 CO2
- SEN54 / SEN55 particulate measurements:
  - PM1
  - PM2.5
  - PM4
  - PM10
- SEN54 / SEN55 VOC
- SEN54 / SEN55 temperature and humidity
- BME280 temperature, humidity, and pressure

### Display

- keeps the stock HALO TFT style
- three-page touch UI:
  - `Overview`
  - `Trends`
  - `Status`
- swipe page switching
- touch wake
- row-tap graph selection on overview
- larger PM2.5 and VOC charts on trends page
- compact graph headers with visible time windows

### Graphs

- selectable overview graph for:
  - `TEMP`
  - `CO2`
  - `PM`
  - `VOC`
  - `RH`
  - `PRESS`
- automatic graph switching with configurable interval
- per-metric configurable history duration
- PM auto-selection uses the highest live PM metric at switch time

### Pressure trend logic

- whole-number pressure display in `hPa`
- `6h` pressure history
- smoothed comparison vs:
  - `1h ago`
  - `3h ago`
  - `6h ago`
- published HA entities for:
  - `Pressure Delta 1h`
  - `Pressure Delta 3h`
  - `Pressure Delta 6h`
  - `Pressure Trend`
  - `Pressure Weather Message`

### LEDs

- configurable LED count with default `4`
- startup blink disabled by default
- manual Home Assistant light control retained
- automatic air-quality LED mode
- day/night LED brightness handling

### Home Assistant

- grouped configuration entity names:
  - `Display: ...`
  - `Graph: ...`
  - `LED: ...`
  - `Sensor: ...`
  - `System: ...`

---

## What Was Removed Or Changed From Upstream

### MiCS gas path removed from the custom variant

This fork removes the MiCS-specific path from the active custom firmware:

- MiCS sensor backend removed from the custom TFT build
- MiCS-only Home Assistant entities removed
- MiCS offset setting removed
- MiCS rows removed from the main TFT UI

### Build setup changed

Instead of depending on the live ESPHome PR reference for RMT, this fork vendors the required component override locally so the custom build is reproducible.

### Wrapper behavior changed

The upstream `Halo-v1.yaml` wrapper fetched the core package remotely.  
This fork changes it to include the custom local core directly:

- easier to diff in the fork
- easier to build locally
- easier to keep custom changes explicit

---

## Build Notes

The recommended build path for this fork is documented in:

- [TFT_LCD/T-Display-Long/V1/Firmware/ESPHome/WSL-Build.md](TFT_LCD/T-Display-Long/V1/Firmware/ESPHome/WSL-Build.md)

Short version:

1. Use a Linux-side workspace in WSL.
2. Enter `TFT_LCD/T-Display-Long/V1/Firmware/ESPHome`.
3. Install the pinned requirements from `requirements-wsl.txt`.
4. Run:

```bash
esphome config Halo-v1.yaml
esphome compile Halo-v1.yaml
```

---

## Known Notes

- some WS2812 setups still benefit from a proper 5V logic level shifter
- Home Assistant may keep stale registry entries when entities were previously disabled or renamed
- the stock Home Assistant device page is still limited by Home Assistant's own layout rules

---

## Original Project

Original upstream repository:

- [yashmulgaonkar/halo](https://github.com/yashmulgaonkar/halo)

Original upstream community/build information:

- [HALO wiki](https://github.com/yashmulgaonkar/halo/wiki)
- [HALO Discord](https://discord.gg/wjqgUjv8Re)

---

## Why This Fork Exists

This fork exists to keep one real HALO device maintained as a practical daily-use firmware build:

- less unsupported hardware baggage
- better local build reproducibility
- more useful TFT interaction
- more useful trend visualization
- more practical Home Assistant integration for this exact hardware setup
