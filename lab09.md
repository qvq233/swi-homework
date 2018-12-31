## 3、仔细观察您洗衣机的运作过程，运用Top-down设计方法和Pseudocode 描述洗衣机控制程序。
假设洗衣机可执行的基本操作如下：

water_in_switch(open_close) // open 打开上水开关，close关闭

water_out_switch(open_close) // open 打开排水开关，close关闭

get_water_volume() //返回洗衣机内部水的高度

motor_run(direction) // 电机转动。left左转，right右转，stop停

time_counter() // 返回当前时间计数，以秒为单位

halt(returncode) //停机，success 成功 failure 失败

1）请使用伪代码分解“正常洗衣”程序的大步骤。包括注水、浸泡等
```
//注水
//浸泡
//洗涤
//排水
```

2）进一步用基本操作、控制语句（IF、FOR、WHILE等）、变量与表达式，写出每个步骤的伪代码
```
//注水
WHILE get_water_volume() < maximum
    water_in_switch(open)
    water_out_switch(close)
ENDWHILE
//浸泡
WHILE time_counter() < 180
    motor_run(stop)
ENDWHILE
//洗涤
WHILE time_counter() < 1380
    IF time_counter() < 480
        motor_run(left)
    ELSE IF time_counter() < 780
            motor_run(right)
        ELSE IF time_counter() < 1080
                motor_run(left)
            ELSE
                motor_run(right)
            ENDIF
        ENDIF
    ENDIF
ENDWHILE
motor_run(stop)
//排水
WHILE get_water_volume() > 0
    water_in_switch(close)
    water_out_switch(open)
ENDWHILE
//检验
IF get_water_volume() = 0
    halt(success)
ELSE
    halt(failure)
ENDIF

```
3）根据你的实践，请分析“正常洗衣”与“快速洗衣”在用户目标和程序上的异同。你认为是否存在改进（创新）空间，简单说明你的改进意见？
1. 用户目标

    相同点：两种洗衣方式都是将衣服洗干净

    不同点：“快速洗衣”的时间比“正常洗衣”的时间要少，但洗衣效果质量比“正常洗衣”要低

2. 程序

    相同点：有一些相同的步骤，如注水，浸泡，洗涤，排水等。

    不同点：每一个步骤的具体操作略有不同，如浸泡时间，洗涤时间等。

改进意见：通过进一步的设计，使用户能够自行调节洗衣的时间、洗衣的步骤等，达到用户所需的洗衣效果。


4）通过步骤3），提取一些共性功能模块（函数），简化“正常洗衣”程序，使程序变得更利于人类理解和修改维护。例如：

wait(time) //等待指定的时间；

注水(volume,timeout) //在指定时间内完成注水，否则停机；

排水(timeout)。 等子程序

电机左转(time) // 电机左转指定时间
电机右转(time) // 电机右转指定时间
洗涤(time) //洗涤指定时间
浸泡(time) //浸泡指定时间