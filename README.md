# EXP-5-DFF-TFF
# Design and Verification of D Flip-Flop and T Flip-Flop
## Aim
To design and verify a D Flip-Flop and a T Flip-Flop using Verilog HDL, and to observe their behaviour using a testbench and VCD waveform.
## Theory / Synopsis
A D Flip-Flop (Data / Delay Flip-Flop) is an edge-triggered sequential element that transfers the value present at its input D to the output Q on the active (rising) edge of the clock. It is commonly used to store one bit of data and eliminates the race-around problem seen in level-triggered latches.
A T Flip-Flop (Toggle Flip-Flop) is an edge-triggered sequential element whose output toggles (complements) on every active clock edge when the toggle input T is high, and holds its previous state when T is low. It is widely used in counter and frequency-divider circuits.
Both flip-flops used here are also provided with an active-high asynchronous reset that forces Q to 0 irrespective of the clock.
## Boolean / Characteristic Equations
### D Flip-Flop
Q(next) = D
### T Flip-Flop
Q(next) = T ⊕ Q   (Q(next) = Q if T = 0, Q(next) = Q' if T = 1)
## Files to be Created
<img width="625" height="50" alt="image" src="https://github.com/user-attachments/assets/65034a51-5642-46ac-940a-f7ee975cea9c" />

## Design / RTL Program

```
module mux2to1 (
    input  wire A,      // input line 0
    input  wire B,      // input line 1
    input  wire S,      // select line
    output wire Y       // output
);
    assign Y = (~S & A) | (S & B);
endmodule
 
module demux1to2 (
    input  wire D,      // data input
    input  wire S,      // select line
    output wire Y0,     // output line 0
    output wire Y1      // output line 1
);
    assign Y0 = (~S) & D;
    assign Y1 = S & D;
endmodule
```
## Testbench Program
```
module tb4;
    reg  A, B, S, D;
    wire Y;
    wire Y0, Y1;
 
    // Instantiate the 2:1 Multiplexer
    mux2to1 mux_uut (
        .A(A),
        .B(B),
        .S(S),
        .Y(Y)
    );
 
    // Instantiate the 1:2 Demultiplexer
    demux1to2 demux_uut (
        .D(D),
        .S(S),
        .Y0(Y0),
        .Y1(Y1)
    );
 
    initial begin
        // ---- VCD dump setup ----
        $dumpfile("mux_demux.vcd");   // name of the VCD file to be generated
        $dumpvars(0, tb4);             // dump all signals in this testbench hierarchy
 
        // ---- Apply all input combinations ----
        $monitor("Time=%0t S=%b A=%b B=%b Y=%b | D=%b Y0=%b Y1=%b",
                  $time, S, A, B, Y, D, Y0, Y1);
 
        // MUX select = 0 -> Y should follow A
        S = 0; A = 0; B = 0; D = 0; #10;
        S = 0; A = 0; B = 1; D = 1; #10;
        S = 0; A = 1; B = 0; D = 1; #10;
        S = 0; A = 1; B = 1; D = 0; #10;
 
        // MUX select = 1 -> Y should follow B
        S = 1; A = 0; B = 0; D = 0; #10;
        S = 1; A = 0; B = 1; D = 1; #10;
        S = 1; A = 1; B = 0; D = 1; #10;
        S = 1; A = 1; B = 1; D = 0; #10;
 
        #10 $finish;
    end
endmodule
```


## Truth Table

<img width="349" height="264" alt="image" src="https://github.com/user-attachments/assets/d05e6f9d-ff68-466e-9eb8-52e22fa1b961" />

## Simulation Procedure

STEP 1 – Open Terminal

Open a terminal in the experiment folder.

STEP 2 – Load Synopsys Environment
source /synopsys/start.sh

STEP 3 – Compile Using VCS
vcs dff_tff.v tb.v -full64
If compilation is successful, VCS generates the simulation executable:
simv

STEP 4 – Run Simulation
./simv
The terminal displays the reset, clock, D/T inputs and the corresponding outputs Qd and Qt at every change (via $monitor). A VCD waveform file is also generated:
dff_tff.vcd

STEP 5 – Open DVE
dve -full64
Other option:
dve -full64 &
A DVE environment will open.

## DVE Waveform Verification

In DVE:
●	Open the testbench hierarchy.

●	Locate the signals: clk, rst, D, Qd, T, Qt.

●	Add the signals to the waveform window.

●	Run / inspect the waveform.

●	Verify that Qd follows D on every rising edge of clk when rst = 0.

●	Verify that Qt toggles on every rising edge of clk when T = 1 and rst = 0, and holds its value when T = 0.

●	Confirm that both Qd and Qt go to 0 immediately whenever rst = 1, independent of clk.

The waveform should agree with the truth table.

## Expected Result

The D Flip-Flop and T Flip-Flop were realized using Verilog HDL:

Q(next) = D   (D Flip-Flop)
Q(next) = T ⊕ Q   (T Flip-Flop)

The design was compiled and simulated using Synopsys VCS, and the functionality was verified using DVE waveform analysis, matching the expected truth table.

**## Output**
<img width="1920" height="1080" alt="Screenshot (19)" src="https://github.com/user-attachments/assets/f1856ad9-b7bb-48a9-8f3b-c4eb3799685f" />
<img width="1920" height="1080" alt="Screenshot (20)" src="https://github.com/user-attachments/assets/d32ce345-aa45-4d2e-82ff-8ebe363a107d" />


## Viva-Voce Questions

●	What is the difference between a latch and a flip-flop?

●	Why is the D flip-flop also called a 'delay' element?

●	How is a T flip-flop derived from a JK flip-flop?

●	What is the characteristic equation of a T flip-flop, and what does it mean when T = 1?

●	What is the difference between synchronous and asynchronous reset? Which one is used here?

●	Why is posedge used in the sensitivity list, and what does 'posedge clk or posedge rst' achieve?

●	What is the purpose of a Verilog testbench?

●	Why is a VCD file generated, and what does $dumpvars(0, tb) do?

●	How would you construct a T flip-flop using a D flip-flop and an XOR gate?

●	How can a chain of T flip-flops be used to build an asynchronous binary counter?

## Result

Thus, the D Flip-Flop and T Flip-Flop were designed, implemented using Verilog HDL, successfully simulated using Synopsys VCS, and verified using Synopsys DVE.
