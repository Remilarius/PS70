---
title: "Week 7: Electronic Output Devices"
date: 2021-03-07
---

Still messing the microphone and TFT display, but in the meantime I had seperately accomplish this goal with other equipment actually.

The BNO085 is an IMU that can be connected to an Arduino, like MKRZero or Wifi 1010, and it's an accelerometer like the one in our kit. It can read the XYZ or roll, pitch, yaw of the board to determine the orientation and outputs the information to a serial monitor.

![BNO085](BNO085.jpg)

This code takes that input and control the motors of a quadcopter to stabilize its flight. When any of the axes exceed a certain angle, it turns on the motors in that axis.

```html

// Sketch for reading the Euler angles from the BNO085, and change drone motors ** Credits to ES 50 Lab 6

#include <Adafruit_BNO08x.h> // Include this line to be able to use the Adafruit_BNO08x.h library

#define BNO08X_RESET -1

Adafruit_BNO08x  bno08x(BNO08X_RESET); // Initialize the BNO08x object that we will use to read the IMU
sh2_SensorValue_t sensorValue; // Create the variable that will store the incoming sensor data from the IMU
float eulerAngles[3]; // Create an array for storing the angles around each axis: index 0 for x (roll), 1 for y (pitch), 2 for z (yaw)

int EN5 = 5;                 // Motor Pin
int EN4 = 4;                 // Motor Pin
int EN3 = 3;                 // Motor Pin
int EN2 = 2;                 // Motor Pin

void setup()

{
  pinMode(EN5, OUTPUT);   // where the motor is connected to
  pinMode(EN2, OUTPUT);   // where the motor is connected to
  pinMode(EN3, OUTPUT);   // where the motor is connected to
  pinMode(EN4, OUTPUT);   // where the motor is connected to
    
  Serial.begin(115200);
  
  Serial.println("Adafruit BNO08x test!");

  // Attempt to initialize the BNO085, if it fails enter an infinite loop
  if (!bno08x.begin_I2C()) {
    Serial.println("Failed to find BNO08x chip");
    while (1) {
      delay(10);
    }
  }
  Serial.println("BNO08x Found!");

  // See definition of setReports() below
  setReports();

  Serial.println("Reading events");
  delay(100);
}

void setReports(void) {
  Serial.println("Setting desired reports");
  if (! bno08x.enableReport(SH2_GAME_ROTATION_VECTOR)) {
    Serial.println("Could not enable game vector");
  }
}


void loop() {
  delay(10);

  // This if statement just checks every loop if the sensor was reset,
  // which can be useful if the sensor is ever disconnected accidentally.
  // In that case, this if statement will be entered and the reports will be re-initialized
  if (bno08x.wasReset()) {
    Serial.print("sensor was reset ");
    setReports();
  }

  // This checks if the values stored in sensorValue have been updated
  if (! bno08x.getSensorEvent(&sensorValue)) {
    return;
  }

  // This if statement checks if the sensorValue that has been updated
  // is the game rotation vector
  if (sensorValue.sensorId == SH2_GAME_ROTATION_VECTOR) {
      toEuler(eulerAngles, sensorValue);

// ******* STUDENT CODE ************

      Serial.print("roll:");
      Serial.print(eulerAngles[0]);
      Serial.print(" pitch:");
      Serial.print(eulerAngles[1]);
      Serial.print(" yaw:");
      Serial.println(eulerAngles[2]);

// ********* If roll or yaw is over 80 deg or less than -80 deg spin specific motors 
// ********* For some reason the pitch readings were very unreliable

      if (eulerAngles[2] > 80) {
          analogWrite(EN4,0);      //sets the motors speed
          analogWrite(EN2,100);
          analogWrite(EN3,0);
          analogWrite(EN5,0);
          delay(250);
      }
      if (eulerAngles[2] < -80) {
          analogWrite(EN4,0);      //sets the motors speed
          analogWrite(EN2,0);
          analogWrite(EN3,100);
          analogWrite(EN5,0);
          delay(250);
      }
      if (eulerAngles[0] > 80) {
          analogWrite(EN4,100);      //sets the motors speed
          analogWrite(EN2,0);
          analogWrite(EN3,0);
          analogWrite(EN5,0);
          delay(250);
      }
      if (eulerAngles[0] < -80) {
          analogWrite(EN4,0);      //sets the motors speed
          analogWrite(EN2,0);
          analogWrite(EN3,0);
          analogWrite(EN5,100);
          delay(250);
      }
    
  }
}

void toEuler(float * angles, sh2_SensorValue_t quat) {
  float w = quat.un.gameRotationVector.real;
  float x = quat.un.gameRotationVector.i;
  float y = quat.un.gameRotationVector.j;
  float z = quat.un.gameRotationVector.k;
  float rawAngles[3] = {0, 0, 0}; // angles in radians

  float sqw = w * w;
  float sqx = x * x;
  float sqy = y * y;
  float sqz = z * z;

  rawAngles[2] = atan2(2.0 * (x*y + z*w), (sqx - sqy - sqz + sqw));
  rawAngles[1] = asin(-2.0 * (x*z - y*w) / (sqx + sqy + sqz + sqw));
  rawAngles[0] = atan2(2.0 * (y*z + x*w), (-sqx - sqy + sqz + sqw));
  
  for (int i = 0; i < 3; i++) {
    angles[i] = rawAngles[i] * (180.0/PI); // convert from radians to degrees
  }
}