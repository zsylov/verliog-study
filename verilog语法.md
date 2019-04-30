verilog语法
=====
1.文件操作fopen,fdisplay,fwrite,fclose.
----

2.生成随机数random;
-----

3.初始化readmemh,readmemb.
----

用verilog实现串并转换
====
```verilog
input [3:0] data_in;
  output [3:0] data_out;
  input [1:0] mode;
  input clk;
  input rst_n;
```
mode 0 ：串行输入data_in[0]，并行输出data_out[3:0]
mode 1 ：并行输入data_in[3:0]，串行输出data_out[0]
mode 2 ：并行输入data_in[3:0]，并行输出data_out[3:0]，延迟1个时钟周期
mode 3 ：并行输入data_in[3:0]，并行反序输出data_out[3:0]，延迟1个时钟周期并且交换bit顺序
data_out[3]=data_in[0]; 
data_out[2]=data_in[1]
data_out[1]=data_in[2]
data_out[0]=data_in[3]
附加要求【选做】
将输入输出的位宽做成参数化

3. 记录一下第2题中用到的工具，包括工具版本，操作步骤或命令选项，遇到的错误，提示信息等。比较一下，与昨天的记录有何相同，有何不同。
=================================
