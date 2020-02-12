---
layout: page
title: Dynamixel AX12 and the Raspberry Pi
---

# Dynamixel AX12 and the Raspberry Pi 

<span>Tags:</span> [robotics](/Tags/robotics), [RaspberryPi](/Tags/RaspberryPi), [Dynamixel](/Tags/Dynamixel)

    How to control Robotis Dynamixel AX-12 from a Raspberry Pi.

The first step is to wire everything up, we use port 4(5V) and 6(GND) for power port 8(TXD) and 10(RXD) for serial RX and TX, and port 12(GPIO 18) for controlling the half duplex communication. For this we need an 74LS241, take a look at the setup:

[http://robottini.altervista.org/dynamixel-ax-12a-and-arduino-how-to-use-the-serial-port](http://robottini.altervista.org/dynamixel-ax-12a-and-arduino-how-to-use-the-serial-port)

or

[http://savageelectronics.blogspot.com/2011/01/arduino-y-dynamixel-ax-12.html](http://savageelectronics.blogspot.com/2011/01/arduino-y-dynamixel-ax-12.html)

We need a 74LS241 connected this way:

*   Pin 2 to Pin 3 (data out to AX-12)
*   Pin 1 to  Pin 19 (connected to RPi port 12 (GPIO 18))
*   Pin 18 (connected to RPi pin 10 RX)
*   Pin 17 (connected to RPi pin 8 TX)
*   Pin 10 to ground (RPi pin 6 GND)
*   Pin 20 to Vcc (5 Volt, RPi pin 4 5V)

Also, the AX12 needs 9-12 Volt

![](http://blobs.oppedijk.com/media/Default/Page/WP_000493.jpg)

## Configuring the Raspberry Pi for serial

Set configuration parameters in /boot/config.txt:  
init_uart_clock=16000000

sudo stty -F /dev/ttyAMA0 1000000

Edit /boot/cmdline.txt and remove all options mentioning ttyAMA0.  
Edit /etc/inittab and comment out any lines mentioning ttyAMA0, especially the getty one.

Reboot

(thanks to: [http://fw.hardijzer.nl/?p=167](http://fw.hardijzer.nl/?p=167))

## Programming

This is an example Python program, no wrapper for the AX12 exists? The values are hardcoded to move Dynamixel 1, Execute an action (5), send 3 bytes of data, MOVE command(1E) and 2 positions(32 03) followed by the CRC (A3). I needed a small sleep(0.1), the port.write takes more time and the output was already LOW.

import serial  
import time  
import RPi.GPIO as GPIO

GPIO.setmode(GPIO.BCM)  
GPIO.setup(18, GPIO.OUT)

port = serial.Serial("/dev/ttyAMA0", baudrate=1000000, timeout=3.0)

while True:  
        GPIO.output(18, GPIO.HIGH)  
        port.write(bytearray.fromhex("FF FF 01 05 03 1E 32 03 A3"))  
        time.sleep(0.1)  
        GPIO.output(18, GPIO.LOW)  
        time.sleep(3)

        GPIO.output(18,GPIO.HIGH)  
        port.write(bytearray.fromhex("FF FF 01 05 03 1E CD 00 0b"))  
        time.sleep(0.1)  
        GPIO.output(18,GPIO.LOW)  
        time.sleep(3)
