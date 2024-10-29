# EMG-CV fusion Exoglove Control

## Sony Spresense Competition

In 2022 Summer, I had an amazing internship experience with Zhenyu and Jimmy for deploying the Tensorflow Lite model in a Sony Spresense Microcontroller.  First, the tensorflow model was trained based on Ninapro Database 5 and was fine-tuned based on Jimmy's EMG signal (he had the most muscle). With index motion of 0, 1, 2, and 3 which are for thumbs up, releasing, grabbing, and O.K sign, we were able to deploy a quantized model in the Sony Spresense board.&#x20;

However, we had to use ESP32 just for the sole purpose of wireless communication. ESP32 only receives the Bluetooth information and sends UART information to the Sony Spresense board.

<figure><img src=".gitbook/assets/ENGR696 Presentation.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/ENGR696 Presentation (1).jpg" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/My-Movie.gif" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/JimmyDemo2.gif" alt=""><figcaption></figcaption></figure>

That is how we got the free PS5 by winning the Sony Spresense competition. However, the goal was to design a portable exoglove that could be fused with growing computer vision system.&#x20;

## Computer Vision&#x20;

Thanks to the Professor Zhuwei Qin who bought different microcontrollers, I was able to test commonly used microcontrollers other than Raspberry Pi&#x20;

<figure><img src=".gitbook/assets/IMG_2142 Large.jpeg" alt=""><figcaption></figcaption></figure>

I wanted to use Sony Spresense for Computer Vision and calculating the distance, but I was only able to use Sony Spresense with EdgeImpulse which yielded really low fps. For Nvidia Jetson 4GB version, the Jetpack was outdated and was stuck at ubuntu 18.04. Although some Pytorch projects from 2019 did not caused the dependency issue, I wanted to test multiple different models.&#x20;

Eventually I ended up using OpenMV H7 for small, compact size setup and Google Coral Dev board for running tensorflow model in high FPS.&#x20;



The goal of those microcontrollers are to calculate the distance between hand model (which is the prototype of the wearable exoglove) and grabbable object and send this filtered distance information (likely to be uint8) to the ESP32 which will receive distance information while processing EMG signals.

The dataset was made by Microsoft VoTT which I wrote in [https://app.gitbook.com/o/sNvhSOVZyDyTTrC5zdxK/s/cv8SWQoAK05Ec2tRdE9v/\~/changes/116/readme/how-to-make-ml-dataset-using-microsoft-vott](machine-learning/how-to-make-ml-dataset-using-microsoft-vott.md)

### OpenMV H7: MQTT communication

I set up the MQTT communication by setting my macbook as broker. Just in case if I have to come back here and remeber what MQTT commnads are:

* Turn on local MQTT server:&#x20;

```
/usr/local/opt/mosquitto/sbin/mosquitto -c /usr/local/etc/mosquitto/mosquitto.conf
```

* Turn on another termial and start:&#x20;

```
mosquitto_sub -h test.mosquitto.org -t "openmv/test" -v
```

* To turn off,&#x20;

```
/opt/homebrew/opt/mosquitto/sbin/mosquitto -c /opt/homebrew/etc/mosquitto/mosquitto.conf
```

Initially, the goal was to save average distance in array of length of 5, remove outlier, and send the average distance value to MQTT communication.&#x20;

<figure><img src=".gitbook/assets/openmvide_v1.gif" alt=""><figcaption></figcaption></figure>

However, in some parts of university, the MQTT communication did not work. To make the system as concise as possible, I was going to make a simple OpenMV H7 extension board with nRF52860, but it took too long to figure out the configuration. Instead, I made a simple shield with ESP8266 using ESP-one. ESP-one is for short-distance communication between two ESP8266 microcontrollers.&#x20;

<figure><img src=".gitbook/assets/IMG_2337 Large.jpeg" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/IMG_2039 Large.jpeg" alt=""><figcaption></figcaption></figure>
