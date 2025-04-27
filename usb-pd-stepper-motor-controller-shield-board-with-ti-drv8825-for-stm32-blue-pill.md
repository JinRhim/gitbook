---
description: For stepper motor controller
---

# USB-PD Stepper Motor Controller Shield Board with TI DRV8825 for STM32 blue pill

The goal of this project is to make a stepper motor controller shield board so that you can easily plug in an STM32 and control the 12V stepper motor for further understanding of the stepper motor function.&#x20;

Chips to use \
\- AP33771C USB PD sink for receiving a 12V signal \
\- Buck converter for 12V --> 5V conversion \
\- STM32 30-pin mounting female socket \
\- TI DRV8825 for controlling stepper motor \


## USB-PD Sink Chip&#x20;

Instead of a 12V barrel jack, a USB-PD sink chip will be used, as USB-PD charging blocks for laptops have become common.\
For minimizing cost and simplist BOM, I will use simplist sink chip. Usually IP2721

