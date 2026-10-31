# 🐭 Micromouse Maze Solver

### An Autonomous Maze-Solving Robot Powered by ESP32

An autonomous Micromouse robot designed to explore, map, and solve mazes using the **FloodFill algorithm**, three VL53L1X Time-of-Flight (ToF) distance sensors, and encoder-based motor control.

🏆 **First Runner-Up – IIIT Nagpur TantraFiesta 2026**

Built using an ESP32 DevKit, TB6612FNG motor driver, N20 geared motors with encoders, and three VL53L1X distance sensors from RoboFly.

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Competition Achievement](#-competition-achievement)
- [Hardware Components](#-hardware-components)
- [Features](#-features)
- [Working Principle](#-working-principle)
- [Algorithm](#-algorithm)
- [Pin Configuration](#-pin-configuration)
- [Circuit and Wiring](#-circuit-and-wiring)
- [Software Requirements](#-software-requirements)
- [Setup Instructions](#-setup-instructions)
- [Code Structure](#-code-structure)
- [Tuning Parameters](#-tuning-parameters)
- [Usage](#-usage)
- [Future Improvements](#-future-improvements)
- [Contributing](#-contributing)
- [License](#-license)

---

## 🔍 Overview

Micromouse is a robotics challenge in which a small autonomous robot must navigate through an unknown maze, detect walls, remember its surroundings, and find the shortest path to a predefined destination.

This project implements an autonomous maze-solving robot using an ESP32 microcontroller.

The robot uses three VL53L1X Time-of-Flight sensors to detect walls in the front, left, and right directions. N20 geared motors with encoders provide movement feedback, while the TB6612FNG motor driver controls the motors.

The robot explores the maze, builds an internal representation of the maze, and uses the FloodFill algorithm to calculate an optimal path toward the destination.

### Key Specifications

| Parameter | Specification |
|---|---|
| Microcontroller | ESP32 DevKit |
| Motor Driver | TB6612FNG |
| Motors | N20 DC Geared Motors with Encoders |
| Distance Sensors | 3 × VL53L1X ToF |
| Sensor Interface | I2C |
| Motor Feedback | Quadrature Encoders |
| Navigation Algorithm | FloodFill |
| Movement Control | Encoder-based control |
| Maze Mapping | Real-time wall detection |
| Chassis | Custom-built |
| Wheels | Caster wheel + motor wheels |

---

## 🏆 Competition Achievement

<image-placeholder>

**First Runner-Up – IIIT Nagpur TantraFiesta 2026**

</image-placeholder>

Proud to secure **First Runner-Up** in the Micromouse competition at IIIT Nagpur's TantraFiesta 2026.

This achievement represents the hard work involved in designing, assembling, programming, debugging, and tuning an autonomous maze-solving robot.

---

## 🔩 Hardware Components

The robot is built using the following hardware components:

| Component | Quantity | Purpose |
|---|---:|---|
| ESP32 DevKit | 1 | Main microcontroller |
| TB6612FNG Motor Driver | 1 | Dual DC motor control |
| VL53L1X ToF Distance Sensor (RoboFly) | 3 | Front, left and right wall detection |
| N20 Geared DC Motors with Encoders | 2 | Robot movement and feedback |
| Prefboard | 1 | Circuit assembly and connections |
| Caster Wheel | 1 | Mechanical balance and support |
| Motor Wheels | 2 | Robot movement |
| Battery | 1 | Power supply |
| Connecting Wires | As required | Electrical connections |
| Robot Chassis | 1 | Mechanical structure |

### Main Components

**1. ESP32 DevKit**

The ESP32 acts as the brain of the robot. It processes sensor readings, executes the maze-solving algorithm, reads encoder feedback, and controls motor movement.

**2. TB6612FNG Motor Driver**

The TB6612FNG is a dual-channel motor driver used to independently control the left and right N20 motors.

**3. VL53L1X ToF Distance Sensors**

Three VL53L1X sensors are used to measure the distance between the robot and maze walls.

- Front sensor – Detects obstacles ahead.
- Left sensor – Detects walls on the left.
- Right sensor – Detects walls on the right.

Each sensor has an independent XSHUT connection, allowing the ESP32 to assign a unique I2C address during initialization.

**4. N20 Motors with Encoders**

N20 geared motors provide movement, while their encoders measure wheel rotation. Encoder feedback helps estimate distance travelled and improves movement accuracy.

**5. Prefboard and Caster Wheel**

The prefboard is used for electrical connections and mounting circuit components. The caster wheel provides support and helps maintain the robot's balance.

---

## ✨ Features

- **FloodFill Algorithm:** Calculates the shortest path to the destination.
- **Three-Direction Wall Detection:** Uses three VL53L1X sensors to detect front, left, and right walls.
- **Real-Time Maze Mapping:** Updates the internal maze representation as the robot explores.
- **Encoder-Based Movement:** Uses motor encoder feedback for accurate distance measurement.
- **Dual Motor Control:** Independently controls left and right motors using TB6612FNG.
- **Autonomous Navigation:** Explores and navigates the maze without continuous human control.
- **I2C Sensor Address Management:** Uses individual XSHUT pins to configure three VL53L1X sensors.
- **ESP32-Based Processing:** Performs sensor reading, navigation, and motor control on a single microcontroller.

---

## 🧠 Working Principle

The Micromouse operates through the following steps:

1. **Initialization:** Initializes the ESP32, motor driver, encoders, and three ToF sensors.
2. **Sensor Configuration:** Assigns unique I2C addresses to the three VL53L1X sensors using XSHUT pins.
3. **Wall Detection:** Measures distances in front, left, and right directions.
4. **Maze Mapping:** Stores detected walls in the internal maze representation.
5. **Path Calculation:** Uses FloodFill to calculate the distance of each reachable cell from the destination.
6. **Movement Decision:** Selects the next cell based on the lowest available flood value.
7. **Motor Control:** Controls the N20 motors through the TB6612FNG.
8. **Encoder Feedback:** Tracks motor rotation to estimate movement distance.
9. **Continuous Exploration:** Repeats wall detection, maze updating, and navigation until the destination is reached.

---

## 🧮 Algorithm

### FloodFill Algorithm

FloodFill is a pathfinding algorithm commonly used in Micromouse competitions.

It assigns a numerical value to each cell based on its distance from the destination.

The robot always tries to move toward an accessible neighboring cell with a lower flood value.

### How It Works

**Step 1: Initialize the Maze**

- Create an internal maze representation.
- Mark known boundary walls.
- Set the destination cell or cells to zero.

**Step 2: Calculate Flood Values**

- Assign zero to the destination.
- Assign increasing values to neighboring cells.
- Continue until all reachable cells have appropriate values.

**Step 3: Detect Walls**

- Read front, left, and right VL53L1X sensors.
- Determine whether walls are present.
- Update the maze representation.

**Step 4: Select the Next Cell**

Choose an accessible neighboring cell with the lowest flood value.

**Step 5: Update the Maze**

Whenever new walls are discovered, recalculate flood values to account for the updated maze.

**Step 6: Reach the Destination**

Continue exploring and navigating until the target is reached.

### FloodFill Example

A simplified representation of flood values:

```text
+---+---+---+---+
| 6 | 5 | 4 | 3 |
+---+---+---+---+
| 7 | 6 | 3 | 2 |
+---+---+---+---+
| 8 | 7 | 2 | 1 |
+---+---+---+---+
| 9 | 8 | 1 | 0 |
+---+---+---+---+
```

The destination is represented by `0`. The robot navigates toward cells with decreasing flood values while avoiding detected walls.

---

## 📌 Pin Configuration

The following GPIO configuration is used in the project.

### 1. TB6612FNG Motor Driver

| TB6612FNG Pin | ESP32 GPIO | Function |
|---|---:|---|
| AIN1 | GPIO 26 | Left motor direction |
| AIN2 | GPIO 25 | Left motor direction |
| PWMA | GPIO 19 | Left motor speed |
| BIN1 | GPIO 27 | Right motor direction |
| BIN2 | GPIO 14 | Right motor direction |
| PWMB | GPIO 18 | Right motor speed |
| STBY | GPIO 13 | Motor driver standby |
| VCC | 3.3V | Logic supply |
| VM | Battery Positive | Motor supply |
| GND | GND | Common ground |

### 2. N20 Motor Encoders

| Encoder Signal | Left Motor | Right Motor |
|---|---:|---:|
| C1 | GPIO 34 | GPIO 2 |
| C2 | GPIO 35 | GPIO 15 |
| VCC | 3.3V | 3.3V |
| GND | GND | GND |

Encoder wire connections:

| Wire Colour | Function |
|---|---|
| White | Motor terminal M1 |
| Red | Motor terminal M2 |
| Black | Encoder VCC |
| Blue | Encoder GND |
| Yellow / Green | Encoder C1 / C2 |

*Note: C1 and C2 signal assignment determines encoder counting direction. If forward movement produces negative counts, adjust the encoder direction sign in the code.*

### 3. VL53L1X ToF Sensors

All three sensors share the same I2C bus but use separate XSHUT pins.

| Sensor Connection | Front | Left | Right |
|---|---:|---:|---:|
| VIN | 3.3V | 3.3V | 3.3V |
| GND | GND | GND | GND |
| SDA | GPIO 21 | GPIO 21 | GPIO 21 |
| SCL | GPIO 22 | GPIO 22 | GPIO 22 |
| XSHUT | GPIO 33 | GPIO 32 | GPIO 4 |
| I2C Address | 0x30 | 0x31 | 0x32 |

### I2C Address Configuration

Since all three VL53L1X sensors have the same default I2C address, individual XSHUT pins are used to initialize them separately.

The initialization sequence is:

1. Set all three XSHUT pins LOW.
2. Activate the front sensor and assign address `0x30`.
3. Activate the left sensor and assign address `0x31`.
4. Activate the right sensor and assign address `0x32`.
5. Start reading all three sensors using the shared I2C bus.

---

## 🔌 Circuit and Wiring

### ESP32 to TB6612FNG

```text
ESP32 DevKit             TB6612FNG
---------------------------------
GPIO 26  --------------> AIN1
GPIO 25  --------------> AIN2
GPIO 19  --------------> PWMA

GPIO 27  --------------> BIN1
GPIO 14  --------------> BIN2
GPIO 18  --------------> PWMB

GPIO 13  --------------> STBY

3.3V     --------------> VCC
Battery + --------------> VM
GND      --------------> GND
```

### ESP32 to VL53L1X Sensors

```text
ESP32 DevKit             All 3 Sensors
---------------------------------------
GPIO 21  --------------> SDA
GPIO 22  --------------> SCL
3.3V     --------------> VIN
GND      --------------> GND

GPIO 33  --------------> Front XSHUT
GPIO 32  --------------> Left XSHUT
GPIO 4   --------------> Right XSHUT
```

### ESP32 to Motor Encoders

```text
ESP32 DevKit             Left       Right
-----------------------------------------
GPIO 34  --------------> C1
GPIO 35  --------------> C2

GPIO 2   --------------------------> C1
GPIO 15  --------------------------> C2

3.3V     --------------> VCC       VCC
GND      --------------> GND       GND
```

### ⚠️ Important Electrical Notes

- All grounds must be connected together: ESP32, motor driver, battery, sensors, and encoders.
- Connect battery positive to the TB6612FNG VM motor supply input only.
- Never connect battery voltage directly to ESP32 GPIO pins or sensor VIN.
- Use the ESP32 3.3V supply for the motor driver's logic VCC, sensors, and encoder supply as specified.
- GPIO 34 and GPIO 35 are input-only pins and do not have internal pull-up resistors. Add external 10kΩ pull-ups if encoder signals require them.
- GPIO 2 and GPIO 15 are ESP32 strapping pins. Disconnect encoder wires temporarily if they interfere with uploading or booting.
- Check the battery voltage and motor-driver current limits before powering the robot.

---

## 💻 Software Requirements

### Development Environment

- Arduino IDE
- ESP32 Board Package
- C/C++ programming language
- Serial Monitor for debugging

### Required Libraries

Install the appropriate libraries for:

- `Wire.h` – I2C communication.
- `VL53L1X` sensor library – Distance measurement.
- ESP32 LEDC PWM functionality – Motor speed control.

The exact VL53L1X library depends on the implementation used in the source code.

---

## 📥 Setup Instructions

### 1. Clone the Repository

```bash
git clone YOUR_REPOSITORY_URL
cd YOUR_REPOSITORY_NAME
```

### 2. Install Arduino IDE

Download and install Arduino IDE.

### 3. Configure ESP32

- Open Arduino IDE.
- Install the ESP32 board package.
- Select `ESP32 Dev Module`.
- Set the upload speed to `115200`.

### 4. Install Required Libraries

Open:

`Sketch → Include Library → Manage Libraries`

Search for and install the VL53L1X library used by your project.

### 5. Connect the Hardware

- Connect the motor driver to the ESP32.
- Connect both N20 motors and encoders.
- Connect all three VL53L1X sensors.
- Verify the XSHUT and I2C connections.
- Check common ground and power connections.

### 6. Upload the Code

- Open the Micromouse project in Arduino IDE.
- Select the correct ESP32 COM port.
- Verify the GPIO configuration.
- Compile and upload the program.

---

## 📂 Code Structure

The project can be organized into the following structure:

```text
Micromouse/
│
├── Micromouse.ino
│
├── sensors/
│   └── SensorControl.h
│
├── motors/
│   └── MotorControl.h
│
├── encoders/
│   └── EncoderControl.h
│
├── navigation/
│   └── FloodFill.h
│
├── control/
│   └── PIDControl.h
│
├── maze/
│   └── MazeMapping.h
│
└── README.md
```

*This is a suggested organization. Adjust it to match the actual files in your repository.*

---

## ⚙️ Tuning Parameters

The following parameters can be adjusted to improve the robot's performance.

| Parameter | Description |
|---|---|
| Cell Distance | Encoder counts required to move one cell |
| Base Speed | Normal motor PWM speed |
| Maximum Speed | Maximum allowed motor speed |
| Minimum Speed | Minimum movement speed |
| Turn Speed | Motor speed during turning |
| Left Wall Threshold | Distance used for left wall detection |
| Right Wall Threshold | Distance used for right wall detection |
| Front Wall Threshold | Distance used for front wall detection |
| Encoder Direction | Direction correction for encoder counts |
| PID Kp | Proportional gain |
| PID Ki | Integral gain |
| PID Kd | Derivative gain |

Tune these values according to the actual robot dimensions, motor characteristics, maze dimensions, and sensor readings.

---

## 🚀 Usage

### Quick Start

1. Place the Micromouse at the starting position.
2. Power on the ESP32 and motor driver.
3. Allow all three VL53L1X sensors to initialize.
4. Verify sensor readings using the Serial Monitor.
5. Start the maze exploration program.
6. The robot detects walls and updates its internal maze.
7. FloodFill calculates the next movement direction.
8. Encoder feedback helps control movement.
9. The robot continues until it reaches the destination.

### Expected Behaviour

```text
Power ON
   |
   v
Initialize ESP32
   |
   v
Initialize ToF Sensors
   |
   v
Read Wall Distances
   |
   v
Update Maze Mapping
   |
   v
Execute FloodFill
   |
   v
Select Next Cell
   |
   v
Control Motors
   |
   v
Read Encoder Feedback
   |
   v
Destination Reached?
   |
   +---- No ----> Repeat
   |
   +---- Yes ---> Stop Robot
```

---

## 🔮 Future Improvements

- Implement optimized speed profiling for faster maze traversal.
- Improve PID-based wall-following accuracy.
- Add more accurate encoder-based position estimation.
- Implement faster maze exploration strategies.
- Optimize turning and alignment for competitive performance.
- Add improved sensor filtering to reduce measurement noise.
- Implement a dedicated shortest-path speed run after maze exploration.

---

## 🤝 Contributing

Contributions, suggestions, and improvements are welcome!

If you have ideas to improve maze-solving algorithms, motor control, sensor accuracy, or robot performance, feel free to contribute.

---

## 📜 License

This project is open for learning, experimentation, and robotics development.

Add an appropriate open-source license to the repository before redistributing the project.

---

## 👨‍💻 About

**Micromouse Maze Solver – ESP32 Based Autonomous Robotics Project**

Designed and developed as a competitive autonomous robotics project.

🏆 **First Runner-Up – IIIT Nagpur TantraFiesta 2026**

Built with passion for robotics, embedded systems, autonomous navigation, and problem-solving.

**If you like this project, don't forget to ⭐ the repository!**

