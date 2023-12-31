# pes_rca 

# Adders Overview

Adders are fundamental digital circuits used in computer systems and other digital devices for performing addition operations. They are an integral part of the arithmetic logic unit (ALU) in central processing units (CPUs) and are used in various applications involving numerical calculations. In general, an adder takes two binary numbers as input and produces their sum as output. Here is a brief overview of adders:

## Purpose and Use

Adders are used in various digital systems and applications, including:

1. **Arithmetic Operations:** Adders are primarily used for performing addition operations, which include both integer and floating-point addition. These operations are fundamental to numerical computations in computer systems.

2. **Subtraction:** Subtraction is often implemented using adders with the use of two's complement representation. By adding a number to the negation of another number, subtraction can be achieved.

3. **Counting:** In digital counters and sequential circuits, adders are used to increment or decrement values to count or keep track of events or time intervals.

4. **Data Processing:** Adders play a crucial role in various data processing tasks, including address calculations, data manipulation, and signal processing.

5. **Multiplication and Division:** High-performance adders are used in multiplication and division operations, as these operations are often implemented as a series of additions and subtractions.

## Types of Adders

There are several types of adders, each with its own advantages and trade-offs. The primary types of adders include:

### Ripple Carry Adder (RCA)

The Ripple Carry Adder is a straightforward and commonly used type of adder. It adds binary numbers by cascading the carry output of each stage to the carry input of the next stage. While simple, it can be slow for long binary numbers, as it requires carry propagation from the least significant bit to the most significant bit.

### Carry Look-Ahead Adder (CLA)

The Carry Look-Ahead Adder improves the speed of addition by calculating carry signals for each stage in parallel, rather than waiting for the carry to ripple through each bit. This type of adder is more complex but faster than the Ripple Carry Adder.

### Carry Save Adder (CSA)

The Carry Save Adder is an adder designed for efficient parallel addition. It is often used in multioperand addition operations and is commonly found in digital signal processing (DSP) applications.

### Carry Select Adder

The Carry Select Adder combines both Ripple Carry and Carry Look-Ahead adders. It selects the carry from either a Ripple Carry Adder or a Carry Look-Ahead Adder depending on the input values, offering a balance between speed and complexity.

### Parallel Prefix Adder (PPA)

Parallel Prefix Adders are designed for high-speed addition and are commonly used in high-performance processors. They use a prefix computation network to calculate carry signals efficiently.

# Ripple Carry Adder
In this repository we will be looking into a specific type of adder which is the ripple carry adder. We will be doing the RTL to GDS procedure with a design of a simple ripple carry adder along with it's testbench.

![image](https://github.com/Pranav1723/pes_rca/assets/78376336/07026c08-6945-4b6c-a3c2-8c0acb55f692)


# Procedure:

- Create a ripple carry adder verilog file along with it's testbench using the gedit command:

```
// top module for ripple carry adder
module pes_rca( 
    input [15:0] A,
    input [15:0] B,
    input Cin,
    output [15:0] Sum,
    output Cout
);

wire [15:0] carry; // Internal carry signals

assign carry[0] = A[0] & B[0] | (A[0] ^ B[0]) & Cin;
assign carry[1] = A[1] & B[1] | (A[1] ^ B[1]) & carry[0];
assign carry[2] = A[2] & B[2] | (A[2] ^ B[2]) & carry[1];
assign carry[3] = A[3] & B[3] | (A[3] ^ B[3]) & carry[2];
assign carry[4] = A[4] & B[4] | (A[4] ^ B[4]) & carry[3];
assign carry[5] = A[5] & B[5] | (A[5] ^ B[5]) & carry[4];
assign carry[6] = A[6] & B[6] | (A[6] ^ B[6]) & carry[5];
assign carry[7] = A[7] & B[7] | (A[7] ^ B[7]) & carry[6];
assign carry[8] = A[8] & B[8] | (A[8] ^ B[8]) & carry[7];
assign carry[9] = A[9] & B[9] | (A[9] ^ B[9]) & carry[8];
assign carry[10] = A[10] & B[10] | (A[10] ^ B[10]) & carry[9];
assign carry[11] = A[11] & B[11] | (A[11] ^ B[11]) & carry[10];
assign carry[12] = A[12] & B[12] | (A[12] ^ B[12]) & carry[11];
assign carry[13] = A[13] & B[13] | (A[13] ^ B[13]) & carry[12];
assign carry[14] = A[14] & B[14] | (A[14] ^ B[14]) & carry[13];
assign carry[15] = A[15] & B[15] | (A[15] ^ B[15]) & carry[14];

assign Sum = A + B + Cin;
assign Cout = carry[15];

endmodule

```

```
// testbench for ripple carry adder
module tb_pes_rca;

    // Inputs
    reg [15:0] A;
    reg [15:0] B;
    reg Cin;

    // Outputs
    wire [15:0] Sum;
    wire Cout;

    // Instantiate the Unit Under Test (UUT)
    pes_rca uut (
        .A(A),
        .B(B),
        .Cin(Cin),
        .Sum(Sum),
        .Cout(Cout)
    );

    // Clock generation
    reg Clock = 0;
    always #10 Clock = ~Clock;

    // Simulation inputs
    initial begin
        // Initialize inputs
        A = 16'h1234;
        B = 16'h5678;
        Cin = 1'b0;

        // Apply inputs after some initial delay
        #20 A = 16'hABCD;
        #20 B = 16'h9876;
        #20 Cin = 1'b1;

        // Wait for a while to observe the results
        #100;

        // Finish simulation
        $finish;
    end

    // Dump VCD file for waveform simulation
    initial begin
        $dumpfile("pes_rca.vcd");
        $dumpvars;
    end
endmodule 

```
  ![Screenshot from 2023-10-18 00-36-44](https://github.com/Pranav1723/pes_Hamming/assets/78376336/5d3a0973-0170-484b-850e-332a9db9294d)


  ![image](https://github.com/Pranav1723/pes_rca/assets/78376336/4189313b-80a4-4267-9c4e-a02da08400a4)


  ![image](https://github.com/Pranav1723/pes_rca/assets/78376336/0d7bcb86-90cf-4e8f-ac31-b41848c81c86)


- To start the simulation use the following code:

  ``` iverilog pes_rca.v tb_pes_rca.v ```

- Use this command to open the gtkwave tool:

  ``` gtkwave pes_rca.vcd ```

![image](https://github.com/Pranav1723/pes_Hamming/assets/78376336/7815ed9c-a21a-42fb-9da6-e37c56858357)

This is what we can see:

![Screenshot from 2023-10-18 00-33-42](https://github.com/Pranav1723/pes_Hamming/assets/78376336/4c044b7a-11a8-495f-8480-2abd08ae6ece)

This is the result of the Pre-synthesis simulation

## RTL Synthesis

- RTL Synthesis: Type this command:
  ``` yosys ```

  ![image](https://github.com/Pranav1723/pes_rca/assets/78376336/adc38b2b-748c-4677-ae37-7b6eda3789ac)

- Type the following commands:

  ``` read_liberty -lib sky130_fd_sc_hd__tt_025C_1v80.lib ```

  ``` read_verilog pes_rca.v ```

  ``` synth -top pes_rca ```
  
      

  ![image](https://github.com/Pranav1723/pes_rca/assets/78376336/ba960a01-9a06-43a9-9d4e-6c5f12d677e8)

  ![image](https://github.com/Pranav1723/pes_rca/assets/78376336/f8e5a0f0-b190-4bfe-be3a-23dd5958c0ef)

  ![image](https://github.com/Pranav1723/pes_rca/assets/78376336/59d74212-5481-410d-acd7-8a56ca8f8b40)

- Now we type this command:

 ``` abc -liberty -lib ./lib/sky130_fd_sc_hd__tt_025C_1v80.lib ```

 ``` show ```

 ![image](https://github.com/Pranav1723/pes_rca/assets/78376336/42ea8fcd-718b-4962-9e4c-eeafaf6ab25f)


 ![image](https://github.com/Pranav1723/pes_rca/assets/78376336/d73b06a7-415f-46dc-b715-d9e82e0f6eaa)


- To generate the netlist we must type the command:

  ``` write_verilog -noattr pes_rca_net.v ```

- To read the design and test bench file we must use the command:

``` iverilog primitives.v sky130_fd_sc_hd.v pes_rca_net.v tb_pes_rca.v ```

![image](https://github.com/Pranav1723/pes_rca/assets/78376336/888e9b2a-0695-40a4-a976-9d1f277bb6d5)

## GLS 

- Type this command to generate the .vcd file

  ``` ./a.out ```

- To view the waveform type the command

  ``` gtkwave pes_rca.vcd ```

![image](https://github.com/Pranav1723/pes_rca/assets/78376336/ef25d2a5-3c2e-463f-b81c-984a0e28cab4)

As we can see the GLS is the same as the pre synthesis simulation and therefore our entire procedure is up to the mark and correct.

## Physical Design
- To start off with the physical design synthesis, floorplan and placement and clock tree synthesis first off install OpenLane and Docker.

### Installation steps:

- To install docker follow the steps accordingly:

``` 
sudo apt-get remove docker docker-engine docker.io containerd runc
sudo apt-get update
sudo apt-get install \
   ca-certificates \
   curl \
   gnupg \
   lsb-release
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo \
   "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin

```

- To download and install OpenLane follow these steps:

```
git clone --depth 1 https://github.com/The-OpenROAD-Project/OpenLane.git
cd OpenLane/
make
make test 

```


- After the installation follow these steps to start off:
- Navigate to the OpenLane directory using this command:

  ``` cd openlane/designs ```

- Once youu are in this directory make another directory whose name reflects the name of your design. In this case the name of the directory is pes_rca.

  ``` mkdir pes_rca ```

  ``` cd pes_rca ```

- Once you are in the directory save your design verilog file here along with the config.json file which is written as such:

 
![image](https://github.com/Pranav1723/pes_rca/assets/78376336/6ecd4fc3-9e20-49cf-8a69-cf9529a0ed83)


- In the same directory run these commands:

``` make mount ```

``` ./flow.tcl -interactive ```

``` package require openlane 0.9 ```

- After these commands type the command:

  ``` prep -design pes_rca ```

  ![image](https://github.com/Pranav1723/pes_rca/assets/78376336/1112071c-ec68-4ce4-9e22-b7ebc7f110b0)

### Synthesis

- To start the process of synthesis use the command:

``` run_synthesis ```

![image](https://github.com/Pranav1723/pes_rca/assets/78376336/c09e7403-f690-4961-aa05-0f01d5642652)

The reports of the synthesis are as follows:


![image](https://github.com/Pranav1723/pes_rca/assets/78376336/4bf96114-ed1d-4344-82cc-ad94a8e8c75b)


![image](https://github.com/Pranav1723/pes_rca/assets/78376336/4f855b28-9271-41cb-baa2-3dfbb10e7e1f)


### Floorplan

- To view the floorplan results type the following command:

``` run_floorplan ```

![image](https://github.com/Pranav1723/pes_rca/assets/78376336/c28fa584-0acb-4f65-a6fb-453f038d50cc)

The floorplan looks as follows:


![image](https://github.com/Pranav1723/pes_rca/assets/78376336/8ba18e5e-340a-4da1-aa2c-a6e60aafc3c1)



### Placement

- To ensure that the procedure for viewing and analysing the placement works, make sure that the project is in the right directory.
- After this is done, run the following magic command:

``` magic -T /home/Desktop/Downloads/sky130A.tech lef read ../../tmp/merged.nom.lef def read pes_rca.def & ```

- After typing this command type the placement command in the OpenLane terminal

``` run_placement ```

![image](https://github.com/Pranav1723/pes_rca/assets/78376336/c6ddc671-3390-4c0a-82d4-8b4ed799fb16)


![image](https://github.com/Pranav1723/pes_rca/assets/78376336/5d742a7b-d9e6-4d48-a982-c5c79406320a)


![image](https://github.com/Pranav1723/pes_rca/assets/78376336/0d12d070-e027-4899-8540-440eeade58d6)


### Clock tree synthesis

- To proceed with the clock tree synthesis type the following command:

``` run_cts ```

Some of the reports generated by the above command are given as follows:

![image](https://github.com/Pranav1723/pes_rca/assets/78376336/2a3ffe11-f963-453b-a0d2-7a236787cd2d)

![image](https://github.com/Pranav1723/pes_rca/assets/78376336/38413cfb-e41f-441f-8631-14d1e3dbef5c)

![image](https://github.com/Pranav1723/pes_rca/assets/78376336/3379d6eb-8d68-495f-9a02-f58ba79bf6be)



### Power report

The following are the power reports for the design


![image](https://github.com/Pranav1723/pes_rca/assets/78376336/a37595fe-8532-4c27-b9c6-0054d1bc3fc0)


### Area report 

![image](https://github.com/Pranav1723/pes_rca/assets/78376336/94d22730-c4c9-4c9c-9eb7-8e57d19cb66c)


### Skew report


![image](https://github.com/Pranav1723/pes_rca/assets/78376336/ec53c8ae-98c2-4fce-b2db-d1305fc2c286)


### Statistics

The statistics of the design are given below:

- Area power = 723.689 um2
- Internal power = 9.34e-05 W
- Swtiching power = 1.60e-05
- Leakage power = 1,07e-09 W
- Total power = 1.09e-05 W











