#include <OneWire.h>
#include <DallasTemperature.h>

#define ONE_WIRE_BUS 2     // DS18B20 on D2
OneWire oneWire(ONE_WIRE_BUS);
DallasTemperature sensors(&oneWire);

const int pHpin = A0;
float offset = 0.00;       // ปรับเทียบหลังแช่น้ำ buffer pH 7

void setup() {
  Serial.begin(9600);
  sensors.begin();
}

void loop() {
  /* อ่าน pH (อนาล็อก 0–1023 → แปลงเป็นแรงดัน → pH) */
  int16_t adc = analogRead(pHpin);
  float voltage = adc * 5.0 / 1023.0;
  float pHValue = 3.5 * voltage + offset;   // สมการคร่าว ๆ ปรับใหม่หลังคาลิเบรต

  /* อ่านอุณหภูมิ */
  sensors.requestTemperatures();
  float tempC = sensors.getTempCByIndex(0);

  Serial.print("Temp: "); Serial.print(tempC); Serial.print(" °C   ");
  Serial.print("pH: ");   Serial.println(pHValue);

  delay(2000);
}

<!---
winll88/winll88 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
