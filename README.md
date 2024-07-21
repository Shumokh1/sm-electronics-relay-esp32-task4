# sm-electronics-relay-esp32-task4
Task 4: Use esp32 along with light sensor and motion sensor and connect them to relay so that 
1) if no light detected but there is motion = turn light on
2) if no light and no motion = led off
3) if there is light but no motion = turn light off.

## Demo
<img width="700" alt="Screen Shot 1446-01-16 at 12 50 37 AM" src="https://github.com/user-attachments/assets/9e83d444-08dc-4413-9ff9-0c3c261823af">

<img width="700" alt="Screen Shot 1446-01-16 at 12 50 49 AM" src="https://github.com/user-attachments/assets/769f577b-10a6-4933-985f-12c540b19e4d">

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
- **NO (Normally Open)**: Connect to anode of the LED
- **COM (Common)**: Connect to a suitable power source (e.g., `5V`)

### LED
- **Anode (positive lead)**: Connect to relay NO (Normally Open)
- **Cathode (negative lead)**: Connect to `GND` on ESP32 
- 
### PIR Sensor
- **VCC**: Connect to `3V3` on ESP32
- **GND**: Connect to `GND` on ESP32
- **OUT**: Connect to `GPIO 14` on ESP32

## Why We Use a Relay

A relay is an electrically operated switch that allows you to control a high-power device (like an LED light) with a low-power signal from a microcontroller (like the ESP32). The relay isolates the low-power ESP32 from the high-power LED circuit, providing safety and allowing the ESP32 to control devices that require higher voltage and current than it can directly handle.

## Code

```cpp
#define PIR_PIN 14
#define RELAY_PIN 4
#define LDR_PIN 36  // Analog pin (VP is GPIO 36)


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

  if (pirValue == HIGH ) { // Movement detected 
    digitalWrite(RELAY_PIN, HIGH); // Turn on the light
  } else {
    digitalWrite(RELAY_PIN, LOW); // Turn off the light
  }

  delay(1000); 
}
```


