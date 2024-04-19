---
description: Weights.csv to Memeresistor circuits in Synopsys HSPICE
---

# Memresistor Mapping

## Abstract&#x20;

In order to classify the MNIST dataset, the simplest model was trained in Python Script.&#x20;

* Input: 28X28 uint8 (0 \~ 255) grayscale Images&#x20;
* Trim the 1st, 2nd, 27th, and 28th row and column (usually those are zero paddings)
* Max pooling 2X2
* Quantize to 4-bit (Range 0 to 15)&#x20;

The resulting MNIST dataset should be a size of 12X12.

<figure><img src="../.gitbook/assets/Screenshot 2024-04-19 at 2.30.05 PM.png" alt=""><figcaption><p>Origial 28X28 Pixel Image</p></figcaption></figure>

<figure><img src="../.gitbook/assets/Screenshot 2024-04-19 at 2.30.51 PM.png" alt=""><figcaption><p>Downsampled 12X12 Image</p></figcaption></figure>

```
# Quantized 12X12 4-bit pixel 
 
 [ 0  0  0  0  0  0  0  0  0  0  0  0]
 [ 0  0  0  0  0  0  1  4  3  6  6  0]
 [ 0  0  1  8 11 14 15 15  8  9  4  0]
 [ 0  0  0 10 13 14  6 10  0  0  0  0]
 [ 0  0  0  0  4 12  0  0  0  0  0  0]
 [ 0  0  0  0  0 11  7  2  0  0  0  0]
 [ 0  0  0  0  0  1 11 13  3  0  0  0]
 [ 0  0  0  0  0  0  0  9 14  1  0  0]
 [ 0  0  0  0  0  3 10 14 13  0  0  0]
 [ 0  0  0  1  9 14 14  8  1  0  0  0]
 [ 0  3 10 14 15  9  1  0  0  0  0  0]
 [ 0  6  7  5  2  0  0  0  0  0  0  0]
```

The resulting Dataset is now fed to the following CNN Model&#x20;

<figure><img src="../.gitbook/assets/Screenshot 2024-04-19 at 3.08.46 PM.png" alt=""><figcaption></figcaption></figure>

* 144 input Layer&#x20;
* 32 hidden layer&#x20;
* 10 output layer&#x20;

In analog circuit, the resistance is represented by the inverse of python script weights.&#x20;
