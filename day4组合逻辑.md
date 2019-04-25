day4组合逻辑
==
1.什么事竞争和冒险
------
 答：竞争（competition）:在组合逻辑中，某个输入经过多条传输途径后，由于每条传输途径的延迟时间不同，到达输出们的时间就有先后。其中有些竞争不会输出错误的结
 成为`非临界竞争`，有些竞争会产生暂时或永久的错误输出这种竞争成为`临界竞争`。
    冒险（risk）:有竞争产生的毛刺。
    
2.设计一个2-4译码器
-----
```verilog
module decode(
                input Sys_clk,
                input Sys_reset,
                input [1:0] data_in,
                output [3:0] data_out
    );
    
  reg [3:0] data_r;
  always @ (posedge Sys_clk , negedge Sys_reset)
  	if(!Sys_reset)
  		data_r <= 4'd0;
  	else case(data_in)
  				2'd0: data_r <= 4'b0001;
  				2'd1: data_r <= 4'b0010;
  				2'd2: data_r <= 4'b0100;
  				2'd3: data_r <= 4'b1000;
  				default : data_r <= 4'b0000;
  			endcase
  			
  assign data_out = data_r;
endmodule
```
3.输入一个8bit数，输出其中1的个数，如果只能使用1bit全加器，最少需要几个？
------
 0~2用一个全加器，3~5用一个全加器，6,7，位分别与前面两个全加器输出的2位组合,输出的4位再组合，最后总共用到7个。
4.如果一个标准单元库只有三个cell：2输入mux(o=s?a:b;),TIEH(输出常数1)，TIEL（输出常数0），如何实现以下功能。
-------
  * 4.1反相器inv
  
  * 4.2缓冲器buffer
  
  * 4.3 两输入与门and2
  
  * 4.4 两输入或门or2
  
  * 4.5 四输入的mux mux4
  
  * 4.6 一位全加器 fa
