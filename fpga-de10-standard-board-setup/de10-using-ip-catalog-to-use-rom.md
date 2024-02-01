# DE10: using IP-Catalog to use ROM

### ROM Setup

1. Create new directory for project&#x20;
2. Verilog -> New Project Wizard -> select the created file&#x20;
3. In the created project directory, create the file 'rom\_initialization.mif' (Make sure the extension is .mif, not .txt)

<figure><img src="../.gitbook/assets/image (25).png" alt=""><figcaption></figcaption></figure>

4. Paste the following sample code to .mif file and save. The following code will initialize ROM content with 8-bit width and 32 rows.&#x20;

```
DEPTH=32; 
WIDTH=8;  
ADDRESS_RADIX=HEX; 
DATA_RADIX=HEX;    
CONTENT 
BEGIN
    0: 01;
    1: 02;
    2: 03;
    3: 04;
    4: 05;
    5: 06;
    6: 07;
    7: 08;
    8: 09;
    9: 0A;
    A: 0B;
    B: 0C;
    C: 0D;
    D: 0E;
    E: 0F;
    F: 10;
    10: 11;
    11: 12;
    12: 13;
    13: 14;
    14: 15;
    15: 16;
    16: 17;
    17: 18;
    18: 19;
    19: 1A;
    1A: 1B;
    1B: 1C;
    1C: 1D;
    1D: 1E;
    1E: 1F;
    1F: 20;
END;
```

4. Tools -> IP-Catalog. \
   Following window will show at the right side of screen

<figure><img src="../.gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>

5. Library -> Basic Functions -> On Chip Memory -> ROM:1-Port \
   For simplicity of this demo, I will use ROM:1-Port to minimize input and output wiring.&#x20;

<figure><img src="../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>

6. Name file as **rom32x8.v**
7. In the setting**,**&#x20;

* 'q' output: 8-bits (rom will output 8 bits for each address)
* 32 words -> there will be 32 rows of 8-bit data
* Select **M10K** for Cyclone V&#x20;

<figure><img src="../.gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>

* Uncheck the 'q' output port&#x20;

<figure><img src="../.gitbook/assets/image (24).png" alt=""><figcaption></figcaption></figure>

* Select the created rom\_initialization.mif file as memory content data&#x20;

<figure><img src="../.gitbook/assets/image (26).png" alt=""><figcaption></figcaption></figure>

* It will generate rom32x8.v and rom32x8\_bb.v

<figure><img src="../.gitbook/assets/image (27).png" alt=""><figcaption></figcaption></figure>

8. Project -> Add/Remove Files in Project -> Add following files:&#x20;

* rom32x8.qip&#x20;
* rom32x8.v&#x20;
* rom32x8\_bb.v&#x20;

<figure><img src="../.gitbook/assets/image (28).png" alt=""><figcaption></figcaption></figure>

9. Change the module name of rom32x8\_bb.v file so that it does not overlap with ram32x8.v

<figure><img src="../.gitbook/assets/Screenshot 2024-01-29 182640.png" alt=""><figcaption></figcaption></figure>

### Creating Top Module&#x20;

1. File -> New -> SystemVerilog HDL File -> Paste following top module.\
   The following code will read ROM content, multiply those content with input wire and output the sum

```
module project7_rom (
    input wire [7:0] pixel,
    input wire clock,
    output reg [15:0] sum_output // Changed to reg since it's assigned in an always block
);

    wire [7:0] rom_output; // Output from the rom_test_bb module
    reg [15:0] sum = 0;
    reg [4:0] rom_pointer = 0; // 5-bit pointer for 32 elements

    // Instantiate the rom_test_bb module
    rom32x8 rom_inst (
        .address(rom_pointer), // Provide rom_pointer as the address
        .clock(clock),
        .q(rom_output)
    );

    always @(posedge clock) begin
	     $display("Time: %t, Pixel: %h, ROM address: %h, ROM Output: %h, Sum: %h, Sum Output: %h", $time, pixel, rom_pointer, rom_output, sum, sum_output);
        if (rom_pointer < 31) begin
            sum <= sum + rom_output * pixel; // Calculate the new sum
            rom_pointer <= rom_pointer + 1; // Increment the pointer
        end else begin
            sum_output <= sum; // Assign the sum to the output
            sum <= 0; // Reset the sum
            rom_pointer <= 0; // Reset the pointer
        end
    end

endmodule
```

The top module must match the project name

<figure><img src="../.gitbook/assets/image (29).png" alt=""><figcaption></figcaption></figure>

2. Compile the project&#x20;

### Modelsim Simulation&#x20;

1. Create 'modelsim' file in project directory&#x20;

<figure><img src="../.gitbook/assets/image (32).png" alt=""><figcaption></figcaption></figure>

2. Open Modelsim -> File -> New Project&#x20;

<figure><img src="../.gitbook/assets/image (34).png" alt=""><figcaption></figcaption></figure>

3 . Add Existing File -> Add&#x20;

* project7\_rom.sv
* rom32x8.v&#x20;
* rom32x8\_bb

4. Create New **SystemVerilog** File -> \[your project name] + "testbench"   &#x20;

<figure><img src="../.gitbook/assets/image (35).png" alt=""><figcaption></figcaption></figure>

5. Paste following code in testbench. For each clock, the testbench will generate random number and feed in to module.

```
`timescale 1ns / 1ns

module project7_testbench;

    // Testbench signals
    reg [7:0] pixel;
    reg clock;
    wire [15:0] sum_output;

    // Instantiate the project4_rom module
    project7_rom uut (
        .pixel(pixel),
        .clock(clock),
        .sum_output(sum_output)
    );

    // Clock generation
    initial begin
        clock = 0;
        forever #10 clock = ~clock; // Generate a clock with a period of 20 ns
    end

    // Stimulus
    initial begin
        // Initialize
        pixel = 0;

        // Apply test vectors
        // Input a new pixel value at each clock cycle
        repeat (100) begin // Repeat for a certain number of cycles
            @(posedge clock) pixel = $random; // Assign a random value at each positive edge of the clock
        end

        #100; // Wait for some time to observe the outputs
        $finish; // End the simulation
    end

    // Optionally, add a waveform dump for debugging
    initial begin
        $dumpfile("testbench.vcd");
        $dumpvars(0, project7_testbench);
    end

endmodule
```

6. Compile Project -> Questionmark should disappear&#x20;

<figure><img src="../.gitbook/assets/image (36).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (37).png" alt=""><figcaption></figcaption></figure>

7. Simulate -> Start Simulation -> Library -> Add **altera\_mf\_ver** (required for RAM/ROM)
8. Select work -> project7\_testbench

<figure><img src="../.gitbook/assets/image (38).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (39).png" alt=""><figcaption></figcaption></figure>

9. Add all the variables to waveforms by typing

```
add wave sim:/your_testbench_name/*
```

<figure><img src="../.gitbook/assets/image (40).png" alt=""><figcaption></figcaption></figure>

If the timescale is set to ns,&#x20;

```
run 1000
```

If the timescale is set to ps, multiply 1000.&#x20;

```
add wave sim:/project7_testbench/*
run 1000000
# Time:                10000, Pixel: 00, ROM address: 00, ROM Output: 00, Sum: 0000, Sum Output: xxxx
# Time:                30000, Pixel: 24, ROM address: 01, ROM Output: 01, Sum: 0000, Sum Output: xxxx
# Time:                50000, Pixel: 81, ROM address: 02, ROM Output: 02, Sum: 0024, Sum Output: xxxx
# Time:                70000, Pixel: 09, ROM address: 03, ROM Output: 03, Sum: 0126, Sum Output: xxxx
# Time:                90000, Pixel: 63, ROM address: 04, ROM Output: 04, Sum: 0141, Sum Output: xxxx
# Time:               110000, Pixel: 0d, ROM address: 05, ROM Output: 05, Sum: 02cd, Sum Output: xxxx
# Time:               130000, Pixel: 8d, ROM address: 06, ROM Output: 06, Sum: 030e, Sum Output: xxxx
# Time:               150000, Pixel: 65, ROM address: 07, ROM Output: 07, Sum: 065c, Sum Output: xxxx
# Time:               170000, Pixel: 12, ROM address: 08, ROM Output: 08, Sum: 091f, Sum Output: xxxx
# Time:               190000, Pixel: 01, ROM address: 09, ROM Output: 09, Sum: 09af, Sum Output: xxxx
# Time:               210000, Pixel: 0d, ROM address: 0a, ROM Output: 0a, Sum: 09b8, Sum Output: xxxx
# Time:               230000, Pixel: 76, ROM address: 0b, ROM Output: 0b, Sum: 0a3a, Sum Output: xxxx
# Time:               250000, Pixel: 3d, ROM address: 0c, ROM Output: 0c, Sum: 0f4c, Sum Output: xxxx
# Time:               270000, Pixel: ed, ROM address: 0d, ROM Output: 0d, Sum: 1228, Sum Output: xxxx
# Time:               290000, Pixel: 8c, ROM address: 0e, ROM Output: 0e, Sum: 1e31, Sum Output: xxxx
# Time:               310000, Pixel: f9, ROM address: 0f, ROM Output: 0f, Sum: 25d9, Sum Output: xxxx
# Time:               330000, Pixel: c6, ROM address: 10, ROM Output: 10, Sum: 3470, Sum Output: xxxx
# Time:               350000, Pixel: c5, ROM address: 11, ROM Output: 11, Sum: 40d0, Sum Output: xxxx
# Time:               370000, Pixel: aa, ROM address: 12, ROM Output: 12, Sum: 4de5, Sum Output: xxxx
# Time:               390000, Pixel: e5, ROM address: 13, ROM Output: 13, Sum: 59d9, Sum Output: xxxx
# Time:               410000, Pixel: 77, ROM address: 14, ROM Output: 14, Sum: 6ad8, Sum Output: xxxx
# Time:               430000, Pixel: 12, ROM address: 15, ROM Output: 15, Sum: 7424, Sum Output: xxxx
# Time:               450000, Pixel: 8f, ROM address: 16, ROM Output: 16, Sum: 759e, Sum Output: xxxx
# Time:               470000, Pixel: f2, ROM address: 17, ROM Output: 17, Sum: 81e8, Sum Output: xxxx
# Time:               490000, Pixel: ce, ROM address: 18, ROM Output: 18, Sum: 97a6, Sum Output: xxxx
# Time:               510000, Pixel: e8, ROM address: 19, ROM Output: 19, Sum: aaf6, Sum Output: xxxx
# Time:               530000, Pixel: c5, ROM address: 1a, ROM Output: 1a, Sum: c19e, Sum Output: xxxx
# Time:               550000, Pixel: 5c, ROM address: 1b, ROM Output: 1b, Sum: d5a0, Sum Output: xxxx
# Time:               570000, Pixel: bd, ROM address: 1c, ROM Output: 1c, Sum: df54, Sum Output: xxxx
# Time:               590000, Pixel: 2d, ROM address: 1d, ROM Output: 1d, Sum: f400, Sum Output: xxxx
# Time:               610000, Pixel: 65, ROM address: 1e, ROM Output: 1e, Sum: f919, Sum Output: xxxx
# Time:               630000, Pixel: 63, ROM address: 1f, ROM Output: 1f, Sum: 04ef, Sum Output: xxxx
# Time:               650000, Pixel: 0a, ROM address: 00, ROM Output: 20, Sum: 0000, Sum Output: 04ef
```

<figure><img src="../.gitbook/assets/image (41).png" alt=""><figcaption></figcaption></figure>

The module should output the sum of multiplied product.\
\
\
If **ROM Output: xx**&#x20;

\-> Check the mif file format
