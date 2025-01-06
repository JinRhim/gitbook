---
description: How does generator from UnitedRentals work?
---

# Power Generator for Trailer

Our company rented a container trailer with a portable generator in Savannah, Georgia where the massive mega factories are being built. Every morning, from Monday to Saturday, My boss and I come here at 7 AM and turn on the 100kW diesel generator. Usually, my work usually ends at 4 PM but my boss does not leave until 6 PM. As an Electrical Engineer, I was curious of how the generator and the trailer office's circuit breaker diagram worked.&#x20;

<figure><img src=".gitbook/assets/IMG_4165 Large.jpeg" alt=""><figcaption></figcaption></figure>

## Cummins C100D6R Generator&#x20;

Unlike the compact, newer generators that convert alternator AC voltages to DC voltage and then back to AC voltage using an inverter, this 100kW C100D6R generator is straightforward - a big diesel engine spins the alternator, and this 3-phase voltage is directly connected to the portable office trailer circuit box

<figure><img src=".gitbook/assets/Screenshot 2025-01-05 at 6.03.28 PM.png" alt=""><figcaption><p>conventional generator versus compact, newer generation generator with inverter</p></figcaption></figure>

In the first case, the diesel engine drives the alternator, which directly generates an AC voltage.&#x20;

* Voltage and frequency depend on the engine's speed and load conditions
* Need to spin at constant speed for exact 60Hz AC generation. Can be wasteful when low load&#x20;
* Natural sine wave from alternator
* Can easily have higher capacity&#x20;

For later case, a smaller diesel engine drives the alternator, and the generatred AC voltage is converted to DC then converted back to AC voltage by inverter

* Can easily make a smaller size generator. In case of increased load, the generator can simply increase RPM for higher power&#x20;
* Due to diode rectifier and inverter, the cost goes up. Especially, higher capacity inverters are hard to make. (usually below <10kW) However, nowadays inverter components are becoming drastically cheaper to make better sine waves with filters and accurate feedback systems.&#x20;
* An old, cheap-quality inverter with few MOSFETs that generates a square wave or sine wave with 2-3 steps can ruin sensitive electronics, but nowadays, all applications that need DC voltage use SPDT, which is insensitive to those kinds of variations.&#x20;

## 4-Pole Alternator RPM&#x20;

According to the datasheet, the Stamford alternator (UCI274D) have 4 poles.&#x20;

* 2 pole alternator, for 1 full rotation where N-S rotates 360 degree, one complete AC cycle is done.&#x20;
* 4-pole alternator, for 1 full rotation, each phase of the stator encounters two N poles and two S poles in one rotation, making two complete AC cycle --> Engine can spin half-time slower&#x20;

<figure><img src=".gitbook/assets/Screenshot 2025-01-05 at 6.46.38 PM (1).png" alt=""><figcaption></figcaption></figure>

Where \[frequency] = \[RPM]\*\[Pole Number]/120. To make a 60Hz AC signal, the Cummins diesel engine needs to rotate at 1800 RPM. 1800 RPM X 4/2 = 3600 AC cycle/min. 3600 divided by 60 seconds equals 60 AC cycle --> 60Hz signal.&#x20;



## Wiring to the Trailer Office&#x20;

How does wiring to the trailer work? You can see the trailer office and generator are interconnected with the L5-50 Power Cable.&#x20;

* The generator make L1(V\_a), L2(V\_b), L3(V\_c), neutral, and ground pins&#x20;
* However, in the L6-50 socket, there are only 3 pins - the 3 cables are likely L1, neutral, ground or L1, L2, neutral.&#x20;

<figure><img src=".gitbook/assets/IMG_4164 Large.jpeg" alt=""><figcaption></figcaption></figure>

### Typical Output Voltage Setup for Generator

| **Configuration** | **Voltage** | **Description**                                                               |
| ----------------- | ----------- | ----------------------------------------------------------------------------- |
| 1 Phase           | 240V        | Standard single-phase output for homes, welder or grinder                     |
| 3 Phase           | 208V        | Three-phase Wye configuration, common for commercial/industrial applications. |
| 3 Phase           | 480V        | High-voltage three-phase output for heavy industrial machinery like motors    |

The reason why there are only 3 wires connected to the trailer office is that it is running in **240/120 VAC/1 phase** mode. In this case, we only need 3 cables - L1, L2, neutral for return.&#x20;

* L1 to L2 is exactly 180 degrees apart (not 120 degrees apart as 3-phase) - L1 to L2 can generate 240V&#x20;
* L1 to Ground = 120V&#x20;
* L2 to Ground = 120V&#x20;

<figure><img src=".gitbook/assets/Screenshot 2025-01-05 at 8.11.44 PM.png" alt=""><figcaption><p>L6 Power Socket Configuration</p></figcaption></figure>

<figure><img src=".gitbook/assets/Untitled presentation.jpg" alt=""><figcaption></figcaption></figure>





