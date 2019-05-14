[时序图工具](https://wavedrom.com/)

{signal: [
  {name: 'clk',         wave:  'P....P...'},
  {name: 'clk1',        wave: 'H.LH.LH.L'},
  {name: 'clk2',        wave: 'lh.lh.lh.l',phase:0.5},
  {},
  {name: 'clk1 & clk2', wave: 'nhlnhlnhp'}
]}
```verilog
```verilog
// Code your design here
`timescale 1ns/1ps
module div3_half(
								input Sys_clk,
								input Sys_reset,
								output div3 ,
  								output clk1,
  								output clk2
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
  else if(count == 2'd1 | count == 2'd2)
		clk1 <= ~clk1;
		
always @(negedge Sys_clk )
	if(!Sys_reset)
		begin
			clk2 <=1'b1;
		end 
  else if(count == 2'd2 | count ==2'd1)
		clk2 <= ~clk2;
		
//------------------------------------------------
assign div3 =clk1 & clk2;
endmodule

```
```verilog
// Code your testbench here
// or browse Examples
`timescale 1ns/1ps
module tset();
  reg Sys_clk;
  reg Sys_reset;
  wire div3;
  initial
    begin
      $dumpfile("d.vcd");
      $dumpvars(1);
      Sys_clk=0;
      Sys_reset = 0;
      #100
      Sys_reset =1;
    end
  always #10 Sys_clk = ~Sys_clk;
  div3_half div3_half_inst(Sys_clk,Sys_reset,div3,clk1,clk2);
endmodule
```
