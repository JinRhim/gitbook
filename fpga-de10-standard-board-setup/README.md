---
description: 'windows screenshot: win + s + shift'
---

# FPGA: DE10-Standard Board Setup

* DE10-Standard Board [Manuals](https://www.terasic.com.tw/cgi-bin/page/archive.pl?Language=English\&CategoryNo=165\&No=1081\&PartNo=2#contents)
* DE10-Standard Board [Project Builder](https://www.terasic.com.tw/cgi-bin/page/archive.pl?Language=English\&CategoryNo=205\&No=1081\&PartNo=4)

### Quartus Installation Steps in Windows 10&#x20;

Install Quartus Prime Standard[ 18.1.0.625 ](https://www.intel.com/content/www/us/en/software-kit/665987/intel-quartus-prime-standard-edition-design-software-version-18-1-for-windows.html)with ModelSim - upper version does not support Cyclone IV, V family

<figure><img src="../.gitbook/assets/p1.png" alt=""><figcaption></figcaption></figure>

Go to Individual Files → Install Intel Cyclone V Device Support. Put .qdz file in the same path as installation file.&#x20;

<figure><img src="../.gitbook/assets/p2.png" alt=""><figcaption></figcaption></figure>

### Setting up Quartus Project&#x20;

1. In the Windows search bar, type "device installer" -> Quartus installer will pop up

<figure><img src="../.gitbook/assets/image (5) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (7) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (9) (1) (1).png" alt=""><figcaption></figcaption></figure>

2. Select the file directory which you stored .qdz file -> Install Cyclone V Library&#x20;
3. Turn on Quartus&#x20;
4. File -> New Project Wizard -> select directory -> Empty Project -> No need to add files&#x20;
5. In Family Device Board Settings, select Cyclone V with model name 5CSXFC6D6F31C6N

<figure><img src="../.gitbook/assets/p3.png" alt=""><figcaption></figcaption></figure>

6. Skip EDA Tool Setting. The output should resemble below pic

<figure><img src="../.gitbook/assets/p4.png" alt=""><figcaption></figcaption></figure>



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

<figure><img src="../.gitbook/assets/p5.png" alt=""><figcaption></figcaption></figure>

3. Project -> add/remove files to project -> add current file/remove unncessary file&#x20;
4. Processing -> Start Compilation

<figure><img src="../.gitbook/assets/image (10) (1) (1).png" alt=""><figcaption></figcaption></figure>

5. In project directory, make folder "modelsim". This will be used to store modelsim files&#x20;
6. Turn on Modelsim -> File -> New -> Project

<figure><img src="../.gitbook/assets/image (11) (1).png" alt=""><figcaption></figcaption></figure>

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

<figure><img src="../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

```
project open D:/DE0-CV/tutorial_1/modelsim/test_simulation
# Loading project test_simulation
# Compile of test_HDL.v was successful.
# Compile of test.v was successful.
# 2 compiles, 0 failed with no errors.
```

10. Go to Simulate -> Start Simulation -> Select testbench&#x20;

<figure><img src="../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

Following screen will pop up.&#x20;

<figure><img src="../.gitbook/assets/p6.png" alt=""><figcaption></figcaption></figure>

11. View -> Wave
12. Enter following Command to console. (This will add all signals to wave window)&#x20;

```
add wave sim:/testbench/*
```

<figure><img src="../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

13. Simulate -> Run 100

<figure><img src="../.gitbook/assets/p7.png" alt=""><figcaption></figcaption></figure>

After seeing the correct waveform, move to next step&#x20;

### RAM testing by IP-Catalog in Verilog&#x20;

1. Create a new Project&#x20;
2. Go to Tools -> IP Catalog. At right side, following screen will pop up.&#x20;

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1).png" alt=""><figcaption><p>IP Catalog </p></figcaption></figure>

3. Select RAM: 1-PORT and name it **ram32x4.v**

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

4. Uncheck 'q' output port&#x20;

<figure><img src="../.gitbook/assets/image (3) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

5. Leave the RAM initialization blank as the testbench will write on it

<figure><img src="../.gitbook/assets/image (4) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (5) (1) (1).png" alt=""><figcaption></figcaption></figure>

6. It will generate ram32x4.v and ram32x4\_bb.v \
   **ram32x4.v** -> RTL file which contains actual implementation \
   **ram32x4\_bb.v** -> black box version that needs to be included in top-level design\

7. After creating files, go to Project -> Add/Remove Files.  Following files should be included:&#x20;

* ram32x4.v&#x20;
* ram32x4\_bb.v&#x20;
* ram32x4.qip&#x20;

<figure><img src="../.gitbook/assets/image (6) (1).png" alt=""><figcaption></figcaption></figure>

8. Double click the ram32x4.v file. Change **ram32x4.v** file module name to **project\_3\_ram\_tutorial**. The module name must match the project name. \
   <mark style="color:blue;">module ram32x4 -> module (Your Project Name)</mark>

<figure><img src="../.gitbook/assets/Screenshot 2024-01-25 214823.png" alt=""><figcaption></figcaption></figure>

8. In project directory, create new file with name 'modelsim'. Open Modelsim and create new project in created file. \
   Name the project as **ram32x4\_testbench**&#x20;

<figure><img src="../.gitbook/assets/image (7) (1).png" alt=""><figcaption></figcaption></figure>

9. Add Existing File -> **ram32x4.v**&#x20;
10. Create New File -> **ram32x4\_testbench** with filetype Verilog&#x20;

<figure><img src="../.gitbook/assets/image (9) (1).png" alt=""><figcaption></figcaption></figure>

Following screen will pop up:

<figure><img src="../.gitbook/assets/image (10) (1).png" alt=""><figcaption></figcaption></figure>

11. Go to [Intel FPGAcademy](https://fpgacademy.org/courses.html) and download lab8 file. It contains testbench file for ram32x4
12. Copy and paste file to **ram32x4\_testbench**. Change the module name to ram32x4\_testbench&#x20;
13. Right click ram32x4\_testbench -> Compile -> Compile All&#x20;

<figure><img src="../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

14. Simulation -> Start Simulation -> Select **ram32x4\_testbench**&#x20;

<figure><img src="../.gitbook/assets/image (3) (1) (1).png" alt=""><figcaption></figcaption></figure>

