# Making Battery Pack

To make a battery pack to power my senior design project which includes 4 7.5V servos, ESP32, and external battery, I planned to make a battery pack using 18650.

<figure><img src="../.gitbook/assets/697_FinalPresentation.jpg" alt=""><figcaption><p>Abstract Concept for Project </p></figcaption></figure>

The battery pack will be used to supply power to following components (some of them are not included in the picture):&#x20;

1. Four FT6335 Servo @7.5V&#x20;
2. L7805 that will convert 7.5V to 5V&#x20;
3. ESP32 that will be using Wireless communication
4. 4 0.96 inch SSD1306 OLED Display (This is not shown in picture)
5. ADS1115 external 16-bit adc&#x20;
6. Two potentiometer that will determine EMG Threshold and Distance Threshold.



## Ordering 18650&#x20;

Do not order 18650 from Amazon, as most of them are either defective or have incorrect amount of battery capacitance. (I realized this after ordering 10 of them from Amazon) Those batteries will refuse to work after 1 week.

I ordered the official LG INR18650 MJ1 3500mAh Battery from 18650 Battery Store.

<figure><img src="../.gitbook/assets/Screenshot 2023-11-02 at 4.58.22 PM (1).png" alt=""><figcaption></figcaption></figure>

According to the Datasheet,&#x20;

* Max Charge Voltage = 4.2V±0.05V&#x20;
* End Voltage = 2.5V&#x20;
* Standard Discharge = 0.2C (3500mAh X 0.2C = 680mA)
* Max Discharge = 10A (INR = High Discharge)

Compared to Samsung 30Q which can discharge maximum of 20A and continuously discharge 15A, LG MJ1 discharge rate might be seen as slower, but way cheaper.&#x20;

<div>

<figure><img src="../.gitbook/assets/IMG_1905 Large (1).jpeg" alt=""><figcaption></figcaption></figure>

 

<figure><img src="../.gitbook/assets/IMG_1902 Large (1).jpeg" alt=""><figcaption></figcaption></figure>

</div>

All of the Batteries at the arrival were charged at 3.6V.

## Ordering BMS Module&#x20;

BMS stands for Battery Management System. For 1s configuration one of the most widely used chip is tp4056 which can both charge and discharge.

However, TP5100 which supports 2s configuration can only charge the 2s battery and does not provide regulated output voltage.&#x20;

Eventually I ended up buying a 2s charge BMS module from Amazon.

<figure><img src="../.gitbook/assets/Screenshot 2023-11-02 at 6.06.48 PM.png" alt="" width="168"><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/IMG_2263 Large.jpeg" alt=""><figcaption></figcaption></figure>

I found the [datasheet](https://seegatecell.com/wp-content/uploads/2021/01/2S-7.4V-4A-DATASHEET-2.pdf) for the model that is similar to this one.&#x20;

* Max continuous Charging Current: 4A&#x20;
* Max continuous Discharging Current: 4A&#x20;
* Overcharge detection voltage: 4.28V&#x20;
* Overdischarge release voltage: 3.0V&#x20;

There is no inductor so I guess there is not buck-boost converter involved. The circuit simply charges and discharges the battery.&#x20;



## Making Battery Pack&#x20;

Out of 8 Batteries I will be using 2 batteries for powering Google Coral Dev Board with Extension board and 6 batteries for controlling servos and ESP32.&#x20;

<div>

<figure><img src="../.gitbook/assets/IMG_2033 Large.jpeg" alt=""><figcaption><p>2s3p battery pack </p></figcaption></figure>

 

<figure><img src="../.gitbook/assets/IMG_2032 Large.jpeg" alt=""><figcaption><p>Charging battery pack @9V </p></figcaption></figure>

</div>

The batteries are connected in 2s3p configuration. To activate the charging mode the battery output should be connected to 9V.&#x20;

(I wonder whether charging the battery through power supply is the reason why the power supply broke after 1 month - maybe I should've included diode when I was charging )

<div>

<figure><img src="../.gitbook/assets/IMG_2261 Large.jpeg" alt=""><figcaption><p>Output </p></figcaption></figure>

 

<figure><img src="../.gitbook/assets/IMG_2259 Large.jpeg" alt=""><figcaption><p>charging </p></figcaption></figure>

</div>

As you can see, output is just the single cell charge value multiplied by two.&#x20;

