2019.4.26时序电路
====
1.dff和latch有什么区别
-----
* dff：在时钟信号作用下，输出结果根据输入状态变化，同步控制。latch:锁存器由电平触发，非同步控制。
* latch容易产生毛刺（glitch），DFF则不易产生毛刺.
* 如果使用门电路来搭建latch和DFF，则latch消耗的门资源比DFF要少，这是latch比DFF优越的地方。
* 所以，在ASIC中使用latch的集成度比DFF高，但在FPGA中正好相反，因为FPGA中没有标准的latch单元，但有DFF单元，一个LATCH需要多个LE才能实现。latch将静态时序分析变得极为复杂。    
* 在绝大多数设计中避免产生latch。latch最大的危害在于不能过滤毛刺。这对于下一级电路是极其危险的。所以，只要能用D触发器的地方，就不用latch。
  
2.什么是同步电路，异步电路？
--------
  * 同步电路：数据的流动主要靠一个全局时钟来同步，例如：
    ```verilog
      always @ (psoedge clk)
    ```
    ![同步电路]()
  * 异步电路：异步电路没有统一的时钟，信号的触发不仅通过一个时钟，例如：
    ```verliog
        always @ (posedge clk , negedge rst)
    ```
    ![异步电路]()
