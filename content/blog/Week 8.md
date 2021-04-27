---
title: "Week 8: Radio Communication"
date: 2021-03-21
draft: false
---

![FTDI programmer](Week8/FDTI.jpg)
The whole process of getting the ESP32 Cam working started out with a hiccup. I couldn't find board that connected the ESP32CAM to my computer for the longest time because it turned out that I had a different board form everyone else. It didn't make much of a difference because nothing worked after I hooked it up.
![Setup](Week8/Hookup.jpg)

I tried to upload the Blink code to the board as a test but I got an LED_BUILTIN error. Naming the LED would fix that error  

Putting either line into the code  
"#define LED_BUILTIN 33" which is the name of the LED on the ESP32CAM board.  
but the Arduino IDE instead spit out another error.  
![ESP32CAM ERROR](Week8/esp32camerror.jpg)

I got advice from Sam and Eric and Sean about how to get it working during lab but still could not upload code to it.

I tried pushing the RST button quite aggressively as Arduino IDE was saying "Connecting..." but no luck.

Notes:
I need to push the RST button as code is being uploaded to get the board to connect.  
GND and IO0 need to be connected on the CAM.  
Procedure once the board starts working:  
Upload code and disconnected GND and IO0 and press RST.  
Wire a capacitor to the ESP32 CAM?  

Things to try:  
Power the ESP32 with 5V? 

It looks like the FDTI was only sending 3.4V

![multimeterreading](Week8/3.4reading.jpg)
![Normal Wire](Week8/Normalwire.jpg)
![5V Wiring](Week8/5VtoGND.jpg)


At least I could get the HUZZAH MAC address..

![MACaddress](Week8/macaddress.jpg)
