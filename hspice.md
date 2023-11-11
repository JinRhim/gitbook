---
description: Analog IC Design
---

# HSPICE

## Creating Inverter

<figure><img src=".gitbook/assets/Screenshot 2023-11-10 at 10.18.41 AM.png" alt=""><figcaption><p>create inverter - Cell name:inverter View Name: schematic</p></figcaption></figure>

##

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

&#x20;Save the PrimeWave Script

<figure><img src=".gitbook/assets/Screenshot 2023-11-07 at 12.59.35 PM.png" alt=""><figcaption></figcaption></figure>

Now, go to simulation --> Netlist/Run

<figure><img src=".gitbook/assets/Screenshot 2023-11-07 at 1.46.21 PM.png" alt=""><figcaption></figcaption></figure>
