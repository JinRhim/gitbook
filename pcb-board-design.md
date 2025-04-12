# PCB Board Design

## TPS54202 Step-down Converter&#x20;

* Min 4.5V \~ 28V Input&#x20;
* Output 5V/3.3V 2A&#x20;
* SOT-23 Package --> Compact PCB Design with only 6 pins, which will be easy to solder&#x20;

I was thinking about using this converter for powering an 8.4V servo while powering the ESP32. The power source will be 2S 18650 battery where the voltage will be between 8.4V to 6.4V. With ESP32 WIFI + BLE + ADC and with two SSD1306 display, I estimate the current consumption to be around 250mA + 25mA\*2 to be around 300mA. However, there might be some current spike so I just assume that it is going to draw 500mA.&#x20;

Looking at the Texas Instrument datasheet, it gives the reference guide for designing.&#x20;

<figure><img src=".gitbook/assets/image (45).png" alt=""><figcaption><p>Texas Instrument TPS54202 5V 2A reference design</p></figcaption></figure>

There are typical buck converter components where&#x20;

* Input capacitor&#x20;
* Voltage divider for setting the input voltage&#x20;
* Bootstrapping Capacitor for turning on the MOSFET&#x20;
* LC filter for PWM&#x20;
* feedback loop

In order to use a smaller Inductor and capacitor for 0.5A application, I need to calculate R4, R5, L1 values









## LM21215 Buck Converter Design&#x20;

<figure><img src=".gitbook/assets/Screenshot 2024-11-26 at 5.27.24 PM.png" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/LM21215_sch.png" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/IMG_3111 Large.jpeg" alt=""><figcaption></figcaption></figure>

## STM32-based 12V to 110V Inverter&#x20;

* This inverter is made with TI UCC27714 and leftover components from the research lab

<figure><img src=".gitbook/assets/inverter_brd (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/inverter_sch (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/IMG_3112 Large.jpeg" alt=""><figcaption></figcaption></figure>



<figure><img src=".gitbook/assets/IMG_3135 Large.jpeg" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/IMG_3133 Large.jpeg" alt=""><figcaption></figcaption></figure>
