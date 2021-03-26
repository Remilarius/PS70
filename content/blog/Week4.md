---
title: "Week 4: Microcontroller Programming"
date: 2021-02-15
draft: true
---

## Piezobuzzer Circuit

I wanted to play some music with a piezo buzzer since I was given the basics in ES50, so I decided to map over knowledge from the class and use the materials from PS70 to make something more extensive.

I used the HUZZAH Feather board instead of the Metro Express or Arduino MKRZero and that required downloading the associated libaries and packets.

### TinkerCad Mockup w/Single pin

I built a quick circuit in TinkerCad to make sure the LED and Piezobuzzer would work as expected, the only modfication I would have to make to this would be to seperate the pin inputs for the LED and Piezo buzzer since they're controlled differently in the Buzzer code libraries.

![TinkerCad Piezobuzzer](piezobuzzercircuit.png)

### Actual Set-up

I built the circuit on the ES50 Analog Discovery Studio since it was sturdier instead of just a floating bread board.

![Piezobuild](piezosetup.jpg)

### Arduino IDE

Sean recommended that I download the Buzzer library that makes it easier to write tunes in Arduino and I hadn't been able to get it to work because of bugs but I imagine it'd be figured out in a bit.

![Buzzer Code](buzzercodeexp1.png)


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