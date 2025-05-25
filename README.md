# Sequence-Detector
## 1.Aim
To design and simulate a sequence detector using both Moore and Mealy state machine models in Verilog HDL, and verify their functionality through a testbench using the Vivado 2023.1 simulation environment. The objective is to detect a specific sequence of bits (e.g., 1011) and compare the Moore and Mealy designs.

## 2.Apparatus Required:
Vivado 2023.1 or equivalent Verilog simulation tool.
Computer system with a suitable operating system.

## 3.Procedure:
Launch Vivado 2023.1:

## 4.Write the Verilog Code for Sequence Detector (Moore and Mealy FSM):

## Design two Verilog modules:


Design two Verilog modules: one for a Moore FSM and another for a Mealy FSM to detect a sequence such as 1011.

## Create the Testbench:

Write a testbench to apply input sequences and verify the output of both FSM designs.

## Add the Verilog Files:
Add the Verilog code for both FSMs and the testbench to the Vivado project.

## Run Simulation:

Run the simulation and observe the output to check if the sequence is detected correctly.

## Observe the Waveforms:

Analyze the waveform to ensure both the Moore and Mealy machines detect the sequence as expected.

## Save and Document Results:

Capture the waveforms and include the results in the final report.

## Verilog Code for Sequence Detector Using Moore FSM

// moore_sequence_detector.v
```
module moore_sequence_detector (
    	input clk,
    	input reset,
    	input seq_in, 
	output reg detected );
parameter  [2:0] S0=2'b000,
                 S1=2'b001,
                 S2=2'b010,
                 S3=2'b011,
                 S4=2'b100;

reg [2:0]  current_state, next_state;

// State transition logic
    always @(posedge clk or posedge reset) begin
        if (reset)
        begin
            current_state <= S0;
            detected = 0;
        end
        else
            current_state <= next_state;
    end
    // Next state and output logic
    always @(*) begin
          // Default: no detection
        case (current_state)
            S0: begin
                if (seq_in)
                    next_state = S1;
                else
                    next_state = S0;
            end
S1: 
	begin
                	if (seq_in)
                   	 	next_state = S1;
                	else
                    		next_state = S2;
           	 end
            S2: begin
                if (seq_in)
                    next_state = S1;
                else
                    next_state = S3;
            end
S3: begin
                if (seq_in) begin
                    next_state = S4;
                    detected = 1;  // Sequence 1001 detected
                end else
                    next_state = S0;
            end
            S4: begin
                next_state = S0;  // Return to the start after detection
            end
            default: next_state = S0;
        endcase
    end
endmodule
```
## Verilog Code for Sequence Detector Using Mealy FSM
```
// mealy_sequence_detector.v
`timescale 1ns / 1ps


module mealy4_detector(
input wire clk,
input wire rst,
input wire seq_in,
output reg z

    );
    
    parameter [2:0] s0=3'b000,
    s1=3'b001,
    s2=3'b010,
    s3=3'b011,
    s4=3'b100;
    
  reg [2:0] current_state,next_state;
    
    always @(posedge clk or posedge rst)
    begin
        if(rst)
        current_state<=s0;
        else
        current_state<=next_state;
        end
     always @(*)
     begin
     
     current_state<=next_state;
     z=0;
     
     case (current_state)
            s0: begin
                if (seq_in) next_state = s1;
            end
            s1: begin
                if (!seq_in) next_state = s2;
            end
            s2: begin
                if (seq_in) next_state = s3;
                else next_state = s0;
            end
            s3: begin
                if (seq_in) begin
                    next_state = s1; // Allow overlapping sequences
                    z = 1;           // Output goes high as soon as 1011 is detected
                end else
                    next_state = s2;
            end
        endcase
    end


           
              
endmodule

```

Testbench for Sequence Detector (Moore and Mealy FSMs)

// sequence_detector_tb.v
`timescale 1ns / 1ps

module sequence_detector_tb;
    // Inputs
    reg clk;
    reg reset;
    reg seq_in;

    // Outputs
    wire moore_detected;
    wire mealy_detected;

    // Instantiate the Moore FSM
    moore_sequence_detector moore_fsm (
        .clk(clk),
        .reset(reset),
        .seq_in(seq_in),
        .detected(moore_detected)
    );

    // Instantiate the Mealy FSM
    mealy_sequence_detector mealy_fsm (
        .clk(clk),
        .reset(reset),
        .seq_in(seq_in),
        .detected(mealy_detected)
    );

    // Clock generation
    always #5 clk = ~clk;  // Clock with 10 ns period

    // Test sequence
    initial begin
        // Initialize inputs
        clk = 0;
        reset = 1;
        seq_in = 0;

        // Release reset after 20 ns
        #20 reset = 0;

        // Apply sequence: 1011
        #10 seq_in = 1;
        #10 seq_in = 0;
        #10 seq_in = 1;
        #10 seq_in = 1;

        // Stop the simulation
        #30 $stop;
    end

    // Monitor the outputs
    initial begin
        $monitor("Time=%0t | seq_in=%b | Moore FSM Detected=%b | Mealy FSM Detected=%b",
                 $time, seq_in, moore_detected, mealy_detected);
    end
endmodule
## OUTPUT (Mealy):

![Screenshot (230)](https://github.com/user-attachments/assets/b4e0fd35-a6f5-41a6-b519-0e05009500a0)

## OUTPUT (Moore):

![Screenshot 2025-05-17 144643](https://github.com/user-attachments/assets/7a46ed2f-850e-4d99-8761-ef031c4b5f75)

## Conclusion
In this experiment, Moore and Mealy FSMs were successfully designed and simulated to detect the sequence 1011. Both designs worked as expected, with the main difference being that the Moore FSM generated the output based on the current state, while the Mealy FSM generated the output based on both the current state and input. The testbench verified the functionality of both FSMs, demonstrating that the Verilog HDL can effectively model both types of state machines for sequence detection tasks.
