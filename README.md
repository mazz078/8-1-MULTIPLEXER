# 8:1 Multiplexer using Verilog

## 📌 Project Overview

This project implements an **8:1 Multiplexer (MUX)** using Verilog HDL.

An 8:1 multiplexer selects one of eight input signals and transfers the selected input to a single output.

Three select lines are required to select one of the eight inputs.

---

## 🎯 Objective

The objective of this project is to design and verify an 8:1 multiplexer using Verilog HDL.

The design is verified using a dedicated Verilog testbench.

---

## 🛠️ Technologies Used

* Verilog HDL
* VS Code
* Icarus Verilog / ModelSim / QuestaSim
* GTKWave (optional)
* GitHub

---

## 📂 Project Structure

```text
8-to-1-Multiplexer/
│
├── README.md
├── src/
│   └── mux_8to1.v
├── testbench/
│   └── tb_mux_8to1.v
└── simulation/
    └── simulation_results.txt
```

---

## 🔢 Inputs and Outputs

The multiplexer contains:

* Eight data inputs: `I0` to `I7`
* Three select inputs: `S2`, `S1`, `S0`
* One output: `Y`

---

## 📋 Selection Table

| S2 | S1 | S0 | Selected Input |
| -- | -- | -- | -------------- |
| 0  | 0  | 0  | I0             |
| 0  | 0  | 1  | I1             |
| 0  | 1  | 0  | I2             |
| 0  | 1  | 1  | I3             |
| 1  | 0  | 0  | I4             |
| 1  | 0  | 1  | I5             |
| 1  | 1  | 0  | I6             |
| 1  | 1  | 1  | I7             |

---

## ⚙️ Logic Expression

The output can be represented as:

```text
Y = I0.S2'.S1'.S0'
  + I1.S2'.S1'.S0
  + I2.S2'.S1.S0'
  + I3.S2'.S1.S0
  + I4.S2.S1'.S0'
  + I5.S2.S1'.S0
  + I6.S2.S1.S0'
  + I7.S2.S1.S0
```

---

## 💻 Verilog Implementation

The design uses indexed vector selection:

```verilog
assign y = i[sel];
```

The 3-bit select signal determines which input bit is connected to the output.

```text
sel = 000 → i[0]
sel = 001 → i[1]
sel = 010 → i[2]
sel = 011 → i[3]
sel = 100 → i[4]
sel = 101 → i[5]
sel = 110 → i[6]
sel = 111 → i[7]
```

---

## 🧪 Testbench

The testbench verifies the multiplexer using two different input patterns:

```text
10110101
11001010
```

For each input pattern, all eight possible select combinations are tested.

This results in **16 test cases**.

---

## ▶️ Simulation Using Icarus Verilog

Open the terminal inside the project directory.

### Compile

```bash
iverilog -o mux8_sim src/mux_8to1.v testbench/tb_mux_8to1.v
```

### Run

```bash
vvp mux8_sim
```

### Expected Output

```text
================================================
              8:1 MULTIPLEXER
================================================
 I7 I6 I5 I4 I3 I2 I1 I0 | S2 S1 S0 | Y
------------------------------------------------
    10110101     |   000  | 1
    10110101     |   001  | 0
    10110101     |   010  | 1
    10110101     |   011  | 0
    10110101     |   100  | 1
    10110101     |   101  | 1
    10110101     |   110  | 0
    10110101     |   111  | 1
    11001010     |   000  | 0
    11001010     |   001  | 1
    11001010     |   010  | 0
    11001010     |   011  | 1
    11001010     |   100  | 0
    11001010     |   101  | 0
    11001010     |   110  | 1
    11001010     |   111  | 1
================================================
```

---

## 📚 Concepts Demonstrated

* Multiplexer design
* Combinational logic
* Select lines
* Verilog vectors
* Indexed vector selection
* Module instantiation
* Testbench development
* Functional verification
* Digital simulation

---

## 🚀 Future Improvements

This project can be extended to:

* 16:1 Multiplexer
* Parameterized multiplexer
* Gate-level implementation
* MUX using smaller multiplexers
* FPGA implementation

---

## 👩‍💻 Author

**Harshu**

B.Tech - Electronics and Communication Engineering

---

## 📄 License

This project is created for educational and academic purposes.
```verilog
`timescale 1ns/1ps

module mux_8to1 (
    input  wire [7:0] i,
    input  wire [2:0] sel,
    output wire y
);

    // 8:1 Multiplexer
    assign y = i[sel];

endmodule
```
```verilog
`timescale 1ns/1ps

module tb_mux_8to1;

    reg [7:0] i;
    reg [2:0] sel;

    wire y;

    // Instantiate the Design Under Test
    mux_8to1 DUT (
        .i(i),
        .sel(sel),
        .y(y)
    );

    initial begin

        $display("================================================");
        $display("              8:1 MULTIPLEXER");
        $display("================================================");
        $display(" I7 I6 I5 I4 I3 I2 I1 I0 | S2 S1 S0 | Y");
        $display("------------------------------------------------");

        // Input pattern: I7 I6 I5 I4 I3 I2 I1 I0
        i = 8'b10110101;

        sel = 3'b000;
        #10;
        $display("    %b     |   %b  | %b", i, sel, y);

        sel = 3'b001;
        #10;
        $display("    %b     |   %b  | %b", i, sel, y);

        sel = 3'b010;
        #10;
        $display("    %b     |   %b  | %b", i, sel, y);

        sel = 3'b011;
        #10;
        $display("    %b     |   %b  | %b", i, sel, y);

        sel = 3'b100;
        #10;
        $display("    %b     |   %b  | %b", i, sel, y);

        sel = 3'b101;
        #10;
        $display("    %b     |   %b  | %b", i, sel, y);

        sel = 3'b110;
        #10;
        $display("    %b     |   %b  | %b", i, sel, y);

        sel = 3'b111;
        #10;
        $display("    %b     |   %b  | %b", i, sel, y);

        // Second input pattern
        i = 8'b11001010;

        sel = 3'b000;
        #10;
        $display("    %b     |   %b  | %b", i, sel, y);

        sel = 3'b001;
        #10;
        $display("    %b     |   %b  | %b", i, sel, y);

        sel = 3'b010;
        #10;
        $display("    %b     |   %b  | %b", i, sel, y);

        sel = 3'b011;
        #10;
        $display("    %b     |   %b  | %b", i, sel, y);

        sel = 3'b100;
        #10;
        $display("    %b     |   %b  | %b", i, sel, y);

        sel = 3'b101;
        #10;
        $display("    %b     |   %b  | %b", i, sel, y);

        sel = 3'b110;
        #10;
        $display("    %b     |   %b  | %b", i, sel, y);

        sel = 3'b111;
        #10;
        $display("    %b     |   %b  | %b", i, sel, y);

        $display("================================================");

        $finish;
    end

endmodule
```
# 8:1 MULTIPLEXER SIMULATION RESULTS

Input = 10110101

Select = 000 → Output = 1
Select = 001 → Output = 0
Select = 010 → Output = 1
Select = 011 → Output = 0
Select = 100 → Output = 1
Select = 101 → Output = 1
Select = 110 → Output = 0
Select = 111 → Output = 1

Input = 11001010

Select = 000 → Output = 0
Select = 001 → Output = 1
Select = 010 → Output = 0
Select = 011 → Output = 1
Select = 100 → Output = 0
Select = 101 → Output = 0
Select = 110 → Output = 1
Select = 111 → Output = 1

================================================
Simulation completed successfully.
==================================
