

## gcc

| Options       | Meaning                                                      |
| :------------ | :----------------------------------------------------------- |
| `-ansi`       | 只支持 ANSI 标准的 C 语法。<br />这一选项将禁止 GNU C 的某些特色， 例如 `asm` 或 `typeof` 关键词。 |
| `-c`          | 只编译并生成目标文件。                                       |
| `-E`          | 只运行 C 预编译器。                                          |
| `-g`          | 生成调试信息。GNU 调试器可利用该信息。                       |
| `-o FILE`     | 生成指定的输出文件。用在生成可执行文件时。                   |
| `-O0`         | 不进行优化处理。                                             |
| `-O` 或 `-O1` | 优化生成代码。                                               |
| `-O2`         | 进一步优化。                                                 |
| `-O3`         | 比 -O2 更进一步优化，包括 inline 函数。                      |
| `-shared`     | 生成共享目标文件。通常用在建立共享库时。                     |
| `-static`     | 禁止使用共享连接。                                           |
| `-w`          | 不生成任何警告信息。                                         |
| `-Wall`       | 生成所有警告信息。                                           |
| `-Werror`     | 将所有警告 warning 当作错误 error                            |



## g++

TODO

## GDB

Provided you've compiled your program with the debugging symbols `-g` enabled.

Ref:

- [GDB Cheat Sheet.pdf](https://darkdust.net/files/GDB Cheat Sheet.pdf)
- [A GDB Tutorial with Examples](http://www.cprogramming.com/gdb.html)
- [Beej's Quick Guide to GDB](https://beej.us/guide/bggdb/)
- [GDB常用调试命令,layout很有用](https://blog.csdn.net/u010697897/article/details/51612503)

Notes:

- 输入参数的几种方法：

  1. `$ gdb --args executablename arg1 arg2 arg3`

  2. create a file with context:

     ```
     run arg1 arg2 arg3
     
     program input
     ```

     And call gdb like:

     ```
     $ gdb prog < file
     ```

  3. Enter `(gdb) run/start arg1 arg2 arg3` after running gdb

- gdb 会自动载入重新编译的程序。因此 debug 的流程可以是：窗口 A，修改、编译程序；窗口 B(gdb) `kill`&`run`。

- 不输入命令，直接回车，结果为重复上一次的命令。

### 运行

| Command      | Abbreviation | Meaning                                                      |
| ------------ | ------------ | ------------------------------------------------------------ |
| `start`      |              | 自动在 main 处设置一个临时断点，运行程序                     |
| `run`        | `r`          | 运行程序，当遇到断点后，程序会在断点处停止运行，等待用户输入下一步的命令 |
| `next`       | `n`          | 单步调试，不会进入函数内部；`n 4` 表示执行 4 条命令，下同    |
| `step`       | `s`          | 单步调试，会进入函数内部                                     |
| `nexti`      | `ni`         | 针对汇编指令，类似于 `n`，单步调试，不会进入函数内部         |
| `stepi`      | `si`         | 针对汇编指令，类似于 `s`，单步调试，会进入函数内部           |
| `continue`   | `c`          | 继续执行被调试程序，直至下一个断点或程序结束                 |
| `jump <loc>` |              | Just like `continue`, except jump to a particular location first. |
| `until`      |              | 运行程序直到退出循环体；`until+行号`： 运行至某行            |
| `finish`     |              | 运行程序，直到当前函数完成返回，并打印函数返回时的堆栈地址和返回值及参数值等信息。 |
| `kill`       |              | 杀死当前程序（不退出 gdb）                                   |
| `quit`       | `q`          | 退出 gdb                                                     |

### 断点

| Command        | Abbreviation | Meaning                                                     |
| -------------- | ------------ | ----------------------------------------------------------- |
| `break <n>`    | `b <n>`      | 在第 n 行处设置断点；<br />可以带上代码路径：`b main.cpp:8` |
| `break <func>` | `b <func>`   | 在函数 func() 的入口处设置断点，如 `b main`                 |
| `disable <n>`  |              | 暂停第 n 个断点                                             |
| `enable <n>`   |              | 开启第 n 个断点                                             |
| `clear <n>`    |              | 清除第 n 行的断点，不带参数会清楚当前行所有断点             |
| `info break`   | `info b`     | 显示当前程序的断点、display、watch 设置情况                 |
| `delete <n>`   |              | 删除对应的断点、display、watch 等等                         |

### 检查代码

| Command        | Abbreviation | Meaning                                  |
| -------------- | ------------ | ---------------------------------------- |
| `list`         | `l`          | 列出程序的源代码，默认每次显示10行       |
| `list <line#>` |              | 将显示当前文件以行号为中心的前后10行代码 |
| `list <func>`  |              | 显示函数的源代码                         |
| `disas`        |              |                                          |
| `disas <func>` |              |                                          |

### 打印

| Command            | Abbreviation | Meaning                                                      |
| ------------------ | ------------ | ------------------------------------------------------------ |
| `print <exp>`      | `p`          | 打印表达式，可以是任何有效的表达式<br />`print ++a`：将把 `a` 中的值加 1，并打印<br />`print fact(5)`：打印 `fact(5)` 的值<br />`print /d a`：以十进制打印 a（默认）<br />`print /t $rip`：以二进制打印 PC 寄存器<br />`print /x $rax`：以十六进制打印返回值（寄存器） |
| `x/nfu <addr>`     |              | 检查内存 ，`n` 为数量，`f` 为格式，`u` 为大小<br />不指定格式，要么按默认格式（以十进制打印 1 个四字节长度），要么按上一次的 `x` 或 `p` 的格式输出<br />`x/4tb $rsp`：以二进制打印 `$rsp` 地址开始的四个字节<br />`x/s $rdi`：打印 `$rdi` 指向的字符串 |
| `display <exp>`    | `disp`       | 每单步调试一次，就会输出表达式                               |
| `undisplay <#>`    | `undisp`     |                                                              |
| `info display`     |              | 显示 display 使用情况                                        |
| `watch <exp>`      |              | 只在表达式改变时，暂停程序，输出表达式的新、旧值             |
| `printf "%d\n", x` |              | 类似于 C 语言函数，格式化输出                                |

### 修改、定义、调用

| Command                | Abbreviation   | Meaning                                              |
| ---------------------- | -------------- | ---------------------------------------------------- |
| `set variable v = 12`  | `set (v = 12)` | 修改变量 `v` 的值为 12                               |
| `set {type}addr = val` |                | 给一个内存地址赋值<br />如：`set {int}0x7ffde40 = 4` |
| `call <func(args..)>`  |                | 调用函数，并传参（可选）                             |
| `define <name>`        |                | 定义一个新的命令，通常用来简化操作                   |

### 分割窗口

| Command           | Abbreviation | Meaning                                                      |
| ----------------- | ------------ | ------------------------------------------------------------ |
| `layout src`      |              | 显示源代码窗口                                               |
| `layout asm`      |              | 显示反汇编窗口                                               |
| `layout regs`     |              | 显示源代码/反汇编和CPU寄存器窗口                             |
| `layout split`    |              | 显示源代码和反汇编窗口                                       |
| `focus <winname>` | `fs`         | Set focus to a particular window by name ("SRC", "CMD", "ASM", or "REG") |
| `Ctrl`+`x` `a`    |              | 关闭窗口                                                     |
| `Ctrl`+`L`        |              | 刷新窗口                                                     |

### 运行信息

| Command             | Abbreviation | Meaning                                         |
| ------------------- | ------------ | ----------------------------------------------- |
| `where`/`backtrace` | `where`/`bt` | 当前运行的堆栈列表                              |
| `set <args..>`      |              | 设置需要传入的参数                              |
| `show <args>`       |              | 查看设置好的参数                                |
| `info program`      |              | Print current status of the program             |
| `info functions`    |              | Print functions in program                      |
| `info stack`        |              | Print backtrace of the stack                    |
| `info frame`        |              | Print information about the current stack frame |
| `info registers`    | `info reg`   | Print registers and their contents              |

### 其他

| Command          | Abbreviation | Meaning            |
| ---------------- | ------------ | ------------------ |
| `help <command>` |              | 查看命令的帮助文件 |
