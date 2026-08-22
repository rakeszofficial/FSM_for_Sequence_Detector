# FSM_for_Sequence_Detector
# EXP NO.6.A. Sequence Detector Using Moore Machine and Mealy Machine

# Aim
To design and simulate a Finite-State-Machine-for-Sequence-Detector-1011 using Verilog HDL, and verify its functionality through a testbench in the Vivado 2023.1 environment.

# Apparatus Required
Vivado 2023.1

# Procedure
1.  Launch Vivado 2023.1 Open Vivado and create a new project.
2.  Design the Verilog Code Write the Verilog code for the RAM,ROM,FIFO Create the Testbench Write a testbench to simulate the memory behavior.
3.  The testbench should apply various and monitor the corresponding output.
4.  Create the Verilog Files Create both the design module and the testbench in the Vivado project. Run Simulation Run the behavioral simulation to verify the output.
5.  Observe the Waveforms Analyze the output waveforms in the simulation window, and verify that the correct read and write operation.
6.  Save and Document Results Capture screenshots of the waveform and save the simulation logs. These will be included in the lab report.

# Code
# Mealy 1011
```
module mealy_1011(
    input clk,
    input rst,
    input x,
    output reg z
);

reg [1:0] state, next_state;

parameter S0 = 2'b00,
          S1 = 2'b01,
          S2 = 2'b10,
          S3 = 2'b11;

always @(posedge clk or posedge rst)
begin
    if(rst)
        state <= S0;
    else
        state <= next_state;
end

always @(*)
begin
    case(state)

        S0:
        begin
            if(x)
                next_state = S1;
            else
                next_state = S0;
            z = 0;
        end

        S1:
        begin
            if(x)
                next_state = S1;
            else
                next_state = S2;
            z = 0;
        end

        S2:
        begin
            if(x)
                next_state = S3;
            else
                next_state = S0;
            z = 0;
        end

        S3:
        begin
            if(x)
            begin
                next_state = S1;
                z = 1;   
            end
            else
            begin
                next_state = S2;
                z = 0;
            end
        end

        default:
        begin
            next_state = S0;
            z = 0;
        end

    endcase
end

endmodule
```

Test bench
```
module mealy_1011_tb;

reg clk;
reg rst;
reg x;
wire z;

mealy_1011 uut(
    .clk(clk),
    .rst(rst),
    .x(x),
    .z(z)
);

always #5 clk = ~clk;

initial
begin
    clk = 0;
    rst = 1;
    x = 0;

    #10 rst = 0;


    #10 x = 1;
    #10 x = 0;
    #10 x = 1;
    #10 x = 1;  

    #10 x = 0;
    #10 x = 1;
    #10 x = 1;
    #10 x = 0;

    #20
    $finish;
end
initial begin
    $mointor("Time=%0t Reset=%b Input=%b Output=%b,$time,rst,x,z);
end

endmodule
```
Output Waveform
<img width="898" height="837" alt="image" src="https://github.com/user-attachments/assets/41bdc94b-f997-4a8f-8df3-9151ba0853ab" />

# Moore 1011
```
module moore_1011 (
    input clk,
    input reset,
    input x,
    output reg z
);

reg [2:0] state, next_state;

parameter S0 = 3'b000,
          S1 = 3'b001,
          S2 = 3'b010,
          S3 = 3'b011,
          S4 = 3'b100;


always @(posedge clk or posedge reset) begin
    if (reset)
        state <= S0;
    else
        state <= next_state;
end


always @(*) begin
    case (state)

        S0: 
            if (x)
                next_state = S1;
            else
                next_state = S0;

        S1:
            if (x)
                next_state = S1;
            else
                next_state = S2;

        S2:
            if (x)
                next_state = S3;
            else
                next_state = S0;

        S3:
            if (x)
                next_state = S4;
            else
                next_state = S2;

        S4:
            if (x)
                next_state = S1;
            else
                next_state = S2;

        default: next_state = S0;

    endcase
end

// Output Logic (Moore Output)
always @(*) begin
    case (state)
       S4: z=1;
       default:
        z=0;
    endcase
end
endmodule
```
 Test bench
 ```
module moore_1011_rom_tb;

reg clk;
reg rst;
reg x;
wire z;

moore_1011_rom uut(
    .clk(clk),
    .rst(rst),
    .x(x),
    .z(z)
);


always #5 clk = ~clk;

initial
begin
    clk = 0;
    rst = 1;
    x = 0;

    #10 rst = 0;

    #10 x = 1;
    #10 x = 0;
    #10 x = 1;
    #10 x = 1;

    #10 x = 0;
    #10 x = 1;
    #10 x = 1;

    #20;
    #finish;
end
initial begin
    $mointor("Time=%0t Reset=%b Input=%b Output=%b,$time,rst,x,z);
end

endmodule
```

 Output Waveform
 <img width="900" height="846" alt="image" src="https://github.com/user-attachments/assets/45edfcd4-3bac-4c2e-bfcf-1d15cae1c17c" />

# Conclusion 
The Mealy and Moore state machine for sequence 1011 was designed and successfully simulated using Verilog HDL. The testbench verified both the write and read functionalities by simulating the sequence operations and observing the output waveforms.



