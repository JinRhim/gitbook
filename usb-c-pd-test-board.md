---
description: No STUUSB4500, no I2C programming, only resistor selection
---

# USB-C PD Test Board

<figure><img src=".gitbook/assets/스크린샷 2025-11-16 192655.png" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/스크린샷 2025-11-16 192751.png" alt=""><figcaption></figcaption></figure>

\
The original goal was to make an AP33771C board for implementing 12V power supply so I could only use my USB-C charger for my all other projects.\
\
However, JLCpcb ran out of TI CSD nmos and numerous other molex connectors + Trump Tariff. \
\
To change this, I change from AP33771C to IP2721 which I commonly saw in some Chinese PCB. \
\


<figure><img src=".gitbook/assets/image (62).png" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/unnamed (1) (1).jpeg" alt=""><figcaption></figcaption></figure>

Two Issues&#x20;

* Sometimes Buck converter output 2.5V --> maybe need soft start / put UVLO resistor&#x20;
* Outputs 15.9V? (I shouldve paid more attention to the silkscreen) &#x20;
* Higher resistor value for current reader - current reading is off

\






