# 机器语言编程实验报告

## 任务1：简单程序

(2)
1. PC寄存器的作用：存放要读取的下一条指令的地址

IR寄存器的作用：存放当前正在执行的一条指令

2. ACC寄存器全称是Accumulator,作用是存放操作数和运算结果

3. LOD #3指令执行过程：

    PC&rarr;RAM&rarr;IR&rarr;Decoder&rarr;MUX

    Decoder&rarr;ALU

    IR&rarr;MUX&rarr;ALU

    PC&rarr;+2&rarr;PC

4. ADD W指令执行过程:

    PC&rarr;RAM&rarr;IR&rarr;Decoder&rarr;MUX

    Decoder&rarr;ALU&rarr;ACC&rarr;ALU

    IR&rarr;RAM&rarr;MUX&rarr;ALU&rarr;ACC

    PC&rarr;+2&rarr;PC

5. LOD #3和ADD W指令的执行在Fetch-Execute周期级别，前者不需要从RAM中读取数据，后者需要从RAM中读取数据W


(3)

![](https://img-blog.csdnimg.cn/20181125172058253.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzM0NzA5NQ==,size_16,color_FFFFFF,t_70)

1. LOD #7指令二进制形式为00010100 00000111
    第四位表示寻址模式，1表示操作数是数值，其后的0100是操作码，最后面的00000111是7的二进制形式。
    
2. RAM有存放指令的地址，也有存放数值的地址，相邻的地址之间间隔2

3. CPU是八位的

4. C语言表达

```
int w = 3;
int x = 7;
int y = x + w;
```

## 任务2：简单循环

(1)
1. 该程序的功能为将x逐渐递减为0

2. C语言表达

```
int x=3;
while (x>0)
    x--;
```

(2)
1. C语言程序

```
int x = 10,y=0;
while (x>0){
    y += x;
    x--;
}  
```

2. 机器语言

![](https://img-blog.csdnimg.cn/20181125171850956.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzM0NzA5NQ==,size_16,color_FFFFFF,t_70)

3. 高级语言与机器语言

联系：都是编程的语言，两者之间能够转化。

区别：高级语言对于人更加易懂，但不能被机器直接识别；机器语言能被机器直接识别，但是人比较难读懂。


