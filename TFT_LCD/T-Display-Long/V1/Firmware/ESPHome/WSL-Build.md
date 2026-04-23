# Halo WSL Build Setup

Use WSL Ubuntu for repeatable local builds instead of building from `G:\My Drive\...`.

## Recommended Workspace

Create a Linux-side working copy:

```bash
mkdir -p ~/src
cp -r /mnt/g/My\ Drive/visual\ studio/repos/ESPHome\ Halo ~/src/halo-air-sensor
cd ~/src/halo-air-sensor/TFT_LCD/T-Display-Long/V1/Firmware/ESPHome
```

## Python Environment

Create a virtual environment and install the pinned ESPHome version:

```bash
sudo apt update
sudo apt install -y python3 python3-venv build-essential git
python3 -m venv .venv
source .venv/bin/activate
pip install --upgrade pip
pip install -r requirements-wsl.txt
```

## Validate and Build

Run the normal ESPHome validation and compile steps:

```bash
source .venv/bin/activate
esphome config Halo-v1.yaml
esphome compile Halo-v1.yaml
```

## Notes

- `Halo-v1.yaml` remains the entrypoint wrapper in `TFT_LCD/T-Display-Long/V1/Firmware/ESPHome`.
- `Halo-v1-Core-local.yaml` contains the local HALO customizations for the no-MiCS build.
- The RMT LED strip compatibility override is vendored under `external_components/esphome_rmt_compat` so builds do not depend on the live ESPHome pull request reference.
