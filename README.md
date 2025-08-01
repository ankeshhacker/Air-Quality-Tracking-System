# Air-Quality-Tracking-System

An Arduino-based system for monitoring PM2.5, CO, CO₂, and temperature/humidity in real time.

# Features
-> Measures PM2.5 (dust particles), CO, CO₂, and Temperature/Humidity in real-time.
-> Displays readings on a 16x2 LCD and Serial Monitor.
-> Can log data or be upgraded for IoT-enabled monitoring (ESP32).

# Hardware Components
1. Arduino Uno
2. Dust Sensor 
3. MQ-7 (for CO gas detection)
4. MQ-135 (for CO₂ & air quality index)
5. DHT11/DHT22 (for temperature & humidity)
6. 16x2 LCD (I2C module recommended)
7. Breadboard, Jumper Wires
8. Power Source (USB/Battery)

# Circuit Diagram

<img width="1779" height="1180" alt="image" src="https://github.com/user-attachments/assets/bc693af1-6224-4e9e-b41a-228934a21485" />

# Arduino Code
cpp
Copy
Edit
#include <LiquidCrystal.h>
#include "DHT.h"

#define DHTPIN 2     
#define DHTTYPE DHT11   

// LCD pins: RS, E, D4, D5, D6, D7
LiquidCrystal lcd(12, 11, 5, 4, 3, 2);
DHT dht(DHTPIN, DHTTYPE);

// Sensor pins
const int dustPin = A0;
const int mq7Pin = A1;
const int mq135Pin = A2;

void setup() {
  lcd.begin(16, 2);
  Serial.begin(9600);
  dht.begin();
  lcd.print("Air Quality Sys");
  delay(2000);
}

void loop() {
  // Dust Sensor (PM2.5)
  int dustVal = analogRead(dustPin);
  float dustDensity = (dustVal * (5.0 / 1023.0)) * 100; // Rough μg/m³

  // MQ-7 CO Sensor
  int coVal = analogRead(mq7Pin);
  float coPPM = map(coVal, 0, 1023, 0, 1000); // Approx ppm

  // MQ-135 CO₂ Sensor
  int co2Val = analogRead(mq135Pin);
  float co2PPM = map(co2Val, 0, 1023, 400, 5000); // Approx ppm

  // Temperature & Humidity
  float temperature = dht.readTemperature();
  float humidity = dht.readHumidity();

  // Print to Serial
  Serial.print("PM2.5: "); Serial.print(dustDensity); Serial.print(" ug/m3");
  Serial.print(" | CO: "); Serial.print(coPPM); Serial.print(" ppm");
  Serial.print(" | CO2: "); Serial.print(co2PPM); Serial.print(" ppm");
  Serial.print(" | Temp: "); Serial.print(temperature);
  Serial.print(" | Hum: "); Serial.println(humidity);

  // Display on LCD
  lcd.clear();
  lcd.setCursor(0,0);
  lcd.print("PM2.5: "); lcd.print(dustDensity,0);
  lcd.setCursor(0,1);
  lcd.print("CO: "); lcd.print(coPPM,0); lcd.print("ppm");
  delay(2000);
}

# Manufacturing Steps
1. Assemble the sensors: Connect PM2.5, MQ-7, MQ-135, and DHT11 to Arduino as per the wiring diagram.
2. Calibrate gas sensors: MQ sensors require burn-in time (~24–48 hrs) for stable readings.
3. Test readings using the Serial Monitor.
4. Enclosure: Use a ventilated case (airflow is crucial for gas sensors).



