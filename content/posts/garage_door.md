---
title: "Garage_door"
date: 2021-12-14T21:07:52-06:00
draft: true
---

I have done many attempts to control my garage door over the years.  I first 
attempted to control the garage door and sensors using a Raspberry Pi with some 
glue logic hooked up to the GPIO pins.  I loosely followed a guide for a project 
called [IoT Gateway](https://lowpowerlab.com/guide/gateway/) from a site called 
Low Power Lab.  That introduced me to the programming language NodeJS.  I 
learned a lot from this project, which involved programming, circuitry, and 
controlling the circuits from the program.  I got the project to work, however, 
it wasn't long before I learned the Raspberry Pi wouldn't last too long in the 
evnironment of my garage.  I later moved to NexxGarage for my garage opener. 
It served it's purpose for a while, but I still didn't like how NexxGarage 
connected to other servers.  Something as simple as a garage opener shouldn't 
connect to another company's servers.  I started learning about ESP8266 and how 
that would withstand the temperature range of my garage. (I later 
learned the NexxGarage contains an ESP8266).  So I bought a NodeMCU and 
interfaced it with the previous circuits I used with my Pi.  I then dusted off 
my old C language skills and created an Arduino sketch based off of 
https://selfhostedhome.com.  (Although now he uses MicroPython).  Then since I 
used HomeAssistant for my home automation, I was able to handle the 
functionality using the ESPhome firmware (used specifically for HomeAssistant) 
and customized yaml files.  This has worked great for the longest time, but now 
I am interested in taking out the middle-man (HomeAssistant) so I can still 
control my garage even if Home Assistant is down indefinately after the latest 
update.  As for the hardware, the NodeMCU and extra circuitry are tried and 
tested.  I just need a new firmware to control that hardware.  That leads me 
back to C/C++ using Arduino, or try something new like MicroPython.  The easiest 
method would be to use my old sketch and be done with it.  However, I seem to 
make things more difficult for myself, so I am going to try something new.  I am 
already familiar with Python, so using it on a microcontroller should be a piece 
of cake.


