#include <WiFi.h>
#include <BlynkSimpleEsp32.h>
#include "DHT.h"

#define DHTPIN 4
#define DHTTYPE DHT22
#define SOIL_PIN 34
#define RELAY_PIN 16

char auth[] = "BLYNK_TOKEN";
char ssid[] = "YOUR_SSID";
char pass[] = "YOUR_PASS";

DHT dht(DHTPIN, DHTTYPE);

void setup() {
  Serial.begin(115200);
  pinMode(RELAY_PIN, OUTPUT);
  digitalWrite(RELAY_PIN, HIGH); // relay off (depends on module)
  Blynk.begin(auth, ssid, pass);
  dht.begin();
}

void loop() {
  Blynk.run();
  static unsigned long last = 0;
  if (millis() - last > 10000) { // every 10s (adjust)
    last = millis();
    float temp = dht.readTemperature();
    int soilRaw = analogRead(SOIL_PIN); // 0..4095 on ESP32
    float soilPercent = (soilRaw / 4095.0) * 100.0; // calibrate as needed

    Serial.printf("Temp: %.1f C, Soil Raw: %d, Soi%%: %.1f\n", temp, soilRaw, soilPercent);

    Blynk.virtualWrite(V0, soilPercent); // soil moisture
    Blynk.virtualWrite(V1, temp);       // temp

    // Simple auto water logic
    float threshold = 30.0; // percent
    if (soilPercent < threshold) {
      digitalWrite(RELAY_PIN, LOW); // turn pump ON (check your relay logic)
      Blynk.notify("Watering ON - soil low");
    } else {
      digitalWrite(RELAY_PIN, HIGH); // pump OFF
    }
  }
}
