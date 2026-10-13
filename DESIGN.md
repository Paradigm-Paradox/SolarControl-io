# SolarControl-io: Design & Architecture Document

## Overview
**SolarControl-io** is an edge-native Home Assistant Add-on providing automated energy orchestration, battery storage (BESS) optimization, Time-of-Use (TOU) arbitrage, excess solar diversion (thermal/EV), and dynamic tariff protection.

For full architectural details, mathematical models, and subsystem specifications, see [docs/DESIGN.md](file:///mnt/data/HomeAssitant-Addon/docs/DESIGN.md).

## Core Capabilities
1. **Real-time Energy Flow & Power Balance**:
   - Closed-loop telemetry uniting Solar ($P_{pv}$), Battery ($P_{batt}$), Grid ($P_{grid}$), Home ($P_{home}$), and Diverted ($P_{diverted}$) loads.
   - High-frequency polling (default 5s) directly through Home Assistant Supervisor Core API.

2. **Battery Energy Storage System (BESS) Management**:
   - Maximizes solar self-consumption, minimizing grid reliance.
   - Time-of-Use (TOU) arbitrage: Pre-charge during super off-peak, discharge during peak cost periods.
   - Storm / severe weather emergency backup reserve auto-bump.
   - Battery health guarding: Min/max SoC buffers and charge rate clamping.

3. **Intelligent Excess Solar Diversion & Load Routing**:
   - Tiered priority dispatch: Proportional water heating, EV smart solar charging, HVAC pre-conditioning, deferrable loads (pool pump, dehumidifier).
   - Anti-hunting hysteresis and minimum cycle timers to protect equipment.

4. **Dynamic Tariff & Negative Price Protection**:
   - Real-time ingestion of dynamic market rates (Nord Pool, Amber Electric, Tibber).
   - Inverter curtailment or instant diversion during negative feed-in tariff conditions.
   - Zero-export and utility-mandated export limit enforcement.

5. **Integrated Ingress Web Dashboard**:
   - Animated SVG/Canvas power flow diagram embedded into Home Assistant's sidebar.
   - One-click mode switching: `Auto Optimizer`, `Self Consumption`, `TOU Arbitrage`, `Backup Reserve`, `Manual`.
   - Real-time savings and self-sufficiency indicators.

## Project Structure
- `repository.yaml` — Repository manifest for the Home Assistant Add-on Store.
- `solar_control/` — The complete add-on distribution.
  - `config.yaml` — Add-on definition, options schema, and ingress settings.
  - `Dockerfile` — Multi-arch Alpine container.
  - `build.yaml` — Multi-architecture build definitions (`aarch64`, `amd64`, `armv7`).
  - `run.sh` — Add-on startup entrypoint with bashio logging.
  - `app/` — Core Python FastAPI backend and responsive Ingress web dashboard.
    - `engine/` — Power flow balance, battery dispatch, load diversion, and tariff optimizer.
- `docs/` — In-depth architectural specifications and user documentation.
