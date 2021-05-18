---
title: "Final Project"
date: 2021-05-13
draft: false
---

## Demo Video

{{< youtube pPO-RpMidgE >}}

## Motivation

I really wanted something that moved around my house because I've been stuck inside for way to long. I wanted something autonomous to keep me company and since my mom wasn't on board with having a dog, I decided to build one.


## Fabrication

Since parts that needed to be lasercut or 3D printed needed to be designed and made first, decisions regarding how to spatially fit everything had to be made early on and all other parts had to conform.

Since the wolf would be a four legged thing, the legs would be the most important bit since they were going be what drives the machine and allow it to be mobile.

I spent quite a bit of time puzzled about how to put everything together, since motors/wheels/axles were decisions that had to be made in tandem, but after rifling our kit, it seems the N20 had custom parts that fit onto lego wheels.

I eneded up designing around that and how the legs of the wolf decided everything else.

![Circuit Motor Simulation](minecraftwolf.jpg)
![Circuit Motor Simulation](legshot.jpg)
![Circuit Motor Simulation](wolfcad.jpg)

### MOSFET ver.

I had previously build a quadcopter with a four motor setup with MOSFETs and flyback diodes and I thought it'd be simple enough to map over that technique since I also wanted to control 4 motors here. Unfortunately, I also knew at some point that'd I would want to back out of tight corners and such so this version was sadly up for the job.

{{< youtube xtvrQd0GNAc >}}


![Circuit Motor Simulation](finalcircuit.jpg)

I ended up using the two L9110 H Bridge Power Driver to control the 4 motors. I'm also using a bluetooth module, the SH-HC-08, and an app, ElegooBLE tool, to communicate with the MetroExpress. 

### ElegooBLE App Controls
![Circuit Motor Simulation](controlboard.PNG)

## Code on the MetroExpress

{{< highlight html >}}
int EN1 = 3;                
int EN2 = 4;

int EN3 = 5;
int EN4 = 6;

char Incoming_value = 0;

void setup()

{
    pinMode(EN1, OUTPUT);   // where the motor is connected to
    pinMode(EN2, OUTPUT);   // where the motor is connected to
    pinMode(EN3, OUTPUT);   // where the motor is connected to
    pinMode(EN4, OUTPUT);   // where the motor is connected to
    Serial.begin(9600);
    Serial1.begin(9600);
}

void loop()

{
  if(Serial1.available() > 0)
  {
    Incoming_value = Serial1.read();   
    if(Incoming_value == 'A'){
       Serial.println(Incoming_value);
       analogWrite(EN2, 0); 
       analogWrite(EN1, 255); 
    }
      else if(Incoming_value == 'B') {      //Checks whether value of Incoming_value is equal to 
        Serial.println(Incoming_value);
        analogWrite(EN1, 0);
        analogWrite(EN2, 255);               
      }        
      else if(Incoming_value == 'C') {      //Checks whether value of Incoming_value is equal to 
        Serial.println(Incoming_value);
        analogWrite(EN3, 0);         
        analogWrite(EN4, 255);     
      }
      else if(Incoming_value == 'D') {      //Checks whether value of Incoming_value is equal to 
        Serial.println(Incoming_value);
        analogWrite(EN4, 0); 
        analogWrite(EN3, 255);                 
      }
      else if(Incoming_value == 'K') {      //Checks whether value of Incoming_value is equal to 
        Serial.println(Incoming_value);
        analogWrite(EN2, 0); 
        analogWrite(EN1, 255);
        analogWrite(EN3, 0); 
        analogWrite(EN4, 255);
      }
      else if(Incoming_value == 'W') {      //Checks whether value of Incoming_value is equal to 
        Serial.println(Incoming_value);
        analogWrite(EN1, 0); 
        analogWrite(EN2   , 255);
        analogWrite(EN4, 0); 
        analogWrite(EN3, 255);         
      }
      else if(Incoming_value == 'S') {      //Checks whether value of Incoming_value is equal to 
        Serial.println(Incoming_value);
        analogWrite(EN1, 0);  
        analogWrite(EN2, 0);
        analogWrite(EN3, 0);  
        analogWrite(EN4, 0);         
      }
}}

{{< /highlight >}}

#### Notes on Code

This is manual control, and each key map to certain movement.

### CAD files

Solidworks Assembly, incomplete but enough to build with.
[Click to download zipped CAD file](files/Wolf.zip)

## Future Consideration

Weight and Size Reduction  
Higher volatge for the motors  
Better power source (the 7.5V pack wasn't reliable, there was a special combination of batteries in my house that worked.)
Tolerance for 3D prints, they came out too type.

### (Mistakes) 0/10 would not recommend anything I did

The 3D printed material is thicker than nominal values and resulted in fit issues where the motors were difficult to secure in their housing. Melting parts of the print with a soldering iron and hammering it in got the job done but it also somewhat damaged the gear train of the N20 motors.

The weight of the hardware was significant and assembling it by hand took a very long time. If it's possible to ensure structural integrity while reducing the amount of mates, needs to be tested.

### Features coming soon

Sounds, like barking and the Minecraft theme music via screeching piezobuzzer  

LED eyes, our kit had RGB LEDs that would be easy to light to do peaceful and aggressive modes.

FASTER

It still needs to autonomous, and the code for that would have to have randomized elements, and also sensors to prevent it from bumping into stuff, namely the ESP32-CAM. 