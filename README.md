# 🚗 Bluetooth Controlled Vehicle

A Bluetooth-controlled robotic vehicle built with an **Arduino Uno** and **HC-05 Bluetooth module**, controllable via a smartphone app. The system supports bidirectional DC motor control with PWM-based speed regulation and full directional maneuverability.

---

## 📌 Features

- Wireless Bluetooth communication up to **10 meters** range
- **Bidirectional DC motor control** — forward, reverse, left-turn, right-turn
- **PWM-based speed regulation** for smooth motor response
- **L298N H-Bridge motor driver IC** for high-current motor drive
- Stable power delivery via **lithium battery pack** with voltage regulation
- Validated through iterative hardware testing for signal latency, motor torque response, and Bluetooth reliability

---

## 🛠️ Hardware Components

| Component | Purpose |
|---|---|
| Arduino Uno | Main microcontroller |
| HC-05 Bluetooth Module | Wireless serial communication |
| L298N H-Bridge Motor Driver | High-current DC motor drive |
| DC Motors (x2) | Vehicle locomotion |
| Lithium Battery Pack | Power supply |
| Multimeter | Debugging & voltage verification |

---

## 🔌 Circuit Overview

```
Smartphone App
      |
   Bluetooth
      |
   HC-05  ──→  Arduino Uno  ──→  L298N H-Bridge  ──→  DC Motors
                                        ↑
                              Lithium Battery Pack
                           (with voltage regulation)
```

- The **HC-05** communicates with the Arduino over serial (RX/TX).
- The **Arduino** sends PWM signals and direction pins to the L298N.
- The **L298N** drives two DC motors independently, enabling differential steering.
- The **lithium battery pack** powers both the motor driver and the microcontroller via regulated output.

---

## 📱 Smartphone Control

Use any standard **Bluetooth RC Controller** app (Android) to send serial commands to the HC-05 module.

| Command | Action |
|---|---|
| `F` | Forward |
| `B` | Reverse |
| `L` | Left Turn |
| `R` | Right Turn |
| `S` | Stop |

> You can customize these commands in the Arduino sketch to match your preferred controller app.

---

## 💻 Software & Tools

- **Arduino IDE** — firmware development
- **Arduino C++** — motor control logic with PWM

---

## 🚀 Getting Started

### Prerequisites
- Arduino IDE installed
- HC-05 Bluetooth module paired with your smartphone
- A Bluetooth RC controller app installed (e.g., *Arduino Bluetooth Controller* on Android)

### Upload & Run

1. Clone this repository:
   ```bash
   git clone https://github.com/MONJIT07/bluetooth-controlled-vehicle.git
   cd bluetooth-controlled-vehicle
   ```

2. Open `bluetooth_car.ino` in Arduino IDE.

3. Connect your Arduino Uno via USB and select the correct **Board** and **Port** under `Tools`.

4. Upload the sketch.

5. Disconnect USB, power the vehicle via the lithium battery pack.

6. Pair your smartphone with the **HC-05** (default PIN: `1234` or `0000`).

7. Open your Bluetooth RC app and start driving.

---

## 📐 Pin Configuration

| Arduino Pin | Connected To |
|---|---|
| D3 (PWM) | L298N ENA (Motor A speed) |
| D5 (PWM) | L298N ENB (Motor B speed) |
| D4 | L298N IN1 |
| D6 | L298N IN2 |
| D7 | L298N IN3 |
| D8 | L298N IN4 |
| D0 (RX) | HC-05 TX |
| D1 (TX) | HC-05 RX |

> ⚠️ Disconnect HC-05 from RX/TX before uploading the sketch to avoid upload errors.

---

## 📊 Testing & Validation

| Test | Result |
|---|---|
| Bluetooth range | Stable up to **10 meters** |
| Signal latency | Low latency, responsive steering |
| Motor torque response | Consistent under load |
| Directional accuracy | All 4 directions validated |

---

## 📁 Project Structure

```
bluetooth-controlled-vehicle/
├── bluetooth_car.ino       # Main Arduino sketch
├── circuit_diagram.png     # Wiring schematic
└── README.md
```

---

## 👤 Author

**Monjit Tamuli**  
B.Tech Electrical Engineering, NIT Silchar  
GitHub: [@MONJIT07](https://github.com/MONJIT07)  
Email: monjittamuli7747@gmail.com
