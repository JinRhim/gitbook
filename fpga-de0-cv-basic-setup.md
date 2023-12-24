# FPGA: DE0-CV basic setup

* DE0-CV Board [Manuals](https://www.terasic.com.tw/cgi-bin/page/archive.pl?Language=English\&CategoryNo=163\&No=921\&PartNo=4)&#x20;

### Quartus Installation Steps in Windows 10&#x20;

Install Quartus Prime Standard[ 18.1.0.625 ](https://www.intel.com/content/www/us/en/software-kit/665987/intel-quartus-prime-standard-edition-design-software-version-18-1-for-windows.html)with ModelSim - upper version does not support Cyclone IV, V family

<figure><img src=".gitbook/assets/p1.png" alt=""><figcaption></figcaption></figure>

Go to Individual Files → Install Intel Cyclone V Device Support. Put .qdz file in the same path as installation file.&#x20;

<figure><img src=".gitbook/assets/p2.png" alt=""><figcaption></figcaption></figure>

### Setting up Quartus Project&#x20;

1. In the Windows search bar, type "device installer" -> Quartus installer will pop up

<figure><img src=".gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

2. Select the file directory which you stored .qdz file -> Install Cyclone V Library&#x20;
3. Turn on Quartus&#x20;
4. File -> New Project Wizard -> select directory -> Empty Project -> No need to add files&#x20;
5. In Family Device Board Settings, select Cyclone V with model name 5CEBA4F23C7&#x20;

<figure><img src=".gitbook/assets/p3.png" alt=""><figcaption></figcaption></figure>

6. Skip EDA Tool Setting. The output should resemble below pic

<figure><img src=".gitbook/assets/p4.png" alt=""><figcaption></figcaption></figure>





### Testing Simple Project

1. File -> New -> Verilog HDL File&#x20;
2. Paste following code. (The module name **MUST** match Project name)

```
module test (a,b,out);
input a, b; 
output out;
assign out = a & b; 
endmodule 
```

<figure><img src=".gitbook/assets/p5.png" alt=""><figcaption></figcaption></figure>

3. Project -> add/remove files to project -> add current file/remove unncessary file&#x20;
4. Processing -> Start Compilation

<figure><img src=".gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

5. In project directory, make folder "modelsim". This will be used to store modelsim files&#x20;
6. Turn on Modelsim -> File -> New -> Project

<figure><img src=".gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

7. Add Existing File -> Add test.v&#x20;

![](https://lh7-us.googleusercontent.com/9kpCm7Mx4594W5cbH\_ULQgAZYtYfgqU04L5DY477MC6vBIrbkbFUu5PRRyH6-eRaMuIN1ZCB6m-vFOLe2hNi3hke6pnwrKYw2LOAWASJQ-BrO4VJqoB80u37w9SNdHipc3F6KBD3ffZMQXHWr7Sa4MA)![](https://lh7-us.googleusercontent.com/\_wLSxeZRQzV7BtpKfy9QT7na2ZObwLY1BHQ5VRU7Ll6r5p0gqKpYglqBuiFv6lYtVT0umYgYmuB9OqNpsemdMuMxb0g1x6Kwi1AfZ5YmWzNBBO5j1FR9nHKQnQ2P8LXQJrVc6KEP30wvrXM7P\_Fy6JQ)

8. Create New File -> Create "test\_HDL".  File Type: Verilog -> Paste Following Code:

```
module testbench;
	reg a;
	reg b;
	wire cout;

initial begin
a = 0;
b = 0;

#20; 

a = 0; b = 0; #20; 
a = 0; b = 1; #20;
a = 1; b = 0; #20;
a = 1; b = 1; #20;

end


test m(
.a( a ),
.b( b ),
.out( cout )
);
endmodule
```

9. Right click the test\_HDL file -> compile -> compile all

<figure><img src=".gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

```
project open D:/DE0-CV/tutorial_1/modelsim/test_simulation
# Loading project test_simulation
# Compile of test_HDL.v was successful.
# Compile of test.v was successful.
# 2 compiles, 0 failed with no errors.
```

10. Go to Simulate -> Start Simulation -> Select testbench&#x20;

<figure><img src=".gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

Following screen will pop up.&#x20;

<figure><img src=".gitbook/assets/p6.png" alt=""><figcaption></figcaption></figure>

11. View -> Wave
12. Enter following Command to console. (This will add all signals to wave window)&#x20;

```
add wave sim:/testbench/*
```

<figure><img src=".gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

13. Simulate -> Run 100

<figure><img src=".gitbook/assets/p7.png" alt=""><figcaption></figcaption></figure>

### Implementing Simple Project to DE0-CV Board

In order to turn on LED, both sliding switch have to be HIGH.&#x20;

* Input a -> Sliding Switch 0 -> PIN\_U13
* Input b -> Sliding Switch 1 -> PIN\_V13
* Output -> LEDR0 -> PIN\_AA2

1. Go to Verilog -> Assignments -> Pin Planner&#x20;

<figure><img src=".gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>

2. Pin Planner should look below:&#x20;

<figure><img src=".gitbook/assets/p8 (3).png" alt=""><figcaption></figcaption></figure>



