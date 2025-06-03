# Project Summary for LLM
**Automated Rocket Launch Stand Control System**
I'm building an automated control system for a hobby rocket launch stand that currently uses manual 3-point adjustment. The system needs to:
**Hardware Setup:**
- Rocket launch stand with 3 adjustable legs (currently manual screw mechanisms)
- Raspberry Pi as main controller
- 3 motors to replace manual adjustment screws
- Existing angle sensor that provides current rocket orientation
- iPhone app for remote control via Bluetooth

**Goal:** Replace manual angle adjustment with automated motor control. User inputs desired launch angle on iPhone app, system automatically adjusts the 3 legs to achieve that angle.
**Key Requirements:**
1. iPhone app connects to Raspberry Pi via Bluetooth
2. Pi controls 3 motors to adjust leg positions
3. Real-time feedback from angle sensor
4. Calculate 3-point kinematics (target angle → leg positions)
5. Safety limits and emergency stop functionality

**Technical Context:**
- Remote operation (Bluetooth range ~30 feet from launch site)
- Precision needed for rocket launch angles
- Outdoor environment, portable setup
- Side-loaded iPhone app (no App Store requirements)

**Looking for advice on:** [motor selection, kinematics calculations, Bluetooth communication, iOS app development, Pi software architecture, safety systems, etc.]

-----------

## Rocket Launcher Control System Solutions

## Software Architecture (iPhone ↔ Raspberry Pi)

### 1. **React Native with Bluetooth** ⭐ *Recommended*
- **App**: React Native with `react-native-bluetooth-serial` or `react-native-ble-plx`
- **Pi**: Node.js server with `noble`/`bleno` for Bluetooth
- **Pros**: JavaScript on both sides, cross-platform, good ecosystem
- **Cons**: iOS Bluetooth libraries can be temperamental

### 2. **Native Swift iOS App**
- **App**: Swift with Core Bluetooth framework
- **Pi**: Python with `bluepy` or `pybluez`
- **Pros**: Best iOS performance, full Bluetooth API access
- **Cons**: iOS-only, requires Swift knowledge

### 3. **Flutter Alternative**
- **App**: Flutter with `flutter_bluetooth_serial`
- **Pi**: Python or Node.js Bluetooth server
- **Pros**: Single codebase, excellent UI framework
- **Cons**: Dart learning curve

---

## Hardware Solutions (Raspberry Pi Motor Control)

### 1. **Stepper Motors + Driver Boards** ⭐ *Recommended for Precision*
**Hardware:**
- NEMA 17 stepper motors (3x)
- A4988 or DRV8825 stepper drivers (3x)
- 12V power supply
- Lead screws or timing belts for linear actuation

**Pros:**
- Precise positioning (no encoder needed)
- High holding torque
- Excellent repeatability
- Easy to control step-by-step

**Cons:**
- Can be slower than servo motors
- More complex wiring
- Higher power consumption

**Software:** Python with `RPi.GPIO` or `gpiozero`

### 2. **Servo Motors with Feedback**
**Hardware:**
- High-torque servo motors (3x)
- Servo driver board (PCA9685)
- Potentiometers for position feedback
- Linear actuators or gear reduction

**Pros:**
- Fast movement
- Built-in position control
- Simpler wiring
- Good torque-to-weight ratio

**Cons:**
- Limited rotation range (unless continuous)
- May need external position feedback
- Less precise than steppers

**Software:** Python with `adafruit-circuitpython-pca9685`

### 3. **DC Motors with Encoders**
**Hardware:**
- DC gear motors with encoders (3x)
- H-bridge motor drivers (L298N or similar)
- Rotary encoders for position feedback
- Limit switches for safety

**Pros:**
- Fast and powerful
- Variable speed control
- Cost-effective
- Good for continuous operation

**Cons:**
- Requires PID control implementation
- Need encoders and limit switches
- More complex control software

**Software:** Python with PID library

### 4. **Linear Actuators** ⭐ *Simplest Mechanical Integration*
**Hardware:**
- Electric linear actuators with feedback (3x)
- Relay modules or motor drivers
- Built-in potentiometer feedback

**Pros:**
- Direct linear motion (matches screw adjustment)
- Built-in position feedback
- Easy mechanical integration
- Self-locking when powered off

**Cons:**
- Can be expensive
- Slower than rotary motors
- Limited stroke length options

**Software:** Simple GPIO control with ADC reading

---

## Kinematics & Control Software

### Core Components Needed:
1. **3-Point Kinematics Calculator**
   - Convert target angle (elevation/azimuth) to 3 leg positions
   - Inverse kinematics for current angle from leg positions

2. **PID Control Loop** (for DC motors)
   - Position control for each motor
   - Coordinated movement to prevent binding

3. **Safety Systems**
   - Limit switches or software limits
   - Emergency stop functionality
   - Motor timeout protection

### Recommended Pi Software Stack:
```textmate
# Example libraries
import RPi.GPIO as GPIO
import time
import bluetooth
import json
import numpy as np  # for kinematics calculations

# Motor control
from adafruit_motor import stepper  # for steppers
# OR
import pigpio  # for servo control
```


---

## Communication Protocol Example:
```json
// iPhone → Pi
{
  "command": "setAngle",
  "elevation": 45,
  "azimuth": 180,
  "speed": "medium"
}

// Pi → iPhone
{
  "status": "moving",
  "currentAngle": {"elevation": 42.3, "azimuth": 178.1},
  "legPositions": [150, 200, 175],  // mm or steps
  "sensorAngle": {"elevation": 42.1, "azimuth": 178.3}
}
```


---

## Getting Started Recommendation:

**Phase 1**: Start with **linear actuators** for easiest mechanical integration
**Phase 2**: Develop the kinematics calculations and basic Pi control
**Phase 3**: Add React Native app with Bluetooth communication
**Phase 4**: Optimize with stepper motors if more precision is needed

The linear actuator approach will get you up and running fastest since it directly replaces the manual screw adjustment mechanism!
