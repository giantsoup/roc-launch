# Complete Rocket Launcher Control System Solutions

## Software Stack ⭐ **Final Recommendations**

### **Mobile App: React Native with Bluetooth 5.0 LE**
- **Framework**: React Native with TypeScript
- **Bluetooth**: `react-native-ble-plx` (supports Bluetooth 5.0 LE)
- **UI Components**: React Native Elements or NativeBase
- **State Management**: React Context API or Redux Toolkit
- **Range**: 300-500 feet in open field conditions

**Key Libraries:**
```json
{
  "react-native": "^0.72.0",
  "react-native-ble-plx": "^3.1.2",
  "react-native-vector-icons": "^10.0.0",
  "react-native-elements": "^3.4.3",
  "@react-native-async-storage/async-storage": "^1.19.0"
}
```


### **Raspberry Pi: Node.js Backend**
- **Runtime**: Node.js 18+ 
- **Bluetooth**: `@abandonware/noble` for Bluetooth 5.0 LE
- **Motor Control**: `pigpio` library via Node.js bindings
- **Math**: `mathjs` for kinematics calculations
- **Safety**: Hardware interrupt handling

**Key Libraries:**
```json
{
  "@abandonware/noble": "^1.9.2-15",
  "pigpio": "^3.3.1",
  "mathjs": "^11.11.0",
  "bleno": "^0.5.0"
}
```


---

## Hardware Solutions

### **Option 1: Linear Actuators** ⭐ *Easiest Integration*

**Hardware List:**
- 3x Electric linear actuators with position feedback (12V, 6-12" stroke)
- 1x Raspberry Pi 4 (built-in Bluetooth 5.0)
- 3x Relay modules or H-bridge drivers (for actuator control)
- 1x 12V power supply (5-10A depending on actuators)
- 3x ADC channels for position feedback (MCP3008)
- Limit switches for safety

**Pros:**
- Direct replacement of screw mechanisms
- Built-in position feedback
- Self-locking when powered off
- Simple mechanical integration
- No complex kinematics needed

**Cons:**
- Higher cost ($50-150 per actuator)
- Slower movement speed
- Limited stroke options

### **Option 2: Stepper Motors** ⭐ *Best Precision*

**Hardware List:**
- 3x NEMA 17 stepper motors
- 3x A4988 or DRV8825 stepper drivers
- 3x Lead screws with nuts (matching current thread pitch)
- 3x Motor mounting brackets
- 1x Raspberry Pi 4
- 1x 12V power supply (3-5A)
- Microstepping capability for smooth movement

**Pros:**
- Extremely precise positioning
- No position feedback required
- Excellent repeatability
- Moderate cost (~$30 per motor + driver)

**Cons:**
- Requires mechanical coupling design
- More complex control software
- Can lose steps if overloaded

### **Option 3: Servo Motors**

**Hardware List:**
- 3x High-torque servo motors (continuous rotation)
- 1x PCA9685 servo driver board
- 3x Rotary encoders for position feedback
- 3x Gear reduction mechanisms
- 1x Raspberry Pi 4
- 1x 6V power supply

**Pros:**
- Fast movement
- Good torque-to-weight ratio
- Moderate cost

**Cons:**
- Requires external position feedback
- Gear reduction complexity
- Less precise than steppers

---

## Bluetooth Hardware Requirements

### **Raspberry Pi Side:**
- **Pi 4**: Built-in Bluetooth 5.0 LE ✅
- **Pi 3 or older**: Add USB Bluetooth 5.0 adapter
  - **Recommended**: ASUS USB-BT500 or TP-Link UB500 (~$20)
  - **Class 1** for maximum range

### **iPhone Requirements:**
- **iPhone 8 or newer**: Built-in Bluetooth 5.0 LE ✅
- **Range optimization**: Keep line of sight when possible

---

## Control System Architecture

### **Communication Protocol:**
```javascript
// Command structure
{
  "command": "setAngle",
  "elevation": 45.0,
  "azimuth": 180.0,
  "speed": "medium",
  "timestamp": 1234567890
}

// Status response
{
  "status": "ready|moving|error",
  "currentAngle": {"elevation": 44.8, "azimuth": 179.2},
  "motorPositions": [150, 200, 175],
  "sensorReading": {"elevation": 44.9, "azimuth": 179.1},
  "batteryLevel": 85
}
```


### **Safety Features:**
- Emergency stop button in app
- Movement limits in software
- Hardware limit switches
- Connection timeout protection
- Manual override capability

---

## **Final Recommendation: Linear Actuators + React Native**

**Why This Combination:**
1. **Fastest Development**: Linear actuators directly replace manual screws
2. **Best User Experience**: React Native provides native iOS feel
3. **Optimal Range**: Bluetooth 5.0 LE easily covers 500 feet in open field
4. **Reliability**: Fewer moving parts, self-locking actuators
5. **Future-Proof**: Easy to add features like automated launch sequences

**Total Estimated Cost:**
- Linear actuators (3x): $150-450
- Raspberry Pi 4 kit: $80-120
- Power supply & electronics: $50-100
- **Total Hardware**: $280-670

**Development Time**: 2-4 weeks for basic functionality

This setup gives you the most reliable, user-friendly system with the least mechanical complexity!
