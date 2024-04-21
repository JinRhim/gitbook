---
description: Basic Synopsys .sp file
---

# SPICE .sp: Basic Components

## Basic Spice Netlist Components

### MOSFET : NMOS&#x20;

<figure><img src="../.gitbook/assets/Screenshot 2024-04-18 at 3.45.05 PM.png" alt=""><figcaption><p>NMOS</p></figcaption></figure>

* Body is connected to ground&#x20;

```
m0 [ Source Gate Drain Body ] [NMOS-TYPE Length FinType]

**** Sample **** 

 m1 0 gg dd 0 nmos_rvt L=7n nfin=1  
 
 - Source: GND 
 - Gate: gg 
 - Drain: dd
 - Body: GND
 
 - NMOS-Type: nmos_rvt (regular-voltage threshold)
 - Length: 7nm 
 - Fintype: one fin 
```

### MOSFET: PMOS

<figure><img src="../.gitbook/assets/Screenshot 2024-04-18 at 3.45.40 PM.png" alt=""><figcaption><p>PMOS</p></figcaption></figure>

* Body is connected to the Power source

```
m0 [Drain Gate Source Body] [PMOS-TYPE Length Fintype]

**** Sample ****

m1 out in vdd vdd pmos_rvt l=7n nfin=1
```

### Capacitor

```
*Output capacitance
cload out 0 5f
```

### Voltage Source: DC Operating Point Analysis && Transient Analysis&#x20;

```
<Voltage-name> <positive node> <negative node> <type> <value>

**** Sample ****

Vds dd 0 DC 0.7 (Drain)
Vgs gg 0 DC 0 (Gate)
Vbs bb 0 DC 0 (Body)
```

### Voltage Source: Transient Analysis (Square Wave Input)

* 2n High, 2n Low

```
Vin <High Voltage> <Low Voltage> <gnd> PULSE <Initial Voltage> <Peak Voltage> <delay time before pulse start> <Rise Time> <Fall time> <High Width> <Total Pulse width>

Vin <High Voltage> <Low Voltage> <gnd> 

PULSE <Initial Voltage> <Peak Voltage> <delay time before pulse start> <Rise Time> <Fall time> <High Width> <Total Pulse width>
```

```
vin in 0 0 pulse 0 0.7 0 50p 50p 2n 4n
```







## Sample Code&#x20;

### ## DC Operating Point Analysis

<figure><img src="../.gitbook/assets/Screenshot 2024-04-19 at 1.33.25 PM.png" alt=""><figcaption></figcaption></figure>

<pre><code><strong>** Vds and Vgs Voltage Source. 
</strong><strong>
</strong><strong>.INCLUDE "7nm_TT_160803.pm"
</strong><strong>
</strong>.OPTION LIST NODE POST

*temperature
.temp 25

*MOSFET NMOS 
m1 0 gg dd 0 nmos_rvt L=7n nfin=1

*Power Source 
Vds dd 0 DC 0.7
Vgs gg 0 DC 0
.OP
*print currents of voltage sources
.print I(Vds) I(Vgs)
.END
</code></pre>

### ## Transient Analysis&#x20;

<figure><img src="../.gitbook/assets/Screenshot 2024-04-19 at 1.47.04 PM.png" alt=""><figcaption></figcaption></figure>

```
.include "7nm_TT_160803.pm"

.temp 100

*MOSFETs
m1 out in vdd vdd pmos_rvt l=7n nfin=1
m2 out in 0 0 nmos_rvt l=7n nfin=1

*Output capacitance
cload out 0 5f

*voltage sources
vdd vdd 0 0.7

*square wave input
vin in 0 0 pulse 0 0.7 0 50p 50p 2n 4n

*CMOS Simulation - the output should be inverse of DC Input. 
*DC input at 0
*vin in 0 DC 0
*DC input at 0.7V
*vin in 0 DC 0.7

.options list node post
*Transient Analysis for 2 cycles
.tran 10p 8n
.end
```





