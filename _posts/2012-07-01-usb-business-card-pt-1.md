---
layout:     post
title:      "USB Business Card"
date:       2012-07-01 12:00:00
header-img: "img/header.png"
---

# Business Card Version 1

Link to the source code, schematic, layout, and BOM can be found at this project's Github page: [https://github.com/Carrigan/USBBusinessCard](https://github.com/Carrigan/USBBusinessCard).

## The Concept

A business card should represent a person’s expertise. My expertise is in creating unique circuit boards, so to celebrate this I wanted to create a USB thumb drive circuit that contains my resume and contact information. This was also a great way to experience coding for USB applications using the Cortex M series of microcontrollers. As a side note, I have done other projects with Cortex M including a simple serial relay and a USB keyboard emulator, but this is the first one that is exciting enough to write about.

## The Hardware

There are two main components that were required to make a USB Mass Storage Class device (MSC from here on) business card: a USB controller and some non-volatile memory. I had recently been playing with the LPC1343, a 48 pin IC with a built-in USB ROM library, and decided to use this for the microcontroller. Looking around the lab I also found an Atmel AT45DB series flash chip that would become the storage. I started prototyping this system on an an inexpensive Olimex Development Board, and when it worked I simply copied the relevant parts of its schematic and added the AT45DB and a USB plug. I also wanted a visual representation of what the board is doing, so there is a red and a green LED to depict read/write operations and a blue LED for when the USB connects successfully.

![USB Business Card Schematic](/img/card_schematic.png)

This circuit could be laid out in a very small area, but since this is for a business card I decided to make the board a little bigger so that I could silkscreen my name, website, email, and phone number on the front. Here is a picture of the final board layout, measuring in at 1.75″ by 1.35″.

![USB Business Card Layout](/img/card_layout.png)

The boards were sent to Seeed Studio for manufacturing, and here is the outcome. The one big disappointment was that they slapped the order number right next to my name, but I have to get a second batch made anyway due to one small error so I will make sure this doesn’t happen on the second set. You can see a small wire correcting the error I am talking about, which is that I never pulled up pin 0_1 which would cause the board to enter ISP mode upon booting up. The attached schematics and pictures above include the correction.

![USB Business Card - Produced](/img/card_produced.jpg)

## The Software

Luckily, the most difficult part of the software (the USB interface) is already done because of the choice of microcontroller. The only real work left is to create software that reads and writes to the AT45DB chip using an SPI interface. The USB library functions in ROM are very specific about what they need and what they will give. For the write function, the library passes you an offset, a length, and a block of data in a byte array. For the read function, the library passes an offset and a length, and expects a block of data back.
The first library written was a basic SPI library with just the basic functions such as initializing the SPI and reading or writing data. The AT45DB library built on top of this functionality by reading and writing the sequences that the flash memory was expecting. These functions were built around the USB library functions, and so they take in an offset, a length, and a pointer to some data.

Note: the code included does not include the Keil generated start-up code or the CMSIS LPC1343 library. To compile this code, you will need to make a Keil project with your own start-up code and then add these files.

# Results

While I wouldn’t want to use the final product as my main USB stick, it does exactly what it was meant to do- hold and transfer a small amount of data. The whopping 1MB of storage (maybe I should’ve just used a floppy drive?) allows for enough room to store my resume, a link to my site, as well as the source code for the whole project. The write speed is very slow due to the wait times between writes and the fact that USB MSC uses 512 byte sectors and the AT45DB is set up in 264 byte sectors, meaning each 512 byte write has to write to 2-3 AT45DB sectors, each of which requires a delay. The reading does not require any delays, however, and somebody opening files on the device will most likely find it is as quick as any other USB device.

![USB Business Card - Action Shot](/img/card_action.jpg)
