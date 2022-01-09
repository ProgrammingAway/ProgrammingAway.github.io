---
title: "Garage Door Controller"
date: 2021-12-08T16:39:10-06:00
draft: false
omit_header_text: true
tags:
- Programming
- IOT
- Python
- Javascript
featured_image: "/images/posts/real-estate-gada567e92_1920.jpg"
disable_share: true
---

{{< figure src="/images/posts/real-estate-gada567e92_1920.jpg" >}}

I have done many attempts to control my garage door with my phone over the years. I first attempted to control the garage door using sensors and relays and a Raspberry Pi. I loosely followed a guide for a project called [IoT Gateway](https://lowpowerlab.com/guide/gateway/) from a site called Low Power Lab. That introduced me to the programming language NodeJS. I learned a lot from this project, which involved programming, circuitry, and controlling the circuits from the program. I got the project to work, however, it wasn’t long before I learned the Raspberry Pi wasn’t built for my garage environment.

I then moved to [NexxGarage](https://getnexx.com/products/nexx-garage) for my connected garage opener. This was an off the shelf solution so it used someone else’s hardware and software.  It served its purpose for a while, but unfortunately it also used NexxGarage’s servers for their service. Something as simple as a garage opener shouldn’t need to connect to another company’s servers. I started learning about ESP8266 and how that can withstand the temperature range of my garage. (I later learned the NexxGarage contains an ESP8266). So I bought a [NodeMCU](http://www.nodemcu.com/index_en.html) and interfaced it with the previous circuits I used with my Raspberry Pi. I then dusted off my old C language skills and created an Arduino sketch based off of a project from [Self Hosted Home](https://selfhostedhome.com/) (although now he uses MicroPython).

The NodeMCU with the arduino sketch worked well for a while.  But in my continuing research, I learned about [ESPhome](https://esphome.io/) which is custom firmware for ESP8266/NodeMCU that can connect directly with [Home Assistant](https://www.home-assistant.io/).  Since I use Home Assistant for my home automation, I decided to give it a try. After customizing some yaml files, this solution was simple and has worked great for quite some time.

However, after my latest update to Home Assistant messed up my connection to some other devices, I am now interested in having alternative ways of controlling my devices. I have a MQTT server and could just add MQTT functionality to my ESPhome firmware, but I am thinking of using something not specifically built for Home Assistant. The hardware I have is tried and tested, I just need a new firmware to control that hardware. That leads me back to C/C++ using Arduino, or try something new like [MicroPython](https://micropython.org/). The easiest method would be to use my old sketch and be done with it. However, I seem to make things more difficult for myself, so I am going to try something new. I am already familiar with Python, so using it on a micro-controller should be a piece of cake, right?

Well, we shall find out. I will document my progress on this site, but if you have any questions or comments about my journey so far, feel free to leave it in the comments below.

 

Brian