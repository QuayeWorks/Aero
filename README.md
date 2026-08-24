# QuayeWorks AERO

**QuayeWorks AERO is an offline-first ground-control, aircraft-configuration, navigation, logging, and simulation platform built for the QuayeWorks Hawk-H7 drone system.**

> **Development status:** Phases A, B, and C are implemented and owner-approved. Phase D, the Simulation Lab, is currently in development.

## What AERO Is

QuayeWorks AERO began as a small multi-drone telemetry concept called AeroNet. It has since grown into a native QuayeWorks aerospace application centered on one complete aircraft workflow: configure the flight controller, monitor the aircraft, plan missions offline, validate safety conditions, preserve configuration history, and test future behavior in simulation before it reaches real hardware.

AERO is **not** merely a renamed Betaflight Configurator and is no longer defined as a Python Bluetooth dashboard. The current application uses a native **Tauri/Rust + Vue/TypeScript** architecture, communicates with the `QUAYEWORKS_HAWK_H7` flight controller through MSP, and keeps required Betaflight compatibility behind a QuayeWorks-owned interface and product design.

The normal application is designed to operate without cloud services, remote map servers, analytics, CDNs, or mandatory internet access.

## Current Capabilities

### Aircraft Dashboard

- Board, firmware, API, and connection status
- Armed/disarmed state, active modes, failsafe state, and arming-disable flags
- CPU load, loop status, receiver condition, and I2C error monitoring
- IMU, accelerometer, barometer, magnetometer, and GPS health
- Correct USB/external-power versus main 6S battery classification
- F850 Hex-X motor layout and hardware profile

### Live Telemetry and 3D Attitude

- Live roll, pitch, yaw, heading, altitude, and vertical-speed display
- QuayeWorks Hawk-H7 F850 three-dimensional aircraft visualization
- Artificial horizon, compass, mode, battery, GPS, and receiver instruments
- Telemetry-stale detection instead of fabricated motion

### Flight Tuning

- Roll, pitch, and yaw PID configuration
- D Max and feedforward
- Horizon and Angle properties
- Rates, expo, throttle curves, and output limits
- Gyro, D-term, dynamic-notch, and yaw filtering
- TPA, I-term Relax, Anti Gravity, dynamic damping, and related controller behavior
- Local drafts, exact before/after diffs, configuration snapshots, export/import, and rollback preparation
- Physical-aircraft writes are gated, disabled by default, and require an explicit verified transaction

### Modes, Safety, Hardware, and Calibration

- ARM, HORIZON, ANGLE, failsafe, and GPS Rescue configuration visibility
- Receiver channels and activation ranges
- Sensor and calibration state
- F850 motor map, PWM motor protocol, battery ADC, buzzer, GPS, and board-power information
- Developer diagnostics remain available without cluttering the normal operator interface

### Offline Navigation and Mission Planning

- Local PMTiles map rendering
- MBTiles metadata import foundation
- Live GPS through the shared telemetry scheduler
- Planning home, distance-to-home, bearing, and read-only RTH visualization
- Circle, polygon, and altitude geofences
- Local waypoint mission editor
- Mission validation, checksums, autosave, import/export, altitude profile, and path preview
- Zero required external network requests

Mission planning is currently local. **Physical mission upload and execution remain disabled** until separately versioned QuayeWorks firmware support is implemented and validated.

### Local Logging

- Versioned telemetry-session storage
- Aircraft identity and connection history
- Attitude, GPS, battery, receiver, mode, health, and motor-output data where available
- JSON and CSV export foundations

## Simulation Lab — In Development

Phase D is building a dedicated QuayeWorks Simulation Lab with:

- A six-degree-of-freedom F850 Hex-X physics model
- Six independently modeled motors with correct CW/CCW reaction torque
- Keyboard, gamepad, recorded-input, and safely mirrored receiver-input support
- Simulator-only PID, rate, filter, Horizon, mission, RTH, and geofence testing
- Virtual GPS, barometer, magnetometer, sonar/rangefinders, and three-axis gimbal
- Wind, battery sag, center-of-gravity offsets, sensor faults, and motor faults
- Telemetry graphs, scenario reports, session recording, and replay

The first backend is **LiteSim**, which is intentionally labeled as approximate physics. A later phase will connect AERO to actual QuayeWorks/Betaflight SITL so the real firmware control loop can run in simulation.

## Primary Aircraft Profile

AERO's first production aircraft profile is the **QuayeWorks Hawk-H7 F850**:

- **Flight controller:** QUAYEWORKS_HAWK_H7 / STM32H743
- **Frame:** JeeFly F850 standard Hex-X
- **Motors:** 6 × Flyroun D4312-400KV
- **ESCs:** 6 × T-Motor Flame 70A
- **Battery:** 6S LiPo
- **Propellers:** 15 inch
- **Receiver:** PPM on PF9
- **GPS:** UBLOX on UART6
- **IMU:** MPU6050
- **Barometer:** BMP388
- **Magnetometer:** QMC5883P
- **Battery sensing:** PH2 through an 11:1 divider
- **Buzzer:** PH6
- **Motor protocol:** PWM

The aircraft profile defines expected hardware and capabilities, but AERO reads live PID, filter, rate, mode, and calibration values from the connected flight controller rather than silently forcing defaults.

## Safety Boundaries

QuayeWorks AERO intentionally separates planning, simulation, and physical-aircraft control.

- There is no normal UI button that arms the real aircraft.
- Physical arming remains transmitter-controlled.
- Mission upload and autonomous waypoint execution are not enabled on the real aircraft.
- RTH visualization does not activate physical GPS Rescue.
- Geofences are planning and validation tools until firmware enforcement exists.
- Sonar visualization is not presented as working obstacle avoidance.
- Gimbal controls remain simulated or gated until physical outputs are audited.
- Simulation values never modify the aircraft automatically.
- Physical receiver mirroring is read-only and subject to bench-safety interlocks.

Features are labeled by actual state: `AVAILABLE`, `EXPERIMENTAL`, `SIMULATION_ONLY`, `FIRMWARE_REQUIRED`, `HARDWARE_REQUIRED`, or `UNAVAILABLE`.

## Development Roadmap

| Phase | Status | Scope |
|---|---|---|
| **A** | Complete | QuayeWorks product shell, branding, native navigation, capability manifest, Hawk-H7 F850 profile |
| **B** | Complete | Dashboard, live telemetry, tuning, modes and safety, hardware and calibration, snapshots, local logging |
| **C** | Complete | Offline maps, live GPS, home/RTH visualization, geofences, mission planning and validation |
| **D** | In development | LiteSim, F850 physics, virtual missions, RTH, geofences, sonar, gimbal, telemetry plots and safety interlocks |
| **E** | Planned | Actual firmware SITL and Gazebo integration |
| **F** | Planned | Physical rangefinder protocol and obstacle-avoidance firmware |
| **G** | Planned | Physical three-axis gimbal control and camera tracking |
| **H** | Planned | Hardware-in-loop autonomous-navigation validation before any controlled flight testing |

## Relationship to Betaflight

QuayeWorks AERO preserves compatibility with the Betaflight/MSP ecosystem and uses portions of the Betaflight App codebase where required for communication and configuration. The normal product identity, operator workflow, aircraft model, offline navigation, safety architecture, and simulation platform are QuayeWorks AERO.

Required upstream attribution, copyright notices, and open-source licenses must remain with the relevant source distributions. Technical protocol names are retained where changing them would break compatibility or misrepresent the underlying firmware behavior.

## Responsible Use

QuayeWorks AERO is an active research-and-development platform. Experimental simulation, navigation, sonar, gimbal, and autonomous-flight functions are not a substitute for physical testing, regulatory compliance, proper maintenance, or safe operating procedures.

Users are responsible for operating aircraft lawfully and safely, validating hardware and firmware behavior, removing propellers during bench testing, and keeping people and property clear of experimental systems.
