```verilog
`timescale 1ns/1ps
module converter #(  parameter WIDTH = 4,
                     parameter CNT_WIDTH  = clogb2(WIDTH-1))
  
  (
    				 input [WIDTH-1:0] data_in,
    				 output reg [WIDTH-1:0] data_out,
    				 input [1:0] mode,
 					 input clk,
 					 input rst_n				 
				);
 // parameter CNT_WIDTH  = clog2(WIDTH-1);
  reg [WIDTH-1:0] data_out_r;
  //reg done;
  reg [CNT_WIDTH-1:0] count;
  //-------------------------------
  always @ (posedge clk , negedge rst_n)
    if(!rst_n)
      begin
      	data_out <= 4'd0;
  		data_out_r<= 4'd0;
       // done <= 1'b0;
     //   count <= 0;
      end
  	else case(mode)
      	2'd0:
          	begin
              data_out <= {data_out[2:0],data_in[0]};
            end
      	2'd1:
          	begin
              data_out <= data_in[count];
                 
            end
      	2'd2:
          	begin
              data_out_r <= data_in;
              data_out <= data_out_r;
            end
      	2'd3:
          	begin
              data_out_r<= data_in;
              data_out[WIDTH-1-count] <= data_out_r[count];
            end
      default:
        data_out <= 0;
    endcase
  //--------------------------------
  //****state change
  reg [1:0]mode_r1;
  reg [1:0]mode_r0; 
  always @ (posedge clk , negedge rst_n)
    if(!rst_n)
      begin
        mode_r1 <= 2'd0;
        mode_r0 <= 2'd0;
      end
  	else 
      begin
        mode_r0 <= mode;
        mode_r1 <= mode_r0;
      end
//----------------------------------
//*********counter
  always @ (posedge clk ,negedge rst_n)
    if(!rst_n)
      count <= 0;
  else if(mode_r0 ^ mode_r1 )
    count <= 0;
  else
    count <= count +1'b1;
//-----------------------------------------
 function integer clogb2 (input integer bit_depth);
begin
    for(clogb2=0; bit_depth>0; clogb2=clogb2+1)
        bit_depth = bit_depth>>1;
end
endfunction
    
endmodule
```
