

�ο��̳̣�https://www.chipverify.com/verilog/verilog-tutorial

������ϰ&���棺https://hdlbits.01xz.net/wiki/Problem_sets

# Verilog���

Verilog��һ��Ӳ���������ԣ�Hardware Description Language��HDL���������﷨��C�������ƣ����Ǳ�������ȫ��ͬ�ģ�C���Ա������˳��ִ�е�ָ���Verilog�ۺϣ�synthesize������һ����·�������ԴӲ�ͬ���󼶱�����һ��Ӳ����·��

| level                                             | description                                                  |
| ------------------------------------------------- | ------------------------------------------------------------ |
| Switch Level�����ؼ�                              | ���Ƹ���mos����ʵ�ֹ���                                      |
| Gate Level���ż�                                  | �ø�����ʵ��                                                 |
| Register Transfer Level��RTL����������            | �����Ĵ�����������һ����assign�͸���wireʵ�֣�Ҳ��˵�������ۺϵĶ����Խ���RTL�� |
| Behavior Level / Algorithm Level����Ϊ�� / �㷨�� | ����ϵͳ�㷨��Ϊ����Ҫ��always��ʵ��                         |
| System Level��ϵͳ��                              | ����ģ�����ܵ�ģ�ͣ�û������ôʵ��                           |

Verilog�ǲ��Ϸ�չ�����ԣ����бȽ���Ҫ����Verilog-1995��Verilog-2001��������׼

# �﷨����

## �������ʶ��

�����ı�ʾ��ʽ��`[size]'[base_format][number]`��number�������»��߸����Է����Ķ�����������������b���˽���o��ʮ����d��ʮ������h

```verilog
8'b0000_0000;     // 8bit��ֵΪ0
9'h1FA;           // 9bit��ֵΪ0x1FA = 506
-8'd3;            // 8bit��ֵΪ3�ķ���
"Hello, world!";  // �ַ�����ascii��һ���ַ�1�ֽڣ������ò��ϣ�

// ���ֲ��õĳ���д��
'd132;            // ʡ����λ���ۺ������������һ��Ĭ��λ��������йأ�ͨ����32λ��
132;              // ʡ����λ��ͽ��ƣ��ᱻ����ʮ���ƣ�������Ĭ��λ��
8'b1;             // �����λ�����ֵ��Ȳ�ͬ����̫ȷ��verilog��׼��û�й涨�������

// x��ʾ����̬��z��?��ʾ�͸���̬
8'b1x11_xxxx;
16'h4azz;         // 16bitʮ������������8λΪ����̬

// ���ų���
// ��ʶ�������֡���ĸ�����ִ�Сд�����»��ߡ�$��ɣ�������$�������ֿ�ͷ�������뱣���ؼ�������
`define UART_CNT 10'd1024   // ��������������Ч�������ڱ��ģ��ʹ��
parameter MSB = unsigned integer 7;
parameter SIZE = MSB + 1;   // parameter�����ڱ�ģ�飬������ģ����������
localparam IDLE = 4'b0001;  // localparam�������ڲ������ݣ�������״̬��״̬����
```

## ��������

Verilog��������Ӳ��������������͵Ķ���͸߼����Խ�Ȼ��ͬ

### Wire

Wire�����ߵĳ��������ܴ���ֵ��ȡֵȡ��������������·��Driver����Wire������assign��������ߣ�Reg���У�ʵ������߼���·

```verilog
wire i1, i2;
wire out;
wire [1:0] data;

assign out = i1 & i2;
assign data = {i2, i1};

// �����һ��net�Ӷ������Դ������ͬ����Դ�ĵ�ƽ��ͬʱ���ߵĵ�ƽΪX
assign out 1'b1;   // ���i1 & i2 == 0��out = X

// implicit assignment
wire out_implicit = i1 & i2;
```

### Reg

Reg��һ���ܴ������ݵĵ�·�ĳ��󣬱�����������������������������Դ��Register

Reg��������ֵ���������ֵ�������ӷ�ʽ

```verilog
a <= c;   // non blocking����ͬ��������ֵ����ִ�У�˳������ν
b = c;    // blocking����һ��ִ����֮ǰ����Ĳ���ִ�У�������һ��Ľ����Ӱ��֮���ֵ��
```

ͨ����ʹ�ù�����

* ʱ���·��ʹ�÷�������ֵ

```verilog
always @(posedge clk) begin
    a <= ~a;
    b <= a;
end
```

<img src="./images/verilog_non-blocking.png" style="zoom: 33%;" />

�ɼ�ʱ���������ص���ʱb��D���뻹����һ�����ڵ�a�������ʼ����`b == ~a`

* ����߼���·��ʹ��������ֵ

```verilog
always @(*) begin
    a = a & in;
    b = a;
end
```

<img src="./images/verilog_blocking.png" style="zoom:33%;" />

���Υ����������ԭ�򣬱�����ʱ���߼���·��ʹ��������ֵ`a = ~a; b = a;`���ۺϺ����a����һ����������b��D�ˣ�����·�Ƚϸ��ӵ�ʱ������һ�������������߼��� / ���ұ�������߼�ʹ�÷�������ֵ������һ��always���л��ʹ�����ָ�ֵͬ��

### Scalar, Vector and Array

Scalar�ǿ��Ϊ1bit�����ݣ�Vector�ǿ�ȴ���1bit�����ݣ�Array�Ƕ�����ݵ�����

```verilog
wire clk;        // ����wire scalar
reg [7:0] addr;  // ����λ��Ϊ8��reg vector������������[msb:lsb]��1��8λ�Ĵ�����
reg y [7:0];     // �������Ϊ8��scalar reg array��8��1λ�Ĵ����ļĴ����飩
reg [7:0] mem [1:0][3:0]  // ����vector reg array��2��4�У�ÿ��8bit��

addr[0] = 1;     // ����msb
addr[7:4] = 4'hF // part-select
addr = 8'hff     // �������
y[7] = 1'b1;     // ����Array��ע��scalar array�������帳ֵ��vector����
mem[0][0] = 8'haa;  // ��0��0��Ԫ�ظ�ֵ0xAA
```

### ����

���ν���һЩ�������������ͣ����������õ����ǣ�ֻ�ǲ���˵����

Wire��Net���Ӽ��������г�����������Net���ͣ�

| name      | description                                        |
| --------- | -------------------------------------------------- |
| tri       | �ж�����������ߡ���wire��ȫһ������������߿ɶ��� |
| wand, wor | ���루wire and�����߻�wire or��                  |

Reg��Variable���Ӽ�����������Variable������

| name    | description                |
| ------- | -------------------------- |
| integer | 32 bits wide               |
| time    | 64 bits wide�����񲻿��ۺ� |
| real    | floating point�������ۺ�   |

## �����

��C�������������ͬ���ر��ڴ��г�λ�����

| Symbol     | Performance |
| ---------- | ----------- |
| &          | ��          |
| \|         | ��          |
| ~          | ȡ��        |
| ^          | ���        |
| `<<`, `>>` | ���ơ�����  |

��Ҫע�⣺`!`��ʾ�߼��ǣ�`~`��ʾȡ����ǰ�����������жϣ���������λ���㡣1bit�������߻���Ҳ�ܵõ���ȷ���������Ϊ�˱�����������ʱŪ�����Ƿ����Ϊ�á�`&&`��`&`������`||`��`|`������ͬ��

���м����ر�������

- ���λ�����������`~&`��ǣ�`~^`ͬ��
- ����̬�����̬�еȣ�`==, !=`��������̬������̬ʱ���Ϊ����̬��`===, !==`��������ȫ��ͬ���磬����x����z��ʱ�õ�1������Ϊ0������õ�����̬��ע��`===, !==`�ǲ����ۺϵ�
- λƴ�������`{}`����`{1'b0, 1'b1}, {var_1, 4{var_2}, 2'b10}`����1���Ϊ`2'b01`����2�н�1��var_1��4��var_2��һ��2'b10ƴ�ӵ�һ��ƴ�ӱ�������λ����
- �����������&B����һλ��ڶ�λ�룬��������λ�룬�������ֱ�����һλ

## ������

ע��verilog��û��break��continue����Ϊ��·�����ܴ�һ����������������

```verilog
// if��֧
if (state == 2'b00) begin
    out <= in0;
    state <= 2'b01;
end else if (state == 2'b01) begin
    out <= in1;
end else
    out <= in2;
    state <= 2'b00;
end

// ����һ����д��ʽ�����ÿ����ֻ֧��һ��
// ��Ҫ���ָ�ʽ����
if (state == 2'b00) out <= in0;
else if (state == 2'b01) out <= in1;
else out <= in2;

// case��֧
// ���ֲ����ۺϵ�case���֣�casez�Ƚ�zʱ�ж�Ϊ��ȣ�casex�Ƚ�x��zʱ�ж�Ϊ���
case (state)
    3'b000: begin
        out <= in0;
        state <= 2'b01
    end
    3'b001: out <= in1;
    3'b010, 3'b011: begin
        out <= in1;
        state <= 2'b00;
    end
    default: begin
        out <= in2;
        state <= 2'b00;
    end
endcase

// ע�⣺�����֧û��else / default���ۺ������ܻ���Ϊû�а���ȫ��������Ӷ�
// �ۺϳ�����ļĴ���

// whileѭ��
while (cnt < 10'h8ff) begin
    cnt <= cnt + 1'b1;
end

// forѭ������whileѭ����ͬ�������������ɶ����ͬ��·
// û̫��������ѭ�����������ǲ�����Ҫ�ۺϵĶ�����Ū�����ٲ���αʼ�
integer i;
for(i = 0; i < 8; i = i + 1) begin
    op[i] <= op[7-i];
end


```

�����ۺϵ�ѭ��

```verilog
// foreverѭ��
forever begin
    #100 vin = ~vin;
end

// repeatѭ��
repeat (100) begin
    #100 vin = ~vin;
end
```



# �����

```verilog
begin: block1
    // this is a block
    integer i = 1;
end

// �ڿ�����ʿ���ľֲ������������ۺϣ�
top.block1.i

// ���ÿ飨�����ۺϣ��������ڷ���ʱ�䵱����break��Ч��
while (1) begin: block2
    if (i == 0) disable block2
end
```

## Always Block

���ź�����ʱ���ᴥ����block�е����˳����У���ͬ��block����ִ�С��������������Ǳ������У���������߸����䣩���ߵ�ƽ���У���ƽ�仯�����������е��ź��Ǳ������С��е��ź��ǵ�ƽ����

```verilog
module tff(input d, input clk, input rst_n, output reg q);
    always @(posedge clk or negedge rst_n) begin
        if (!rst_n)
            q <= 1'b0;
        else
            q <= q;
    end
endmodule
```

������always blockʵ������߼���·��

```verilog
module combo (input a, b, c, d, e,
              output reg z);
    always @ (a or b or c or d or e) begin
        // ����д��always @ (*)���Զ�����������߼���������ĵ�ƽ����
        // ע�⣺z��reg�����ۺϳ�����һ������߼���������ǼĴ���
        z = ((a & b) | (c ^ d) & ~e);
    end
endmodule
```

<img src="./images/verilog_combo_with_always.png" style="zoom: 33%;" />

��ͼ��Quartus�ۺϵĽ����reg����z�ۺ�Ϊ����߼�������������ǼĴ������ߴ�������ʹ��Vivado�ۺϣ��õ�����һ�����ұ����������Ŀ��Ӳ�����ۺϽ����Ż����в�ͬ�ɡ�ͬʱҲ���Կ�����reg��һ����������/�������������Ҫ��������������

����ʱ������always���������

```verilog
always #10 clk = ~clk;
```

## Generate Block

Ӧ����һ������ĺ꣬�ۺϵ�ʱ��չ���ɺ��ʵĽṹ���������ã�

```verilog
genvar i;   // ����generate variable���ۺϺ�����������
generate
    for (i=0; i < 8; i++) begin
        // do something, e.g. instanciate an array of submodules
    end
endgenerate
```

## ���п�

�����ۺϣ������ڷ����ʱ����ơ����п��������ͬʱִ�У����������ʱ����0��ʼ

```verilog
fork
    vin = 1'b0;
    #10 vin = 1'b1;
    #50 vin = 1'b0;
join
```

## ��ʼ����

�е�Ӳ�������ۺϳ�ʼ���飬�е�Ӳ�����ܡ�����һ��Ҳֻ���ۺϼĴ�����ʼ���Ĵ���

```verilog
initial begin
    // ��ʼ���Ĵ���
end
```

# ģ�飨Module��

���ۺϵĴ��붼Ҫ������module�С�module������һ�������ض��߼����ܵĵ�·ģ�飬�������������������˿ڣ�Port���Լ��ڲ����߼���·��������һ��ͬ�������D������module��

```verilog
// �ɷ��verilog-95��ģ������
module dff(d, clk, rst_n, q);
    // ����IO
    input        d;
    input        clk;
    input        rst_n;
    output reg   q;

    // �߼�����
    always @ (posedge clk) begin
        if (!rst_n)
            q <= 0;
        else
            q <= d;
    end
endmodule
```

<img src="./images/verilog_dff_sync_schematic.png" style="zoom:67%;" />

һ��module�п��԰���������module������������һ��4bit��λ�Ĵ�����

```verilog
// verilog-2001����ģ������
module shift_reg(
    input    d,
    input    clk,
    input    rst_n,
    output   q
);  // ��Ҫ����ģ���������ķֺţ���

    // �����ڲ�����
    wire [2:0] q_net;

    // ʵ����(instantiate)D������
    dff u0 (d, clk, rst_n, q_net[0]);   // Port connection by ordered list
    dff u1 (.d(q_net[0]), .clk(clk), .rst_n(rst_n), .q(q_net[1]));  // by name
    dff u2 (.d(q_net[1]), .clk(clk), .rst_n(rst_n), .q(q_net[2]));
    dff u3 (.d(q_net[2]), .clk(clk), .rst_n(rst_n), .q(q));
endmodule
```

<img src="./images/verilog_dff_shift_reg_schematic.png" style="zoom:60%;" />

�˿�����ʱ����������`.name()`�ķ�ʽ��ʾ��������

`input`��`inout`���͵Ķ˿�Ҫ���ⲿ��·��������˲�����reg����

����ʱ������һ��top-level module��������ϵͳ������ģ�鲻�ᱻ������ģ��ʵ������ģ�����ʱʹ�õ�testbenchҲ��һ�ֶ���ģ��

```verilog
// �������parameter��ģ��
module counter #(
    parameter N = 10
)(
    input clk, rst_n, en,
    output reg[N-1:0] out
);
    
    always @(posedge clk) begin
        if (!rst_n) out <= 0;
        else if (en) out <= out + 1'b1;
        else out <= out;
    end
endmodule

// ʵ��������parameter��ģ��
counter #(
    .N(4)
) cnt4b (
    .clk(clk),
    .rst_n(rst_n),
    .en(en)
);
```



# ����Task���뺯����Function��

����ͺ�����Ҫ���ڷ��档ʵ�ֿ��ۺϵĴ���ʱ��Ӧ��ʹ��ģ�飬��Ҫʹ�������뺯������д�����ۺϵĴ���ʱ�������뺯����ģ��д��������һЩ

```verilog
module testbench ();
    reg [7:0] Mux_Addr_Data = 0;
    reg       Addr_Valid = 1'b0;
    reg       Data_Valid = 1'b0;

    // ����task
    task do_write;
        // ��addr��ַд��data
        input [7:0] addr, data; 
        begin
            Addr_Valid = 1'b1;
            Mux_Addr_Data = addr;
            #10;
            Addr_Valid = 1'b0;
            Data_Valid = 1'b1;
            Mux_Addr_Data = data;
            #10;
            Data_Valid = 1'b0;
            #10;
        end
    endtask
    
    // ����function
    // ����һ�����ڼ���/����߼�������ʹ��ʱ����ƣ�Ҳ���ܵ�������
    // ����һ�����룬����ֵ���뺯����ͬ������
    // �������ܵݹ���ã�Ҳ�����ڶദ��ͬʱ����
    function calc_parity(input [7:0] data);
        calc_parity = ^data;
    endfunction

    initial begin
        #10;
        do_write(8'h00, 8'hAB);     // ����task
        parity = calc_parity(8'hAB);// ����function
        do_write(8'h01, 8'hBC);
        do_write(8'h02, 8'hCD);
    end
endmodule
```



# ����

## testbench��д

һ�㶨��һ��ר�����������testbench����ģ�飬��������������ź��ṩ�������Ե�ģ�飬�۲��������Ƿ���ȷ�������ֻ���ڷ���ʱ���еĲ����������г����õļ���

```verilog
module testbench();
    wire clk, foo, bar;             // ��������ʱ��Ҫ�õ��ı���
    some_sub_module sub(foo, bar);  // �������ģ��
    
    always #10 clk = ~clk;

    initial begin
        clk <= 0;        // ����ʱ����������ֱ�Ӹ�ֵ���������������ֵ�ͻ���X
        foo <= 0;
        bar <= 0;
        #10 foo = 1;     // ��ʽ��ʱ�������ۺ�
        #20 bar = 1;
        #400 $finish;    // ��������
    end

    always @(posedge clk) begin
        $display("%d", foo);
        $write("%d", bar);  // display�������Զ����У�write����
    end
endmodule
```

ModelSimʹ��

1. File - New - Library��ѡ���ļ��У�֮�����������ļ�����������Ҳ���Ը������������ȷ��֮������ļ��л���ʾ��Library���
2. Compile - Compile��ѡ��������Ҫ�����Դ�ļ�������ɹ�֮��Library����»������Ǳ������module
3. ˫��top-level module�����Wave��壬Sim����Objects���
4. ��Objects���ѡ����Ҫ�鿴�Ĳ��Σ���ӵ�Wave��壬Simulate - Run

# ״̬��

## ����

ʱ���߼���·�����Գ���״̬�������Ľṹ���Գ������ͼ

![�½� Microsoft Visio Drawing](images/FSM.svg)

һ�㽫״̬������Ϊ

- Mealy״̬��������������йأ�`O = O(I, S)`
- Moore״̬��������������޹أ�`O = O(S)`

Moore״̬������ȫͬ���ģ�����ȶ��ԺͿ�������������ǿ��״̬S����ʱ��ͬ���ģ����`O(S)`��ʱ�Ӳ���ͬʱ���䣬����̬�ɿء���֮������Mealy�ͣ����`I`��ʱ���ظ������䣬`O(I, S)`�п��ܺ�ʱ��ͬʱ���䣬����һ����������̬

��״̬ͼ��ʾ��

```mermaid
stateDiagram-v2
    s1: ״̬1/Moore���
    s2: ״̬2/Moore���
    s1 --> s2: ����/Mealy�����
```

## ״̬����д��

```verilog
//------------------------------------------------------------------------------
// һ��ʽ״̬��
// ��״̬ת��������״̬ת�������д��һ������
// ���������Խϸߣ����÷���������Լ��ʱ��������д�����淶�Ĵ���
// �����Ǽ��о���д���С״̬����������Ҫ��
wire in;
reg state;
reg out;
always @(posedge clk) begin
    case (state)
        STATE_IDLE: begin
            if (start) begin
                state <= STATE_1;   // ״̬ת������ & ״̬ת��
                out <= 1;           // ���
            end else begin
                state <= STATE_IDLE;
                out <= 0;
            end
        end
        STATE_1:
            state <= IDLE;
            out <= 0;       // ע�⣺����һ��״̬��������ڴ˶�ӦIDLE̬���
        end
        default: begin
            state <= STATE_IDLE;
            out <= 0;
        end
    endcase
end

//------------------------------------------------------------------------------
// ����ʽ״̬��
// ��ʱ���·��״̬ת����д��һ�����ڣ���ϵ�·��״̬ת�������������д����һ������
// �������࣬�ұȽϹ淶����������߼�����������ë��
reg state;
reg next_state;

always @(posedge clk) begin
    // ״̬ת��
    state <= next_state;
end

always @(*) begin
    // ״̬ת������
    case (state)
        STATE_IDLE: next_state = start ? STATE_1 : STATE_IDLE;
        STATE_1:    next_state = STATE_IDLE;
        default:    next_state = STATE_IDLE;
    endcase
    
    // �����ע������д�����������ϵ�·����ë��
    // �����case�ŵ�������always�����棬����ȥҲ��������ʽ����Ч������һ��
    case (state)
        STATE_IDLE: out = 0;
        STATE_1:    out = 1;
        defualt:    out = 0;
    endcase
end

//------------------------------------------------------------------------------
// ����ʽ״̬��
// ��״̬ת��������״̬ת��������ֿ�����������
// ��淶���Ҿ�ΪMoore���
reg state;
reg next_state;

always @(*) begin
    // ״̬ת������
    case (state)
        STATE_IDLE: next_state = start ? STATE_1 : STATE_IDLE;
        STATE_1:    next_state = STATE_IDLE;
        default:    next_state = STATE_IDLE;
    endcase
end
    
always @(posedge clk) begin
    // ״̬ת��
    state <= next_state;
end

always @(posedge clk) begin
    // ���
    case (next_state)   // ע��ʹ��next_state���жϣ�������һ�����ڵ��ӳ�
        STATE_IDLE: out <= 0;
        STATE_1:    out <= 1;
        default:    out <= 0;
    endcase
end
```



��������

```verilog
//------------------------------------------------------------------------------
// ͬ����λ���첽��λ

// ����һ��ʽ״̬�����������ڸ�λ�ź��Ƿ����������б���
always @(posedge clk) begin                 // ͬ����λ
    if (rst) state <= STATE_IDLE;
    else ; // ״̬������
end

always @(posedge clk or posedge rst) begin  // �첽��λ
    if (rst) state <= STATE_IDLE;
    else ; // ״̬������
end

// ���ڶ��ʽ״̬����ͬ����λд������߼����첽��λд��ʱ���߼�
always @(*) begin                           // ͬ����λ
    if (rst) next_state = STATE_IDLE;
    else ; // ״̬ת����������
end

always @(posedge clk or posedge rst) begin  // �첽��λ
    if (rst) state <= STATE_IDLE;
    else ; // ״̬ת������
end

//------------------------------------------------------------------------------
// ���ٵ�״̬������״̬�������ϵ����
module fast_fsm(
    input clk,
    input rst_n,
    input A,
    output [1:0] K
);
    reg [4:0] state;
    assign K = state[4:3];
    
    // Ȼ����״̬��
endmodule
```

