# SolarControl-io

[![GitHub Release](https://img.shields.io/badge/release-v0.1.0-blue.svg)](https://github.com/Paradigm-Paradox/SolarControl-io)
[![Home Assistant Add-on](https://img.shields.io/badge/home--assistant-add--on-blue.svg?logo=home-assistant)](https://www.home-assistant.io/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
![Supported Architectures](https://img.shields.io/badge/arch-aarch64%20|%20amd64%20|%20armv7-green.svg)

> **Intelligent, edge-native solar energy management, battery storage optimization, dynamic excess diversion, and tariff arbitrage for Home Assistant.**

---

## ⚡ Overview

**SolarControl-io** turns your Home Assistant instance into a smart, local energy management system (EMS). It coordinates your solar inverters, home battery storage (BESS), EV chargers, and household loads in real time to maximize self-consumption, protect against peak electric rates, and route excess solar into hot water and vehicle batteries.

```
       [ ☀️ Solar PV ]
              │
              ▼
    [ ⚡ Hybrid Inverter ] ◄──────► [ 🔋 Battery (BESS) ]
        ▲            │
        │            ▼
 [ 🌐 Grid ]   [ 🏠 Home Load ]
                     │
         ┌───────────┴───────────┐
         ▼                       ▼
  [ 🚗 EV Charger ]     [ ♨️ Water Heater / HVAC ]
```

---

## 🚀 Key Features

- **⚡ Real-time Power Flow & Balance Engine**: Unifies PV generation, battery charge/discharge, grid import/export, and load telemetry with low-latency closed-loop control.
- **🔋 Battery Energy Storage Optimization**:
  - **Self-Consumption Maximization**: Zero out grid draw using stored solar energy.
  - **Time-of-Use (TOU) Arbitrage**: Intelligent pre-charging on super off-peak tariffs and aggressive peak-shaving.
  - **Storm & Outage Emergency Reserve**: Automatic battery reserve floor bumping on severe weather warnings.
- **♨️ Excess Solar Diversion (Zero-Waste Energy)**:
  - Dynamically routes surplus solar into water heaters, heat pump thermal storage, and EV chargers instead of exporting for pennies.
  - Anti-hunting hysteresis and minimum cycle timers protect equipment against cloud flicker.
- **📉 Dynamic Tariff & Negative Pricing Protection**:
  - Direct integration with Nord Pool, Tibber, Amber Electric, and wholesale dynamic tariffs.
  - Automatic curtailment or diversion trigger when feed-in rates turn negative.
  - Utility export limit and zero-export enforcement.
- **🖥️ Native Home Assistant Ingress UI**:
  - Sleek, responsive web dashboard with live animated power flow diagrams.
  - Mode switches (`Auto Optimizer`, `Self Consumption`, `TOU Arbitrage`, `Backup Reserve`, `Manual`).
  - Diversion priority ordering and instant manual overrides.

---

## 📦 Installation in Home Assistant

1. In your Home Assistant instance, navigate to **Settings** -> **Add-ons** -> **Add-on Store**.
2. Click the three dots (top right) -> **Repositories**.
3. Add this repository URL:
   ```text
   https://github.com/Paradigm-Paradox/SolarControl-io
   ```
4. Find **SolarControl-io** in the add-on store list and click **Install**.
5. Configure your inverter, battery, and meter entities in the **Configuration** tab.
6. Click **Start** and select **Show in sidebar** to access the Ingress dashboard.

---

## 📁 Repository Structure

```text
├── solar_control/               # Complete Home Assistant Add-on
│   ├── app/                     # Python FastAPI server & energy engine
│   │   ├── engine/              # Power flow, battery manager, load diverter
│   │   ├── config.py            # Options loader & pydantic models
│   │   ├── ha_client.py         # Home Assistant Supervisor API client
│   │   └── main.py              # Ingress web application
│   ├── config.yaml              # Add-on configuration & options schema
│   ├── build.yaml               # Multi-arch container base images
│   ├── Dockerfile               # Production container definition
│   ├── run.sh                   # Startup script
│   ├── DOCS.md                  # Detailed user guide
│   └── CHANGELOG.md             # Version history
├── docs/
│   └── DESIGN.md                # System design & architecture specification
├── DESIGN.md                    # Architecture summary
├── repository.yaml              # HA Add-on Repository manifest
└── README.md
```

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
