---
title: "Week 9: WiFi and Bluetooth (IoT)"
date: 2021-03-30
draft: false
---
### BLE
I thought I'd try the BLE subset of the assignment since my team had been thinking about ways to control a drone and BLE was one of the options we were considering.

I followed the instructions on the class website and uploaded the BLE_write program from examples 

For the LED specifically, there was this code also on the class website.
{{< highlight html >}}
/*
    Based on Neil Kolban example for IDF: https://github.com/nkolban/esp32-snippets/blob/master/cpp_utils/tests/BLE%20Tests/SampleWrite.cpp
    Ported to Arduino ESP32 by Evandro Copercini
*/
#include <BLEDevice.h>
#include <BLEUtils.h>
#include <BLEServer.h>
// See the following for generating UUIDs:
// https://www.uuidgenerator.net/
#define SERVICE_UUID        "4fafc201-1fb5-459e-8fcc-c5c9c331914b"
#define CHARACTERISTIC_UUID "beb5483e-36e1-4688-b7f5-ea07361b26a8"
int LED_PIN = 5;
char LED_STATUS;
class MyCallbacks: public BLECharacteristicCallbacks {
    void onWrite(BLECharacteristic *pCharacteristic) {
      std::string value = pCharacteristic->getValue();
      if (value.length() > 0) { 
        for (int i = 0; i < value.length(); i++){
          LED_STATUS = value[i];
        }
      }
    }
};
void setup() {
  Serial.begin(115200);
  BLEDevice::init("MyESP32");
  BLEServer *pServer = BLEDevice::createServer();
  BLEService *pService = pServer->createService(SERVICE_UUID);
  BLECharacteristic *pCharacteristic = pService->createCharacteristic(
                                         CHARACTERISTIC_UUID,
                                         BLECharacteristic::PROPERTY_READ |
                                         BLECharacteristic::PROPERTY_WRITE
                                       );
  pCharacteristic->setCallbacks(new MyCallbacks());
  pCharacteristic->setValue("Hello World");
  pService->start();
  BLEAdvertising *pAdvertising = pServer->getAdvertising();
  pAdvertising->start();
  pinMode(LED_PIN, OUTPUT);
  digitalWrite(LED_PIN, LOW);
}
void loop() {
  int val = (int)LED_STATUS;  // cast the char* as an int 
  if (val == 49)              // ASCII code for the number 1
    digitalWrite(LED_PIN, HIGH);
  else 
    digitalWrite(LED_PIN, LOW);

  delay(2000);
}
{{< /highlight >}}


### BLE Scanner App
I downloaded the basic BLE scanner from the app store and found my device on the scanner (there were a lot of devices that showed on the device surprisingly).

![Devicepage](Week9/devicepage.PNG)
![write](Week9/write.jpg)
![Write 1](Week9/write1.jpg)
![LEDon](Week9/LEDON.jpg)

I notice a 1-2 second lag between writing 1 or 0 in the app and the LED turning on and off. After getting rid of the delay, the response was almost instantaneous.

I was originally concerned about the lag in piloting something like a drone, but now that I know it's possible for the board to be pretty responsive, the problem is getting a better UI than having to write in numbers or HEX.

One solution to being limited by having to write text or HEX is to build command packets but I don't exactly know to what to map over as of yet.