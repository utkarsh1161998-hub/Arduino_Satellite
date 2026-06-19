#include <Wire.h>
#include <Adafruit_Sensor.h>
#include <Adafruit_BME280.h>

Adafruit_BME280 bme; // I2C setup
unsigned long packetCount = 0;

void setup() {
  Serial.begin(9600);
  Serial.println(F("--- CanSat Arduino Sat Initialize ---"));

  if (!bme.begin(0x76)) {
    Serial.println(F("Could not find a valid BME280 sensor, check wiring!"));
    while (1);
  }
}

void loop() {
  packetCount++;
  
  float temperature = bme.readTemperature();
  float pressure = bme.readPressure() / 100.0F;
  float altitude = bme.readAltitude(1013.25); // Baseline sea-level pressure

  // Format data as a CSV string for easy transmitting/logging
  Serial.print("PKT:"); Serial.print(packetCount);
  Serial.print(",TEMP:"); Serial.print(temperature);
  Serial.print(",PRES:"); Serial.print(pressure);
  Serial.print(",ALT:"); Serial.println(altitude);

  delay(1000); // Sample data every second
}
