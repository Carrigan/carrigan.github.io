---
layout:     post
title:      "AWS IoT Gateway Tutorial"
subtitle:   "Part 1: Defining Our Problem and Approach"
date:       2017-10-24 12:00:00
header-img: "img/header.png"
---

I've recently been exploring the AWS IoT platform more and more and think that it can be very valuable in a lot of applications that use connected devices. It solves device authentication (via the device registry), being able to easily use the incoming data (via the rules engine and lambdas), and synchronization issues that may arise due to connectivity (via the device shadow). There is no silver bullet for the IoT since there are an endless number of use cases with different data rates, connectivity schemes, latency requirements, and processing capabilities, but AWS IoT seems like a solid, extensible tool to know for the right use cases.

Unfortunately, many of the examples of AWS IoT have focused on applications where every device is directly connected to the cloud. In many of the application I have worked on, this is not the case; often there is a gateway that has internet connectivity and a host of sensors or actuators that talk to that gateway via some other wireless protocol like 802.15.4, LoRa, Bluetooth, or others. So, I set out to explore making such an application using a Raspberry Pi and some much smaller chips that all talk via XBee.

This series will detail my journey in making a simple IoT gateway for a specific application. It is meant for people who are familiar with Arduino and Raspberry Pi and have considered making the jump to a connected solution using AWS IoT. It will also help if you are familiar with the general concepts used in AWS IoT such as device shadows, but we will cover them more in depth later in the tutorial. For now, let me introduce you to the problem we will be tackling.

## Introducing the HabitSwitch

Once upon a time, I was a very avid guitar and bass player. When I went away to college I stopped teaching lessons and playing with groups, causing me to play much less frequently and fall out of practice. Years later, I miss playing with a band and being around other musicians though, so I have started to practice guitar, bass, and even singing much more. What I would love is for a tool that can remind me to make time for guitar and make it easy to track how much time I am playing for.

So to explore AWS IoT, lets talk about a system that tracks how much practice time I am putting in on guitar and bass. This system will automatically log time I spend playing these instruments by using switches placed in the guitar holders. It will also have an LED device that visually outputs if I am meeting my practice goals. This system will go in my office where it can hopefully change behavior when I am making decisions about how to spend leisure time.

While I will be using this system for playing guitar, it would be easy to adapt it to just about any habit you would like to adopt (or break). Looking to go to the gym or run more? Place a switch under your gym shoes and it will track how often they are being used. Want to watch less TV? Place a switch were you keep your remote. Any time-based goal can be tracked easily like this.

Without further ado, lets jump into how to build this system with the help of AWS IoT.

## System Architecture

Imagine this hell: you see this system on sale and want to use it, and then have to setup every switch or LED to connect to your home WiFi. Each one starts by broadcasting an SSID that you have to connect to. You then have to open some special webpage (or heaven forbid download an app just for this) and configure it to connect to your network. You now have an additional 5 devices on your network so that they can send a couple of data packets per day. These devices are either not able to be battery powered, or go through their batteries very quickly since connecting to the WiFi every time they turn on uses extensive battery power. These devices will ideally have a good amount of processing since they will be interacting with JSON packets directly, so microcontrollers are not an ideal fit.

Now instead imagine this: you configure just the gateway using this process, and then all of the other devices connect to the gateway using some local wireless protocol. When these devices wake up, they can shoot off a packet without needing to negotiate a network connection and fall back asleep easily. For the switches in this application, they can go into a deep sleep mode until an event occurs again, allowing for a single battery to last a long time. These sensors can keep their packets in efficient, packed structures for the gateway to handle; an easy task for a microcontroller to handle.

To me, a gateway based application is a clear winner here. Since this is a one-off and not a commercial product, we will also be cutting some corners during provisioning and configuring the WiFi and wireless chips beforehand. I've chosen XBee for this application mainly because I own a couple and they are easy to get up and running. For a primer on building sensor networks with XBee, you can check out [Building Wireless Sensor Networks](https://www.amazon.com/Building-Wireless-Sensor-Networks-Processing/dp/0596807732/) by Robert Faludi.

## Integration with AWS IoT

It is best to go in with a plan for how we are going to use AWS IoT to make this application work. The following shows the different components for this system and how they will interact:

- Sensors will send their info to the gateway, which will simply relay it to AWS. The gateway will post a message to a device topic, such as `sensors/gibson` with only one field: whether the switch became opened or closed. For our application, we will look at the total amount of time that the sensor spent in the `open` position. We will set up a rule that posts this data to a DynamoDB database for processing later.
- The gateway will register a device shadow for the LED device and subscribe to it. This is crucial: even though the LED device is not directly connected to AWS IoT, applications can still interact with it through the gateway in a natural way.
- A lambda will run periodically and calculate the amount of time accumulated by the sensors over a certain period by looking at data from the DynamoDB database. It will then change the shadow state of the LED device to reflect the state of the goal.

---

This sounds pretty simple, but there is quite a bit of work ahead. Time for me to order some limit switches and another XBee chip. Tune in next week for getting our Raspberry Pi connected to AWS IoT!


