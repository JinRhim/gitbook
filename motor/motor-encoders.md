---
description: Incremental vs Digital
---

# Motor Encoders

There are a lot of different types of encoders used in industrial application. Although I did not experienced much of them, there are three major ones that I have experience with.&#x20;

* Incremental Encoder&#x20;
  * Analog 1Vpp Encoder&#x20;
  * Digital Encoder&#x20;
* Absolute Encoder&#x20;
  * Biss-C&#x20;

## Incremental Encoder

Usually, Incremental encoders have 4 signals&#x20;

* COS+
* COS-&#x20;
* SIN+
* SIN-&#x20;

Some encoders have extra index signals for homing&#x20;

* Index+&#x20;
* Index-&#x20;

### Incremental Analog Encoder&#x20;

Those are the raw output of Analog Encoder, and is 1Vpp sine and cosine functions. If your motor controller has the Interpolator (which converts 1Vpp sine and cosine function to 3.3V logic level), you can directly connect those analog signals to motor controller.&#x20;

However, because those are analog signal with 1Vpp, those signals are extremely susceptible to noise, especially without encoder wire shield or&#x20;

<figure><img src="../.gitbook/assets/10m_cable_after_COS_10k_res (2) (2).png" alt=""><figcaption></figcaption></figure>
