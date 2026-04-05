---
description: Incremental vs Digital
---

# Motor Encoders

There are a lot of different types of encoders used in industrial applications. Although I did not experienced much of them, there are three major ones that I have experience with.&#x20;

* Incremental Encoder&#x20;
  * Analog 1Vpp Encoder&#x20;
  * Digital Encoder&#x20;
* Absolute Encoder&#x20;
  * Biss-C&#x20;

## Incremental Encoder

Usually, Incremental encoders have 4 signals&#x20;

* COS+/COS-&#x20;
* SIN+/SIN-&#x20;

Some encoders have extra index signals for homing&#x20;

* Index+&#x20;
* Index-&#x20;

### Incremental Analog Encoder&#x20;

* 1Vpp sine and cosine signal. Direct output from optical/magnetic encoder sensor &#x20;

Your motor controller should have an internal interpolator ic. (1Vpp Analog signal -> 3.3V/5V logic)&#x20;

Because those are analog signals with \~500mVpp, they are extremely susceptible to noise. With long wires, the signal ca go down to \~400mV range. \
Also, with the Motor Phase Cable passing nearby, those encoder signals are easily distorted.&#x20;

Especially with high interpolation, which it divides one cosine/sine period into multiple small segments, the encoder is likely to skip some bits.

<figure><img src="../.gitbook/assets/10m_cable_after_COS_10k_res (2) (2).png" alt=""><figcaption><p>Purple: COS+(500mV)<br>Yellow: COS- (500mV) <br>White: COS+ - COS- Differential (1Vpp)</p></figcaption></figure>

Generally,

### CW - SINE is leading, COS is following&#x20;

### CCW - COS is leading, SINE is following&#x20;

For the linear stage,&#x20;

* Stage moving away from the connector side - positive&#x20;
* Stage moving toward the connector side - negative&#x20;

However, the above rule is not always true. as some manufacturers install an encoder scale in a flipped way or program the encoder IC with different settings.&#x20;





## Index Pulse&#x20;

Unlike Absolute Encoder where user can program home in specific location (ex: 70mm, 140mm, 90 degree), Incremental Encoder does not have a way to know its absolute location.&#x20;

For the linear stage, one of the simplest ways to have a physical 'anchor' location is just to push linear stage to edge as far as possible and set this location as 0. However, because it is relying on metal wall or screw, those location can have few mm deviation.&#x20;

In order to make a absolute zero location more accurate, usually there is a index mark at the middle of linear stage.&#x20;

<figure><img src="../.gitbook/assets/BK000011.png" alt=""><figcaption><p>Purple: COS+<br>Yellow: SIN+<br>Cyan, Green: INDEX Differential</p></figcaption></figure>

Above is the index+ and Index - in cyan and green. Usually, index markers are really narrow.&#x20;

<figure><img src="../.gitbook/assets/PP-18_DB-9_IDX_DIFF (2).png" alt=""><figcaption></figcaption></figure>

#### &#x20;

