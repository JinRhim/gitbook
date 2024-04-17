---
description: Basic Synopsys .sp file
---

# SPICE Tutorial

## Basic Spice Netlist Components

### MOSFET : NMOS&#x20;

* Body is connected to ground&#x20;

```
m0 [ Source Gate Drain Body ] [NMOS-TYPE Length FinType]

**** Sample **** 

 m1 0 gg dd bb nmos_rvt L=7n nfin=1  
 
 - Source: GND 
 - Gate: gg 
 - Drain: dd
 - Body: bb 
 
 - NMOS-Type: nmos_rvt (regular-voltage threshold)
 - Length: 7nm 
 - Fintype: one fin 
```

### MOSFET: PMOS

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
Vin <High Voltage> <Low Voltage(gnd)> PULSE <Initial Voltage> <Peak Voltage> <delay time before pulse start> <Rise Time> <Fall time> <High Width> <Total Pulse width>

Vin <High Voltage> <Low Voltage(gnd)> 

PULSE <Initial Voltage> <Peak Voltage> <delay time before pulse start> <Rise Time> <Fall time> <High Width> <Total Pulse width>
```

```
vin in 0 0 pulse 0 0.7 0 50p 50p 2n 4n
```
