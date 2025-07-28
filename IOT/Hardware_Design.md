
## **2. Hardware Design: IoT Device**

### **2.1 Hardware Architecture Overview**

```
┌─────────────────────────────────────────────────────────────────┐
│                    IoT Device Hardware Architecture             │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│  │   Power         │    │   Processing     │    │   Communication │
│  │   Management    │    │   Unit           │    │   Modules       │
│  │                 │    │                 │    │                 │
│  │ • Battery       │    │ • ESP32 MCU     │    │ • Wi-Fi Module  │
│  │ • Charging      │    │ • Flash Memory  │    │ • BLE Module    │
│  │ • Power Reg     │    │ • RAM           │    │ • Antenna       │
│  │ • Voltage Mon   │    │ • RTC           │    │ • Connectors    │
│  └─────────────────┘    └─────────────────┘    └─────────────────┘
│           │                       │                       │
│           └───────────────────────┼───────────────────────┘
│                                   │
│  ┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│  │   Sensors       │    │   User          │    │   Protection    │
│  │   & Inputs      │    │   Interface     │    │   & Safety      │
│  │                 │    │                 │    │                 │
│  │ • GPS Module    │    │ • SOS Button    │    │ • Enclosure     │
│  │ • Microphone    │    │ • LED Indicators│    │ • Waterproof    │
│  │ • Accelerometer │    │ • Vibration     │    │ • Shock Absorb  │
│  │ • Temperature   │    │ • Speaker       │    │ • EMI Shield    │
│  └─────────────────┘    └─────────────────┘    └─────────────────┘
```

### **2.2 Detailed Component Specifications**

#### **2.2.1 Main Processing Unit**

**ESP32-WROOM-32 Module**
```
Specifications:
- Microcontroller: ESP32 (Dual-core, 240MHz)
- Flash Memory: 4MB SPI Flash
- RAM: 520KB SRAM
- Wi-Fi: 802.11 b/g/n (2.4GHz)
- Bluetooth: BLE 4.2 + BR/EDR
- Operating Voltage: 3.3V
- Operating Temperature: -40°C to +85°C
- Dimensions: 18mm × 25.5mm × 3.1mm

Features:
- Ultra-low power consumption
- Advanced security features (AES, SHA, RSA)
- Rich peripheral interfaces
- Built-in antenna
- Industrial-grade reliability
- Deep sleep modes for power saving

Connections:
- GPIO: 34 pins available
- UART: 3 channels (GPS, Debug, External)
- SPI: 4 channels (Flash, SD Card, Display)
- I2C: 2 channels (Sensors, RTC)
- PWM: 16 channels (LEDs, Motor)
- ADC: 18 channels (12-bit, Battery, Sensors)
```

#### **2.2.2 GPS Module**

**NEO-6M GPS Receiver**
```
Specifications:
- Chipset: u-blox NEO-6M
- Frequency: L1, 1575.42MHz
- Channels: 50 channels
- Sensitivity: -161dBm tracking, -147dBm acquisition
- Time to First Fix: 27 seconds (cold), 1 second (hot)
- Accuracy: 2.5m CEP (Circular Error Probable)
- Update Rate: 1Hz (configurable)
- Operating Voltage: 3.3V
- Current Consumption: 45mA (continuous), 35mA (tracking)
- Operating Temperature: -40°C to +85°C
- Dimensions: 25mm × 35mm × 4mm

Features:
- High sensitivity for indoor/outdoor use
- Low power consumption modes
- Built-in ceramic patch antenna
- NMEA 0183 protocol support
- PPS (Pulse Per Second) output
- Backup battery for hot start
- SBAS (WAAS, EGNOS, MSAS) support

Connections:
- VCC: 3.3V power supply
- GND: Ground
- TX: UART transmit (to ESP32 RX)
- RX: UART receive (from ESP32 TX)
- PPS: Pulse per second output
- V_BCKP: Backup battery (optional)
```

#### **2.2.3 Power Management System**

**Battery Pack**
```
Specifications:
- Battery Type: Li-ion 18650
- Capacity: 3000mAh (3.7V)
- Nominal Voltage: 3.7V
- Charging Voltage: 4.2V
- Discharge Cut-off: 3.0V
- Cycle Life: 500+ cycles
- Operating Temperature: -20°C to +60°C
- Dimensions: 18mm × 65mm
- Weight: 45g

Protection Features:
- Overcharge protection (>4.25V)
- Over-discharge protection (<2.5V)
- Overcurrent protection (>3A)
- Short circuit protection
- Temperature protection (>60°C)
- Cell balancing (for multi-cell packs)
```

**Charging Circuit**
```
Components:
- TP4056 Li-ion charger IC
- USB-C connector (reversible)
- LED charging indicator (red/green)
- Current limiting resistor (1.2A)
- Protection diodes (reverse polarity)
- Thermal protection

Specifications:
- Input Voltage: 5V USB (4.5V-5.5V)
- Charging Current: 1A (programmable)
- Charging Efficiency: >90%
- Protection: Overcharge, overcurrent, thermal
- Status Indicators: Charging (red), Full (green), Error (blinking)
- Automatic charge termination
```

**Power Management IC**
```
Component: AP2112K-3.3V
Specifications:
- Input Voltage: 2.5V to 6V
- Output Voltage: 3.3V (fixed)
- Output Current: 600mA
- Efficiency: >90% at full load
- Dropout Voltage: 300mV at 600mA
- Quiescent Current: 50μA
- Protection: Overcurrent, thermal shutdown
- Package: SOT-23-5
- Operating Temperature: -40°C to +85°C
```

#### **2.2.4 User Interface Components**

**SOS Button**
```
Specifications:
- Type: Tactile push button (momentary)
- Size: 12mm diameter
- Operating Force: 160g
- Travel: 0.5mm
- Life Cycles: 1,000,000
- Operating Temperature: -40°C to +85°C
- Protection: IP67 (waterproof and dustproof)
- Material: Stainless steel with rubber seal

Features:
- Large, easy-to-press design
- Waterproof and dustproof
- High reliability and durability
- Visual feedback (LED ring)
- Haptic feedback (vibration)
- Emergency override functionality
- Anti-accidental press design
```

**LED Indicators**
```
Components:
- Power LED: Green (3.3V, 20mA)
- Status LED: Blue (3.3V, 20mA)
- Alert LED: Red (3.3V, 20mA)
- Charging LED: Yellow (3.3V, 20mA)

Specifications:
- Type: SMD LED (0603 package)
- Forward Voltage: 3.3V
- Forward Current: 20mA
- Brightness: 1000mcd
- Viewing Angle: 120°
- Life: 50,000 hours
- Color Accuracy: High CRI
- Power Consumption: 66mW each

Features:
- Low power consumption
- High brightness for outdoor visibility
- Multiple blink patterns for different states
- PWM dimming capability
- Automatic brightness adjustment
```

**Vibration Motor**
```
Specifications:
- Type: Eccentric rotating mass (ERM)
- Operating Voltage: 3.3V
- Current Consumption: 80mA
- Vibration Frequency: 130Hz
- Vibration Amplitude: 1.2G
- Dimensions: 10mm × 3mm
- Weight: 1.2g
- Life: 50,000 hours

Features:
- Low power consumption
- High reliability
- Customizable vibration patterns
- Waterproof design
- Silent operation
- Multiple intensity levels
- Pattern recognition for different alerts
```

#### **2.2.5 Audio Components**

**Microphone Module**
```
Component: MAX9814 Electret Microphone Amplifier
Specifications:
- Microphone: Electret condenser
- Sensitivity: -38dB (1V/Pa at 1kHz)
- Frequency Response: 20Hz - 20kHz
- Signal-to-Noise Ratio: 58dB
- Operating Voltage: 3.3V
- Current Consumption: 2.5mA
- Gain: 20dB, 40dB, 60dB (selectable)
- Package: SOIC-8

Features:
- Low noise amplifier
- Automatic gain control (AGC)
- Shutdown mode for power saving
- Small form factor
- High sensitivity for voice detection
- Noise cancellation capability
- Multiple gain settings
```

**Speaker (Optional)**
```
Specifications:
- Type: Piezoelectric buzzer
- Operating Voltage: 3.3V
- Frequency: 2.7kHz
- Sound Pressure: 85dB at 10cm
- Current Consumption: 30mA
- Dimensions: 12mm diameter
- Weight: 2g

Features:
- Low power consumption
- High reliability
- Waterproof design
- Multiple tone options
- Volume control
- Emergency sound patterns
```

#### **2.2.6 Environmental Sensors**

**Accelerometer**
```
Component: MPU6050 6-axis Motion Sensor
Specifications:
- Accelerometer: ±2g, ±4g, ±8g, ±16g (selectable)
- Gyroscope: ±250, ±500, ±1000, ±2000°/s (selectable)
- Resolution: 16-bit
- Operating Voltage: 3.3V
- Current Consumption: 3.9mA (active), 10μA (sleep)
- Operating Temperature: -40°C to +85°C
- Communication: I2C (400kHz)
- Package: QFN-24

Features:
- Motion detection and classification
- Fall detection algorithm
- Activity monitoring
- Low power modes
- Digital motion processor (DMP)
- Interrupt capabilities
- Self-test functionality
```

**Temperature Sensor**
```
Component: DS18B20 Digital Temperature Sensor
Specifications:
- Temperature Range: -55°C to +125°C
- Accuracy: ±0.5°C (-10°C to +85°C)
- Resolution: 9-12 bits (0.5°C, 0.25°C, 0.125°C, 0.0625°C)
- Operating Voltage: 3.3V
- Current Consumption: 1.5mA (active), 1μA (standby)
- Communication: 1-Wire protocol
- Package: TO-92 or SOIC-8
- Waterproof probe available

Features:
- High accuracy and resolution
- Digital output (no ADC required)
- Multiple sensors on one bus
- Low power consumption
- Unique 64-bit serial number
- Alarm functionality
- Waterproof version for outdoor use
```

### **2.3 PCB Design Specifications**

#### **2.3.1 PCB Layout**
```
Layer Stack:
- Top Layer: Components and signal traces
- Ground Plane: Internal ground layer
- Power Plane: Internal power distribution
- Bottom Layer: Ground plane and thermal relief

Specifications:
- Board Size: 50mm × 70mm
- Thickness: 1.6mm
- Material: FR-4 (TG150)
- Copper Weight: 1oz (35μm)
- Surface Finish: ENIG (Electroless Nickel Immersion Gold)
- Solder Mask: Green (UL94V-0)
- Silkscreen: White (UL94V-0)
- Via Type: Through-hole and blind vias
- Minimum Trace Width: 0.15mm
- Minimum Via Size: 0.3mm
```

#### **2.3.2 Component Placement**
```
Top Layer Layout:
┌─────────────────────────────────────┐
│  [GPS Module]    [ESP32 Module]     │
│                                     │
│  [SOS Button]    [LED Indicators]   │
│                                     │
│  [Microphone]    [Vibration Motor]  │
│                                     │
│  [USB-C Connector] [Battery Conn]   │
│                                     │
│  [Accelerometer] [Temperature]      │
└─────────────────────────────────────┘

Design Considerations:
- GPS antenna positioned for optimal signal reception
- SOS button easily accessible
- LED indicators visible through enclosure
- USB connector accessible for charging
- Sensors positioned for accurate readings
- Thermal management for power components
```

#### **2.3.3 Power Distribution**
```
Power Rails:
- 3.7V: Battery voltage (unregulated)
- 3.3V: Main system voltage (regulated)
- 1.8V: GPS module (if required)

Power Distribution:
- Battery → Protection Circuit → Charging Circuit
- Battery → Power Management → 3.3V Rail
- 3.3V → ESP32, GPS, Sensors, LEDs
- USB → Charging Circuit → Battery
- 3.3V → Voltage Divider → Battery Monitoring

Protection Features:
- Reverse polarity protection
- Overvoltage protection
- Undervoltage lockout
- Current limiting
- Thermal protection
```

### **2.4 Enclosure Design**

#### **2.4.1 Physical Design**
```
Enclosure Specifications:
- Material: ABS Plastic (UL94V-0)
- Color: Black with colored accents
- Dimensions: 55mm × 75mm × 25mm
- Weight: ~50g (including battery)
- Protection: IP67 (waterproof and dustproof)
- Impact Resistance: 1.5m drop test
- Operating Temperature: -20°C to +60°C
- UV Resistance: Yes (outdoor use)

Design Features:
- Ergonomic shape for easy carrying
- Large SOS button for emergency access
- LED indicators visible through transparent window
- USB-C charging port with waterproof cover
- Belt clip or lanyard attachment
- Child-safe design (no sharp edges)
- Anti-slip surface for secure handling
- Ventilation for thermal management
```

#### **2.4.2 Mounting Options**
```
Mounting Methods:
- Belt clip: Spring-loaded metal clip (stainless steel)
- Lanyard: 1.5m adjustable strap (nylon)
- Velcro strap: Adjustable for different sizes
- Magnetic mount: For temporary attachment
- Pocket clip: For clothing attachment
- Carabiner: For backpack attachment

Design Considerations:
- Secure attachment to prevent loss
- Easy removal for charging
- Comfortable for child to wear
- Durable materials for long-term use
- Multiple attachment options for flexibility
```

### **2.5 Firmware Requirements**

#### **2.5.1 Core Functions**
```
Firmware Modules:
- GPS Data Processing and filtering
- MQTT Communication with backend
- BLE Advertising for proximity detection
- Power Management and sleep modes
- Sensor Reading and data processing
- Alert Generation and handling
- Data Logging and storage
- OTA (Over-The-Air) Updates
- Security and encryption
- Error handling and recovery
```

#### **2.5.2 Power Management**
```
Power Modes:
- Active Mode: Full functionality (100mA)
- Sleep Mode: GPS off, sensors on (20mA)
- Deep Sleep: Only RTC active (10μA)
- Hibernation: Only SOS button active (1μA)

Battery Life:
- Active Mode: 30 hours
- Sleep Mode: 150 hours
- Deep Sleep: 30 days
- Hibernation: 6 months

Power Optimization:
- Dynamic frequency scaling
- Selective sensor activation
- Adaptive GPS update intervals
- Smart sleep scheduling
- Battery level monitoring
```

### **2.6 Testing Specifications**

#### **2.6.1 Functional Testing**
```
Test Categories:
- GPS accuracy and reliability testing
- MQTT communication testing
- BLE functionality and range testing
- Power consumption and battery life testing
- Sensor accuracy and calibration testing
- Button response and reliability testing
- LED indicators and patterns testing
- Vibration feedback testing
- Waterproof testing (IP67)
- Drop testing (1.5m onto concrete)
- Temperature cycling testing
- EMI/EMC testing
```

#### **2.6.2 Environmental Testing**
```
Test Conditions:
- Temperature: -20°C to +60°C
- Humidity: 10% to 90% RH
- Waterproof: IP67 rating (1m for 30 minutes)
- Drop Test: 1.5m onto concrete (6 sides)
- Vibration: 10-55Hz, 1g amplitude
- EMC: FCC Part 15 compliance
- Salt Spray: 24 hours (for corrosion)
- UV Exposure: 1000 hours (for outdoor use)
```

### **2.7 Bill of Materials (BOM)**

#### **2.7.1 Complete BOM List**
```
Component List:
1. ESP32-WROOM-32 Module
2. NEO-6M GPS Module with antenna
3. Li-ion 18650 Battery (3000mAh)
4. TP4056 Charging Module
5. AP2112K-3.3V Voltage Regulator
6. SOS Push Button (12mm, IP67)
7. LED Indicators (4x SMD 0603)
8. Vibration Motor (10mm ERM)
9. MAX9814 Microphone Module
10. MPU6050 Accelerometer
11. DS18B20 Temperature Sensor
12. USB-C Connector (reversible)
13. Battery Protection Circuit
14. PCB (4-layer, 50x70mm)
15. Enclosure (ABS Plastic, IP67)
16. Mounting Hardware (belt clip, lanyard)
17. Cables and Connectors
18. Passive Components (Resistors, Capacitors, Inductors)
19. Crystal Oscillator (32.768kHz)
20. Reset Button
```

#### **2.7.2 Cost Estimation**
```
Component Costs (per unit):
- ESP32 Module: $8.00
- GPS Module: $12.00
- Battery: $6.00
- Charging Circuit: $3.00
- Sensors: $8.00
- Enclosure: $5.00
- PCB: $3.00
- Other Components: $7.00
- Total Components: $52.00

Manufacturing Costs:
- PCB Assembly: $15.00
- Enclosure Assembly: $8.00
- Testing and QC: $5.00
- Total Manufacturing: $28.00

Total Cost per Unit: $80.00

Volume Discounts:
- 100 units: $70 per unit
- 500 units: $60 per unit
- 1000 units: $50 per unit
```

### **2.8 Compliance & Certifications**

#### **2.8.1 Safety Certifications**
```
Required Certifications:
- CE Marking (European Union)
- FCC Part 15 (United States)
- RoHS Compliance (Restriction of Hazardous Substances)
- REACH Compliance (Registration, Evaluation, Authorization)
- UL Safety Certification (Underwriters Laboratories)
- IP67 Rating (Ingress Protection)
```

#### **2.8.2 Wireless Certifications**
```
Wireless Standards:
- Wi-Fi: IEEE 802.11 b/g/n
- Bluetooth: Bluetooth 4.2 BLE
- GPS: GPS L1 C/A code
- FCC ID: Required for US market
- CE RED: Required for EU market
```

---

## **3. Integration & Testing**

### **3.1 System Integration**

#### **3.1.1 Hardware-Software Integration**
```
Integration Points:
- GPS data processing and filtering
- Sensor data fusion and analysis
- Power management integration
- Communication protocol implementation
- Security and encryption
- Error handling and recovery
- OTA update mechanism
```

#### **3.1.2 Mobile App Integration**
```
Integration Features:
- Real-time location tracking
- Alert notifications
- Device pairing and management
- Geofence configuration
- User interface synchronization
- Data synchronization
- Push notifications
```

### **3.2 Quality Assurance**

#### **3.2.1 Testing Strategy**
```
Testing Phases:
- Unit Testing: Individual component testing
- Integration Testing: Component interaction testing
- System Testing: End-to-end functionality testing
- User Acceptance Testing: User experience testing
- Performance Testing: Load and stress testing
- Security Testing: Vulnerability assessment
```

#### **3.2.2 Test Cases**
```
Test Scenarios:
- Normal operation scenarios
- Emergency scenarios (SOS button)
- Power failure scenarios
- Communication failure scenarios
- Environmental stress scenarios
- User error scenarios
- Security breach scenarios
```

---

**Document Version**: 1.0  
**Last Updated**: December 2024  
**Project**: ChildGuard - Real-Time Child Monitoring System  
**Status**: Detailed Design Complete

---

## **4. Hardware Design: IoT Device Component List**

### **4.1 Hardware Architecture Overview**

```
┌─────────────────────────────────────────────────────────────────┐
│                    IoT Device Hardware Architecture             │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│  │   Power         │    │   Processing     │    │   Communication │
│  │   Management    │    │   Unit           │    │   Modules       │
│  │                 │    │                 │    │                 │
│  │ • Battery       │    │ • ESP32 MCU     │    │ • Wi-Fi Module  │
│  │ • Charging      │    │ • Flash Memory  │    │ • BLE Module    │
│  │ • Power Reg     │    │ • RAM           │    │ • Antenna       │
│  │ • Voltage Mon   │    │ • RTC           │    │ • Connectors    │
│  └─────────────────┘    └─────────────────┘    └─────────────────┘
│           │                       │                       │
│           └───────────────────────┼───────────────────────┘
│                                   │
│  ┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│  │   Sensors       │    │   User          │    │   Protection    │
│  │   & Inputs      │    │   Interface     │    │   & Safety      │
│  │                 │    │                 │    │                 │
│  │ • GPS Module    │    │ • SOS Button    │    │ • Enclosure     │
│  │ • Microphone    │    │ • LED Indicators│    │ • Waterproof    │
│  │ • Accelerometer │    │ • Vibration     │    │ • Shock Absorb  │
│  │ • Temperature   │    │ • Speaker       │    │ • EMI Shield    │
│  └─────────────────┘    └─────────────────┘    └─────────────────┘
```

### **4.2 Detailed Component Specifications**

#### **4.2.1 Main Processing Unit**

**ESP32-WROOM-32 Module**
```
Specifications:
- Microcontroller: ESP32 (Dual-core, 240MHz)
- Flash Memory: 4MB SPI Flash
- RAM: 520KB SRAM
- Wi-Fi: 802.11 b/g/n (2.4GHz)
- Bluetooth: BLE 4.2 + BR/EDR
- Operating Voltage: 3.3V
- Operating Temperature: -40°C to +85°C
- Dimensions: 18mm × 25.5mm × 3.1mm

Features:
- Ultra-low power consumption
- Advanced security features
- Rich peripheral interfaces
- Built-in antenna
- Industrial-grade reliability

Connections:
- GPIO: 34 pins available
- UART: 3 channels
- SPI: 4 channels
- I2C: 2 channels
- PWM: 16 channels
- ADC: 18 channels (12-bit)
```

#### **4.2.2 GPS Module**

**NEO-6M GPS Receiver**
```
Specifications:
- Chipset: u-blox NEO-6M
- Frequency: L1, 1575.42MHz
- Channels: 50 channels
- Sensitivity: -161dBm tracking
- Time to First Fix: 27 seconds (cold), 1 second (hot)
- Accuracy: 2.5m CEP (Circular Error Probable)
- Update Rate: 1Hz
- Operating Voltage: 3.3V
- Current Consumption: 45mA (continuous)
- Operating Temperature: -40°C to +85°C
- Dimensions: 25mm × 35mm × 4mm

Features:
- High sensitivity
- Low power consumption
- Built-in antenna
- NMEA 0183 protocol
- PPS (Pulse Per Second) output
- Backup battery for hot start

Connections:
- VCC: 3.3V power supply
- GND: Ground
- TX: UART transmit (to ESP32 RX)
- RX: UART receive (from ESP32 TX)
- PPS: Pulse per second output
```

#### **4.2.3 Power Management**

**Battery Pack**
```
Specifications:
- Battery Type: Li-ion 18650
- Capacity: 3000mAh (3.7V)
- Nominal Voltage: 3.7V
- Charging Voltage: 4.2V
- Discharge Cut-off: 3.0V
- Cycle Life: 500+ cycles
- Operating Temperature: -20°C to +60°C
- Dimensions: 18mm × 65mm

Protection Features:
- Overcharge protection
- Over-discharge protection
- Overcurrent protection
- Short circuit protection
- Temperature protection
```

**Charging Circuit**
```
Components:
- TP4056 Li-ion charger IC
- USB-C connector
- LED charging indicator
- Current limiting resistor
- Protection diodes

Specifications:
- Input Voltage: 5V USB
- Charging Current: 1A (programmable)
- Charging Efficiency: >90%
- Protection: Overcharge, overcurrent
- Status Indicators: Charging, full, error
```

**Power Management IC**
```
Component: AP2112K-3.3V
Specifications:
- Input Voltage: 2.5V to 6V
- Output Voltage: 3.3V (fixed)
- Output Current: 600mA
- Efficiency: >90%
- Dropout Voltage: 300mV
- Quiescent Current: 50μA
- Protection: Overcurrent, thermal shutdown
```

#### **4.2.4 User Interface Components**

**SOS Button**
```
Specifications:
- Type: Tactile push button
- Size: 12mm diameter
- Operating Force: 160g
- Travel: 0.5mm
- Life Cycles: 1,000,000
- Operating Temperature: -40°C to +85°C
- Protection: IP67 (waterproof)

Features:
- Large, easy-to-press design
- Waterproof and dustproof
- High reliability
- Visual feedback (LED)
- Haptic feedback (vibration)
```

**LED Indicators**
```
Components:
- Power LED: Green (3.3V, 20mA)
- Status LED: Blue (3.3V, 20mA)
- Alert LED: Red (3.3V, 20mA)
- Charging LED: Yellow (3.3V, 20mA)

Specifications:
- Type: SMD LED (0603)
- Forward Voltage: 3.3V
- Forward Current: 20mA
- Brightness: 1000mcd
- Viewing Angle: 120°
- Life: 50,000 hours
```

**Vibration Motor**
```
Specifications:
- Type: Eccentric rotating mass (ERM)
- Operating Voltage: 3.3V
- Current Consumption: 80mA
- Vibration Frequency: 130Hz
- Dimensions: 10mm × 3mm
- Weight: 1.2g

Features:
- Low power consumption
- High reliability
- Customizable vibration patterns
- Waterproof design
```

#### **4.2.5 Audio Components**

**Microphone Module**
```
Component: MAX9814 Electret Microphone Amplifier
Specifications:
- Microphone: Electret condenser
- Sensitivity: -38dB
- Frequency Response: 20Hz - 20kHz
- Signal-to-Noise Ratio: 58dB
- Operating Voltage: 3.3V
- Current Consumption: 2.5mA
- Gain: 20dB, 40dB, 60dB (selectable)

Features:
- Low noise amplifier
- Automatic gain control
- Shutdown mode for power saving
- Small form factor
- High sensitivity
```

**Speaker (Optional)**
```
Specifications:
- Type: Piezoelectric buzzer
- Operating Voltage: 3.3V
- Frequency: 2.7kHz
- Sound Pressure: 85dB at 10cm
- Current Consumption: 30mA
- Dimensions: 12mm diameter

Features:
- Low power consumption
- High reliability
- Waterproof design
- Multiple tone options
```

#### **4.2.6 Environmental Sensors**

**Accelerometer**
```
Component: MPU6050 6-axis Motion Sensor
Specifications:
- Accelerometer: ±2g, ±4g, ±8g, ±16g
- Gyroscope: ±250, ±500, ±1000, ±2000°/s
- Resolution: 16-bit
- Operating Voltage: 3.3V
- Current Consumption: 3.9mA
- Communication: I2C
- Operating Temperature: -40°C to +85°C

Features:
- Motion detection
- Fall detection
- Activity monitoring
- Low power mode
- Digital motion processor
```

**Temperature Sensor**
```
Component: DS18B20 Digital Temperature Sensor
Specifications:
- Temperature Range: -55°C to +125°C
- Accuracy: ±0.5°C (-10°C to +85°C)
- Resolution: 9-12 bits (0.5°C, 0.25°C, 0.125°C, 0.0625°C)
- Operating Voltage: 3.3V
- Current Consumption: 1.5mA
- Communication: 1-Wire
- Waterproof probe available

Features:
- High accuracy
- Digital output
- Multiple sensors on one bus
- Low power consumption
```

### **4.3 PCB Design Specifications**

#### **4.3.1 PCB Layout**
```
Layer Stack:
- Top Layer: Components and signal traces
- Ground Plane: Internal ground layer
- Power Plane: Internal power distribution
- Bottom Layer: Ground plane and thermal relief

Specifications:
- Board Size: 50mm × 70mm
- Thickness: 1.6mm
- Material: FR-4
- Copper Weight: 1oz
- Surface Finish: ENIG (Electroless Nickel Immersion Gold)
- Solder Mask: Green
- Silkscreen: White
```

#### **4.3.2 Component Placement**
```
Top Layer Layout:
┌─────────────────────────────────────┐
│  [GPS Module]    [ESP32 Module]     │
│                                     │
│  [SOS Button]    [LED Indicators]   │
│                                     │
│  [Microphone]    [Vibration Motor]  │
│                                     │
│  [USB-C Connector] [Battery Conn]   │
│                                     │
│  [Accelerometer] [Temperature]      │
└─────────────────────────────────────┘
```

#### **4.3.3 Power Distribution**
```
Power Rails:
- 3.7V: Battery voltage
- 3.3V: Main system voltage
- 1.8V: GPS module (if required)

Power Distribution:
- Battery → Protection Circuit → Charging Circuit
- Battery → Power Management → 3.3V Rail
- 3.3V → ESP32, GPS, Sensors, LEDs
- USB → Charging Circuit → Battery
```

### **4.4 Enclosure Design**

#### **4.4.1 Physical Design**
```
Enclosure Specifications:
- Material: ABS Plastic
- Color: Black with colored accents
- Dimensions: 55mm × 75mm × 25mm
- Weight: ~50g (including battery)
- Protection: IP67 (waterproof and dustproof)
- Impact Resistance: 1.5m drop test

Design Features:
- Ergonomic shape for easy carrying
- Large SOS button for emergency access
- LED indicators visible through transparent window
- USB-C charging port with waterproof cover
- Belt clip or lanyard attachment
- Child-safe design (no sharp edges)
```

#### **4.4.2 Mounting Options**
```
Mounting Methods:
- Belt clip: Spring-loaded metal clip
- Lanyard: 1.5m adjustable strap
- Velcro strap: Adjustable for different sizes
- Magnetic mount: For temporary attachment
- Pocket clip: For clothing attachment
```

### **4.5 Firmware Requirements**

#### **4.5.1 Core Functions**
```
Firmware Modules:
- GPS Data Processing
- MQTT Communication
- BLE Advertising
- Power Management
- Sensor Reading
- Alert Generation
- Data Logging
- OTA Updates
```

#### **4.5.2 Power Management**
```
Power Modes:
- Active Mode: Full functionality (100mA)
- Sleep Mode: GPS off, sensors on (20mA)
- Deep Sleep: Only RTC active (10μA)
- Hibernation: Only SOS button active (1μA)

Battery Life:
- Active Mode: 30 hours
- Sleep Mode: 150 hours
- Deep Sleep: 30 days
- Hibernation: 6 months
```

### **4.6 Testing Specifications**

#### **4.6.1 Functional Testing**
```
Test Categories:
- GPS accuracy and reliability
- MQTT communication
- BLE functionality
- Power consumption
- Sensor accuracy
- Button response
- LED indicators
- Vibration feedback
- Waterproof testing
- Drop testing
```

#### **4.6.2 Environmental Testing**
```
Test Conditions:
- Temperature: -20°C to +60°C
- Humidity: 10% to 90% RH
- Waterproof: IP67 rating
- Drop Test: 1.5m onto concrete
- Vibration: 10-55Hz, 1g amplitude
- EMC: FCC Part 15 compliance
```

### **4.7 Bill of Materials (BOM)**

#### **4.7.1 Complete BOM List**
```
Component List:
1. ESP32-WROOM-32 Module
2. NEO-6M GPS Module
3. Li-ion 18650 Battery (3000mAh)
4. TP4056 Charging Module
5. AP2112K-3.3V Voltage Regulator
6. SOS Push Button (12mm)
7. LED Indicators (4x SMD 0603)
8. Vibration Motor (10mm)
9. MAX9814 Microphone Module
10. MPU6050 Accelerometer
11. DS18B20 Temperature Sensor
12. USB-C Connector
13. Battery Protection Circuit
14. PCB (4-layer, 50x70mm)
15. Enclosure (ABS Plastic)
16. Mounting Hardware
17. Cables and Connectors
18. Passive Components (Resistors, Capacitors, Inductors)
```

#### **4.7.2 Cost Estimation**
```
Component Costs (per unit):
- ESP32 Module: $8.00
- GPS Module: $12.00
- Battery: $6.00
- Charging Circuit: $3.00
- Sensors: $8.00
- Enclosure: $5.00
- PCB: $3.00
- Other Components: $7.00
- Total: $52.00

Manufacturing Costs:
- PCB Assembly: $15.00
- Enclosure Assembly: $8.00
- Testing: $5.00
- Total Manufacturing: $28.00

Total Cost per Unit: $80.00
```

---

**Document Version**: 1.0  
**Last Updated**: December 2024  
**Project**: ChildGuard - Real-Time Child Monitoring System  
**Status**: UI/UX & Hardware Design Complete 