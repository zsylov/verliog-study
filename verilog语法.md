verilog语法
=====
1.文件操作fopen,fdisplay,fwrite,fclose.
----
* fopen
用法1：<文件句柄>=$fopen("<文件名>");
用法2：$fopen("<File Name>");
  注意：用$fopen打开文件会将原来的文件清空，若要读数据就用$readmemb,$readmemh就可以了，这个语句不会清空原来文件中的数据。
用$fopen的情况是为了取得句柄，即文件地址，也就是写文件时用$fdisplay（desc,"display1"）；时才用。
* fdisplay
  用法：$fdisplay(<文件描述符(句柄，用于确定是写哪一个文件)>，p1,p2（写入内容）);例如：
  例如：
H1=$fopen("File Name");     %取一个文件的句柄
$fdisplay("Data");     %将数据写入文件
  fdisplay写完会自动换行。
* fwrite
  用法：与fdisplay相似，但写完不会自动换行。
```verilog
  integer data_w;
  integer i;
  initial
    begin
      data_w = $fopen ("wenjianluojing","w");
    for(i = 0;i < 100; i = i + 1)
      begin
         @(clk)
           $fwrite(data_w,"%d\n",Sig[i]);
      end
               $fclose(data_w);
    end
    
```
* fclose
  fclose(<FileHandle>);关闭文件，<File Handle>为所获得的句柄。

2.生成随机数random;
-----
当函数被调用时返回一个32bit的随机数。它是一个带符号的整形数。
用法：$ramdom% b ,其中 b>0.它给出了一个范围在（-b+1):(b-1)中的随机数。
      
3.初始化readmemh,readmemb.
----
读取的内容只包括：空白位置（空格、换行、制表格(tab和form-feeds),注释行、二进制或十六进制的数字。
数字中不能包含位宽说明和格式说明，其中readmemb要求每个数字是二进制数，readmemh要求每个数字必须是十六进制数字。数字中不定值x或X，高阻值z或Z，和下划线(_)的使用方法和代表意义与一般Verilog HDL程序中的用法一致。
（1）$readmemb("<数据文件名>",<存储器名>);
（2）$readmemb("<数据文件名>",<存储器名>,<起始地址>);
（3）$readmemb("<数据文件名>",<存储器名>,<起始地址>,<终止地址>);
（4）$readmemh("<数据文件名>",<存储器名>);
（5）$readmemh("<数据文件名>",<存储器名>,<起始地址>);
（6）$readmemh("<数据文件名>",<存储器名>,<起始地址>,<终止地址>);

串并转换
===
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
```verilog
// Code your design here

```
```verilog
// Code your testbench here
// or browse Examples

```

记录一下第2题中用到的工具，包括工具版本，操作步骤或命令选项，遇到的错误，提示信息等。比较一下，与昨天的记录有何相同，有何不同。
=================================
