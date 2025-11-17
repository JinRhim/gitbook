---
description: Analog IC Design
---

# HSPICE

## Creating Inverter

<figure><img src="../../.gitbook/assets/Screenshot 2023-11-10 at 10.18.41 AM.png" alt=""><figcaption><p>create inverter - Cell name:inverter View Name: schematic</p></figcaption></figure>

Inside Inverter, create nmos and pmos&#x20;

<div><figure><img src="../../.gitbook/assets/Screenshot 2023-11-15 at 11.13.03 AM.png" alt=""><figcaption><p>create nmos4t-0.25um </p></figcaption></figure> <figure><img src="../../.gitbook/assets/Screenshot 2023-11-15 at 11.11.02 AM.png" alt=""><figcaption><p>create pmos4t-0.5um</p></figcaption></figure></div>

Connect NMOS and PMOS with wires and add wire names and tags&#x20;

<figure><img src="../../.gitbook/assets/Screenshot 2023-11-15 at 11.18.27 AM.png" alt=""><figcaption><p>VIN, VOUT, AVDD, AVSS</p></figcaption></figure>

Go to cellview (or Press Y)

<figure><img src="../../.gitbook/assets/Screenshot 2023-11-15 at 11.21.50 AM.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/Screenshot 2023-11-15 at 11.22.47 AM.png" alt=""><figcaption></figcaption></figure>

Press OK ---> Press save. The cell should look like the below pic

<figure><img src="../../.gitbook/assets/Screenshot 2023-11-15 at 11.24.37 AM.png" alt=""><figcaption></figcaption></figure>

## Creating Testbench

Create new cellview file --> name it inverter\_testbench

* Add Inverter from the library that you created above
* Add vsource, vpulse, gnd from analoglib&#x20;
* Connect and name wires - VDD, VOUT, VIN(pulse signal), GND&#x20;
* **Remove GND wire name!! IT will cause error**

<figure><img src="../../.gitbook/assets/Screenshot 2023-11-15 at 11.26.25 AM.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/Screenshot 2023-11-15 at 11.28.56 AM.png" alt=""><figcaption><p>ADD Inverter from library</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/Screenshot 2023-11-15 at 11.30.24 AM.png" alt=""><figcaption><p>ADD vsource from analoglib</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/Screenshot 2023-11-15 at 11.31.11 AM.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/Screenshot 2023-11-15 at 11.36.38 AM.png" alt=""><figcaption><p>ADD wire names</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/Screenshot 2023-11-15 at 11.47.30 AM.png" alt=""><figcaption><p>edit DC voltage source --> 1.2v </p></figcaption></figure>

<figure><img src="../../.gitbook/assets/Screenshot 2023-11-15 at 11.49.29 AM.png" alt=""><figcaption><p>Edit Vpulse --> 1.2v to 0V</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/Screenshot 2023-11-15 at 11.50.58 AM.png" alt=""><figcaption><p>ADD VOUT Tag </p></figcaption></figure>

## Running Testbench&#x20;

After designing the testbench, Click Tools -> PrimeWave&#x20;

<figure><img src="../../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

In the Primewave, go to the Simulation --> Options&#x20;

Select PrimeSim HSPICE.&#x20;

Save

<figure><img src="../../.gitbook/assets/Screenshot 2023-11-06 at 1.03.37 PM.png" alt=""><figcaption></figcaption></figure>

Now with the changed setting in the Primewave, Go to Setup --> Model Files&#x20;

<figure><img src="../../.gitbook/assets/Screenshot 2023-11-06 at 12.36.31 PM.png" alt=""><figcaption></figcaption></figure>

For the Testbench file,&#x20;

* /packages/process\_kit/generic/generic\_90nm/updated\_oct2010/SAED\_PDK90nm/hspice/SAED90nm.lib&#x20;

For Section,&#x20;

* TT\_12

Save&#x20;

<figure><img src="../../.gitbook/assets/Screenshot 2023-11-06 at 12.50.04 PM.png" alt=""><figcaption></figcaption></figure>

Now, go to Setup --> Analysis&#x20;

<figure><img src="../../.gitbook/assets/Screenshot 2023-11-06 at 12.53.15 PM.png" alt=""><figcaption></figcaption></figure>

In the testbench, select Tran

<figure><img src="../../.gitbook/assets/Screenshot 2023-11-07 at 12.31.38 PM.png" alt=""><figcaption></figcaption></figure>

* Start Time: 0&#x20;
* Time Step: 10p
* Stop Time: 100m&#x20;
* Set "Enable"

In the testbench, select DC&#x20;

<figure><img src="../../.gitbook/assets/Screenshot 2023-11-07 at 12.32.40 PM.png" alt=""><figcaption></figcaption></figure>

* Select Power source from the Testbench&#x20;

<figure><img src="../../.gitbook/assets/Screenshot 2023-11-07 at 12.42.20 PM.png" alt=""><figcaption><p>the red one - /V2</p></figcaption></figure>

Now, the PrimeWave Simulator should have the following table in 'value'

<figure><img src="../../.gitbook/assets/Screenshot 2023-11-07 at 12.35.34 PM.png" alt=""><figcaption><p>testbench following value </p></figcaption></figure>

Set the input/output variables&#x20;

<figure><img src="../../.gitbook/assets/Untitled (1).jpg" alt=""><figcaption></figcaption></figure>

#### Whenever selecting the outputs,&#x20;

* vin --> wire from vpulse&#x20;
* vout --> wire that goes out of inverter&#x20;
* isupply --> Click the power supply vsource&#x20;
* Remove checkmark from /VMINUS. Select the checkmark for /VPLUS

Save the PrimeWave Script

<figure><img src="../../.gitbook/assets/Screenshot 2023-11-07 at 12.59.35 PM.png" alt=""><figcaption></figcaption></figure>

Now, go to simulation --> Netlist/Run

<figure><img src="../../.gitbook/assets/Screenshot 2023-11-07 at 1.46.21 PM.png" alt=""><figcaption></figcaption></figure>

### In order to see Node Voltages&#x20;

First, include OP Sweep in testbench.&#x20;

<figure><img src="../../.gitbook/assets/• Analyses - ENGR849wilson_current_mirrorschematic.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

After running testbench, press Ctrl + B -> Check all&#x20;

<figure><img src="../../.gitbook/assets/Backannotate Operating Points -• ENOR849cascode_current_mirr....png" alt=""><figcaption></figcaption></figure>

## Creating Netlist from testbench&#x20;

Simulation -> Netlist -> Create

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Following is the sample netlist created from 4t4r testbench

<figure><img src="../../.gitbook/assets/image (2) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

```
*  Generated for: PrimeSim
*  Design library name: test_1
*  Design cell name: 4t4r
*  Design view name: schematic


.param vg=1
.option PARHIER = LOCAL
.option PORT_VOLTAGE_SCALE_TO_2X = 1
.option RUNLVL = 3

.option WDF=1
.temp 25
.lib '/packages/process_kit/generic/generic_90nm/updated_oct2010/SAED_PDK90nm/hspice/SAED90nm.lib' TT_12

*Custom Compiler Version S-2021.09-5
*Wed Mar 13 14:44:54 2024

.global gnd!
********************************************************************************
* Library          : test_1
* Cell             : 4t4r
* View             : schematic
* View Search List : hspice hspiceD schematic spice veriloga
* View Stop List   : hspice hspiceD
********************************************************************************
m4 net16 gate2 te2 gnd! n12 w=0.24u l=0.1u nf=1 m=1
m3 net12 gate1 te1 gnd! n12 w=0.24u l=0.1u nf=1 m=1
m2 net8 gate2 te2 gnd! n12 w=0.24u l=0.1u nf=1 m=1
m1 net4 gate1 te1 gnd! n12 w=0.24u l=0.1u nf=1 m=1
r1026 te2 gnd! r=1
r2 s1 net8 r=10k
r4 s2 net16 r=20K
r1025 te1 gnd! r=1
r1 s1 net4 r=5k
r3 s2 net12 r=15K
vg1 gate1 gnd! dc='vg'
vs1 s1 gnd! dc=1.2
vg2 gate2 gnd! dc='vg'
vs2 s2 gnd! dc=1.2


.dc vg 0 1.0 0.01
.op All 0
.option opfile=1 split_dp=1
.option probe=1
.probe dc v(*) level=1
.probe dc i(r1025) i(r1026)



.end
```





