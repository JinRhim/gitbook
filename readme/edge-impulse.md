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



However, in the same manner, you can connect OV7670 and train a simple model with an input size of 32X32 RGB. However, In order to capture image it will take around 2 seconds.&#x20;

<figure><img src="../.gitbook/assets/Untitled2 Large.jpeg" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/ble33.gif" alt=""><figcaption><p>images shown through Ipad</p></figcaption></figure>

The inference time is 113ms, but acturally the time BLE33 take to process the image takes significant portion

<figure><img src="../.gitbook/assets/Screenshot 2023-08-27 at 3.19.10 PM.png" alt=""><figcaption></figcaption></figure>

It is better to use other powerful microcontrollers that have dedicate Camera ports and processors.&#x20;



## Sony Spresense Board: image inference with Edge Impulse&#x20;

Designed for Machine Learning, Sony Spresense board is one of the alternative board you could choose for computer vision. &#x20;

* Sony's CXD5602 Arm Cortex M4F x 6 cores at 156mhz&#x20;
* 1.5 MB SRAM
* 8 MB flash memory&#x20;
* 5 megapixel camera&#x20;
* Have extension board that support Accelerometer/Microphone&#x20;

Although it doesn't have Cortex M7 which can run heavier object detection models, it officially supports tensorflow library in Arduino IDE so its easy to port CIFAR-10 image classification in there easily with microSD card. Support from Edge Impulse also makes this board versatile.&#x20;

<figure><img src="../.gitbook/assets/IMG_2142 Large.jpeg" alt=""><figcaption><p> sony spresense extension board Arduino Nicla Vision OpenMV H7 Google Coral Dev Board Nvidia Jetson Nano 4GB </p></figcaption></figure>

Sony Spresense board is acturally tiny without the extension board. If you are only using computer vision, then you do not need expansive extension board as the mainboard have camera port.

<figure><img src="../.gitbook/assets/IMG_2146 Large.jpeg" alt=""><figcaption><p>Sony spresense + Camera</p></figcaption></figure>

Be careful with direction of camera port. Also, camera wires are flimsy.&#x20;

So, implementing object detection through Edge Impulse will be similar to steps that you have to go through BLE 33.&#x20;



### Download [Sony Spresense Board Edge Impulse Firmware](https://cdn.edgeimpulse.com/firmware/sony-spresense.zip)

Run flash\_mac.command&#x20;

<figure><img src="../.gitbook/assets/Screenshot 2023-08-27 at 4.06.15 PM.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/Screenshot 2023-08-27 at 4.05.32 PM.png" alt=""><figcaption><p>flash sony spresense firmware</p></figcaption></figure>

### Create new Project

<figure><img src="../.gitbook/assets/Screenshot 2023-08-27 at 4.07.49 PM.png" alt=""><figcaption></figcaption></figure>

### In terminal,&#x20;

```
$ brew update
$ brew install node
$ brew install python3 
$ npm install -g edge-impulse-cli --force
$ brew install arduino-cli
$ edge-impulse-daemon 
```

<figure><img src="../.gitbook/assets/Screenshot 2023-08-27 at 4.08.41 PM.png" alt=""><figcaption></figcaption></figure>

### Go to the Project and take Photos&#x20;

![](https://lh3.googleusercontent.com/5wx6PShdL86gZtkNMRB3PGH\_r6V9GVKzIEIddkSlI0lbqDD3BFQR5IgJBtLcuNCeedz2ojqiPe3Idw9PWrxRBbqrtfD0\_pQjb4fcWwMCL1rZUsNLnsYeSH-GMbV1muDUFLv1vXMoHOO\_e-D45mOJmonqxQ=s2048)![](https://lh5.googleusercontent.com/V7KSbce5o03zJ2PgeXHC3gLCgg3c4DDRhzSOGIqo4hPjHxy1yCiMW3QoP4TTMRChnKSJH-WGwM9-esKjwQI65pEBJZWoBuso-TWIm09QBXSqkI-cti-FiQhQ1xVpqvPYETxJ8uXheOHPgL\_1-ieDvvvt7g=s2048)\


<figure><img src="../.gitbook/assets/1.1_.gif" alt=""><figcaption><p>sony spresense video stream 96X96</p></figcaption></figure>

### Take at least 50 image per object and draw bounding box for test/train dataset

<figure><img src="../.gitbook/assets/Screenshot 2023-08-27 at 4.12.43 PM.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/Screenshot 2023-08-27 at 4.15.48 PM.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/Screenshot 2023-08-27 at 4.16.13 PM.png" alt=""><figcaption></figcaption></figure>

Above is my sample label. This was my testing program so I did not labeled much.&#x20;



### Select/Train the model&#x20;

* 96X96 RGB &#x20;
* 2 classes - banana, cup&#x20;
* FOMO MobileNetV2.0.1&#x20;
* 200 epoch + 0.001 LR&#x20;
* 128 Batch Size (You can go to export mode by pressing button next to Neural Network Settings and change Batch size. Default is 32.)

I used 200 epoch + 0.001 LR. (Epoch higher than 70 does not bring significant improvement and the accuracy will be plateaued). Everything will be done in cloud.&#x20;

However, the model training is limited to 20 minutes of session. If you have lots of train dataset, then you can choose to lower epoch + higher learning rate + increased batch size&#x20;

Try to select the models that are officially supported by Edge Impulse as other community models crashed in my case

<figure><img src="../.gitbook/assets/Screenshot 2023-08-27 at 4.23.52 PM.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/Screenshot 2023-08-27 at 4.23.36 PM.png" alt=""><figcaption></figcaption></figure>

Now you can do live classification with your model.&#x20;

### Live Classification

<figure><img src="../.gitbook/assets/Screenshot 2023-08-27 at 4.29.51 PM.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/Screenshot 2023-08-27 at 4.30.03 PM.png" alt=""><figcaption></figcaption></figure>

### Exporting Model&#x20;

You can choose arduino library or firmware

<figure><img src="../.gitbook/assets/Screenshot 2023-08-27 at 4.37.24 PM.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/Screenshot 2023-08-27 at 4.30.23 PM.png" alt=""><figcaption></figcaption></figure>

In the downloaded file, there will be firmware flash file for Spresense Board&#x20;

<figure><img src="../.gitbook/assets/Screenshot 2023-08-27 at 4.30.40 PM.png" alt=""><figcaption></figcaption></figure>

Just like when you connect Sony Spresense to the Edge Impulse, type&#x20;

```
edge-impulse-run-daemon
```

<figure><img src="../.gitbook/assets/1.1_ (1).gif" alt=""><figcaption></figcaption></figure>

You can choose to download arduino file which you can modify more than firmware.

Download the Arduino Library&#x20;

<figure><img src="../.gitbook/assets/Screenshot 2023-08-27 at 4.45.27 PM.png" alt=""><figcaption></figcaption></figure>

Go to Arduino IDE --> Sketch --> Include Library --> Add .ZIP Library --> ei-sony\_spresense

<figure><img src="../.gitbook/assets/Screenshot 2023-08-27 at 4.49.16 PM.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/Screenshot 2023-08-27 at 4.45.43 PM.png" alt=""><figcaption></figcaption></figure>

Flash the sample code and turn on the Serial Monitor.&#x20;

<figure><img src="../.gitbook/assets/Screenshot 2023-08-27 at 4.45.57 PM.png" alt=""><figcaption></figcaption></figure>
