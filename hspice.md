---
description: Analog IC Design
---

# HSPICE

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

