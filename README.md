Downloads, docs, tutorials: https://www.blynk.io
 Sketch generator:https://examples.blynk.cc
 Blynk community:https://community.blynk.cc
 Follow us:https://www.fb.com/blynkapp
 https://twitter.com/blynk_app
 Blynk library is licensed under MIT license
 This example code is in public domain.
*************************************************************
 This example runs directly on ESP8266 chip.
 NOTE: This requires ESP8266 support package:
 https://github.com/esp8266/Arduino
 Please be sure to select the right ESP8266 module
 in the Tools -> Board menu!
 Change WiFi ssid, pass, and Blynk auth token to run :)
 Feel free to apply it to any other example. It's simple!
*************************************************************
Comment this out to disable prints and save space
#define BLYNK_PRINT Serial
/* Fill in information from Blynk Device Info here */
#define BLYNK_TEMPLATE_ID "TMPL3G6L4dcN1"
#define BLYNK_TEMPLATE_NAME "HELMET SAFETY"
#defineBLYNK_AUTH_TOKEN "VMWwk3L5JVg5uVaNOZXri6U4rVEXGDlq"
#include <ESP8266WiFi.h>
#include <BlynkSimpleEsp8266.h>
BlynkTimer timer;
char ssid[] = "iot2025";
char pass[] = "testing2025";
int GAS=D5;
int GAS_VALUE=0;
int RELAY=D4;
int HELMET=D6;
int HELMET_VALUE=0;
#include <Wire.h>
#include <Adafruit_Sensor.h>
#include <Adafruit_ADXL345_U.h>
Adafruit_ADXL345_Unified accel = Adafruit_ADXL345_Unified(12345);
void ENGINE_STATE()
{
GAS_VALUE=digitalRead(GAS);
Serial.print("GAS_VALUE : ");
Serial.println(GAS_VALUE);
HELMET_VALUE=digitalRead(HELMET);
Serial.print("HELMET_VALUE");
Serial.println(HELMET_VALUE);
if(HELMET_VALUE==0 && GAS_VALUE==1)
{
digitalWrite(RELAY,LOW);
Serial.println("RELAY iS ON");
DEMO_MSG();
delay(50);
}
else
{
digitalWrite(RELAY,HIGH);
Serial.println("RELAY iS OFF");
}
delay(50);
}
void DEMO_MSG()
{
sensors_event_t event;
 accel.getEvent(&event);
 Serial.print("X: "); Serial.print(event.acceleration.x);
 Serial.print(" Y: "); Serial.print(event.acceleration.y);
 Serial.print(" Z: "); Serial.print(event.acceleration.z);
 Serial.println(" m/s^2");
 delay(500);
if(event.acceleration.z < -10.00)
{
Blynk.logEvent("accident_msg","accident pls help");
Serial.println("MSG SEND");
delay(1000);
}
delay(100);
}
void setup()
{
 pinMode(GAS,INPUT);
 pinMode(HELMET,INPUT_PULLUP);
 pinMode(RELAY,OUTPUT);
 digitalWrite(RELAY,HIGH);
 Serial.begin(9600);
 timer.setInterval(2000L,ENGINE_STATE);
 Blynk.begin(BLYNK_AUTH_TOKEN, ssid, pass);
 Wire.begin(D2, D1); // SDA, SCL
 if(!accel.begin()) {
 Serial.println("ADXL345 not detected!");
 while(1);
 }
 Serial.println("ADXL345 Ready!");
 accel.setRange(ADXL345_RANGE_16_G); // ±16g range
}
void loop()
{
 Blynk.run();
 timer.run();
}
