# HSPICE Export/Import Netlist

### 1. Create a simple 4t4r configuration and testbench to export file&#x20;

use resistor from analoglib and nmos4t from SAED\_PDK\_90

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

### 2. Create a simple testbench with op-sweep

Go to Simulation -> Netlist -> Create. IT will create a Netlist with voltage source + resistors + nmos

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

###

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



### 4. Create a new library for importing.&#x20;

* After creating new library, File -> Import -> Schematic fron Netlist&#x20;

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

* Select .sp file generated. In my case, **4t4r\_HSPICE\_export.sp**&#x20;
* In **"Device Mapping,"** select **"SAED\_PDK\_90\_analogLib\_device\_map.xml"**

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

* In options, select **SAED\_PDK\_90** and **analogLib**

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>



### 5. Checking Schematic&#x20;

* If the SAED\_PDK\_90 Library import failed, it wil load the schematic without any nmos.

```
[warning] Unknown device parameters
          Following parameter(s) are invalid for device "m1" : nf
[warning] Unknown device parameters
          Following parameter(s) are invalid for device "m2" : nf
[warning] Unknown device parameters
          Following parameter(s) are invalid for device "m3" : nf
[warning] Unknown device parameters
          Following parameter(s) are invalid for device "m4" : nf
```

<figure><img src="../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

* Correctly imported Netlist file should look below:

<figure><img src="../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>
