# Windows hotkeys

win + R：运行。建立一个文件夹，把它加入系统环境变量PATH中，再把想要的东西的快捷方式放进去，就能够直接从win + R启动

win + Tab: 虚拟桌面

# mingw编译&make

常用编译选项

| 选项 | 作用                 |
| ---- | -------------------- |
| `-c` | 生成目标文件，不链接 |
| `-l` | 链接库文件           |
| `-o` | 指定输出文件名       |
| `-I` | 头文件搜索目录       |
| `-L` | 库文件搜索目录       |

```powershell
# 例1：编译+链接
g++ -c main.cpp  # 编译得到目标文件main.o
g++ -c func.cpp  # 编译得到目标文件func.o
g++ main.o func.o -o release.exe

# 例2：编译+链接库文件。例子中与pdcurses.dll进行链接
gcc main.cpp -I "./include" -L "./lib" -lpdcurses
```

## make

```powershell
mingw32-make -f Makefile
```

最简单的编译命令由三个部分组成：

1. 目标：相当于标识符，通常用目标文件名
2. 依赖项：用于判断目标构建次序、判断目标是否需要重新构建（如果依赖项的时间戳比目标文件的更新，就需要重新构建）。如果.PHONY作为目标，不会进行检查，总会重新构建
3. 指令：构建目标的指令

```makefile
target: prerequisites    # 注意依赖项最好包括头文件
	commands             # 必须一个tab缩进！不可以用空格

# 例：
test.exe: main.cpp func.cpp func.h
	g++ -g -o test.exe main.cpp func.cpp
```

* 模式匹配

```makefile
%.o: %.c
	gcc -c $< -o $@
```

其中的`%.c`匹配了当前目录下的所有.c文件，`%.o`表示目标是同名.o文件（%相当于wildcard）。不过，带匹配符的目标无法直接构造，完整用法如下

```makefile
all: $(subst .c,.o,$(wildcard *.c))
# wildcard搜索出了全部.c文件，subst将后缀改为.o，因此依赖项就是所有.o文件
# make的时候首先构造all，依赖项是各种.o文件，再从匹配符构造.o文件

%.o: %.c
	gcc -c $< -o $@
```

* 变量

虽然叫做变量，但是实际上更类似于宏，因为只是文本替换

```makefile
HELLO = hello, world!
test:
	@echo $(HELLO)
	# 默认会打印每一条指令，开头加@的不打印
```

命令行参数好像也会变成宏（例：`make -f Makefile DEBUG=Y`）

有一系列特殊的变量，如

* Implicit Variables：`$(CC)`为当前编译器
* Automatic Variables
  * `$@`：当前目标
  * `$<, $^`：第一个依赖项，所有依赖项
  * `$*`：匹配符 % 匹配的部分
  * `$(@D), $(@F)`：当前目标的目录名、文件名
  * `$(<D), $(<F)`：第一个依赖项的目录名、文件名

* 判断与循环

```makefile
ifeq ($(CC), gcc)
	commands
else
	commands
endif
```

完整make文件示例：

```makefile
# Usage: make -f Makefile.txt [DEBUG=Y]

ifdef DEBUG
	TARGET = test.exe
	OPT = -g
else
	TARGET = main.exe
	OPT =
endif

obj_dir = obj
src_dir = src
header_dir = include
lib_dir = lib

SRCS = $(wildcard src/*.cpp)
OBJS = $(subst $(src_dir),$(obj_dir),$(subst .cpp,.o,$(SRCS)))

# all:
# 	@echo $(SRCS)
# 	@echo $(OBJS)

$(TARGET): $(OBJS)
	g++ $(OPT) $^ -L $(lib_dir) -lFTD3XX -o $(TARGET)


$(obj_dir)/%.o: $(src_dir)/%.cpp
	g++ $(OPT) -I $(header_dir) -c $< -o $@
```



# Word

## 页眉与页脚

插入 - 页眉和页脚

不同部分的页眉页脚不同，比如说从第三页开始标页码：首先在分节处插入布局 - 分隔符 - 分节符，然后选中页眉页脚，设计 - 导航 - 取消链接到前一节

调整页眉的横线：选中页眉 - 开始 - 段落 - 边框 - 无边框

## 公式行宽

如果想要公式不比平常的行宽，选中公式，右键，段落-取消“如果定义了文档网格，则对齐到文档网格”

# 解压日语压缩包

用7zip命令行解压，可以用mcp参数指定[code page](https://en.wikipedia.org/wiki/Code_page#Microsoft_code_pages)，shift-jis的code page是932

语法：`7z.exe <command> <archive_name> [arguments]`

常用指令：l (List), x (eXtract with full path)

使用例：

```
"C:\Program Files\7-Zip\7z.exe" x "shift-jis.zip" -mcp=932
```