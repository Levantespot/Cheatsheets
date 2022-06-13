# Makefile tutorial

### Ref

- [跟我一起写 Makefile](https://seisman.github.io/how-to-write-makefile/index.html)

## Quick Start

### 预处理，编译，汇编，链接

- 预处理 preprocessing：预处理器根据源文件（ `.c`、 `.cpp`、`.h`）中 `#` 开头的命令，修改原始的程序，将把系统中的头文件插入到程序文本中，得到 `.i` 文件。
  - 预处理后依然是文本。
  - 使用 `gcc` 预处理 `gcc -E -o test.i test.c`
- 编译 compile：将预处理文件转换为汇编代码 assembly code。
  - 汇编代码依然是文本。
  - 使用 `gcc` 编译 `gcc -S -o test.s test.i` or `gcc -S -o test.s test.c`
- 汇编：汇编代码转换成机器码，生成目标文件，即 Object File。
  - 目标文件是二进制格式，无法直接查看。
  - 在 Windows 下也就是 `.obj` 文件，UNIX下是 `.o` 文件。
  - 使用 `gcc` 汇编：`gcc -c -o test.o test.s ` or `gcc -c-o test.o test.c `
- 链接 link：使用链接器将该目标文件与其他目标文件、库文件、启动文件等链接起来生成可执行文件（如 UNIX 的 `.out`，Windows 的 `.exe`）。
  - 链接器并不管函数所在的源文件，只管函数的 Object File，在大多数时候，由于源文件太多，编译生成的中间目标文件太多，而在链接时需要明显地指出中间目标文件名，这对于编译很不方便。所以，我们要给中间目标文件打个包，在 Windows 下这种包叫「库文件，Library File」，也就是 `.lib` 文件，在 UNIX 下，是 Archive File，也就是 `.a` 文件。
  - 使用 `gcc` 链接目标文件：`gcc -o test.out test.o ` or `gcc -o test.out test.c`

### 基本的语法

```bash
<target>: [ <prerequisites> ... ]
 [TAB]  command
 [TAB]  -command # ignore errors
 [TAB]  @command # suppress echoing
```

+ target
  + 可以是一个 object file（目标文件），也可以是一个执行文件，还可以是一个标签（label）
+ prerequisites
  + 生成该 target 所依赖的文件和/或 target
+ command
  + 该 target 要执行的命令（任意的 shell 命令）

> prerequisites 中如果有一个以上的文件比 target 文件要新的话，command 所定义的命令就会被执行。

### 示例

如果一个工程有 3 个头文件和 8 个源码文件，makefile 可以是下面的这样：

```makefile
edit : main.o kbd.o command.o display.o \
        insert.o search.o files.o utils.o
    gcc -o edit main.o kbd.o command.o display.o \
        insert.o search.o files.o utils.o

main.o : main.c defs.h
    gcc -c main.c
kbd.o : kbd.c defs.h command.h
    gcc -c kbd.c
command.o : command.c defs.h command.h
    gcc -c command.c
display.o : display.c defs.h buffer.h
    gcc -c display.c
insert.o : insert.c defs.h buffer.h
    gcc -c insert.c
search.o : search.c defs.h buffer.h
    gcc -c search.c
files.o : files.c defs.h buffer.h command.h
    gcc -c files.c
utils.o : utils.c defs.h
    gcc -c utils.c
clean :
    rm edit main.o kbd.o command.o display.o \
        insert.o search.o files.o utils.o
```

反斜杠 `\` 是换行符。我们可以把这个内容保存在名字为 `makefile` 或 `Makefile` 的文件中，然后在该目录下直接输入命令 `make` 就可以生成执行文件 `edit`。如果要删除执行文件和所有的中间目标文件，那么，只要简单地执行一下 `make clean` 就可以了。

在这个 makefile 中，目标文件（target）包含：执行文件edit和中间目标文件（ `*.o` ），依赖文件（prerequisites）就是冒号后面的那些 `.c` 文件和 `.h` 文件。每一个 `.o` 文件都有一组依赖文件，而这些 `.o` 文件又是执行文件 `edit` 的依赖文件。依赖关系的实质就是说明了目标文件是由哪些文件生成的，换言之，目标文件是哪些文件更新的。

其中需要注意的是， `clean` 不是一个文件，它只不过是一个动作名字 `label`。`make` 就不会自动去找它的依赖性，也就不会自动执行其后所定义的命令。要执行其后的命令，就要在 `make` 命令后明显得指出这个 `label` 的名字。这样的方法非常有用，我们可以在一个 makefile 中定义不用的编译或是和编译无关的命令，比如程序的打包，程序的备份，等等。

### makefile 中使用变量

使用 `objects = ...` 就可以定义一个叫做 `objects` 的变量，等于 `=` 后面的内容。于是，我们就可以很方便地在 makefile 中以 `$(objects)` 的方式来使用这个变量了：

```makefile
objects = main.o kbd.o command.o display.o \
    insert.o search.o files.o utils.o

edit : $(objects)
    cc -o edit $(objects)
main.o : main.c defs.h
    cc -c main.c
kbd.o : kbd.c defs.h command.h
    cc -c kbd.c
command.o : command.c defs.h command.h
    cc -c command.c
display.o : display.c defs.h buffer.h
    cc -c display.c
insert.o : insert.c defs.h buffer.h
    cc -c insert.c
search.o : search.c defs.h buffer.h
    cc -c search.c
files.o : files.c defs.h buffer.h command.h
    cc -c files.c
utils.o : utils.c defs.h
    cc -c utils.c
clean :
    rm edit $(objects)
```

### 让 make 自动推导

只要 make 看到一个 `.o` 文件，它就会自动的把 `.c` 文件加在依赖关系中，如果 make 找到一个 `whatever.o` ，那么 `whatever.c` 就会是 `whatever.o` 的依赖文件。并且 `cc -c whatever.c` 也会被推导出来。

```makefile
objects = main.o kbd.o command.o display.o \
    insert.o search.o files.o utils.o

edit : $(objects)
    cc -o edit $(objects)

main.o : defs.h
kbd.o : defs.h command.h
command.o : defs.h command.h
display.o : defs.h buffer.h
insert.o : defs.h buffer.h
search.o : defs.h buffer.h
files.o : defs.h buffer.h command.h
utils.o : defs.h

clean :
    rm edit $(objects)
```

### 另类风格的 makefile

由于自动推导能补充对应名称的源文件和编译的命令，那么可以将使用相同的 `.h` 文件集中在一起：

```makefile
objects = main.o kbd.o command.o display.o \
    insert.o search.o files.o utils.o

edit : $(objects)
    cc -o edit $(objects)

$(objects) : defs.h
kbd.o command.o files.o : command.h
display.o insert.o search.o files.o : buffer.h

clean :
    rm edit $(objects)
```

这种风格，让我们的 makefile 变得很简单，但我们的文件依赖关系就显得有点凌乱了。一是文件的依赖关系看不清楚，二是如果文件一多，要加入几个新的 `.o` 文件，那就理不清楚了。

### 清空目标文件

一般的风格都是：

```makefile
clean:
    rm edit $(objects)
```

更为稳健的做法是：

```makefile
.PHONY : clean
clean :
    -rm edit $(objects)
```

上面文件内容中， `.PHONY` 表示 `clean` 是个伪目标文件。而在 `rm` 命令前面加了一个小减号的意思就是，也许某些文件出现问题，但不要管，继续做后面的事。当然， `clean` 的规则不要放在文件的开头，不然，这就会变成make的默认目标，相信谁也不愿意这样。不成文的规矩是——clean从来都是放在文件的最后。

### 引用其他 Makefile

在Makefile使用 `include` 关键字可以把别的 Makefile 包含进来：

```makefile
include <filename>
```

`filename` 可以是当前操作系统Shell的文件模式（可以包含路径和通配符）。

在 `include` 前面可以有一些空字符，但是绝不能是 `Tab` 键开始。 `include` 和 `<filename>` 可以用一个或多个空格隔开。举个例子，你有这样几个Makefile： `a.mk` 、 `b.mk` 、 `c.mk` ，还有一个文件叫 `foo.make` ，以及一个变量 `$(bar)` ，其包含了 `e.mk` 和 `f.mk` ，那么，下面的语句：

```makefile
include foo.make *.mk $(bar)
```

等价于：

```makefile
include foo.make a.mk b.mk c.mk e.mk f.mk
```

make命令开始时，会找寻 `include` 所指出的其它Makefile，并把其内容安置在当前的位置。就好像C/C++的 `#include` 指令一样。如果文件都没有指定绝对路径或是相对路径的话，make会在当前目录下首先寻找，如果当前目录下没有找到，那么，make还会在下面的几个目录下找：

1. 如果make执行时，有 `-I` 或 `--include-dir` 参数，那么make就会在这个参数所指定的目录下去寻找。
2. 如果目录 `<prefix>/include` （一般是： `/usr/local/bin` 或 `/usr/include` ）存在的话，make也会去找。

如果有文件没有找到的话，make会生成一条警告信息，但不会马上出现致命错误。它会继续载入其它的文件，一旦完成makefile的读取，make会再重试这些没有找到，或是不能读取的文件，如果还是不行，make才会出现一条致命信息。如果你想让make不理那些无法读取的文件，而继续执行，你可以在include前加一个减号“-”。

### Makefile 组成

1. 显式规则。显式规则说明了如何生成一个或多个目标文件。这是由 Makefile 的书写者明显指出要生成的文件、文件的依赖文件和生成的命令。
2. 隐晦规则。由于我们的 `make` 有自动推导的功能，所以隐晦的规则可以让我们比较简略地书写 Makefile，这是由 `make` 所支持的。
3. 变量的定义。在 Makefile 中我们要定义一系列的变量，变量一般都是字符串，这个有点像你C语言中的宏，当 Makefile 被执行时，其中的变量都会被扩展到相应的引用位置上。
4. 文件指示。其包括了三个部分，一个是在一个 Makefile 中引用另一个 Makefile，就像 C 语言中的 include 一样；另一个是指根据某些情况指定 Makefile 中的有效部分，就像 C 语言中的预编译#if一样；还有就是定义一个多行的命令。
5. 注释。Makefile 中只有行注释，和 UNIX 的 Shell 脚本一样，其注释是用 `#` 字符，这个就像 C/C++ 中的 `//` 一样。如果你要在你的 Makefile 中使用 `#` 字符，可以用反斜杠进行转义，如： `\#` 。

### make 的工作方式

GNU的 `make` 工作时的执行步骤如下：

1. 读入所有的 Makefile。
2. 读入被 include 的其它 Makefile。
3. 初始化文件中的变量。
4. 推导隐晦规则，并分析所有规则。
5. 为所有的目标文件创建依赖关系链。
6. 根据依赖关系，决定哪些目标要重新生成。
7. 执行生成命令。

## TODO:书写规则

## TODO:书写命令

## TODO:使用变量

用 `=` 号给变量赋值，用 `$()` 或 `${}` 取得变量的值：

```makefile
CFLAGS = -g -O2 -Wall
LIBS = -pthread

objects = object1.o object2.o

target: $(objects)
	cc -o target $(objects) $(CFLAGS) $(LIBS)
```

- `$@` 表示目标文件
- `$^` 表示所有的依赖文件
- `$<` 表示第一个依赖文件
- `$?` 表示比目标还要新的依赖文件列表

```makefile
foo: foo.c
    cc -o foo foo.c
```

 可以简写为：

```
foo: foo.c
	cc -o $@ $^
```



## TODO:使用条件判断

## TODO:使用函数

## TODO:隐含规则

最简单的隐含规则：

```makefile
foo : foo.o bar.o
    cc –o foo foo.o bar.o $(CFLAGS) $(LDFLAGS)
foo.o : foo.c
    cc –c foo.c $(CFLAGS)
bar.o : bar.c
    cc –c bar.c $(CFLAGS)
```

可以简写为：

```makefile
foo : foo.o bar.o
    cc –o ^@ $^ $(CFLAGS) $(LDFLAGS)
```



