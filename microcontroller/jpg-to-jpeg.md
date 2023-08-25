---
description: Arduino BLE33 with OV7670
---

# JPG to JPEG

<figure><img src="../.gitbook/assets/IMG_1754.jpg" alt=""><figcaption><p>Arduino BLE 33 with OV7670</p></figcaption></figure>

At first, I was surprised at how many GPIO pins are required to connect camera and whether it is possible for Cortex M4 microcontroller to manage all the GPIO input/output at such high frequency while outputting all the pixels values through UART port with 256 kB memory.



The BLE 33 sends all of the uncompressed, raw pixel value through the uart usb cable. Thankfully, there is RGB 565/888 processing [program](https://processing.org/download) that can visualize hex value in UART communication in real-time video streaming, made by MIT graduate student.

<div>

<figure><img src="../.gitbook/assets/1.1_.png" alt=""><figcaption></figcaption></figure>

 

<figure><img src="../.gitbook/assets/1.1_ (1).png" alt=""><figcaption><p>my lab</p></figcaption></figure>

</div>

Every time BLE33 captures image, it needs to send 320X240X1 pixels of information through the UART communication.&#x20;

Each pixels contain RGB565.&#x20;

* Red - 5 bit&#x20;
* Green - 6 bit&#x20;
* Blue - 5 bit&#x20;

0b.RRRR.RGGG.GGGB.BBBB;&#x20;

which is total of 16 bit = 2 bytes.

So, if the video is 5 fps, the UART needs to send 320X240X16X5 = 6,144,000 zeros and ones every 1 seconds.

If you turn on the Serial monitor, it will constantly lag then crash due to sheer number of bytes being sent from BLE33.

<figure><img src="../.gitbook/assets/Screen Shot 2023-08-25 at 1.01.18 AM.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/1.1_ (2) (1).png" alt=""><figcaption><p>crashing serial monitor</p></figcaption></figure>

<figure><img src="../.gitbook/assets/1.1_.gif" alt=""><figcaption></figcaption></figure>

The easiest attempt to alleviate such bottleneck is to lower the resolution. Ov7760 Camera driver shows different resolution options

<figure><img src="../.gitbook/assets/1.1_ (3).png" alt=""><figcaption><p>OV7760 Camera driver</p></figcaption></figure>

<figure><img src="../.gitbook/assets/Screen Shot 2023-08-25 at 1.04.12 AM.png" alt=""><figcaption><p>OV7760 output different resolution</p></figcaption></figure>



&#x20;





