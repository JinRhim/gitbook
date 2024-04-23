---
description: How to generate SPICE file and test it through the PrimeSim
---

# HSPICE Export/Import Netlist

### 1. Create a simple 4t4r configuration and testbench to export the file&#x20;

use resistor from analoglib and nmos4t from SAED\_PDK\_90

<figure><img src="../.gitbook/assets/image (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### 2. Create a simple testbench with op-sweep

Go to Simulation -> Netlist -> Create. IT will create a Netlist with voltage source + resistors + nmos

<figure><img src="../.gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>



### 3. Download "SAED\_PDK\_90\_analogLib\_device\_map.xml"

Default transistor and resistor will load in Synopsys Hspice without loading device map. However, in order to use SAED\_PDK\_90 library and analogLib library, we nee to import .xml file that define models and ports.&#x20;



Copy _**/Users/faculty/jianghao/jin/SAED\_PDK\_90\_analogLib\_device\_map.xml**_ and paste to current Hspice Directory. This file must be included when importing Netlist.&#x20;

````
```xml
<deviceMappings>
    <deviceMapping>
      <devType>M</devType>
      <cellType>NMOS</cellType>
      <lcv>SAED_PDK_90/nmos4t/symbol</lcv>
      <models>n12</models>
      <portProp/>
      <ports>D G S B</ports>
      <propMatching/>
      <stringParameters>model</stringParameters>
      <propMapping>w=wtot mname=model l=l m=m ad=ad as=as pd=pd ps=ps nrd=nrd nrs=nrs</propMapping>
    </deviceMapping>
```
````

## To test the created SPICE File,

1. Copy and paste .sp file inside the HSPICE directory&#x20;

<figure><img src="../.gitbook/assets/Screenshot 2024-04-23 at 1.36.28 PM.png" alt=""><figcaption></figcaption></figure>

2. Run the Simulation&#x20;

```
$csh -c 'source /packages/synopsys/setup/full_custom.csh; hspice test1.sp > test1.out'
```

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

3. In the .out file, it should generate the following files:

```
[922385903@mahmoodi test1]$ ls 
test1.ic0  test1.out  test1.sp  test1.st0  test1.tr0
```

## PrimeSim: testing the result

1. Turn on PrimeWave&#x20;

```
$ export DISPLAY="127.0.0.1:10.0"; csh -c 'source /packages/synopsys/setup/full_custom.csh; primewave &'

Setting up environment variables for Custom Compiler version S-2021.09-5
[922385903@mahmoodi test1]$ Setting up environment variables for Primewave version S-2021.09-2-T-20211216
Setting up environment variables for HSPICE version O-2018.09-SP1-1
Setting up environment variables for Star RC version Q-2019.12-SP5-3
Setting up environment variables for IC Validator version S-2021.06-2
```

2. Design Manager -> File -> Create Test Suite -> Name {filename}\_tb

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

3. Select PrimeSim HSPICE and select Netlist File&#x20;

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

5. Simulation -> Run&#x20;

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

&#x20;The current sensed at the 200mV power supply becomes 0 as the PWM signal ends.&#x20;

