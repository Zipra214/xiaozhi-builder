# XiaoZhi ESP32 Builder

Web tool to configure, build, and flash XiaoZhi AI Chatbot firmware for ESP32 boards.

## Features

- **Pin Mapper UI** — Visual GPIO configuration per board
- **GitHub Actions Build** — Cloud build via ESP-IDF (no local toolchain needed)
- **WebSerial Flash** — Flash directly from browser using WebSerial API

## Usage

1. Open `web/index.html` in Chrome/Edge
2. Select your board and configure GPIO pins
3. Click "Build Firmware" → GitHub Actions builds it (~6 min)
4. Download the `.bin` from GitHub Artifacts
5. Click "Flash via USB" → Flash directly to ESP32

## How Build Works

```
Web UI → GitHub API dispatch → Actions workflow
    → checkout + submodule (78/xiaozhi-esp32)
    → generate-config.js (merge pins into config.h + sdkconfig)
    → ESP-IDF build (espressif/esp-idf-ci-action)
    → Upload .bin as Artifact (90-day retention)
```

## Project Structure

```
scripts/generate-config.js     # JSON → config.h + sdkconfig
.github/workflows/build.yml    # GitHub Actions workflow
web/index.html                 # Web UI
xiaozhi-esp32/                 # Submodule → 78/xiaozhi-esp32
```

## Manual Build (Terminal)

```bash
gh workflow run build.yml \
  -f board=bread-compact-wifi \
  -f chip=esp32s3 \
  -f language=vi_VN \
  -f config_json='{"pins":{"led":48,"boot_btn":0}}'
```

## License

XiaoZhi ESP32: MIT-0 | Builder tool: MIT
