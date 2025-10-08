# Learning-I2C-and-SPI-on-arduino-Nano-using-2-BMP280

# BMP280 Dual Sensor Experiments (Arduino Nano)

This repo demonstrates using **two BMP280 barometric pressure and temperature sensors** with an **Arduino Nano**, first in **I²C mode** and then in **SPI mode**.  
It is designed for learning and experimenting with communication protocols.

---

## 🧪 Experiments

### 1. Dual BMP280 on I²C
- Both sensors share **SDA** and **SCL** lines.  
- Unique I²C addresses assigned by wiring the **SDO pin**:
  - Sensor 1: SDO → GND → address `0x76`  
  - Sensor 2: SDO → 3.3V → address `0x77`  
- **CSB pins tied HIGH** to force I²C mode.  

**Connections (Arduino Nano → BMP280s):**
- A4 → SDA  
- A5 → SCL  
- 3.3V → VCC  
- GND → GND  
- CSB → 3.3V  
- SDO → GND (sensor 1) / 3.3V (sensor 2)  


![WhatsApp Image 2025-09-28 at 01 21 36_8fca4690](https://github.com/user-attachments/assets/071b3b22-31a9-4113-9103-9b86ce5ffaa1)
![WhatsApp Image 2025-09-28 at 01 21 37_e873135a](https://github.com/user-attachments/assets/4e365eac-b145-492e-9a55-bc3617de709b)
![WhatsApp Image 2025-09-28 at 01 21 38_412549a1](https://github.com/user-attachments/assets/0b8e1a55-5a52-4c26-9cba-4615ffb98fe1)

---

### 2. Dual BMP280 on SPI
- Both sensors share **SCK, MOSI, MISO**.  
- Each sensor has a **dedicated CS pin** from the Arduino.  

**Connections (Arduino Nano → BMP280s):**
- D13 → SCK  
- D11 → MOSI (SDI)  
- D12 → MISO (SDO)  
- D10 → CSB (sensor 1)  
- D9  → CSB (sensor 2)  
- 3.3V → VCC  
- GND → GND  
![WhatsApp Image 2025-09-28 at 01 21 38_19071819](https://github.com/user-attachments/assets/e9264766-5776-4cc4-a57b-6879ab9856b3)
![WhatsApp Image 2025-09-28 at 01 21 39_911e3c57](https://github.com/user-attachments/assets/ac8e6fdd-f2b5-4822-8fe9-16beef6de847)

---

## 📂 Code

### I²C Example (`I2C_BMP280/I2C_BMP280.ino`)
```cpp
#include <Wire.h>
#include <Adafruit_BMP280.h>

Adafruit_BMP280 bmp1; // 0x76
Adafruit_BMP280 bmp2; // 0x77

void setup() {
  Serial.begin(9600);

  if (!bmp1.begin(0x76)) { Serial.println("BMP280 #1 not found!"); while (1); }
  if (!bmp2.begin(0x77)) { Serial.println("BMP280 #2 not found!"); while (1); }
}

void loop() {
  Serial.print("Sensor 1 Temp: "); Serial.println(bmp1.readTemperature());
  Serial.print("Sensor 1 Pressure: "); Serial.println(bmp1.readPressure() / 100.0);

  Serial.print("Sensor 2 Temp: "); Serial.println(bmp2.readTemperature());
  Serial.print("Sensor 2 Pressure: "); Serial.println(bmp2.readPressure() / 100.0);

  Serial.println();
  delay(2000);
}

---


## 📂 Code

### SPI Example (`SPI_BMP280/SPI_BMP280.ino`)
```cpp
#include <SPI.h>
#include <Adafruit_BMP280.h>

// Hardware SPI, just pass CS pins
Adafruit_BMP280 bmp1(10); // CS = D10
Adafruit_BMP280 bmp2(9);  // CS = D9

void setup() {
  Serial.begin(9600);

  if (!bmp1.begin()) { Serial.println("BMP280 #1 not found!"); while (1); }
  if (!bmp2.begin()) { Serial.println("BMP280 #2 not found!"); while (1); }
}

void loop() {
  Serial.print("Sensor 1 Temp: "); Serial.println(bmp1.readTemperature());
  Serial.print("Sensor 1 Pressure: "); Serial.println(bmp1.readPressure() / 100.0);

  Serial.print("Sensor 2 Temp: "); Serial.println(bmp2.readTemperature());
  Serial.print("Sensor 2 Pressure: "); Serial.println(bmp2.readPressure() / 100.0);

  Serial.println();
  delay(2000);
}

