# Changes From Upstream

This fork keeps the upstream HALO TFT firmware as the base, then adds a custom hardware-focused variant for a build without the MiCS gas board.

## Main firmware changes

- added `TFT_LCD/T-Display-Long/V1/Firmware/ESPHome/Halo-v1-Core-local.yaml` as the custom HALO core
- updated `TFT_LCD/T-Display-Long/V1/Firmware/ESPHome/Halo-v1.yaml` to include the local custom core directly
- switched dashboard import to the fork repository path
- vendored the ESPHome RMT compatibility override under `TFT_LCD/T-Display-Long/V1/Firmware/ESPHome/external_components/esphome_rmt_compat`

## Sensor and hardware changes

- removed MiCS-4514 support from the custom firmware variant
- removed MiCS-only TFT rows and Home Assistant entities
- removed the MiCS NO2 offset setting
- enabled BME280 support for the actual hardware build and forced I2C address `0x76`
- aligned BME280 pressure sampling to `60s` for pressure-trend logic

## Display and UI changes

- preserved the stock HALO TFT visual style while compacting the layout
- added a pressure row to the overview page
- added a three-page touch UI:
  - `Overview`
  - `Trends`
  - `Status`
- added touch wake, swipe navigation, and row-tap graph selection
- added selectable history graphs on the overview page
- added larger PM2.5 and VOC charts on the trends page
- added compact graph header labels and time-window labels

## Graph and trend changes

- added per-metric history buffers for display graphs
- added configurable graph durations for:
  - temperature
  - CO2
  - PM1
  - PM2.5
  - PM4
  - PM10
  - VOC
  - RH
  - pressure
- added automatic graph switching with configurable interval
- graph auto-switch order is:
  - `TEMP`
  - `CO2`
  - `PM` (highest current PM metric)
  - `VOC`
  - `RH`
  - `PRESS`

## Pressure weather logic

- added `6h` pressure history
- added smoothed pressure comparisons for:
  - `1h`
  - `3h`
  - `6h`
- added Home Assistant entities for:
  - `Pressure Delta 1h`
  - `Pressure Delta 3h`
  - `Pressure Delta 6h`
  - `Pressure Trend`
  - `Pressure Weather Message`
- added pressure weather states such as:
  - `Storm Risk`
  - `Rapid Drop`
  - `Improving`
  - `Stable`
  - `Minor Drift`

## LED changes

- added configurable LED count with a default of `4`
- disabled startup blink by default
- added auto air-quality LED mode
- added auto graph switching and display-related controls in Home Assistant
- tuned LED brightness defaults for a small WS2812 setup

## Home Assistant changes

- reorganized user-facing config entity names into grouped prefixes:
  - `Display: ...`
  - `Graph: ...`
  - `LED: ...`
  - `Sensor: ...`
  - `System: ...`
- documented the Home Assistant entity-registry caveat for renamed or previously disabled entities

## Build and maintenance changes

- added `TFT_LCD/T-Display-Long/V1/Firmware/ESPHome/WSL-Build.md`
- added `TFT_LCD/T-Display-Long/V1/Firmware/ESPHome/requirements-wsl.txt`
- extended the ESPHome `.gitignore` with Python cache ignores
- added `Project.md` to track the custom firmware project state and roadmap
