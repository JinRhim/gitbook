# EMG-CV fusion Exoglove Control

In 2022 Summer, I had amazing internship experience with Zhenyu and Jimmy for deploying Tensorflow Lite model in Sony Spresense Microcontroller.  First, the tensorflow model was trained based on Ninapro Database 5 and was fine-tuned based on Jimmy's EMG signal (he had most muscle). With index motion of 0, 1, 2, 3 which are for thumbs up, releasing, grabbing, and O.K sign, we were able to deploy quantized model in Sony Spresense board.&#x20;

However, we had to use ESP32 just for the sole purpose of wireless communication. ESP32 only receives the Bluetooth information and sends UART information to the Sony Spresense board.

<figure><img src=".gitbook/assets/ENGR696 Presentation.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/ENGR696 Presentation (1).jpg" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/My-Movie.gif" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/JimmyDemo2.gif" alt=""><figcaption></figcaption></figure>
