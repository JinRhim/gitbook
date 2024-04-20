---
description: Weights.csv to Memeresistor circuits in Synopsys HSPICE
---

# Memresistor Mapping

Abstract&#x20;

In order to classify the MNIST dataset, the CNN model was trained in Python script and tested in the Cadence. The goal is to transcribe the Python/Cadence model to Synopsys HSPICE.&#x20;

## Dataset input&#x20;

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

## CNN Model Structure&#x20;

<figure><img src="../.gitbook/assets/Untitled presentation.jpg" alt=""><figcaption></figcaption></figure>

* 144 input Layer&#x20;
* 32 hidden layer&#x20;
* 10 output layer&#x20;

## Conductance to Resistance Conversion

The weights are given by the Excel files which were verified in Cadence.&#x20;

* W1QRun3.csv -> 64 Row, 144 Column&#x20;
* W2QRun3.csv -> 10 Row, 64 Column&#x20;

In the analog circuit, the resistance is represented by the inverse of Excel file weights.&#x20;

The goal is to map the Excel file weight values to resistance.&#x20;

* High Weights -> High Current, Conductance -> Need low resistance
* Low Weights -> Low Current, Conductance -> Need high resistance

The total possible values of Excel weight files are:&#x20;

```
W1: [-0.44 -0.3 -0.16 -0.02 0. 0.02 0.16 0.3 ]
W2: [-0.44 -0.3 -0.16 -0.02 0. 0.02 0.16 0.3 0.44 0.58 0.72 0.86 1. ]
```

However, **negative resistance is not possible**. All the negative weights will form another subtractor circuit so that the negative circuit subtracts the current from the positive circuit.
