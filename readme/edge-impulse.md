---
description: machine learning in microcontroller
---

# Edge Impulse

Edge impulse is probably the most easiest and simplest way to implement machine learning to the microcontroller. With several button clicks, you can have your own tiny machine learning running on the microcontroller.&#x20;

It is marvelous how Edge Impulse manages to implement object detection in a tiny microcontroller with less than 1MB SRM. If you download the tensorflow model from tensorflow zoo and train with your dataset, the compressed tensorflow lite model will be 3 MB which is impossible to run in the microcontroller. Within the first 4 layers of the convolution operation, the tensorArena memory will run out.&#x20;



## Arduino Nano BLE 33 Sense: Voice Recognition through Edge Impulse&#x20;

Arduino Nano BLE 33 sense is a tiny microcontroller that has an onboard microphone that can be used for simple voice recognition. However, it is possible to connect the OV7670 camera and use it for computer vision projects too.&#x20;

<figure><img src="../.gitbook/assets/IMG_2138 2 Large.jpeg" alt=""><figcaption><p>BLE33 + OV7670</p></figcaption></figure>



The first thing to do is create [Edge Impulse](https://studio.edgeimpulse.com/login) Account. Do not use a Google account as it might cause an error while logging into the account in terminal.

* create new project --> name the new project --> select \[Audio] for voice recognition

<figure><img src="../.gitbook/assets/Screenshot 2023-08-27 at 2.06.24 PM.png" alt=""><figcaption></figcaption></figure>

In the Mac Terminal,&#x20;

```
$ brew update
$ brew install node 
$ brew install python3 
$ pm install -g edge-impulse-cli --force
$ brew install arduino-cli 
```

You probably have node and python3 already

Download [Nano 33 BLE Sense Edge Impulse Firmware](https://www.google.com/url?q=https://cdn.edgeimpulse.com/firmware/arduino-nano-33-ble-sense.zip\&sa=D\&source=editors\&ust=1693173294541943\&usg=AOvVaw0j2Pt7cyz48l72BZ05oKzM) and run flash\_mac.command&#x20;

<figure><img src="../.gitbook/assets/Screenshot 2023-08-27 at 1.57.13 PM.png" alt=""><figcaption></figcaption></figure>

&#x20;After flashing in the BLE33 Sense, go to the terminal and type&#x20;

```
$ edge-impulse-daemon 
```

* Type your Edge Impulse ID and Password&#x20;
* Select the project from the edge impulse&#x20;
* name the device&#x20;

To change the project of connected device,&#x20;

```
$ edge-impulse-daemon --clean
```

Now, the microcontroller is connected to the Edge Impulse. In your project, you should see the connected device

<figure><img src="../.gitbook/assets/Screenshot 2023-08-27 at 2.11.16 PM.png" alt=""><figcaption></figcaption></figure>

### Collecting Data in Edge Impulse&#x20;

* In the "record new data", select the sensor as "Built-in microphone".&#x20;
* Select the length as 3000 ms or the length of the word you want to record
* Press start sampling
* Label the recording. In my case, I recorded "yes" or "no"

<figure><img src="../.gitbook/assets/Screenshot 2023-08-27 at 2.24.55 PM.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/Screenshot 2023-08-27 at 2.26.42 PM.png" alt=""><figcaption><p>select processing block</p></figcaption></figure>

### Start Training

<figure><img src="../.gitbook/assets/Screenshot 2023-08-27 at 2.27.53 PM.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/Screenshot 2023-08-27 at 2.29.32 PM.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/Screenshot 2023-08-27 at 2.33.48 PM.png" alt=""><figcaption></figcaption></figure>

Unfortunately, for this voice recognition project, I did not leave much documentation because I had to focus on Computer Vision.&#x20;



However, in the same manner, you can connect OV7670 and train a simple model with an input size of 32X32 RGB&#x20;







