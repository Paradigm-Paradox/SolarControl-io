# SolarControl-io: ESP32 Edge Nodes & Microcontroller Integration

SolarControl-io leverages Espressif **ESP32** microcontrollers as distributed edge nodes for local, real-time interfacing with high-voltage solar hardware, battery management systems (BMS), and proportional diversion loads.

## Why ESP32?
- **Low Cost & Energy Efficient**: Consumes < 0.5W while providing dual-core 240MHz processing.
- **Versatile Networking Options**:
  - **Wi-Fi (802.11 b/g/n)**: Ubiquitous wireless connectivity for sub-panels and sensors.
  - **Wired Ethernet (10/100 Mbps)**: Eliminates wireless interference in noisy inverter environments (supports WT32-ETH01, Olimex ESP32-POE, or LAN8720 / W5500 modules).
  - **Bluetooth / BLE**: Direct wireless polling of BLE battery BMS units (JK-BMS, JBD, Daly) without running extra communication cables.
- **Rich Hardware Interfaces**: Hardware UARTs for Modbus RS485, hardware CAN controller (TWAI), high-speed PWM for SSR heater control, and 1-Wire thermal probes.
- **Fail-Safe Watchdogs**: Node-level safety watchdogs autonomously shut off high-power diverters or lock inverters into safe autonomous states if the central network or Home Assistant drops offline.

---

## Ready-to-Flash Node Configurations

| Configuration | Target Hardware | Function | Interface / Protocol |
| :--- | :--- | :--- | :--- |
| [`inverter_modbus_rs485.yaml`](inverter_modbus_rs485.yaml) | ESP32-WROOM / WT32-ETH01 | Inverter telemetry & control | RS485 Modbus RTU (SunSpec, Growatt, Deye, Solis) |
| [`bms_can_ble_monitor.yaml`](bms_can_ble_monitor.yaml) | ESP32 / ESP32-S3 | Battery cell health & SoC | CAN Bus (Pylontech/EG4) & BLE (JK/Daly BMS) |
| [`solar_diverter_ssr.yaml`](solar_diverter_ssr.yaml) | ESP32 DevKit | Excess solar water heating | Zero-cross SSR PWM + DS18B20 1-Wire temp |

---

## Hardware Schematics & Wiring

### 1. Inverter Modbus RS485 Bridge
```
ESP32 Pin               MAX485 / SP3485 Module           Inverter RJ45/Terminal
─────────────────────────────────────────────────────────────────────────────
GPIO17 (TX)    ───►     DI (Driver Input)
GPIO16 (RX)    ◄───     RO (Receiver Output)
GPIO4  (RTS)   ───►     DE & RE (Transmit Enable)
3.3V / 5V      ───►     VCC
GND            ───►     GND
                        A (Non-inverting)        ───►    Inverter RS485-A (+)
                        B (Inverting)            ───►    Inverter RS485-B (-)
                        GND (Shield)             ───►    Inverter Comm GND
```

### 2. Battery CAN Bus Transceiver (Pylontech / Seplos / EG4)
```
ESP32 Pin               SN65HVD230 / VP230 (3.3V)        BMS CAN Port
─────────────────────────────────────────────────────────────────────────────
GPIO5 (TX)     ───►     CTX (CAN TX)
GPIO4 (RX)     ◄───     CRX (CAN RX)
3.3V           ───►     3V3
GND            ───►     GND
                        CANH                     ───►    CAN High (Pin 4/7 RJ45)
                        CANL                     ───►    CAN Low  (Pin 5/8 RJ45)
```

### 3. Thermal Diverter (Zero-Crossing Solid State Relay)
```
ESP32 Pin               Solid State Relay (SSR-40DA)     Heater Element
─────────────────────────────────────────────────────────────────────────────
GPIO19 (PWM)   ───►     Input (+) [3-32V DC]
GND            ───►     Input (-)
                        Output Terminal 1        ───►    AC Line (Live)
                        Output Terminal 2        ───►    Water Heater Element
```
> [!CAUTION]
> Working with AC mains voltage (120V/240V) carries risk of electrical shock or fire. Ensure power is disconnected at the circuit breaker before wiring. Install a manual high-limit thermal cutout on all hot water elements.

---

## Flashing via ESPHome

1. Install the **ESPHome** add-on in Home Assistant.
2. Create a new device and paste the desired configuration from this directory.
3. Configure `!secret wifi_ssid` and `!secret wifi_password` in your ESPHome `secrets.yaml`.
4. Connect the ESP32 via USB and click **Install**.
5. Once flashed, Home Assistant will automatically discover the node via the native ESPHome API.
