```verilog
// Code your design here
`timescale 1ns/1ps
module converter #(parameter WIDTH = 4)
  
  (
    				 input [WIDTH-1:0] data_in,
    				 output reg [WIDTH-1:0] data_out,
    				 input [1:0] mode,
 					 input clk,
 					 input rst_n				 
				);
  parameter CNT_WIDTH =log2(WIDTH);
  reg [WIDTH-1:0] data_out_r;
  reg done;
  reg [CNT_WIDTH-1:0] count;
  always @ (posedge clk , negedge rst_n)
    if(!rst_n)
      begin
      	data_out <= 4'd0;
  		data_out_r<= 4'd0;
       // done <= 1'b0;
        count <= 0;
      end
  	else case(mode)
      	2'd0:
          	begin
              data_out <= {data_out[3:1],data_in};
            end
      	2'd1:
          	begin
                  data_out <= data_in[count];
                  count <= count + 1'b1;
              if(count ==WIDTH-1)
               	begin	
               		 count <=0;        		
                end
              
            end
      	2'd2:
          	begin
              data_out_r <= data_in;
              data_out <= data_out_r;
            end
      	2'd3:
          	begin
              data_out_r<= data_in[0:WIDTH-1];
              data_out <= data_out_r;
            end
      default:
        data_out <= 0;
    endcase
  function integer log2;
    input integer value;
    begin
      value =value-1;
      for(log=2;value>0;log2=log2+1)
        value = value >>1;
    end
  endfunction
    
endmodule
