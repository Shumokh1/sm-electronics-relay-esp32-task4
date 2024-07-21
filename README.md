# sm-electronics-relay-esp32-task4
task 4: Use esp32 along with light sensor and motion sensor and connect them to relay so that 
1) if no light detected but there is motion = turn light on
2) if no light and no motion = led off
3) if there is light but no motion = turn light off.


## Components

- ESP32
- Relay module
- LED (or any light)
- Light sensor (LDR - Light Dependent Resistor)
- Movement sensor (PIR sensor)


## Circuit Connections

### Relay Module
- **VCC**: Connect to `5V` on ESP32
- **GND**: Connect to `GND` on ESP32
- **IN**: Connect to `GPIO 4` on ESP32
- **NO (Normally Open)**: Connect one end of the LED
- **COM (Common)**: Connect to a suitable power source (e.g., `5V`)

### Light Sensor (LDR)
- **One end**: Connect to `3V3` on ESP32
- **Other end**: Connect to one side of a 10k ohm resistor
- **Junction (LDR and resistor)**: Connect to `GPIO 36` (Analog pin VP) on ESP32
- **Other side of the resistor**: Connect to `GND`

### PIR Sensor
- **VCC**: Connect to `3V3` on ESP32
- **GND**: Connect to `GND` on ESP32
- **OUT**: Connect to `GPIO 15` on ESP32

## Why We Use a Relay

A relay is an electrically operated switch that allows you to control a high-power device (like an LED light) with a low-power signal from a microcontroller (like the ESP32). The relay isolates the low-power ESP32 from the high-power LED circuit, providing safety and allowing the ESP32 to control devices that require higher voltage and current than it can directly handle.

## Code

```cpp
#define PIR_PIN 15
#define RELAY_PIN 4
#define LDR_PIN 36  // Analog pin (VP is GPIO 36)
#define ANALOG_THRESHOLD 800

void setup() {
  pinMode(PIR_PIN, INPUT);
  pinMode(RELAY_PIN, OUTPUT);
  Serial.begin(115200);
}

void loop() {
  int pirValue = digitalRead(PIR_PIN);
  int ldrValue = analogRead(LDR_PIN);

  Serial.print("PIR Value: ");
  Serial.println(pirValue);
  Serial.print("LDR Value: ");
  Serial.println(ldrValue);

  if (pirValue == HIGH && ldrValue < ANALOG_THRESHOLD) { // Movement detected and it's dark
    digitalWrite(RELAY_PIN, HIGH); // Turn on the light
  } else {
    digitalWrite(RELAY_PIN, LOW); // Turn off the light
  }

  delay(1000); // Check every second
}