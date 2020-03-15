# Hotkey

按键组合::
指令
Return
按键组合可以是多个按键（同时按下就生效），也可以是按键1 & 按键2（此时按住&前按键不放，并按下&后的键生效）

# Hotstring

::字符串::内容
Return
注：输完内容后按空格、回车、tab、括号、分号等EndChars进行转换。可以改成
:*:字符串::内容
这样不需空格直接转换

# Send指令

send, 按键序列
send,
(
按键序列
)

#ifWinActive/ifWinExist 窗口名称
语句序列
#ifWinActive/ifWinExist
只再特定窗口作用的指令
注：后一个不带窗口名称的#ifWinActive/ifWinExist是标志了contest sensitivity的结束

; 注释
（在指令中）%变量名%表示变量的内容
比较相等只需要一个=
变量赋值使用:=（若不用，需要var1=%var2%的形式）

# 指令(Commands)

sleep, 时间（单位：ms）
Run, 文件路径/网址
Msgbox, 字符串
SoundBeep

函数(Functions)

# 特殊按键

```
#    Win	
!    Alt
^    Ctrl
-    Shift
LButton	鼠标左键
RButton	鼠标右键
{特殊符号}能打出它们原本的意思
{按键 down}, {按键 up}按下/放开某个按键
~按键	按键效果不会被屏蔽
```

​	