---
layout:     post
title:      "Coffeebot"
subtitle:   "An IoT Device For Keeping Coffee Fresh"
date:       2017-02-14 12:00:00
header-img: "img/header.png"
---

[Video of Coffeebot In Action](https://vimeo.com/204036166)

Imagine this tragedy: you get into work in the morning and go to get a cup of coffee, only to finally sip it and find that it is yesterday’s luke-warm spoils. A simple paper solution for this would be to add a post-it to the coffee with the time that it was brewed, but this requires buy-in from the team. A digital solution for this would be an internet connected button that alerts the office when the coffee was brewed, but this does not help people who are physically at the machine. We set out to build a hybrid of these two ideas; a device that would both give immediate information at the coffee pot and also have a digital alert system for anyone waiting for a new pot to be brewed. We call our creation Coffeebot.

## Take One

For our first take on this project, we wanted to use our LABS TIME to build a minimum viable product in a few hours or less. Luckily, we had a PARTICLE board in the office which allowed us to move quickly and have both a physical product as well as an internet connected tie-in. Coffeebot’s entire user interface is a screen and two buttons; whenever a new pot of coffee is made, the brewer simply presses a button that corresponds to that type of coffee.

![](/img/coffeebot_prototype.jpg)

When a button is pressed, it restarts the timer that is shown on the screen and uses IFTTT to post a message to our company’s Slack app. The first iteration was not pretty or polished, but it was adopted immediately thanks to how easy and friendly it was to use.

![](/img/coffeebot_slack.png)

## Refining the Design

The Coffeebot prototype quickly became one of the most used Labs projects in the office. There was only one problem: the mess of prototype wires was unruly and prone to disconnecting. We decided to solve this using our 3D printer and a custom Printed Circuit Board (PCB). We started by designing a simple PCB that would hold the Particle, a couple of buttons, and the LCD. We got 3 copies of this made at OSHPARK for $30.

![](/img/coffeebot_board.jpg)

Next, we designed and 3D printed an enclosure that would hold the finished board. One cool feature of this enclosure is that the buttons are cut out of the case itself. The entire enclosure can be printed as a single piece and can press the buttons on the PCB without needing any additional parts.

![](/img/coffeebot_finished.jpg)

While we use this project for tracking coffee, the general concept of an internet-connected screen with four buttons can be used for all kinds of things: a time tracker for work, a Slack responder, or even an automatic Pizza ordering system. If you have any questions feel free to drop them in the Github issue tracker. Happy brewing!

_Originally posted at the Smashing Boxes' blog: [link](https://smashingboxes.com/blog/introducing-coffeebot-slack-powered-alert-system-office-coffee-pot/)_