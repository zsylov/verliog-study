[时序图工具](https://wavedrom.com/)

{signal: [
  {name: 'clk',         wave:  'P....P...'},
  {name: 'clk1',        wave: 'H.LH.LH.L'},
  {name: 'clk2',        wave: 'lh.lh.lh.l',phase:0.5},
  {},
  {name: 'clk1 & clk2', wave: 'nhlnhlnhp'}
]}
```verilog
`timescale 1ns/1ps
module div3_half(
								input Sys_clk,
								input Sys_reset,
								output div3 
							);
reg clk1;//2/3 is high posedge
reg clk2;//2/3 is high negedge
//counter
reg [1:0]count;
always @ (posedge Sys_clk )
	if(!Sys_reset)
		count <= 2'b0;
	else if(count ==2'd2) 
				count <= 2'b0;
			else
				count <= count +1'b1;
				
always @(posedge Sys_clk )
	if(!Sys_reset)
		begin
			clk1 <=1'b1;
		end 
	else if(count == 2'd2 |count == 2'd0)
		clk1 <= ~clk1;
		
always @(negedge Sys_clk )
	if(!Sys_reset)
		begin
			clk2 <=1'b1;
		end 
	else if(count == 2'd2 |count == 2'd0)
		clk2 <= ~clk2;
		
//------------------------------------------------
assign div3 =clk1 & clk2;
endmodule

```
