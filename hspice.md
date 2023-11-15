---
description: Analog IC Design
---

# HSPICE

## Creating Inverter

<figure><img src=".gitbook/assets/Screenshot 2023-11-10 at 10.18.41 AM.png" alt=""><figcaption><p>create inverter - Cell name:inverter View Name: schematic</p></figcaption></figure>

Inside Inverter, create nmos and pmos&#x20;

<div>

<figure><img src=".gitbook/assets/Screenshot 2023-11-15 at 11.13.03 AM.png" alt=""><figcaption><p>create nmos4t-0.25um </p></figcaption></figure>

 

<figure><img src=".gitbook/assets/Screenshot 2023-11-15 at 11.11.02 AM.png" alt=""><figcaption><p>create pmos4t-0.5um</p></figcaption></figure>

</div>

Connect NMOS and PMOS with wires and add wire names and tags&#x20;

<figure><img src=".gitbook/assets/Screenshot 2023-11-15 at 11.18.27 AM.png" alt=""><figcaption><p>VIN, VOUT, AVDD, AVSS</p></figcaption></figure>

Go to cellview (or Press Y)

<figure><img src=".gitbook/assets/Screenshot 2023-11-15 at 11.21.50 AM.png" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/Screenshot 2023-11-15 at 11.22.47 AM.png" alt=""><figcaption></figcaption></figure>

Press OK ---> Press save. The cell should look like the below pic

<figure><img src=".gitbook/assets/Screenshot 2023-11-15 at 11.24.37 AM.png" alt=""><figcaption></figcaption></figure>

## Creating Testbench

Create new cellview file --> name it inverter\_testbench

* Add Inverter from the library that you created above
* Add vsource, vpulse, gnd from analoglib&#x20;
* Connect and name wires - VDD, VOUT, VIN(pulse signal), GND&#x20;
* **Remove GND wire name!! IT will cause error**

<figure><img src=".gitbook/assets/Screenshot 2023-11-15 at 11.26.25 AM.png" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/Screenshot 2023-11-15 at 11.28.56 AM.png" alt=""><figcaption><p>ADD Inverter from library</p></figcaption></figure>

<figure><img src=".gitbook/assets/Screenshot 2023-11-15 at 11.30.24 AM.png" alt=""><figcaption><p>ADD vsource from analoglib</p></figcaption></figure>

<figure><img src=".gitbook/assets/Screenshot 2023-11-15 at 11.31.11 AM.png" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/Screenshot 2023-11-15 at 11.36.38 AM.png" alt=""><figcaption><p>ADD wire names</p></figcaption></figure>

<figure><img src=".gitbook/assets/Screenshot 2023-11-15 at 11.47.30 AM.png" alt=""><figcaption><p>edit DC voltage source --> 1.2v </p></figcaption></figure>

<figure><img src=".gitbook/assets/Screenshot 2023-11-15 at 11.49.29 AM.png" alt=""><figcaption><p>Edit Vpulse --> 1.2v to 0V</p></figcaption></figure>

<figure><img src=".gitbook/assets/Screenshot 2023-11-15 at 11.50.58 AM.png" alt=""><figcaption><p>ADD VOUT Tag </p></figcaption></figure>

## Running Testbench&#x20;

After designing the testbench, Click Tools --> PrimeWave&#x20;

<figure><img src=".gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

In the Primewave, go to the Simulation --> Options&#x20;

Select PrimeSim HSPICE.&#x20;

Save

<figure><img src=".gitbook/assets/Screenshot 2023-11-06 at 1.03.37 PM.png" alt=""><figcaption></figcaption></figure>

Now with the changed setting in the Primewave, Go to Setup --> Model Files&#x20;

<figure><img src=".gitbook/assets/Screenshot 2023-11-06 at 12.36.31 PM.png" alt=""><figcaption></figcaption></figure>

For the Testbench file,&#x20;

* /packages/process\_kit/generic/generic\_90nm/updated\_oct2010/SAED\_PDK90nm/hspice/SAED90nm.lib&#x20;

For Section,&#x20;

* TT\_12

Save&#x20;

<figure><img src=".gitbook/assets/Screenshot 2023-11-06 at 12.50.04 PM.png" alt=""><figcaption></figcaption></figure>

Now, go to Setup --> Analysis&#x20;

<figure><img src=".gitbook/assets/Screenshot 2023-11-06 at 12.53.15 PM.png" alt=""><figcaption></figcaption></figure>

In the testbench, select Tran

<figure><img src=".gitbook/assets/Screenshot 2023-11-07 at 12.31.38 PM.png" alt=""><figcaption></figcaption></figure>

* Start Time: 0&#x20;
* Time Step: 10p
* Stop Time: 100m&#x20;
* Set "Enable"

In the testbench, select DC&#x20;

<figure><img src=".gitbook/assets/Screenshot 2023-11-07 at 12.32.40 PM.png" alt=""><figcaption></figcaption></figure>

* Select Power source from the Testbench&#x20;

<figure><img src=".gitbook/assets/Screenshot 2023-11-07 at 12.42.20 PM.png" alt=""><figcaption><p>the red one - /V2</p></figcaption></figure>

Now, the PrimeWave Simulator should have the following table in 'value'

<figure><img src=".gitbook/assets/Screenshot 2023-11-07 at 12.35.34 PM.png" alt=""><figcaption><p>testbench following value </p></figcaption></figure>

Set the input/output variables&#x20;

<figure><img src=".gitbook/assets/Untitled (1).jpg" alt=""><figcaption></figcaption></figure>

#### Whenever selecting the outputs,&#x20;

* vin --> wire from vpulse&#x20;
* vout --> wire that goes out of inverter&#x20;
* isupply --> Click the power supply vsource&#x20;
* Remove checkmark from /VMINUS. Select the checkmark for /VPLUS

Save the PrimeWave Script

<figure><img src=".gitbook/assets/Screenshot 2023-11-07 at 12.59.35 PM.png" alt=""><figcaption></figcaption></figure>

Now, go to simulation --> Netlist/Run

<figure><img src=".gitbook/assets/Screenshot 2023-11-07 at 1.46.21 PM.png" alt=""><figcaption></figcaption></figure>
