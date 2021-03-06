[TOC]

# IO 模块 io module

## 函数

* `raw_input([提示符])`
  读取输入（原样)

* `input([提示符])`
  读取输入（支持python表达式）

* `open(文件名 [, 打开模式][, 缓存])`
  打开文件（创建文件对象）

  打开模式:

  |      |                  |
  | ---- | ---------------- |
  | r    | 读（默认）            |
  | w    | 写，先清除已有文件        |
  | x    | 写，建立新文件          |
  | a    | 写，附加在已有文件后       |
  | b    | 二进制模式            |
  | t    | 文本模式（默认）         |
  | +    | 打开一个新文件用于更新（读写）  |
  | U    | 普遍新行符模式（已过时）文件对象 |

## 文件对象

```xmind
closed
文件是否已关闭
mode
返回文件访问模式
name
返回文件名称
softspace
是否用print输出后必须跟一个空格
close()
关闭文件
flush()
刷新文件内部缓冲
write(字符串)
将字符串写入文件
read([计数])
读取文件
readline([计数])
读取整行，包括“\n”字符
readlines([期望行数量])
读取多行
tell()
返回当前位置
seek(偏移[, 参考位置])
改变当前位置
	参考位置
		0文件开头
		1当前位置
		2文件末尾
truncate([字节])
截取文件
writelines(序列)
写入多行，需要自己加换行符
```

