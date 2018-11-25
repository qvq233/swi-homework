# 机器语言编程实验报告

## 任务1：简单程序

(2)
1. PC寄存器的作用：存放要读取的下一条指令的地址

IR寄存器的作用：存放当前正在执行的一条指令

2. ACC寄存器全称是Accumulator,作用是存放操作数和运算结果

3. LOD #3指令执行过程：
```graph
graph TD;
    PC-->RAM;
    RAM-->IR;
    Decoder-->MUX;
    Decoder-->ALU;
    IR-->MUX;
    MUX-->ALU;
    PC-->+2;
    +2-->PC;
```
4. 