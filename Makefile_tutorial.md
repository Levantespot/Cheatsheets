# Makefile tutorial

源文件首先会生成中间目标文件，再由中间目标文件生成执行文件。在编译时，编译器只检测程序语法和函数、变量是否被声明。如果函数未被声明，编译器会给出一个警告，但可以生成 Object File。而在链接程序时，链接器会在所有的 Object File 中找寻函数的实现，如果找不到，那到就会报链接错误码（Linker Error）。

基本的语法

```bash
target: [ prerequisites ... ]
 [TAB]  command1
 [TAB]  -command2 # ignore errors
 [TAB]  @command3 # suppress echoing
```

+ target
  + 可以是一个 object file（目标文件），也可以是一个执行文件，还可以是一个标签（label）
+ prerequisites
  + 生成该 target 所依赖的文件和/或 target
+ command
  + 该 target 要执行的命令（任意的 shell 命令）

> prerequisites 中如果有一个以上的文件比 target 文件要新的话，command 所定义的命令就会被执行。

### Makefile 组成

1. 显式规则。显式规则说明了如何生成一个或多个目标文件。这是由 Makefile 的书写者明显指出要生成的文件、文件的依赖文件和生成的命令。
2. 隐晦规则。由于我们的 make 有自动推导的功能，所以隐晦的规则可以让我们比较简略地书写 Makefile，这是由 make 所支持的。
3. 变量的定义。在 Makefile 中我们要定义一系列的变量，变量一般都是字符串，这个有点像你C语言中的宏，当 Makefile 被执行时，其中的变量都会被扩展到相应的引用位置上。
4. 文件指示。其包括了三个部分，一个是在一个 Makefile 中引用另一个 Makefile，就像 C 语言中的 include 一样；另一个是指根据某些情况指定 Makefile 中的有效部分，就像 C 语言中的预编译#if一样；还有就是定义一个多行的命令。
5. 注释。Makefile 中只有行注释，和 UNIX 的 Shell 脚本一样，其注释是用 `#` 字符，这个就像 C/C++ 中的 `//` 一样。如果你要在你的 Makefile 中使用 `#` 字符，可以用反斜杠进行转义，如： `\#` 。

### make的工作方式

GNU的make工作时的执行步骤如下：

1. 读入所有的Makefile。
2. 读入被include的其它Makefile。
3. 初始化文件中的变量。
4. 推导隐晦规则，并分析所有规则。
5. 为所有的目标文件创建依赖关系链。
6. 根据依赖关系，决定哪些目标要重新生成。
7. 执行生成命令。