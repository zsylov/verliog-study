# 设计占空比为50%的三分频电路

[时序图工具](https://wavedrom.com/)

{signal: [  
  {name: 'clk',         wave:  'P....P...'},  
  {name: 'clk1',        wave: 'H.LH.LH.L'},
  {name: 'clk2',        wave: 'lh.lh.lh.l',phase:0.5},  
  {},  
  {name: 'clk1 & clk2', wave: 'nhlnhlnhp'}  
]}  
目前各个FPGA厂家一般都有集成的锁相环资源，但在设计对于时钟要求不高的基本设计，通过逻辑进行时钟分频依然有效，还可以节省芯片内部的锁相环资源，其中分频又分为，<kbd>偶数分频</kbd>,<kbd>奇数数分频</kbd>，<kbd>小数分频</kbd>，此次主要涉及奇数分频，设计一个占空比为50%的三分频电路，仿真环境采用edaplayground.com.
* 奇数分频原理
分别采用上升沿进行一个占空比为2/3的始终，在次用下降样设计同样的占空比，最后将两者进行相与，得到占空比为50%的三分频电路。
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
