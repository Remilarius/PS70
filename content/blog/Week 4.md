---
title: "Week 4: Microcontroller Programming"
date: 2021-02-15
draft: false
---

## Piezobuzzer Circuit

I wanted to play some music with a piezo buzzer since I was given the basics in ES50, so I decided to map over knowledge from the class and use the materials from PS70 to make something more extensive.

I used the HUZZAH Feather board instead of the Metro Express or Arduino MKRZero and that required downloading the associated libaries and packets.

### TinkerCad Mockup w/Single pin

I built a quick circuit in TinkerCad to make sure the LED and Piezobuzzer would work as expected, the only modfication I would have to make to this would be to seperate the pin inputs for the LED and Piezo buzzer since they're controlled differently in the Buzzer code libraries.

![TinkerCAD](Week4/piezobuild.jpg)

### Build #1

I built the circuit on the ES50 Analog Discovery Studio since it was sturdier instead of just a floating bread board.

![Piezobuild](Week4/piezosetup.jpg)

### Build #2

This one was more compact since I tried to fit it on half a bread board after the accidently-melt-part-of-the-Huzzah incident but this had wires crossing which probably isn't good.

![Piezobuild 2](Week4/piezobuild2.jpg)

### Set up in Arduino IDE

Sean recommended that I download the Buzzer library that makes it easier to write tunes in Arduino. There was a minor noTone/Tone not defined error but going on the github page of the library, there was code that code fix it.

Add this code into the Buzzer.cpp of the library to fix the noTone/Tone undefined error.
{{< highlight html >}}

#define TONE_LEDC_CHANNEL (0)

void tone(uint8_t pin, unsigned int frequency)
{
    if (ledcRead(TONE_LEDC_CHANNEL))
    {
        Serial.printf("Tone channel %d is already in use\r\n", ledcRead(TONE_LEDC_CHANNEL));
        return;
    }

    ledcAttachPin(pin, TONE_LEDC_CHANNEL);
    ledcWriteTone(TONE_LEDC_CHANNEL, frequency);
}

void noTone(uint8_t pin)
{
    ledcDetachPin(pin);
    ledcWrite(TONE_LEDC_CHANNEL, 0);
}
{{< /highlight >}}

### Code for Fur Elise

Note: Pin 13 is for the piezobuzzer and pin 12 is for the LED.

{{< highlight html >}}
#include <Buzzer.h>

Buzzer buzzer(13, 12);

void setup() {
  // put your setup code here, to run once:
}

void loop() {
  int time = 500;

  buzzer.begin(10);

  buzzer.sound(NOTE_E7, time / 2);
  buzzer.sound(NOTE_DS7, time / 2);
  buzzer.sound(NOTE_E7, time / 2);
  buzzer.sound(NOTE_DS7, time / 2);
  buzzer.sound(NOTE_E7, time / 2);
  
  buzzer.sound(NOTE_B6, time / 2);
  buzzer.sound(NOTE_D7, time / 2);
  buzzer.sound(NOTE_C7, time / 2);
  buzzer.sound(NOTE_A6, time / 2);
  
  buzzer.sound(NOTE_C6, time / 2);
  buzzer.sound(NOTE_E6, time / 2);
  buzzer.sound(NOTE_A6, time / 2);
  buzzer.sound(NOTE_B6, time / 2);

  buzzer.sound(NOTE_E6, time / 2);
  buzzer.sound(NOTE_GS6, time / 2);
  buzzer.sound(NOTE_B6, time / 2);
  buzzer.sound(NOTE_C7, time);
  
  buzzer.sound(NOTE_E7, time / 2);
  buzzer.sound(NOTE_DS7, time / 2);
  buzzer.sound(NOTE_E7, time / 2);
  buzzer.sound(NOTE_DS7, time / 2);
  buzzer.sound(NOTE_E7, time / 2);

  buzzer.sound(NOTE_B6, time / 2 );
  buzzer.sound(NOTE_D7, time / 2);
  buzzer.sound(NOTE_C7, time / 2);
  buzzer.sound(NOTE_A6, time / 2 );

  buzzer.sound(NOTE_C6, time / 2);
  buzzer.sound(NOTE_E6, time / 2);
  buzzer.sound(NOTE_A6, time / 2);
  buzzer.sound(NOTE_B6, time / 2);

  buzzer.sound(NOTE_E6, time / 2);
  buzzer.sound(NOTE_C7, time / 2);
  buzzer.sound(NOTE_B6, time / 2);
  buzzer.sound(NOTE_A6, time);

  buzzer.sound(NOTE_B6, time / 2);
  buzzer.sound(NOTE_C7, time / 2);
  buzzer.sound(NOTE_D7, time / 2);
  buzzer.sound(NOTE_E7, time / 2);

  buzzer.sound(NOTE_G6, time / 2);
  buzzer.sound(NOTE_F7, time / 2);
  buzzer.sound(NOTE_E7, time / 2);
  buzzer.sound(NOTE_D7, time);

  buzzer.sound(NOTE_F6, time / 2);
  buzzer.sound(NOTE_E7, time / 2);
  buzzer.sound(NOTE_D7, time / 2);
  buzzer.sound(NOTE_C7, time / 2);

  buzzer.sound(NOTE_E6, time / 2);
  buzzer.sound(NOTE_D7, time / 2);
  buzzer.sound(NOTE_C7, time / 2);
  buzzer.sound(NOTE_E6, time / 2);
{{< /highlight >}}


{{< youtube gRDhW-fkg3Y >}}