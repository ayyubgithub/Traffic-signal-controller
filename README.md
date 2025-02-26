# Traffic_signal_controller

![Screenshot (2474)](https://github.com/user-attachments/assets/e1430f97-b4e2-438f-91f3-696339f81b9f)

> Given Specification
- The traffic signal for the main highway gets highest priority because cars are continuously present on the main highway. Thus, the main highway signal remains green by default.
- Occasionally, cars from the country road arrive at the traffic signal. The traffic signal for the country road must turn green only long enough to let the cars on the country road go.
- As soon as there are no cars on the country road, the country road traffic signal turns yellow and then red and the traffic signal on the main highway turns green again.
- There is a sensor to detect cars waiting on the country road. The sensor sends a signal X as input to the controller. X = 1 if there are cars on the country road; otherwise, X=0.
- There are delays on transitions from S1 to S2, from S2 to S3, and from S4 to SO. The delays must be controllable.

 > FINITE STATE MACHINE (FSM) DIAGRAM.
  
![Screenshot (2476)](https://github.com/user-attachments/assets/a3a03f14-445f-4ad8-9438-f269a408c011)

## Verilog Code
```Verilog

`timescale 1ns / 1ps
//////////////////////////////////////////////////////////////////////////////////
// Company: 
// Engineer: 
// 
// Create Date: 02/26/2025 09:52:12 AM
// Design Name: 
// Module Name: Signal_control
// Project Name: 
// Target Devices: 
// Tool Versions: 
// Description: 
// 
// Dependencies: 
// 
// Revision:
// Revision 0.01 - File Created
// Additional Comments:
// 
//////////////////////////////////////////////////////////////////////////////////

`define TRUE 1'b1
`define FALSE 1'b0
`define RED 2'd0
`define YELLOW 2'd1
`define GREEN 2'd2

`define S0 3'd0
`define S1 3'd1
`define S2 3'd2
`define S3 3'd3
`define S4 3'd4

`define Y2RDELAY 3
`define R2GDELAY 2

module Signal_control(hwy, cntry, X, clock, clear);
  
  output reg [1:0] hwy, cntry;  // Fixed reg declaration

  input X;
  input clock, clear;

  reg [2:0] state;
  reg [2:0] next_state;
  reg [3:0] counter;  // Added counter for delay implementation

  initial begin
    state = `S0;
    next_state = `S0;
    hwy = `GREEN;
    cntry = `RED;
  end

  always @(posedge clock)
    state <= next_state;  // Fixed non-blocking assignment

  always @(state) begin
    case (state)
      `S0: begin 
          hwy = `GREEN;
          cntry = `RED;
      end
      `S1: begin 
          hwy = `YELLOW; 
          cntry = `RED; 
      end
      `S2: begin 
          hwy = `RED;
          cntry = `RED;
      end
      `S3: begin
          hwy = `RED;
          cntry = `GREEN;
      end
      `S4: begin 
          hwy = `RED;
          cntry = `YELLOW;
      end
      default: begin  // Added default case
          hwy = `RED;
          cntry = `RED;
      end
    endcase 
  end

  always @(state or clear or X) begin 
    if (clear)
      next_state = `S0;
    else begin
      case (state)
        `S0: if (X)
          next_state = `S1;
        else
          next_state = `S0;
        
        `S1: begin
          counter = 0;
          while (counter < `Y2RDELAY) counter = counter + 1; 
          next_state = `S2;
        end

        `S2: begin
          counter = 0;
          while (counter < `R2GDELAY) counter = counter + 1;  
          next_state = `S3;
        end
        
        `S3: if (X)
          next_state = `S3;
        else
          next_state = `S4;
        
        `S4: begin
          counter = 0;
          while (counter < `Y2RDELAY) counter = counter + 1; 
          next_state = `S0;
        end

        default: next_state = `S0;
      endcase
    end
  end

endmodule

## Test Bench

```Verilog

`timescale 1ns / 1ps
//////////////////////////////////////////////////////////////////////////////////
// Company: 
// Engineer: 
// 
// Create Date: 02/26/2025 11:11:37 AM
// Design Name: 
// Module Name: Signal_control_sim
// Project Name: 
// Target Devices: 
// Tool Versions: 
// Description: 
// 
// Dependencies: 
// 
// Revision:
// Revision 0.01 - File Created
// Additional Comments:
// 
//////////////////////////////////////////////////////////////////////////////////


module Signal_control_sim;
     reg clock, clear ; 
     reg CAR_ON_CNTRY_RD ;
     wire [1:0] hwy, cntry ;
     
 Signal_control uut(.clock(clock), .clear(clear), .X(CAR_ON_CNTRY_RD), .hwy(hwy), .cntry(cntry)) ;
 
   initial begin 
       clock = 0;
    forever #5 clock = ~clock;
  end
        
     initial begin
    clear = 1;
    repeat (5) @(negedge clock);
    clear = 0;
  end
  
   initial begin
    CAR_ON_CNTRY_RD = 0;
    #20 CAR_ON_CNTRY_RD = 1;
    #40 CAR_ON_CNTRY_RD = 0;
    #60 CAR_ON_CNTRY_RD = 1;
    #80 CAR_ON_CNTRY_RD = 0;
    #100 CAR_ON_CNTRY_RD = 1;
    #120 CAR_ON_CNTRY_RD = 0;
    #200 $finish;
  end

endmodule


