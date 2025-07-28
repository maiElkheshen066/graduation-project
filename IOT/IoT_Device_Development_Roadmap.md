# 🔧 IoT Device Development Roadmap for ChildGuard Project

## Overview
This document provides a comprehensive step-by-step development plan for the ChildGuard IoT device, including all phases from hardware design to final deployment.

---

## **Phase 1: Hardware Design & Component Selection (Week 1-2)**

### **Step 1.1: Core Hardware Components**
- [ ] **ESP32 Development Board Selection**:
  - ESP32-WROOM-32 or ESP32-WROVER
  - Dual-core 240MHz processor
  - 520KB SRAM, 4MB Flash
  - Built-in Wi-Fi and Bluetooth

- [ ] **GPS Module Selection**:
  - NEO-6M GPS module
  - UART communication (9600 baud)
  - 5-10 meter accuracy
  - 3.3V operation

- [ ] **Power Management Components**:
  - 3.7V Li-ion battery (2000mAh)
  - TP4056 charging module
  - LM317 voltage regulator (3.3V)
  - Battery level monitoring circuit

### **Step 1.2: Additional Components**
- [ ] **SOS Button**:
  - Tactile push button
  - GPIO input with pull-up resistor
  - Debouncing circuit

- [ ] **Microphone Module**:
  - MAX9814 electret microphone amplifier
  - Analog output to ESP32 ADC
  - Sound level detection capability

- [ ] **LED Indicators**:
  - Power status LED
  - GPS fix LED
  - Wi-Fi connection LED
  - SOS button LED

### **Step 1.3: PCB Design & Enclosure**
- [ ] Create schematic diagram
- [ ] Design PCB layout
- [ ] Select appropriate enclosure
- [ ] Plan component placement
- [ ] Design battery compartment

---

## **Phase 2: Development Environment Setup (Week 3-4)**

### **Step 2.1: Arduino IDE Setup**
- [ ] Install Arduino IDE 2.x
- [ ] Add ESP32 board support
- [ ] Install required libraries:
  ```cpp
  // Essential libraries
  #include <WiFi.h>
  #include <BluetoothSerial.h>
  #include <PubSubClient.h>  // MQTT
  #include <TinyGPS++.h>     // GPS parsing
  #include <ArduinoJson.h>   // JSON handling
  #include <Preferences.h>   // EEPROM storage
  ```

### **Step 2.2: Project Structure**
- [ ] Create organized project structure:
  ```
  childguard_iot/
  ├── childguard_iot.ino
  ├── config.h
  ├── gps_handler.h/cpp
  ├── ble_handler.h/cpp
  ├── mqtt_handler.h/cpp
  ├── sound_handler.h/cpp
  ├── sos_handler.h/cpp
  ├── power_manager.h/cpp
  └── utils.h/cpp
  ```

### **Step 2.3: Configuration Management**
- [ ] Create configuration file for settings:
  ```cpp
  // config.h
  #define DEVICE_ID "CHILD_001"
  #define MQTT_BROKER "your-mqtt-broker.com"
  #define MQTT_PORT 1883
  #define GPS_UPDATE_INTERVAL 10000  // 10 seconds
  #define BLE_ADV_INTERVAL 1000      // 1 second
  #define SOUND_CHECK_INTERVAL 5000  // 5 seconds
  ```

---

## **Phase 3: GPS Module Integration (Week 5-6)**

### **Step 3.1: GPS Hardware Connection**
- [ ] Connect NEO-6M to ESP32:
  - VCC → 3.3V
  - GND → GND
  - TX → GPIO16 (RX2)
  - RX → GPIO17 (TX2)

### **Step 3.2: GPS Software Implementation**
- [ ] Create GPS handler class:
  ```cpp
  class GPSHandler {
    private:
      TinyGPSPlus gps;
      HardwareSerial gpsSerial;
      
    public:
      void begin();
      bool update();
      float getLatitude();
      float getLongitude();
      float getAccuracy();
      bool hasFix();
      String getLocationString();
  };
  ```

### **Step 3.3: GPS Data Processing**
- [ ] Implement NMEA parsing
- [ ] Add location validation
- [ ] Create location caching
- [ ] Implement accuracy filtering
- [ ] Add GPS fix monitoring

---

## **Phase 4: Bluetooth Low Energy (BLE) Implementation (Week 7-8)**

### **Step 4.1: BLE Advertising Setup**
- [ ] Configure BLE advertising:
  ```cpp
  class BLEHandler {
    private:
      BLEServer* pServer;
      BLEAdvertising* pAdvertising;
      
    public:
      void begin();
      void startAdvertising();
      void stopAdvertising();
      void updateRSSI();
  };
  ```

### **Step 4.2: BLE Service Implementation**
- [ ] Create custom BLE service
- [ ] Implement device identification
- [ ] Add RSSI broadcasting
- [ ] Create proximity detection logic
- [ ] Implement BLE power management

### **Step 4.3: BLE Power Optimization**
- [ ] Implement sleep/wake cycles
- [ ] Optimize advertising intervals
- [ ] Add power consumption monitoring
- [ ] Create battery-aware BLE operation

---

## **Phase 5: MQTT Communication (Week 9-10)**

### **Step 5.1: MQTT Client Setup**
- [ ] Configure MQTT client:
  ```cpp
  class MQTTHandler {
    private:
      WiFiClient wifiClient;
      PubSubClient mqttClient;
      
    public:
      void begin();
      bool connect();
      bool publishLocation(float lat, float lng);
      bool publishSOS();
      bool publishSoundAlert(String soundType);
      bool publishStatus();
  };
  ```

### **Step 5.2: MQTT Topics & Messages**
- [ ] Define MQTT topic structure:
  ```
  child/{deviceId}/location
  child/{deviceId}/sos
  child/{deviceId}/sound
  child/{deviceId}/status
  child/{deviceId}/battery
  ```

- [ ] Create JSON message formats:
  ```json
  {
    "deviceId": "CHILD_001",
    "timestamp": "2024-12-15T10:30:00Z",
    "latitude": 30.012345,
    "longitude": 31.123456,
    "accuracy": 5.2,
    "battery": 85
  }
  ```

### **Step 5.3: MQTT Reliability**
- [ ] Implement connection retry logic
- [ ] Add message queuing for offline operation
- [ ] Create heartbeat mechanism
- [ ] Implement QoS levels
- [ ] Add message acknowledgment

---

## **Phase 6: SOS Button Implementation (Week 11-12)**

### **Step 6.1: Hardware Integration**
- [ ] Connect SOS button to GPIO
- [ ] Implement debouncing circuit
- [ ] Add LED indicator
- [ ] Create button state monitoring

### **Step 6.2: SOS Software Logic**
- [ ] Create SOS handler:
  ```cpp
  class SOSHandler {
    private:
      int buttonPin;
      bool lastButtonState;
      unsigned long lastDebounceTime;
      
    public:
      void begin(int pin);
      void checkButton();
      void triggerSOS();
      bool isPressed();
  };
  ```

### **Step 6.3: SOS Alert System**
- [ ] Implement immediate MQTT publish
- [ ] Add LED flashing pattern
- [ ] Create SOS confirmation
- [ ] Implement SOS timeout
- [ ] Add SOS history logging

---

## **Phase 7: Sound Detection System (Week 13-14)**

### **Step 7.1: Microphone Integration**
- [ ] Connect microphone to ADC
- [ ] Implement analog reading
- [ ] Create sound level calibration
- [ ] Add noise filtering

### **Step 7.2: Sound Analysis**
- [ ] Create sound handler:
  ```cpp
  class SoundHandler {
    private:
      int micPin;
      int threshold;
      unsigned long lastCheck;
      
    public:
      void begin(int pin);
      int getSoundLevel();
      bool detectAbnormalSound();
      void setThreshold(int level);
  };
  ```

### **Step 7.3: Sound Alert Logic**
- [ ] Implement sound level monitoring
- [ ] Create abnormal sound detection
- [ ] Add sound pattern recognition
- [ ] Implement configurable thresholds
- [ ] Create sound alert messages

---

## **Phase 8: Power Management (Week 15-16)**

### **Step 8.1: Battery Monitoring**
- [ ] Implement battery level detection
- [ ] Create voltage divider circuit
- [ ] Add battery calibration
- [ ] Implement low battery alerts

### **Step 8.2: Power Optimization**
- [ ] Create power manager:
  ```cpp
  class PowerManager {
    private:
      int batteryPin;
      float batteryVoltage;
      
    public:
      void begin(int pin);
      float getBatteryLevel();
      void enterSleepMode();
      void wakeUp();
      bool isLowBattery();
  };
  ```

### **Step 8.3: Sleep Mode Implementation**
- [ ] Implement deep sleep cycles
- [ ] Create wake-up triggers
- [ ] Add power consumption monitoring
- [ ] Implement adaptive power management
- [ ] Create battery life optimization

---

## **Phase 9: System Integration & Testing (Week 17-18)**

### **Step 9.1: Main Application Loop**
- [ ] Create main application structure:
  ```cpp
  void setup() {
    // Initialize all components
    gpsHandler.begin();
    bleHandler.begin();
    mqttHandler.begin();
    sosHandler.begin(SOS_PIN);
    soundHandler.begin(MIC_PIN);
    powerManager.begin(BATTERY_PIN);
  }
  
  void loop() {
    // Main application loop
    gpsHandler.update();
    bleHandler.update();
    sosHandler.checkButton();
    soundHandler.check();
    powerManager.update();
    mqttHandler.loop();
  }
  ```

### **Step 9.2: Component Integration Testing**
- [ ] Test GPS functionality
- [ ] Verify BLE advertising
- [ ] Test MQTT communication
- [ ] Validate SOS button
- [ ] Test sound detection
- [ ] Verify power management

### **Step 9.3: System Testing**
- [ ] End-to-end communication testing
- [ ] Battery life testing
- [ ] Range testing for BLE
- [ ] GPS accuracy testing
- [ ] Stress testing
- [ ] Environmental testing

---

## **Phase 10: Final Integration & Deployment (Week 19-20)**

### **Step 10.1: Backend Integration**
- [ ] Test MQTT communication with backend
- [ ] Verify message formats
- [ ] Test alert delivery
- [ ] Validate data flow

### **Step 10.2: Mobile App Integration**
- [ ] Test BLE scanning with mobile app
- [ ] Verify proximity detection
- [ ] Test device pairing
- [ ] Validate alert reception

### **Step 10.3: Production Preparation**
- [ ] Create production firmware
- [ ] Implement OTA updates
- [ ] Add device configuration
- [ ] Create deployment documentation
- [ ] Prepare hardware assembly guide

---

## **Hardware Components List**

### **Core Components:**
- **ESP32-WROOM-32** Development Board
- **NEO-6M GPS Module** with antenna
- **3.7V 2000mAh Li-ion Battery**
- **TP4056 Charging Module**
- **LM317 Voltage Regulator**
- **Tactile Push Button** (SOS)
- **MAX9814 Microphone Amplifier**
- **Electret Microphone**
- **LEDs** (Power, GPS, Wi-Fi, SOS)
- **Resistors and Capacitors**

### **Development Tools:**
- **Arduino IDE 2.x**
- **ESP32 Board Support**
- **USB-C Cable**
- **Breadboard and Jumper Wires**
- **Multimeter**
- **Oscilloscope** (optional)
- **3D Printer** for enclosure

### **Testing Equipment:**
- **GPS Simulator** (optional)
- **BLE Scanner App**
- **MQTT Client** (MQTT Explorer)
- **Battery Tester**
- **Sound Level Meter**

---

## **Key Features & Specifications**

### **GPS Tracking:**
- **Update Rate**: Every 10-30 seconds
- **Accuracy**: 5-10 meters
- **Fix Time**: < 30 seconds (cold start)
- **Power Consumption**: ~25mA during fix

### **BLE Proximity:**
- **Range**: 5-10 meters (indoor)
- **Advertising Interval**: 1 second
- **Power Consumption**: ~15mA
- **Device Identification**: Unique MAC address

### **SOS Button:**
- **Response Time**: < 100ms
- **Debounce Time**: 50ms
- **Visual Feedback**: LED indicator
- **MQTT Priority**: Highest

### **Sound Detection:**
- **Sensitivity**: Configurable (30-90 dB)
- **Update Rate**: Every 5 seconds
- **Detection Types**: Crying, screaming, silence
- **False Positive Filtering**: Time-based averaging

### **Power Management:**
- **Battery Life**: 8-12 hours continuous
- **Sleep Mode**: Deep sleep with periodic wake
- **Low Battery Alert**: < 20% capacity
- **Charging**: USB-C with LED indicator

---

## **MQTT Message Examples**

### **Location Update:**
```json
{
  "topic": "child/CHILD_001/location",
  "payload": {
    "deviceId": "CHILD_001",
    "timestamp": "2024-12-15T10:30:00Z",
    "latitude": 30.012345,
    "longitude": 31.123456,
    "accuracy": 5.2,
    "battery": 85,
    "satellites": 8
  }
}
```

### **SOS Alert:**
```json
{
  "topic": "child/CHILD_001/sos",
  "payload": {
    "deviceId": "CHILD_001",
    "timestamp": "2024-12-15T10:30:00Z",
    "latitude": 30.012345,
    "longitude": 31.123456,
    "alertType": "SOS",
    "priority": "HIGH"
  }
}
```

### **Sound Alert:**
```json
{
  "topic": "child/CHILD_001/sound",
  "payload": {
    "deviceId": "CHILD_001",
    "timestamp": "2024-12-15T10:30:00Z",
    "soundType": "CRYING",
    "confidence": 85,
    "soundLevel": 75,
    "duration": 3000
  }
}
```

### **Device Status:**
```json
{
  "topic": "child/CHILD_001/status",
  "payload": {
    "deviceId": "CHILD_001",
    "timestamp": "2024-12-15T10:30:00Z",
    "battery": 85,
    "online": true,
    "gpsFix": true,
    "wifiSignal": -45,
    "uptime": 3600
  }
}
```

---

## **Next Steps After IoT Device Completion**

1. **Backend Integration**: Test complete MQTT communication
2. **Mobile App Testing**: Verify BLE scanning and proximity
3. **System Integration**: End-to-end testing with all components
4. **Performance Testing**: Battery life and reliability testing
5. **User Testing**: Real-world testing with target users
6. **Documentation**: Complete hardware and firmware documentation
7. **Production Planning**: Manufacturing and assembly planning
8. **Presentation**: Prepare for graduation project presentation

---

## **Troubleshooting Guide**

### **Common Issues:**
- **GPS Not Fixing**: Check antenna connection and clear sky view
- **Wi-Fi Connection Issues**: Verify credentials and signal strength
- **MQTT Disconnection**: Check network stability and broker settings
- **High Power Consumption**: Optimize sleep cycles and component usage
- **BLE Range Issues**: Check for interference and adjust power settings

### **Debug Tools:**
- **Serial Monitor**: For real-time debugging
- **MQTT Explorer**: For message monitoring
- **BLE Scanner**: For proximity testing
- **GPS Test Apps**: For location verification

---

**Document Version**: 1.0  
**Last Updated**: December 2024  
**Project**: ChildGuard - Real-Time Child Monitoring System 