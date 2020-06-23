# 模块的结构

```verilog
module block(port1, .port2(signal2));
    //IO声明
    input [width-1:0] port1;
    output [width-1:0] port2;

    //内部信号声明
    reg reg_var;
    wire wire_var;
    
    //逻辑功能定义
    //assign声明语句
    assign assignment_statement;
    //用元件说明
    //逻辑门 #从输入到输出的延迟 门名称(out, in)
    and #2 and_gate(port2, port1);
    //过程块（略）
endmodule
```

# 不同抽象级别

| level    | description             |
| -------- | ----------------------- |
| 开关级   | 控制各种mos器件实现功能 |
| 门级     | 用各种门实现            |
| 数据流级 | 用assign和各种wire实现  |
| 行为级   | 用always块              |

# 运算

## 常量

```
int          位宽 进制 数字, e.g. 8'b1010_1100, -8'hxz
             默认为10进制32位
parameter    parameter name = expression
```

## 变量

```
wire      wire [n-1:0] var_name 或 wire[n:1] var_name
          n表示数据位宽，默认为1
          是电路电平（如门之间的连线处电平）的抽象
reg       声明同wire，是寄存器存储单元的抽象
          初始值为x
          只有寄存器变量才能赋值
memory    reg [n-1:0] mem_name[m-1:0]
          等同于含m个n位reg的数组
          索引方式和c语言的数组类似
          不能用在module的输入输出里
```



## 运算符

大部分同C
复习：位运算符
~反, &与, |或, ^异或, ^~同或
不同宽度的数据做位运算，较短的一个高位自动补0
移位运算符a>>n, a<<n，左移/右移n位，空出来的补0
不同之处：==, !=遇到x, z时结果为x; ===， !==要求完全一致
位拼接运算符 {1'b0, 1'b1}, {var_1, 4{var_2}, 2'b10}。例1结果位2'b01，例2中将var_1, 4个var_2, 2'b10拼接到一起（必须声明位数）
缩减运算符：&B，第一位与第二位与，结果与第三位与，如此类推直到最后一位

## 赋值

wire变量只能在assign语句赋值，reg才能在块里面赋值
non blocking     b <= a  //到块结束再做赋值
blocking         b = a   //立刻赋值

# 语句

## 块语句

```
顺序块 begin 语句1; 语句2; end    //顺序执行，每条前面#lag延迟相对于上一条命令，相当于C的大括号
并行块 fork ... join              //并行执行，每一条同时进行，#lag延迟相对于开始时间
begin 块名: 语句  //定义块名，可以定义局部变量，块能被别的语句调用，可以随时查看块内变量(引用方式：块.子块.变量)
```



## 条件，循环语句

if, while, for同C语言，x与z被判定为假
case 类似c，需要endcase
casez 忽略（不比较)z
casex 忽略z, x
forever 语句       无限循环，必须写在initial块
repeat(次数) 语句  重复指定次数

## 生成语句

相当于一种特殊的宏

genvar i    //用于生成的变量，之后把生成语句展开，它也就不存在了
generate 循环/条件/case endgenerate
好像不常用

## 结构说明语句

```
initial 语句            //开始时立即执行，只执行一次
always 时序控制 语句    //不断循环执行，时序控制举例:
#10
@(posedge signal1 or negedge signal2) //边沿敏感时间
@(clk, sig) //电平敏感事件，不能同时有边沿敏感和电平敏感

task和function:略

$ display(格式, 输出表列)
$ write
display输出完会自动换行，write不会。格式类似c语言printf的格式
```

# 串口通信基础

同步通信：两设备有同步的时钟，或传数据的同时传时钟信号。基于时钟传数据。
异步通信：没有同步的时钟，数据编码是self-clocking的

传输码
unipolar encoding: 高电平代表1，低电平代表0，需要同步时钟

bipolar encoding: 正电平代表1，负电平代表0，零电平含义依具体编码而定，没有了直流成分。之后的几种都属于bipolar encoding
return-to-zero: 简称RZ或RTZ，每传输一个比特(正电位或负电位)之后回到0电位。归零相当于把时钟数据编码进了信号里，称自同步(self-clocking)信号，不需要另一条线传输时钟，但带宽需求加倍
non-return-to-zero: 简称NRZ，和传输开始时发送一段SYNC同步域，把自己的频率告诉接收者，之后用高低电平传数据
non-return-to-zero inverted: 简称NRZI，类似NRZ，但用电平翻转表示0、电平保持表示1。因为它用电平变化表示数据，属相位调制型传输。由于没有时钟信号，频率的些许误差可能导致传输的信息错误，USB协议规定遇到连续多个1要做bit-stuffing，连续6个1之后强制插一个0
Manchester code: 在一个比特传输周期正中间由负跳变到正表示1，由正跳变到负表示0，周期开始/结束时可以跳变，但不传输数据。旧的标准里0和1是反过来的。类似RTZ，它能自同步；类似NRZI，它是相位型

I2C: 分两条线，时钟SCL(应该是指slave clock)，数据SDA(应该是指slave data)
SCL高电平时，SDA下降沿=开始传输，上升沿=结束传输
传输过程中，SCL高电平时SDA应该是稳定的。每个SCL上升沿处的SDA电平就是送过去的数据
主机向从机写数据：start, 7bit从机地址，1bit标识符(0表示写，1表示读)，1bit ACK应答(0表示成功)，8bit想要写入的寄存器地址，1bit ACK，8bit数据，1bit应答，最后主机发停止信号

UART: 单线传输，先约定好传输的波特率(比较常用的有9600bps和115200bps)，空闲为1，起始位为0(即拉低电平表示开始)，停止位为1(拉高电平表停止)
常用两条线RxD(Receive Data)和TxD(Transmit Data)进行双向传输