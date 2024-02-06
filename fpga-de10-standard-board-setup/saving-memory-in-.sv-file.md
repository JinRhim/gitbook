# Saving memory in .sv file

int\_weight1\_bram.sv

```
`timescale 1ns/1ns
module int_weight1_bram (
    input wire clk,
    input wire [9:0] addr,
    output reg [511:0] data
);

reg [511:0] mem[0:783];

initial begin
    mem[0] = 512'fff2ffcfffbeffe90055ffa8009e004e0068ffb8004affacffc90005003ffff2fff10027ffa90025ff990071ffd6ff20ffaeffcaffa4ffec0036ffedffe8005f;
    mem[1] = 512'hff83009c01a70002ffefff8fffb400a300620039ff650030ffb2001f007b008affd300b00064ffe4ff91ff9eff9fffe0007e00500024008e003dffb00002fff2;
    mem[2] = 512'h005bfff100370057008a00512ff7eff4c0002fff200ba001e0051ffceffe000e600420030ff880020ffccffefffb8ffc00062004c0046ffeeff910064ffefff4a;
    ...
    mem[783] = 512'h00790026ff43fffeffccffbbffd70033004f0081ffd30042ffd1ff6200de004effb900370056fff4004a008a0019002c007e00210093007a005b00b5000c001c;
end

//always @(posedge clk) begin
always @(negedge clk) begin
    data <= mem[addr];
	 //$display("Weight BRAM Called:  = %h", data);
end

endmodule
```

Top module to parse memory for 16 bits

```
`timescale 1ns/1ns

module project11_bram(
    input wire clk,
	 output reg[15:0] segment_out
);

// Internal signals
wire [511:0] bram_data;             // BRAM data output
reg [9:0] addr = 10'b0;        // To track changes in address
reg [4:0] segment_sel = 5'b0;       // To cycle through segments'
reg [8:0] start_index = 9'b0;

// Instantiate the int_weight1_bram module
int_weight1_bram bram (
    .clk(clk),
    .addr(addr),
    .data(bram_data)
);

// At every positive clock edge, update segment and possibly print address change
always @(posedge clk) begin
	segment_sel <= (segment_sel+1);
	
	
	start_index <= 511 - (segment_sel * 16);
	
	$display("Memory address: %d Segment Selection: %d   Start Index: %d", addr, segment_sel, start_index);
   $display("Weight BRAM Called:  = %h", bram_data);
	 
   // Select the current segment based on `segment_sel`
   segment_out <= bram_data[start_index -: 16];
	
	if (segment_sel == 31) begin
		segment_sel <= 0;
		addr <= addr+1;
		if (addr == 784) begin
			$display("\n\n=============End of Memory ======================\n\n\n\n\n\n\n\n");
		end
		start_index <= 0; 
	end
end
endmodule
```

Testbench&#x20;

```

`timescale 1ns/1ns

module project11_tb;

reg clk;

// Instantiate the DUT (Device Under Test)
project11_bram dut(
    .clk(clk)
);

// Clock generation
initial begin
    clk = 0;
    forever #5 clk = ~clk; // 100MHz clock
end

// Test duration or termination condition
initial begin
    // Run the simulation for a specific time
    #1000; // Simulate for 1000ns
    
    $finish; // Terminate simulation
end

endmodule
```

Output

```
 Loading work.int_weight1_bram
run 1000
# Memory address:    0 Segment Selection:  0   Start Index:   0
# Weight BRAM Called:  = 0001ffcfffbeffe90055ffa8009e004e0068ffb8004affacffc90005003ffff2fff10027ffa90025ff990071ffd6ff20ffaeffcaffa4ffec0036ffedffe8005f
# Time: 10, Segment Out: Xxxx
# Memory address:    0 Segment Selection:  1   Start Index: 511
# Weight BRAM Called:  = 0001ffcfffbeffe90055ffa8009e004e0068ffb8004affacffc90005003ffff2fff10027ffa90025ff990071ffd6ff20ffaeffcaffa4ffec0036ffedffe8005f
# Time: 15, Segment Out: 0001
# Memory address:    0 Segment Selection:  2   Start Index: 495
# Weight BRAM Called:  = 0001ffcfffbeffe90055ffa8009e004e0068ffb8004affacffc90005003ffff2fff10027ffa90025ff990071ffd6ff20ffaeffcaffa4ffec0036ffedffe8005f
# Time: 25, Segment Out: ffcf
# Memory address:    0 Segment Selection:  3   Start Index: 479
# Weight BRAM Called:  = 0001ffcfffbeffe90055ffa8009e004e0068ffb8004affacffc90005003ffff2fff10027ffa90025ff990071ffd6ff20ffaeffcaffa4ffec0036ffedffe8005f
# Time: 35, Segment Out: ffbe
# Memory address:    0 Segment Selection:  4   Start Index: 463
# Weight BRAM Called:  = 0001ffcfffbeffe90055ffa8009e004e0068ffb8004affacffc90005003ffff2fff10027ffa90025ff990071ffd6ff20ffaeffcaffa4ffec0036ffedffe8005f
# Time: 45, Segment Out: ffe9
# Memory address:    0 Segment Selection:  5   Start Index: 447
# Weight BRAM Called:  = 0001ffcfffbeffe90055ffa8009e004e0068ffb8004affacffc90005003ffff2fff10027ffa90025ff990071ffd6ff20ffaeffcaffa4ffec0036ffedffe8005f
# Time: 55, Segment Out: 0055
# Memory address:    0 Segment Selection:  6   Start Index: 431
# Weight BRAM Called:  = 0001ffcfffbeffe90055ffa8009e004e0068ffb8004affacffc90005003ffff2fff10027ffa90025ff990071ffd6ff20ffaeffcaffa4ffec0036ffedffe8005f
# Time: 65, Segment Out: ffa8

```
