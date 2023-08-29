---
description: How to output UART communication
---

# Coral TPU: Output UART

Unfortunately, Google Coral Dev Board does not support BLE.&#x20;



In order to communicate with other microcontroller, it is possible to use MQTT communication, as I used it for sending distance from OpenMV H7 to ESP32.&#x20;



However, the school WIFI sometimes block the WIFI communication and the broker is sometimes not ablue to receive any information. It means I can only demo my project in my Professor's Lab.&#x20;



To fix this problem, I am going to output distance through UART communication and make Bluetooth HM-10 to send the distance information.&#x20;



To install the GPIO library,&#x20;

```
sudo python3 -m pip install python-periphery 
```

[Google documentation](https://coral.ai/docs/dev-board/gpio/#header-pinout) shows the pinout.&#x20;

* 5V pin = Lower row rightmost pin
* GND pin = Lower row right to the 3rd pin
* UART3 TX pin = Upper row right to the 4th pin&#x20;

UART3 TX (output) pin is the /dev/ttymxc2.&#x20;

### Connect Arduino to see output

<figure><img src="../../.gitbook/assets/IMG_2159 Large.jpeg" alt=""><figcaption></figcaption></figure>

I already have a arduino debugging tool that I made to test UART communication specific with little i2c display. This will display the UART output through the I2C display so I do not have to look at Serial Monitor.&#x20;

<figure><img src="../../.gitbook/assets/IMG_2158 Large.jpeg" alt=""><figcaption></figcaption></figure>

### Writing simple script to test whether UART works or not&#x20;

lets make a simple script

```
# UART_tester.py
# just check whether the uart outputs the number

from periphery import Serial
import time

# Configure UART3, 9600 baud
# PIN 7 
# Lower row 4th pin 

uart3 = Serial("/dev/ttymxc2", 9600)

def send_number(num):
    uart3.write(f"{num}".encode())
    print(num)

try:
    while True:
        for num in range(1, 101):
            send_number(num)
            time.sleep(0.5)  # lets make port rest
        # Loop back to 1 once 100 is reached
except KeyboardInterrupt:
    print("Exiting...")
finally:
    uart3.close()
```

This code will simply print out the number 1 to 100 through the UART port3 tx. &#x20;

I am settting baud rate to 9600 because I will use it for the bluetooth modules that uses 9600 as default baud rate.&#x20;

You can either write this on vim or write it on desktop and transfer it.&#x20;

Now, run the code

```
sudo python3 uart_tester.py 
```

Do not forget the 'sudo' as it will give you warning that you do not have permission for port&#x20;

If it keep causing permission error

```
sudo chomd a+rw /dev/ttymxc2
```

To get permission.&#x20;

<figure><img src="../../.gitbook/assets/IMG_2160 Large.jpeg" alt=""><figcaption></figcaption></figure>

It should print out number from 1 to 100.&#x20;



However, if you do not have logic analyzer or simple UART debugger, you can use below super simple code to test whether UART is working or not.&#x20;

<pre><code><strong>//ESP32 receives UART serial 2 (Port 16, 17) 
</strong>//ESP32 then reflects back to UART cable serial 0. (USB cable)

void setup() {
  // put your setup code here, to run once:
  Serial2.begin(9600);
  Serial.begin(115200);
}

void loop() {
  if (Serial2.available()) {
    Serial.write(Serial2.read());
  }
}
</code></pre>

