# Solar Concentrator :sunny:

This project is a homemade automated solar concentrator that uses mirrors to focus sunlight and generate concentrated heat energy. It can produce up to **1000 watts of thermal power** while consuming only **3.8 watts of electrical power** - an incredible 260:1 efficiency ratio!

![Solar Concentrator](solar_concentrator.jpg)

- [Overview](#overview)
- [Safety Warning](#safety-warning)
- [Performance & Features](#performance--features)
- [System Architecture](#system-architecture)
- [Hardware Components](#hardware-components)
- [Software Components](#software-components)
- [Build Instructions](#build-instructions)
- [Development Environment](#development-environment)
- [Future Expansion](#future-expansion)
- [Licenses](#licenses)

## Overview

The solar concentrator consists of three main subsystems:

1. **Mechanical System**: An orientable mirror panel with 48 focusing mirrors (1m² total reflective area) that automatically tracks the sun using a cable-driven positioning system
2. **Electronics**: Custom supervisor board with ESP32-CAM for computer vision and control, plus Arduino Pro Mini for motor control
3. **Software**: Distributed embedded system with sun tracking algorithms, computer vision target detection, and web-based monitoring interface

### Key Innovation

Instead of traditional dual-axis tracking with independent motors, this design uses a novel **cable-bot mechanism** that provides precise positioning with cheaper components and simpler mechanical construction.

## Safety Warning

> [!CAUTION]
> :warning::warning: **THIS PROJECT MAY HARM PEOPLE OR ANIMALS IN THE SURROUNDINGS** :warning::warning:
>
> - Can **cause permanent blindness, burn skin or ignite objects**
> - Concentrates sunlight **48 times** in a small area reaching temperatures of **210°C+**
> - The focal point **constantly moves** along unpredictable paths
> - **Must be continuously monitored** during operation
> - Mirrors must be covered immediately in case of any anomaly

## Performance & Features

### Achieved Performance

- **Thermal Output**: Up to 1000W of concentrated heat
- **Electrical Consumption**: Only 3.8W
- **Efficiency Ratio**: 260:1 energy multiplication
- **Peak Temperature**: 210°C achieved in 30 minutes
- **Tracking Accuracy**: ±0.5° precision
- **Operating Duration**: 7+ hours of continuous automated tracking

### Key Features

- **Fully Automated**: Sun tracking with computer vision feedback
- **Low-Cost Design**: Uses standard, cheap, and available components
- **Hand-Buildable**: No high-precision manufacturing required
- **Modular Architecture**: Easy to understand, modify, and expand
- **Open Source**: Complete hardware and software designs available

### Current Limitations

- No automated safety systems implemented
- Requires manual initial panel orientation
- Single panel operation only
- Weather-dependent (clear skies required)
- Continuous human supervision necessary

## System Architecture

### Control System Hierarchy

```
┌─────────────────┐    ┌──────────────────┐    ┌─────────────────┐
│   Web Interface │◄──►│ ESP32-CAM        │◄──►│ Arduino Pro Mini│
│   (Monitoring)  │    │ (Supervisor)     │    │ (Motor Control) │
└─────────────────┘    └──────────────────┘    └─────────────────┘
                              │                          │
                              ▼                          ▼
                       ┌─────────────┐            ┌─────────────┐
                       │   Camera    │            │   Motors    │
                       │ (Computer   │            │ (Physical   │
                       │  Vision)    │            │ Movement)   │
                       └─────────────┘            └─────────────┘
```

## Hardware Components

### Mechanical Components

- **48 decorative bathroom mirrors** (15×15cm each) - €30
- **2 model reduction motors** (MFA-950D8101LN) - €30 each
- **Wood frame structure** with aluminum tubing
- **Cable-driven positioning system** with pulleys
- **3kg counterweight** for balance
- **Concrete oven target** with tempered glass window

### Electronic Components

- **ESP32-CAM module** - €14.30 (main controller with camera)
- **Arduino Pro Mini** - €8.95 (motor controller)
- **Custom supervisor PCB** (designed with LibrePCB)
- **Power management**: 12V to 5V regulation
- **Motor drivers**: GT1112 dual motor controllers
- **Switches and connectors** for system control

**Total Hardware Cost**: ~€250-300

### Target System

- Concrete oven construction
- Black metal heat absorption plate
- Tempered glass window (repurposed IKEA shelf)
- Aluminum foil insulation

## Software Components

### ESP32-CAM Supervisor Controller

- **Sun Tracker**: Astronomical calculations for precise solar positioning
- **Target Detector**: Computer vision for closed-loop feedback control
- **Web Interface**: Real-time monitoring and manual control dashboard
- **Camera System**: 30 FPS image processing for target detection
- **Safety Systems**: Watchdog timers and emergency shutdown

### Arduino Pro Mini Motor Controller

- **Motor Control**: PWM control with position feedback
- **Serial Communication**: Command protocol with supervisor
- **Safety Interlocks**: Emergency stop functionality

### Development Tools

- **Simulator**: Panda3D-based power calculation and system modeling
- **Testing Framework**: Unit and integration tests for all components
- **Docker Environment**: Containerized development with USB passthrough

### Key Algorithms

- **Astronomical Sun Tracking**: ±0.1° accuracy solar ephemeris calculations
- **Computer Vision**: Real-time bright spot detection and centroid calculation
- **PID Control**: Smooth motor positioning with error correction
- **Safety Monitoring**: Continuous system health checks

## Build Instructions

### Prerequisites

- Basic woodworking tools
- Soldering equipment
- 3D printer or CNC machine (for custom brackets)
- Electronics assembly tools

### Assembly Process

1. **Mechanical Construction**

   - Build wood frame structure
   - Install motor blocks and pulleys
   - Mount mirror array on positioning board
   - Set up cable-driven movement system

2. **Electronics Assembly**

   - Solder custom supervisor board
   - Program ESP32-CAM and Arduino Pro Mini
   - Install in weatherproof enclosure
   - Connect power and communication cables

3. **Target Construction**

   - Build concrete oven structure
   - Install heat absorption plate
   - Mount tempered glass window

4. **System Integration**
   - Calibrate camera and positioning
   - Test safety systems
   - Configure web interface

## Development Environment

### Docker Setup

The project includes a complete containerized development environment:

```bash
cd docker/
./docker_build_image     # Build development container
./docker_up_with_usb     # Start with USB device access
./docker_bash            # Enter development shell
```

### Building Software

```bash
# ESP32 Firmware
cd /solar_concentrator/software/supervisor_controller
./esp32_configure_cmake
./esp32_build_all
./esp32_flash_usb

# Run Tests
cd build_tests
cmake -DTESTS_ON_HOST=1 -DCMAKE_BUILD_TYPE:STRING=DEBUG ..
ninja && ctest -V

# Simulator
cd /solar_concentrator/software/simulator/sources
python3 solar_concentrator_simulator.py
```

### Hardware Design Tools

- **LibrePCB**: Open-source PCB design for electronics
- **OpenSCAD**: 3D modeling for mechanical parts
- **AISLER**: PCB manufacturing service

## Future Expansion

The modular architecture enables scaling to industrial applications:

### Multi-Panel Systems

- Control multiple panels for kilowatt-scale power output
- Distributed processing architecture
- Advanced coordination algorithms

### Potential Applications

- **Industrial Heating**: Metal/glass melting, large-scale cooking
- **Water Treatment**: Desalination and sterilization systems
- **Enhanced Solar PV**: Concentrated photovoltaics integration
- **Research Platform**: Solar energy experimentation

### Planned Improvements

- Machine learning optimization for weather adaptation
- Advanced safety systems for unattended operation
- Cloud-based monitoring and control
- Mobile app interface

## Licenses

- **Software**: GNU GPL v3
- **Hardware**: CERN Open Hardware Licence v2 Strongly Reciprocal

### Third-Party Components

- **ESP-IDF**: Espressif IoT Development Framework
- **CImg**: C++ Template Image Processing Toolkit
- **Panda3D**: Python 3D engine for simulation
- **LibrePCB**: Open-source PCB design software

---

**This project demonstrates that sophisticated solar energy concentration can be achieved using readily available components and open-source design principles. The combination of mechanical simplicity, electronic sophistication, and software intelligence creates a powerful platform for solar energy research and applications.**
