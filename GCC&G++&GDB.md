

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

| Command                               | Description                                                  |
| ------------------------------------- | ------------------------------------------------------------ |
| **Running**                           |                                                              |
| `$ gdb <program> [core dump]`         | Start GDB (with optional core dump).                         |
| `$ gdb --pid <pid>`                   | Start GDB and attach to process.                             |
| `set args <args...>`                  | Set arguments to pass to program to be debugged.             |
| `run`                                 | Run the program to be debugged.                              |
| `kill`                                | Kill the running program.                                    |
| **Breakpoints**                       |                                                              |
| `break <where>`                       | Set a new breakpoint.                                        |
| `delete <breakpoint#>`                | Remove a breakpoint.                                         |
| `clear`                               | Delete all breakpoints.                                      |
| `disable <breakpoint#>`               | Disable a breakpoint.                                        |
| `enable <breakpoint#>`                | Enable a disabled breakpoint.                                |
| **Watchpoints**                       |                                                              |
| `watch <where>`                       | Set a new watchpoint.                                        |
| `delete/enable/disable <watchpoint#>` | Like breakpoints.                                            |
| **Stepping**                          |                                                              |
| `step`                                | Go to next instruction (source line), di-<br/>ving into function. |
| `next`                                | Go to next instruction (source line) but <br/>donʻt dive into functions. |
| `finish`                              | Continue until the current function re-<br/>turns.           |
| `continue`                            | Continue normal execution.                                   |
| **Variables and memory**              |                                                              |
| `print/format <what>`                 | Print content of variable/memory locati-<br/>on/register.    |
| `display/format <what>`               | Like "print", but print the information <br/>after each stepping instruction. |
| `undisplay <display#>`                | Remove the "display" with the given <br/>number.             |

