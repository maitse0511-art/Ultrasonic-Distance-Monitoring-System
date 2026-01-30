# Ultrasonic Distance Monitoring System

An Arduino-based embedded system that measures object distance using the HC-SR04 ultrasonic sensor and provides visual feedback via LEDs and a 16×4 LCD display.

## Features
- Real-time distance measurement using time-of-flight principles
- Distance calculation using `pulseIn()` timing
- Visual zone indication:
  - 🟢 Safe Zone (> 20 cm)
  - 🟠 Caution Zone (10–20 cm)
  - 🔴 Danger Zone (< 10 cm) with blinking warning
- LCD-based user interface
- Serial output for debugging

## Hardware Components
- Arduino Uno
- HC-SR04 Ultrasonic Sensor
- 16×4 LCD (parallel interface)
- Red, Green, Orange LEDs
- Resistors and jumper wires

## How It Works
The Arduino sends a 10 µs trigger pulse to the HC-SR04 sensor.  
The echo pulse duration is measured using the `pulseIn()` function and converted into distance based on the speed of sound.

Threshold-based logic determines the system state and controls LED indicators and LCD warnings accordingly.

## Skills Demonstrated
- Embedded C/C++ (Arduino)
- Digital I/O and timing control
- Sensor interfacing
- Human–machine interface design
- Debugging and system testing

## Future Improvements
- Non-blocking timing using `millis()`
- Noise filtering / averaging of distance readings
- Buzzer integration
- Power optimization
