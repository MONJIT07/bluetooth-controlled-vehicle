# Bluetooth Controlled Vehicle 🚗

A Bluetooth-controlled robotic vehicle built using **Arduino Uno, HC-05 Bluetooth module, L298N motor driver, and two DC motors**. The vehicle receives movement commands wirelessly from a smartphone and controls the motors according to the received command.

## 📌 Project Overview

This project demonstrates a basic embedded control system in which a smartphone acts as the command source, the HC-05 provides wireless communication, the Arduino Uno processes the commands, and the L298N motor driver controls the DC motors.

The vehicle supports:

* Forward movement
* Backward movement
* Left turn
* Right turn
* Stop
* PWM-based motor control
* Wireless control through Bluetooth
* Differential steering

## 🏗️ System Architecture

```text
              ┌──────────────────────┐
              │      Smartphone      │
              │    Bluetooth App     │
              └──────────┬───────────┘
                         │
                  F / B / L / R / S
                         │
                         ▼
              ┌──────────────────────┐
              │        HC-05         │
              │ Bluetooth Module     │
              └──────────┬───────────┘
                         │
                    UART / Serial
                      9600 baud
                         │
                         ▼
              ┌──────────────────────┐
              │     Arduino Uno      │
              │                      │
              │ Command Reception    │
              │ Command Decoding     │
              │ Motor Control Logic  │
              └──────────┬───────────┘
                         │
                 Direction + PWM
                         │
                         ▼
              ┌──────────────────────┐
              │        L298N         │
              │    H-Bridge Driver   │
              └──────────┬───────────┘
                         │
                 ┌───────┴───────┐
                 ▼               ▼
            Left Motor       Right Motor
                 │               │
                 └───────┬───────┘
                         ▼
                    Vehicle Motion
```

## 🔧 Components Used

| Component     | Purpose                                |
| ------------- | -------------------------------------- |
| Arduino Uno   | Main microcontroller and control logic |
| HC-05         | Bluetooth wireless communication       |
| L298N         | Dual H-bridge motor driver             |
| DC Motors × 2 | Vehicle movement                       |
| Battery       | Power supply                           |
| Robot chassis | Mechanical structure                   |
| Smartphone    | Wireless command source                |

## 🔌 Pin Configuration

The current implementation uses the following Arduino pin configuration:

### HC-05 Bluetooth

| Arduino Pin | HC-05 | Function            |
| ----------- | ----- | ------------------- |
| D9          | TX    | Arduino software RX |
| D10         | RX    | Arduino software TX |

The Arduino code uses:

```cpp
SoftwareSerial bluetoothSerial(9, 10);
```

Communication is configured at:

```cpp
bluetoothSerial.begin(9600);
```

### L298N Motor Driver

| Arduino Pin | L298N Pin | Function              |
| ----------- | --------- | --------------------- |
| D5          | ENA       | Left motor PWM        |
| D6          | IN1       | Left motor direction  |
| D7          | IN2       | Left motor direction  |
| D3          | ENB       | Right motor PWM       |
| D8          | IN3       | Right motor direction |
| D4          | IN4       | Right motor direction |

## 📡 Bluetooth Command Protocol

The smartphone sends single-character commands to the HC-05.

| Command | Function |
| ------- | -------- |
| `F`     | Forward  |
| `B`     | Backward |
| `L`     | Left     |
| `R`     | Right    |
| `S`     | Stop     |

The Arduino reads the received character and uses a `switch` statement to execute the corresponding motor-control function.

## ⚙️ Motor Control Logic

### Forward

```text
Left Motor  → Forward
Right Motor → Forward
```

Arduino logic:

```cpp
IN1 = HIGH
IN2 = LOW

IN3 = HIGH
IN4 = LOW
```

### Backward

```text
Left Motor  → Backward
Right Motor → Backward
```

```cpp
IN1 = LOW
IN2 = HIGH

IN3 = LOW
IN4 = HIGH
```

### Left

The left motor rotates backward while the right motor rotates forward.

```text
Left Motor  → Backward
Right Motor → Forward
```

### Right

The left motor rotates forward while the right motor rotates backward.

```text
Left Motor  → Forward
Right Motor → Backward
```

### Stop

Both motor enable signals are set to zero and all direction pins are set LOW.

```cpp
analogWrite(ENA, 0);
analogWrite(ENB, 0);
```

## 🎛️ PWM Speed Control

The L298N enable pins are controlled using PWM:

```cpp
analogWrite(ENA, motorSpeed);
analogWrite(ENB, motorSpeed);
```

The current implementation uses:

```cpp
int motorSpeed = 255;
```

Arduino Uno provides an 8-bit PWM value from **0 to 255**.

```text
0   → 0% duty cycle
128 → approximately 50%
255 → approximately 100%
```

PWM allows the motor speed to be adjusted without changing the motor-control logic.

## 🔄 Differential Steering

The vehicle uses **differential steering**.

Instead of using a mechanical steering mechanism, the direction of the vehicle is controlled by independently controlling the left and right motors.

For example:

```text
LEFT TURN

Left Motor  → Reverse
Right Motor → Forward
```

This causes the vehicle to rotate toward the left.

Similarly:

```text
RIGHT TURN

Left Motor  → Forward
Right Motor → Reverse
```

## 💻 Software Flow

```text
Start
  │
  ▼
Initialize GPIO pins
  │
  ▼
Initialize Bluetooth at 9600 baud
  │
  ▼
Stop motors
  │
  ▼
Check Bluetooth data
  │
  ├── No data ──────► Check again
  │
  ▼
Read command
  │
  ▼
Decode command
  │
  ├── F ──► Forward
  ├── B ──► Backward
  ├── L ──► Left
  ├── R ──► Right
  └── S ──► Stop
```

## 🧠 Working Principle

1. The user presses a movement button on the smartphone.
2. The smartphone sends the corresponding character through Bluetooth.
3. The HC-05 receives the character.
4. The HC-05 transfers the data to the Arduino through serial communication.
5. The Arduino checks whether data is available.
6. The Arduino reads the received character.
7. A `switch` statement identifies the requested movement.
8. The Arduino generates direction signals for the L298N.
9. PWM is applied to the motor enable pins.
10. The L298N drives the two DC motors.
11. The vehicle performs the requested movement.

## 🧩 Main Arduino Code Structure

The firmware is organized into separate functions for each movement:

```cpp
forward();
back();
left();
right();
Stop();
```

This makes the motor-control logic easier to understand and maintain.

The main loop is responsible for receiving and decoding commands, while the individual functions handle motor control.

## 🔋 Power Architecture

The battery provides the electrical power required by the vehicle.

The system can be conceptually divided into:

```text
Battery
   │
   ├──────────────► Motor Driver ───► DC Motors
   │
   └──────────────► Arduino / Control Electronics
```

Proper grounding between the control electronics and motor-driver circuitry is important for reliable operation.

## 🔄 Communication

The project uses asynchronous serial communication between the HC-05 and Arduino.

The current implementation uses:

```text
Baud Rate: 9600
Data: Single-character commands
Interface: UART-style serial communication
```

The Arduino uses `SoftwareSerial` so that digital pins 9 and 10 can be used for the Bluetooth interface.

## 🟢 Advantages

* Simple wireless control
* Low-cost hardware
* Easy-to-understand embedded architecture
* Simple command protocol
* PWM-based motor control
* Differential steering
* Modular motor-control functions

## ⚠️ Current Limitations

The current implementation is intentionally simple and has some limitations:

* It is an **open-loop control system**.
* There is no wheel encoder feedback.
* There is no PID-based speed control.
* There is no obstacle detection.
* There is no battery monitoring.
* There is no Bluetooth command timeout.
* There is no acknowledgement or error-checking mechanism for commands.
* The current motor speed is fixed at `255`.

If Bluetooth communication is interrupted while the vehicle is moving, the current firmware does not automatically stop the motors because there is no communication timeout mechanism.

## 🚀 Possible Future Improvements

### 1. Bluetooth Timeout / Fail-Safe

Add a timeout using `millis()` so that the vehicle automatically stops if no valid command is received for a specified period.

```text
No command for timeout period
             ↓
        Stop motors
```

### 2. Wheel Encoders

Add encoders to measure actual wheel rotation.

```text
Motor → Wheel Encoder → Arduino
                         │
                         ▼
                    Feedback
```

This would enable closed-loop control.

### 3. PID Speed Control

With encoder feedback, PID control could be implemented to maintain a desired wheel speed.

### 4. Variable Speed Control

Instead of keeping:

```cpp
motorSpeed = 255;
```

the smartphone could send speed commands, allowing the vehicle to operate at different speeds.

### 5. Obstacle Detection

Ultrasonic or other distance sensors could be added to detect obstacles and automatically stop or change direction.

### 6. Robust Communication Protocol

The simple single-character protocol could be extended using:

* Start/end markers
* Command IDs
* Sequence numbers
* Checksums/CRC
* Acknowledgements

This would make communication more robust.

## 🛠️ Technologies Used

* **Arduino C++**
* **Arduino Uno**
* **HC-05 Bluetooth**
* **UART / Serial Communication**
* **SoftwareSerial**
* **PWM**
* **GPIO**
* **L298N H-Bridge**
* **DC Motor Control**
* **Differential Steering**

## 📁 Project Structure

```text
bluetooth-controlled-vehicle/
│
├── Bluetooth RC Car.txt
└── README.md
```

## 🎯 Learning Outcomes

Through this project, I worked with:

* Microcontroller-based embedded programming
* GPIO configuration
* UART-style serial communication
* Bluetooth communication
* PWM generation
* H-bridge motor control
* DC motor direction control
* Differential steering
* Embedded C++ programming
* Hardware-software integration
* Basic debugging and testing

## 📌 Project Summary

This project demonstrates how a microcontroller can receive wireless commands and convert them into real-time actuator control.

The complete control chain is:

```text
Smartphone
    ↓
Bluetooth
    ↓
HC-05
    ↓
Serial Communication
    ↓
Arduino Uno
    ↓
Command Processing
    ↓
PWM + Direction Signals
    ↓
L298N H-Bridge
    ↓
DC Motors
    ↓
Vehicle Movement
```

The current implementation provides a simple and reliable foundation that can be extended with feedback control, safety mechanisms, sensors, and more robust communication.
