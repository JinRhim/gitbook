---
description: 5V, 10A output in smallest footprint
---

# LM61495 Test Board

<figure><img src="../.gitbook/assets/lm61495_compressed.JPG" alt=""><figcaption></figcaption></figure>

* LM61495 is buck converter that can output max 10A output current without having an external FET and comparatively small footprint.
* This is great for the motherboard, which serves the purpose of power distribution in hot environment
* I thought about using LM5148 Buck controller + LMG2100 GaNFET for 5V \~15A option, but the GaNFET is out of stock

<figure><img src="../.gitbook/assets/image (63).png" alt=""><figcaption></figcaption></figure>

For above configuration, the maximum stack will be 5, so 5V 2A output barely meets the requirements. Transient surge will be suppressed by extra additional capacitors

## LM61495 EVM Testing&#x20;

After testing the default LM61495 board, the IC chip and inductor got considerable amount of\
heat.

I want:

* Lowest switching frequency
* Lowest possible heat
* Footprint size does not matter&#x20;

At 5V, 400kHz output, the recommended inductor is\
L: _2.4 uH_ (744325240)\
COUT: _4 x 47 uF + 100 uF electrolytic + 2 x 2.2 uF_\
CIN: _4 x 10 uF + 2 x 470 nF + 100 uF electrolytic_



## Inductor Selection:&#x20;

| Inductor                            | 744325240            | 744393665068         |
| ----------------------------------- | -------------------- | -------------------- |
| **Inductance**                      | 2.4 µH               | 6.8 µH               |
| **Saturation Current ($I\_{sat}$)** | 17 A                 | 20 A                 |
| **DC Resistance (DCR)**             | 4.75 mΩ              | 9.00 mΩ              |
| **Dimensions (L × W × H)**          | 10.5 × 10.2 × 5.0 mm | 11.3 × 10.0 × 6.0 mm |
