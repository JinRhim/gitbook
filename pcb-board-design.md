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



### Calculating EN Voltage Divider Value&#x20;

In order to use a smaller Inductor and capacitor for 0.5A application, I need to calculate R4, R5, L1 values

<figure><img src=".gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

* I<sub>p</sub> = 0.7 µA
* I<sub>h</sub> = 1.55 µA
* V<sub>EN\_falling</sub> = 1.19 V
* V<sub>EN\_rising</sub> = 1.22 V
* In my case, Vstart = 8.4V (4.2V\*2) and Vstop = 6.4V (3.2V\*2), as 2S 18650 BMS module cutoff voltage is set to 3.2V for battery life&#x20;

<figure><img src=".gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

The calculated R4 Value is around 1.17MΩ

<figure><img src=".gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

R5 = (1.14435\*10^6 \*1.19)/(6.4V - 1.19V+1.14435\*10^6\*(0.7+1.55)\*10^-6)&#x20;

R5 = 175kΩ

### Calculating Inductor Value&#x20;

<figure><img src=".gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

* Vout = 5V&#x20;
* Vinmax = 8.4V&#x20;
* Kind = 0.3 (using Murata low ESR capacitor)
* Iout = 0.5A&#x20;
* Switching Freq = 500kHz&#x20;

Lmin = 5\*(8.4-5)/(8.&#x34;_&#x30;\*0.3\*0.5\*500\*_&#x31;0^3) = 2.7\*10^-6 = 27uH&#x20;

<figure><img src=".gitbook/assets/image (46).png" alt=""><figcaption></figcaption></figure>

(5\*(8.4-5)/(8.4&#x30;_&#x32;.&#x37;_&#x31;0^-&#x36;_&#x35;0&#x30;_&#x31;0^3\*0.8))^2 = 3.5114&#x20;

RMS Inductor Current = sqrt(0.5^2+1/12\*3.5114) = 736mA&#x20;

<figure><img src=".gitbook/assets/image (47).png" alt=""><figcaption></figcaption></figure>

Peak Inductor Current = 0.736 + {5V\*(8.4V-5V)}/{1.6\*8.4\*27\*10^-6\*500\*10^3}&#x20;

I\_peak = 829mA&#x20;

For the output capacitor, I will just use 100uF 0805 X7R capacitors @25V lying around labs, parallel with a 100nF capacitor for higher frequency filtering and reducing ESR.&#x20;





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
