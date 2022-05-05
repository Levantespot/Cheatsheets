# Linux tutorials

[TOC]

## Quick guide

### Things to do After installation system

```bash
sudo passwd # set password of root account

ping www.baidu.com -c 4 # to see if you're connnected to the Internet

su - # switch to the root account
su root # switch to the root account

adduser 'username' sudo # add your account to the sudo group

echo "deb http://mirrors.tuna.tsinghua.edu.cn/debian/ stable main" > /etc/apt/sources.list # update the APT source file

sudo apt update # retrieve software information from the sources

sudo apt install package_name_1 package_name_2 # install packages
```

### Commands for beginners

```bash
$ ls              # 列出当前目录下的所有文件(或目录)
$ ls -l           # 显示详细信息
$ pwd 			  # 列出当前所在的目录
$ mkdir temp      # 创建一个目录temp
$ cd temp         # 切换到目录temp
$ touch newfile   # 创建一个空文件newfile
$ mkdir newdir    # 创建一个目录newdir
$ cd newdir       # 切换到目录newdir
$ cp ../newfile . # 将上级目录中的文件newfile复制到当前目录下
$ cp newfile aaa  # 将文件newfile复制为新文件aaa
$ mv aaa bbb      # 将文件aaa重命名为bbb
$ mv bbb ..       # 将文件bbb移动到上级目录
$ cd ..           # 切换到上级目录
$ rm bbb          # 删除文件bbb
$ cd ..           # 切换到上级目录
$ rm -r temp      # 递归删除目录temp
```

## Get some help

**whatis**

```bash
$ whatis command # display one-line short manual page descriptions
eg:
$ whatis printf
printf (1)           - format and print data
printf (3)           - formatted output conversion
```

**man**

| section numbers | types of pages                                               |
| --------------- | ------------------------------------------------------------ |
| 1               | Executable programs or shell commands                        |
| 2               | System calls (functions provided by the kernel)              |
| 3               | Library calls (functions within program libraries)           |
| 4               | Special files (usually found in /dev)                        |
| 5               | File formats and conventions eg /etc/passwd                  |
| 6               | Games                                                        |
| 7               | Miscellaneous(including macro packages and conventions)，e.g. man(7), groff(7) |
| 8               | System administration commands (usually only for root)       |
| 9               | Kernel routines [Non standard]                               |

Conventional section names include

- NAME - 命令名
- SYNOPSIS - 使用方法大纲
- CONFIGURATION - 配置
- DESCRIPTION - 功能说明
- OPTIONS - 可选参数说明
- EXIT STATUS - 退出状态, 这是一个返回给父进程的值
- RETURN VALUE - 返回值
- ERRORS - 可能出现的错误类型
- ENVIRONMENT - 环境变量
- FILES - 相关配置文件
- VERSIONS - 版本
- CONFORMING TO - 符合的规范
- NOTES - 使用注意事项
- BUGS - 已经发现的bug
- EXAMPLE - 一些例子
- AUTHORS - 作者
- SEE ALSO - 功能或操作对象相近的其它命令

```bash
$ man command # the system's manual pager 没有内建与外部命令的区分
eg:
$ man 1 printf
...

$ man -k keyword # search for keyword
eg:
$ man -k printf
asprintf (3)         - print to allocated string
dprintf (3)          - formatted output conversion
fprintf (3)          - formatted output conversion
fwprintf (3)         - formatted wide-character output conversion
printf (1)           - format and print data
printf (3)           - formatted output conversion
```

**help**

```bash
$ help command # 用于内部命令
eg:
$ help cd
...

$ command -? | -h | --help # 用于外部命令
eg:
$ printf -?
...
$ ls --help
...
```

**info**

```bash
$ info command # more information
eg:
$ info printf
...
```

**which**

```bash
$ which command # locate a command
eg:
$ which printf
/usr/bin/printf
```

**whereis**

```bash
$ whereis command #locate the binary, source, and manual page files for a command 常用于安装了某个软件的多个版本，确定使用的是哪一个版本
eg:
$ whereis printf
printf: /usr/bin/printf /usr/include/printf.h /usr/share/man/man1/printf.1.gz /usr/share/man/man3/printf.3.gz
```



## Package management

List of Debian package management tools

| Package    | Description                                                  |
| ---------- | ------------------------------------------------------------ |
| `apt`      | For all interactive command line operations, including package installation, removal and dist-upgrades. |
| `apt-get`  | For calling Debian package management system from scripts. It is also a fallback option when `apt` is not available (often with older Debian systems) |
| `aptitude` | For an interactive text interface to manage the installed packages and to search the available packages |

### Basic package management

| `apt` syntax       | `apt-get`/`apt-cache` syntax | description                                                  |
| ------------------ | ---------------------------- | ------------------------------------------------------------ |
| `apt update`       | `apt-get update`             | update package archive metadata                              |
| `apt install foo`  | `apt-get install foo`        | install candidate version of `foo` package with its dependencies |
| `apt upgrade`      | `apt-get upgrade`            | install candidate version of installed packages without removing any other packages |
| `apt full-upgrade` | `apt-get dist-upgrade`       | install candidate version of installed packages while removing other packages if needed |
| `apt remove foo`   | `apt-get remove foo`         | remove `foo` package while leaving its configuration files   |
| `apt autoremove`   | `apt-get autoremove`         | remove auto-installed packages which are no longer required  |
| `apt purge foo`    | `apt-get purge foo`          | purge `foo` package with its configuration files             |
| `apt clean`        | `apt-get clean`              | clear out the local repository of retrieved package files completely |
| `apt autoclean`    | `apt-get autoclean`          | clear out the local repository of retrieved package files for outdated packages |
| `apt show foo`     | `apt-cache show foo`         | display detailed information about `foo` package             |

## Unix-like filesystem

### Unix file basics

* Filenames are case sensitive. "MYFILE" $\neq$ "MyFile"
* *root directory* is the root of filesystem referred as `/`. The home directory for the root user: `/root`
* Each file or directory is designated by a *fully-qualified filename*, *absolute filename*, or *path*. The three terms are synonymous. `/usr/share/us.map.gz`
* There's no special directory path name component that corresponds to a physical device

**List of usage of key directories**

| directory   | usage of the directory                                |
| ----------- | ----------------------------------------------------- |
| `/`         | the root directory                                    |
| `/etc/`     | system wide configuration files                       |
| `/var/log/` | system log files                                      |
| `/home/`    | all the home directories for all non-privileged users |



### Filesystem permissions

Filesystem permissions of Unix-like system are defined for three categories of affected users.

- The **user** who owns the file (**u**)
- Other users in the **group** which the file belongs to (**g**)
- All **other** users (**o**) also referred to as "world" and "everyone"

For the file, each corresponding permission allows following actions.

- The **read** (**r**) permission allows owner to examine contents of the file.
- The **write** (**w**) permission allows owner to modify the file.
- The **execute** (**x**) permission allows owner to run the file as a command.

For the directory, each corresponding permission allows following actions.

- The **read** (**r**) permission allows owner to list contents of the directory.
- The **write** (**w**) permission allows owner to add or remove files in the directory.
- The **execute** (**x**) permission allows owner to access files in the directory (not only to allow reading of files in that directory but also to allow viewing their attributes).

"`ls`" is used to display permission information (and more) for files and directories. When it is invoked with the "`-l`" option, it displays the following information in the order given.

- **Type of file** (first character)
- Access **permission** of the file (nine characters, consisting of three characters each for user, group, and other in this order)
- **Number of hard links** to the file
- Name of the **user** who owns the file
- Name of the **group** which the file belongs to
- **Size** of the file in characters (bytes)
- **Date and time** of the file (mtime)
- **Name** of the file

| character | meaning               |
| --------- | --------------------- |
| `-`       | normal file           |
| `d`       | directory             |
| `l`       | symlink               |
| `c`       | character device node |
| `b`       | block device node     |
| `p`       | named pipe            |
| `s`       | socket                |



+ `chown` is used from the root account to change the owner of the file. 

+ `chgrp` is used from the file's owner or root account to change the group of the file. 
+ `chmod` is used from the file's owner or root account to change file and directory access permissions.

```bash
$ chown <newowner> foo
$ chgrp <newgroup> foo
$ chmod  [ugoa][+-=][rwxXst][,...] foo
```

There are three more special permission bits.

+ The **set user ID** bit (**s** or **S** instead of user's **x**)
  + on an executable file allows a user to execute the executable file with the owner ID of the file (for example **root**)
+ The **set group ID** bit (**s** or **S** instead of group's **x**)
  + on an executable file allows a user to execute the executable file with the group ID of the file (for example **root**)
  + on a directory enables the BSD-like file creation scheme where all files created in the directory belong to the group of the directory (for example **root**)
+ The **sticky** bit (**t** or **T** instead of other's **x**)
  + on a directory prevents a file in the directory from being removed by a user who is not the owner of the file

Here the output of "`ls -l`" for these bits is **capitalized** if execution bits hidden by these outputs are **unset**.

Note : In order to secure contents of a file in world-writable directories such as "`/tmp`" or in group-writable directories, one must not only reset the **write** permission for the file but also set the **sticky bit** on the directory.

There is an alternative numeric mode to describe file permissions with chmod(1). This numeric mode uses 3 to 4 digit wide octal (radix=8) numbers.

| digit                | meaning                                                      |
| -------------------- | ------------------------------------------------------------ |
| 1st digit (optional) | sum of **set user ID** (=4), **set group ID** (=2), and **sticky bit** (=1) |
| 2nd digit            | sum of **read** (=4), **write** (=2), and **execute** (=1) permissions for **user** |
| 3rd digit            | ditto for **group**                                          |
| 4th digit            | ditto for **other**                                          |

```bash
$ touch foo bar
$ chmod u=rw,go=r foo
$ chmod 644 bar
$ ls -l foo bar
-rw-r--r-- 1 penguin penguin 0 Oct 16 21:39 bar
-rw-r--r-- 1 penguin penguin 0 Oct 16 21:35 foo
```

### Permissions for newly created files

What permissions are applied to a newly created file or directory is restricted by the `umask` shell builtin command.

> ```
> (file permissions) = (requested file permissions) & ~(umask value)
> ```

| umask | file permissions  created | directory  permissions created | usage                      |
| ----- | ------------------------- | ------------------------------ | -------------------------- |
| 0022  | -rw-r--r--                | -rwxr-xr-x                     | writable  only by the user |
| 0002  | -rw-rw-r--                | -rwxrwxr-x                     | writable  by the group     |

### Permissions for groups of users (group)

In order to make group permissions to be applied to a particular user, that user needs to be made a member of the group using both

1. "`sudo vigr`" for `/etc/group` 
2. "`sudo vigr -s`" for `/etc/gshadow`

You need to login after logout (or run "`exec newgrp`") to enable the new group configuration.

The hardware devices are just another kind of file on the Debian system. If you have problems accessing devices such as CD-ROM and USB memory stick from a user account, you should make that user a member of the relevant group.

List of notable system-provided groups for file access

| group   | description for  accessible files and devices                |
| ------- | ------------------------------------------------------------ |
| dialout | full  and direct access to serial ports ("/dev/ttyS[0-3]")   |
| dip     | limited  access to serial ports for Dialup IP connection to trusted peers |
| cdrom   | CD-ROM,  DVD+/-RW drives                                     |
| audio   | audio  device                                                |
| video   | video  device                                                |
| scanner | scanner(s)                                                   |
| adm     | system  monitoring logs                                      |
| staff   | some  directories for junior administrative work: "/usr/local", "/home" |

List of notable system provided groups for particular command executions

| group   | accessible  commands                                         |
| ------- | ------------------------------------------------------------ |
| sudo    | execute sudo without their password                          |
| lpadmin | execute  commands to add, modify, and remove printers from printer databases |

### Timestamps

| type      | meaning (historic Unix definition)     |
| --------- | -------------------------------------- |
| **mtime** | the file modification time (`ls -l`)   |
| **ctime** | the file status change time (`ls -lc`) |
| **atime** | the last file access time (`ls -lu`)   |

### Links

There are two methods of associating a file "`foo`" with a different filename "`bar`".

+ Hard link
  + Duplicate name for an existing file
  + "`ln foo bar`"
+ Symbolic link or symlink
  + Special file that points to another file by name
  + "`ln -s foo bar`"

```bash
$ umask 002
$ echo "Original Content" > foo
$ ls -li foo
1449840 -rw-rw-r-- 1 penguin penguin 17 Oct 16 21:42 foo
$ ln foo bar     # hard link
$ ln -s foo baz  # symlink
$ ls -li foo bar baz
1449840 -rw-rw-r-- 2 penguin penguin 17 Oct 16 21:42 foo
1449840 -rw-rw-r-- 2 penguin penguin 17 Oct 16 21:42 bar
1450180 lrwxrwxrwx 1 penguin penguin  3 Oct 16 21:47 baz -> foo
$ rm foo
$ echo "New Content" > foo
$ ls -li foo bar baz
1450183 -rw-rw-r-- 1 penguin penguin 12 Oct 16 21:48 foo
1449840 -rw-rw-r-- 1 penguin penguin 17 Oct 16 21:42 bar
1450180 lrwxrwxrwx 1 penguin penguin  3 Oct 16 21:47 baz -> foo
$ cat bar
Original Content
$ cat baz
New Content
```

Note : It is generally preferable to use symbolic links rather than hardlinks unless you have a good reason for using a hardlink.

The "`.`" directory links to the directory that it appears in, thus the link count of any new directory starts at 2. The "`..`" directory links to the parent directory, thus the link count of the directory increases with the addition of new subdirectories.

### Named pipes (FIFOs)

A named pipe is a file that acts like a pipe. You put something into the file, and it comes out the other end. Thus it's called a FIFO, or First-In-First-Out: the first thing you put in the pipe is the first thing to come out the other end.

If you write to a named pipe, the process which is writing to the pipe doesn't terminate until the information being written is read from the pipe. If you read from a named pipe, the reading process waits until there is nothing to read before terminating. The size of the pipe is always zero --- it does not store data, it just links two processes like the functionality offered by the shell `|` syntax. However, since this pipe has a name, the two processes don't have to be on the same command line or even be run by the same user. Pipes were a very influential innovation of Unix.

```shell
$ cd; mkfifo mypipe
$ echo "hello" >mypipe & # put into background
[1] 8022
$ ls -l mypipe
prw-rw-r-- 1 penguin penguin 0 Oct 16 21:49 mypipe
$ cat mypipe
hello
[1]+  Done                    echo "hello" >mypipe
$ ls mypipe
mypipe
$ rm mypipe
```

### Sockets

Sockets are used extensively by all the Internet communication, databases, and the operating system itself. It is similar to the named pipe (FIFO) and allows processes to exchange information even between different computers. For the socket, those processes do not need to be running at the same time nor to be running as the children of the same ancestor process. This is the endpoint for [the inter process communication (IPC)](https://en.wikipedia.org/wiki/Inter-process_communication). The exchange of information may occur over the network between different hosts. The two most common ones are [the Internet socket](https://en.wikipedia.org/wiki/Internet_socket) and [the Unix domain socket](https://en.wikipedia.org/wiki/Unix_domain_socket).

"`netstat -an`" provides a very useful overview of sockets that are open on a given system.

### Device files

Device files refer to physical or virtual devices on your system, such as your hard disk, video card, screen, or keyboard. An example of a virtual device is the console, represented by "/dev/console".

There are 2 types of device files.

- **Character device**
  - Accessed one character at a time
  - 1 character = 1 byte
  - E.g. keyboard device, serial port, …
- **Block device**
  - accessed in larger units called blocks
  - 1 block > 1 byte
  - E.g. hard disk, …

### Special device files

| device  file | action | description of  response                                     |
| ------------ | ------ | ------------------------------------------------------------ |
| /dev/null    | read   | return  "end-of-file (EOF) character"                        |
| /dev/null    | write  | return  nothing (a bottomless data dump pit)                 |
| /dev/zero    | read   | return  "the \0 (NUL)  character" (not the same as the number zero ASCII) |
| /dev/random  | read   | return  random characters from a true random number generator, delivering real  entropy (slow) |
| /dev/urandom | read   | return  random characters from a cryptographically secure pseudorandom number  generator |
| /dev/full    | write  | return  the disk-full (ENOSPC) error                         |

### procfs and sysfs

The [procfs](https://en.wikipedia.org/wiki/Procfs) and [sysfs](https://en.wikipedia.org/wiki/Sysfs) mounted on "`/proc`" and "`/sys`" are the pseudo-filesystem and expose internal data structures of the kernel to the userspace. In other word, these entries are virtual, meaning that they act as a convenient window into the operation of the operating system.

The directory "`/proc`" contains (among other things) one subdirectory for each process running on the system, which is named after the process ID (PID). System utilities that access process information, such as ps(1), get their information from this directory structure.

The directories under "`/proc/sys/`" contain interfaces to change certain kernel parameters at run time. (You may do the same through the specialized sysctl(8) command or its preload/configuration file "`/etc/sysctl.conf`".)

### tmpfs

The [tmpfs](https://en.wikipedia.org/wiki/Tmpfs#Linux) is a temporary filesystem which keeps all files in the [virtual memory](https://en.wikipedia.org/wiki/Virtual_memory). The data of the tmpfs in the [page cache](https://en.wikipedia.org/wiki/Page_cache) on memory may be swapped out to the [swap space](https://en.wikipedia.org/wiki/Paging) on disk as needed.

### Midnight Commander (MC)

```shell
$ sudo apt install mc # install mc
$ mc # open mc
# Use ^o to move between shell and mc
# Press F1 to get help
# Use F10 to close mc and return to shell
```

**Command-line tricks in MC**

+ `cd` command changes the directory shown on the selected screen.
+ `Ctrl-Enter` or `Alt-Enter` copies a filename to the command line. Use this with cp(1) and mv(1) commands together with command-line editing.
+ One can specify the starting directory for both windows as arguments to MC; for example, "`mc /etc /root`".
+ Pressing `Esc` before the key has the same effect as pressing the `Alt` and the key together.; i.e., type `Esc` + `c` for `Alt-C`. `Esc` is called meta-key and sometimes noted as "`M-`".

**FTP virtual filesystem of MC**

MC can be used to access files over the Internet using FTP. Go to the menu by pressing `F9`, then type "`p`" to activate the FTP virtual filesystem. Enter a URL in the form "`username:passwd@hostname.domainname`", which retrieves a remote directory that appears like a local one.

## The basic Unix-like work environment

### The login shell

Although POSIX-like shells share the basic syntax, they can differ in behavior for things as basic as shell variables and glob expansions.

You can select your login shell with chsh(1).

| package | description                                                  |
| ------- | ------------------------------------------------------------ |
| bash    | Bash: the GNU Bourne Again SHell (de facto standard)         |
| tcsh    | TENEX C Shell: an enhanced version of Berkeley csh           |
| dash    | Debian Almquist Shell, good for shell script                 |
| tmux    | Tmux is a terminal multiplexer: it enables a number of terminals to be created, accessed, and controlled from a single screen |

In this tutorial chapter, the interactive shell always means `bash`.

### Customizing bash

You can customize bash(1) behavior by "`~/.bashrc`".

### Special key strokes

Please note that on a normal Linux character console, only the left-hand `Ctrl` and `Alt` keys work as expected.

| key                 | description of  key binding                                  |
| ------------------- | ------------------------------------------------------------ |
| Ctrl-U              | erase line before cursor                                     |
| Ctrl-W              | erase a word before the cursor                               |
| Ctrl-H or Backspace | erase a character before cursor                              |
| Ctrl-D              | terminate input (exit shell if you are using shell)          |
| Ctrl-C              | terminate a running program                                  |
| Ctrl-Z              | temporarily stop program by moving it to the background job  |
| Ctrl-S              | halt output to screen                                        |
| Ctrl-Q              | reactivate output to screen                                  |
| Ctrl-Alt-Del        | reboot/halt the system, see inittab(5)                       |
| Left-Alt-key        | q::meta-key for Emacs and the similar UI                     |
| Up-arrow            | start command history search under bash                      |
| Ctrl-R              | start incremental command history search under bash          |
| Tab                 | complete input of the filename to the command line under bash |
| Ctrl-V Tab          | input Tab without expansion to the command line under bash   |

### Unix style mouse operations

Unix style mouse operations are based on the 3 button mouse system.

| action                    | response                                               |
| ------------------------- | ------------------------------------------------------ |
| Left-click-and-drag mouse | select  and copy to the clipboard                      |
| Left-click                | select  the start of selection                         |
| Right-click               | select  the end of selection and copy to the clipboard |
| Middle-click              | paste  clipboard at the cursor                         |

### Text editor

+ Vim
  + See Vim_Tutorial.md
+ Emacs

### Recording the shell activities

The output of the shell command may roll off your screen and may be lost forever. It is a good practice to log shell activities into the file for you to review them later. This kind of record is essential when you perform any system administration tasks.

The basic method of recording the shell activity is to run it under script(1).

For example, try the following

```bash
$ script
Script started, file is typescript
```

Do whatever shell commands under `script`.

Press `Ctrl-D` to exit `script`.

```bash
$ vim typescript
```

### Basic Unix commands

| command                         | description                                                  |
| ------------------------------- | ------------------------------------------------------------ |
| pwd                             | display name of current/working directory                    |
| whoami                          | display current user name                                    |
| id                              | display current user identity (name, uid, gid, and associated groups) |
| file <foo>                      | display a type of file for the file "<foo>"                  |
| type -p <commandname>           | display a file location of command "<commandname>"           |
| which <commandname>             | , ,                                                          |
| type <commandname>              | display information on command "<commandname>"               |
| apropos <key-word>              | find commands related to "<key-word>"                        |
| man -k <key-word>               | , ,                                                          |
| whatis <commandname>            | display one line explanation on command "<commandname>"      |
| man -a <commandname>            | display explanation on command "<commandname>" (Unix  style) |
| info <commandname>              | display rather long explanation on command  "<commandname>" (GNU style) |
| ls                              | list contents of directory (non-dot files and directories)   |
| ls -a                           | list contents of directory (all files and directories)       |
| ls -A                           | list contents of directory (almost all files and directories, i.e., skip  ".." and ".") |
| ls -la                          | list all contents of directory with detail information       |
| ls -lai                         | list all contents of directory with inode number and detail information |
| ls -d                           | list all directories under the current directory             |
| tree                            | display file tree contents                                   |
| lsof <foo>                      | list open status of file "<foo>"                             |
| lsof -p <pid>                   | list files opened by the process ID: "<pid>"                 |
| mkdir <foo>                     | make a new directory "<foo>" in the current directory        |
| rmdir <foo>                     | remove a directory "<foo>" in the current directory          |
| cd <foo>                        | change directory to the directory "<foo>" in the current  directory or in the directory listed in the variable "$CDPATH" |
| cd /                            | change directory to the root directory                       |
| cd                              | change directory to the current user's home directory        |
| cd /<foo>                       | change directory to the absolute path directory "/<foo>"     |
| cd ..                           | change directory to the parent directory                     |
| cd ~<foo>                       | change directory to the home directory of the user  "<foo>"  |
| cd -                            | change directory to the previous directory                   |
| </etc/motd pager                | display contents of "/etc/motd" using the default pager      |
| touch <junkfile>                | create a empty file "<junkfile>"                             |
| cp <foo> <bar>                  | copy a existing file "<foo>" to a new file  "<bar>"          |
| rm <junkfile>                   | remove a file "<junkfile>"                                   |
| mv <foo> <bar>                  | rename an existing file "<foo>" to a new name  "<bar>" ("<bar>" must not exist) |
| mv <foo> <bar>                  | move an existing file "<foo>" to a new location  "<bar>/<foo>" (the directory "<bar>"  must exist) |
| mv <foo> <bar>/<baz>            | move an existing file "<foo>" to a new location with a  new name "<bar>/<baz>" (the directory  "<bar>" must exist but the directory  "<bar>/<baz>" must not exist) |
| chmod 600 <foo>                 | make an existing file "<foo>" to be non-readable and  non-writable by the other people (non-executable for all) |
| chmod 644 <foo>                 | make an existing file "<foo>" to be readable but  non-writable by the other people (non-executable for all) |
| chmod 755 <foo>                 | make an existing file "<foo>" to be readable but  non-writable by the other people (executable for all) |
| find . -name <pattern>          | find matching filenames using shell "<pattern>" (slower)     |
| locate -d . <pattern>           | find matching filenames using shell "<pattern>" (quicker  using regularly generated database) |
| grep -e "<pattern>"  *.html     | find a "<pattern>" in all files ending with  ".html" in current directory and display them all |
| top                             | display process information using full screen, type "q" to quit |
| ps aux \| pager                 | display information on all the running processes using BSD style output |
| ps -ef \| pager                 | display information on all the running processes using Unix system-V  style output |
| ps aux \| grep -e "[e]xim4*"    | display all processes running "exim" and "exim4"             |
| ps axf \| pager                 | display information on all the running processes with ASCII art output |
| kill <1234>                     | kill a process identified by the process ID: "<1234>"        |
| gzip <foo>                      | compress "<foo>" to create "<foo>.gz"  using the Lempel-Ziv coding (LZ77) |
| gunzip <foo>.gz                 | decompress "<foo>.gz" to create "<foo>"                      |
| bzip2 <foo>                     | compress "<foo>" to create "<foo>.bz2"  using the Burrows-Wheeler block sorting text compression algorithm, and  Huffman coding (better compression than gzip) |
| bunzip2 <foo>.bz2               | decompress "<foo>.bz2" to create "<foo>"                     |
| xz <foo>                        | compress "<foo>" to create "<foo>.xz"  using the Lempel–Ziv–Markov chain algorithm (better compression  than bzip2) |
| unxz <foo>.xz                   | decompress "<foo>.xz" to create "<foo>"                      |
| tar -xvf <foo>.tar              | extract files from "<foo>.tar" archive                       |
| tar -xvzf <foo>.tar.gz          | extract files from gzipped "<foo>.tar.gz" archive            |
| tar -xvjf <foo>.tar.bz2         | extract files from "<foo>.tar.bz2" archive                   |
| tar -xvJf <foo>.tar.xz          | extract files from "<foo>.tar.xz" archive                    |
| tar -cvf <foo>.tar <bar>/       | archive contents of folder "<bar>/" in  "<foo>.tar" archive  |
| tar -cvzf <foo>.tar.gz <bar>/   | archive contents of folder "<bar>/" in compressed  "<foo>.tar.gz" archive |
| tar -cvjf <foo>.tar.bz2  <bar>/ | archive contents of folder "<bar>/" in  "<foo>.tar.bz2" archive |
| tar -cvJf <foo>.tar.xz <bar>/   | archive contents of folder "<bar>/" in  "<foo>.tar.xz" archive |
| zcat README.gz \| pager         | display contents of compressed "README.gz" using the default  pager |
| zcat README.gz > foo            | create a file "foo" with the decompressed content of  "README.gz" |
| zcat README.gz >> foo           | append the decompressed content of "README.gz" to the end of  the file "foo" (if it does not exist, create it first) |

## The simple shell command

A simple command is a sequence of components.

1. Variable assignments (optional)
2. Command name
3. Arguments (optional)
4. Redirections (optional: `>` , `>>` , `<` , `<<` , etc.)
5. Control operator (optional: `&&` , `||` , <newline> , `;` , `&` , `(` , `)` )

### Environment variables

| Variable name | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| `$LANG`       |                                                              |
| `$PATH`       | The shell searches the command in the list of directories contained in the "`$PATH`" environment variable. |
| `$HOME`       | The home directory is identified by the environment variable "`$HOME`". |

### Shell glob

Often you want a command to work with a group of files without typing all of them. The filename expansion pattern using the shell **glob**, (sometimes referred as **wildcards**), facilitate this need.

| shell glob pattern | description of match rule                                    |
| ------------------ | ------------------------------------------------------------ |
| *                  | filename (segment) not started with "."                      |
| .*                 | filename (segment) started with "."                          |
| ?                  | exactly one character                                        |
| […]                | exactly one character with any character enclosed in brackets |
| [a-z]              | exactly one character with any character between "a" and  "z" |
| [^…]               | exactly one character other than any character enclosed in brackets  (excluding "^") |

```bash
$ mkdir junk; cd junk; touch 1.txt 2.txt 3.c 4.h .5.txt ..6.txt
$ echo *.txt
1.txt 2.txt
$ echo *
1.txt 2.txt 3.c 4.h
$ echo *.[hc]
3.c 4.h
$ echo .*
. .. .5.txt ..6.txt
$ echo .*[^.]*
.5.txt ..6.txt
$ echo .[^.]*
.5.txt
$ echo [^1-3]*
4.h
$ cd ..; rm -rf junk
```

### Return value of the command

Each command returns its exit status (variable: "`$?`") as the return value.

| command exit status | numeric return value | logical return value |
| ------------------- | -------------------- | -------------------- |
| success             | zero, 0              | TRUE                 |
| error               | non-zero, -1         | FALSE                |

### Typical command sequences and shell redirection

| command  idiom            | description                                                  |
| ------------------------- | ------------------------------------------------------------ |
| command &                 | **background** execution of command in the **subshell**      |
| command1 \| command2      | **pipe** the standard output of command1 to the standard input of command2 (concurrent execution) |
| command1 2>&1 \| command2 | **pipe** both standard output and standard error of command1 to the standard input  of command2 (concurrent execution) |
| command1 ; command2       | execute command1 and command2 **sequentially**               |
| command1 && command2      | execute command1; if successful,  execute command2 **sequentially** (return success if  both command1 and command2 are successful) |
| command1 \|\| command2    | execute command1; if not successful,  execute command2 **sequentially** (return success  if command1 or command2 are successful) |
| command > foo             | redirect standard output of command to  a file foo (overwrite) |
| command 2> foo            | redirect standard error of command to  a file foo (overwrite) |
| command >> foo            | redirect standard output of command to  a file foo (append)  |
| command 2>> foo           | redirect standard error of command to  a file foo (append)   |
| command > foo 2>&1        | redirect both standard output and standard error of command to a file foo |
| command < foo             | redirect  standard input of command to a file foo            |
| command << delimiter      | redirect  standard input of command to the following lines until "delimiter" is met (here document) |
| command <<- delimiter     | redirect  standard input of command to the following lines until "delimiter" is met (here document, the leading tab characters are  stripped from input lines) |

The shell allows you to open files using the`exec` builtin with an arbitrary file descriptor.

```bash
$ echo Hello >foo
$ exec 3<foo 4>bar  # open files, 3 and 4 are now file descriptors
$ cat <&3 >&4       # redirect stdin to 3, stdout to 4
$ exec 3<&- 4>&-    # (&-) close files
$ cat bar
Hello
```

The file descriptor 0-2 are predefined.

| device | description      | file descriptor |
| ------ | ---------------- | --------------- |
| stdin  | standard  input  | 0               |
| stdout | standard  output | 1               |
| stderr | standard  error  | 2               |

### Command alias

You can set an alias for the frequently used command.

For example, try the following

```bash
$ alias la='ls -la'
```

Now, "`la`" works as a short hand for "`ls -la`" which lists all files in the long listing format.

You can list any existing aliases by `alias`.

```bash
$ alias
...
alias la='ls -la'
```

You can identity exact path or identity of the command by `type` 

```bash
$ type ls
ls is hashed (/bin/ls)
$ type la
la is aliased to ls -la
$ type echo
echo is a shell builtin
$ type file
file is /usr/bin/file
```

Here `ls` was recently searched while "`file`" was not, thus "`ls`" is "hashed", i.e., the shell has an internal record for the quick access to the location of the "`ls`" command.

## Unix-like text processing

In Unix-like work environment, text processing is done by piping text through chains of standard text processing tools. This was another crucial Unix innovation.

### Unix text tools

- No regular expression is used:
  - cat(1) concatenates files and outputs the whole content.
  - tac(1) concatenates files and outputs in reverse.
  - cut(1) selects parts of lines and outputs.
  - head(1) outputs the first part of files.
  - tail(1) outputs the last part of files.
  - sort(1) sorts lines of text files.
  - uniq(1) removes duplicate lines from a sorted file.
  - tr(1) translates or deletes characters.
  - diff(1) compares files line by line.
- Basic regular expression (**BRE**) is used:
  - grep(1) matches text with patterns.
  - ed(1) is a primitive line editor.
  - sed(1) is a stream editor.
  - vim(1) is a screen editor.
  - emacs(1) is a screen editor. (somewhat extended **BRE**)
  - find
- Extended regular expression (**ERE**) is used:
  - egrep(1) matches text with patterns.
  - awk(1) does simple text processing.
  - tcl(3tcl) can do every conceivable text processing: See re_syntax(3). Often used with tk(3tk).
  - perl(1) can do every conceivable text processing. See perlre(1).
  - pcregrep(1) from the `pcregrep` package matches text with [Perl Compatible Regular Expressions (PCRE)](https://en.wikipedia.org/wiki/Perl_Compatible_Regular_Expressions) pattern.
  - python(1) with the `re` module can do every conceivable text processing. See "`/usr/share/doc/python/html/index.html`".

### Regular expressions

The regular expression describes the matching pattern and is made up of text characters and **metacharacters**.

A **metacharacter** is just a character with a special meaning. There are 2 major styles, **BRE** and **ERE**, depending on the text tools as described above.

Ref: [1](https://learnbyexample.github.io/learn_gnused/breere-regular-expressions.html), 

| BRE                        | description of the regular expression                        |
| -------------------------- | ------------------------------------------------------------ |
| BRE |Basic Regular Expression, enabled by default|
| ERE | Extended Regular Expression, enabled using `-E` option |
|  | **note**: only ERE syntax is covered below |
| metacharacters | characters with special meaning in REGEXP |
| `c`                        | match non-metacharacter `c`                               |
| `\c`                       | match a literal character `c` even if `c` is metacharacter by itself |
| \d                         | match single digit from 0 to 9, equivalent to [0-9],        |
| \D                         | match any non-digit character                                |
| \b                         | match the boundary between a word and a non-word character   |
| \w                         | match a character, equivalent to [A-Za-z0-9_]                |
| \W                         | match any non-alphanumeric character, e.g. punctuation.      |
| \s                         | match a space, including \\n,\\t,\\r                         |
| \S                         | match any non-whitespace character                           |
| .                          | match single character including newline                     |
| ^r                         | position at the beginning of a string                        |
| (r)                        | capture groups of characters for further processing, e.g. (IMG\d+).pdf |
| (?:r)                      | just make groups, without capture, thus can not be referred  |
| (r1(r2))                   | capture nested groups by nested parenthesis, e.g. (IMG(\d+))\.png |
| [abc…]                     | match single characters in "abc…"                            |
| [^abc…]                    | match single characters except in "abc…"                     |
| r*                         | match zero or more regular expressions identified by "r"     |
| r\\+                       | match one or more regular expressions identified by "r"      |
| r\\?                       | match zero or one regular expressions identified by "r"      |
| w{3}                       | match a character 'w' 3 times. 'w' can also be like [a-zA-Z] |
| w{1,3}                     | match a character 'w' no more than 3 times, but no less than once |
| a(?=b)                     | positive lookahead 先行断言，a 只有在 b 前面才匹配           |
| a(?!b)                     | negative lookahead 先行否定断言，a 只有不在 b 前面才匹配     |
| (?<=b)a                    | positive lookbehind 后行断言，a 只有在 b 后面才匹配          |
| (?<!b)a                    | negative lookbehind 后行否定断言，a 只有不在 b 后面才匹配    |

Regular expression cheat sheet

| Regex         | Description     |
| ------------- | --------------- |
| ^$            | 空行 empty line |
| [\w\|\s\|,]*. | 一个英文句子    |
| .*            | 一行            |

### Replacement expressions

| replacement  expression | description of  the text to replace the replacement expression |
| ----------------------- | ------------------------------------------------------------ |
| &                       | what  the regular expression matched (use \& in emacs)       |
| \n                      | what  the n-th bracketed regular  expression matched ("n" being number) |

### Script snippets for piping commands

| script  snippet (type in one line) | effect of command                                            |
| ---------------------------------- | ------------------------------------------------------------ |
| find /usr -print                   | find  all files under "/usr"                                 |
| seq 1 100                          | print  1 to 100                                              |
| \| xargs -n 1 <command>            | run  command repeatedly with each item from pipe as its argument |
| \| xargs -n 1 echo                 | split  white-space-separated items from pipe into lines      |
| \| xargs echo                      | merge  all lines from pipe into a line                       |
| \| grep -e <regex_pattern>         | extract  lines from pipe containing <regex_pattern>          |
| \| grep -v -e <regex_pattern>      | extract  lines from pipe not containing <regex_pattern>      |
| \| cut -d: -f3 -                   | extract  third field from pipe separated by ":" (passwd file etc.) |
| \| awk '{ print $3 }'              | extract  third field from pipe separated by whitespaces      |
| \| awk -F'\t' '{ print $3 }'       | extract  third field from pipe separated by tab              |
| \| col -bx                         | remove  backspace and expand tabs to spaces                  |
| \| expand -                        | expand  tabs                                                 |
| \| sort\| uniq                     | sort  and remove duplicates                                  |
| \| tr 'A-Z' 'a-z'                  | convert  uppercase to lowercase                              |
| \| tr -d '\n'                      | concatenate  lines into one line                             |
| \| tr -d '\r'                      | remove  CR                                                   |
| \| sed 's/^/# /'                   | add  "#" to the start of  each line                          |
| \| sed 's/\.ext//g'                | remove  ".ext"                                               |
| \| sed -n -e 2p                    | print  the second line                                       |
| \| head -n 2 -                     | print  the first 2 lines                                     |
| \| tail -n 2 -                     | print  the last 2 lines                                      |

## The shell script

The shell script is a text file with the execution bit set and contains the commands in the following format.

```shell
#!/bin/sh
 ... command lines
```

The first line specifies the shell interpreter which read and execute this file contents.

### POSIX shell compatibility

Avoid writing a shell script with **bashisms** or **zshisms** to make it portable among all POSIX shells. You can check it using checkbashisms(1).

| Good:  POSIX                        | Avoid: bashism                          |
| ----------------------------------- | --------------------------------------- |
| `if [ "$foo" = "$bar" ] ; then ...` | `if  [ "\$foo" == "$bar" ] ; then ... ` |
| `diff -u file.c.orig file.c`        | `diff  -u file.c{.orig,}`               |
| `mkdir /foobar /foobaz`             | `mkdir  /foo{bar,baz}`                  |
| `funcname() { … }`                  | `function  funcname() { … }`            |
| `octal format: "\377"`              | `hexadecimal  format: "\xff"`           |

### First Script

```bash
# First script
$ echo "#!/bin/sh" > my_script.sh # create shell script
$ echo "Hello World" >> my_script.sh # add a command
$ echo "# This is a comment!" >> my_script.sh # add a comment
$ chmod 755 my_script.sh # add execution permisstion
$ ./my_script.sh # run script
```

### Variables

Special shell parameters are frequently used in the shell script.

| CMD  | Description                                                  | Example                            |
| ---- | ------------------------------------------------------------ | ---------------------------------- |
| $$   | PID  of current shell                                        | `echo  "my PID = $$"`              |
| $!   | PID  of last background command                              | `ls  & echo "PID of ls = $!"`      |
| $?   | exit  status of last command                                 | `ls  ; echo "ls returned code $?"` |
| $0   | Name  of current command (as called)                         | `echo  "I am $0"`                  |
| $1   | Name  of current command's first parameter                   | `echo  "My first argument is $1"`  |
| $9   | Name  of current command's ninth parameter                   | `echo  "My ninth argument is $9"`  |
| $@   | All  of current command's parameters (preserving whitespace and quoting) | `echo  "My arguments are $@"`      |
| $*   | All  of current command's parameters (not preserving whitespace and quoting) | `echo  "My arguments are $*"`      |

**List of shell parameter expansions**

| parameter  expression form | value if var is set | value if var is not set                        |
| -------------------------- | ------------------- | ---------------------------------------------- |
| ${var:-string}             | "$var"              | "string"                                       |
| ${var:+string}             | "string"            | "null"                                         |
| ${var:=string}             | "$var"              | "string" (and run "var=string")                |
| ${var:?string}             | "$var"              | echo  "string" to stderr (and exit with error) |

Here, the colon "`:`" in all of these operators is actually optional.

+ with ":" = operator test for exist and not null
+ without ":" = operator test for exist only

List of key shell parameter substitutions

| parameter  substitution form | result                          |
| ---------------------------- | ------------------------------- |
| ${var%suffix}                | remove  smallest suffix pattern |
| ${var%%suffix}               | remove  largest suffix pattern  |
| ${var#prefix}                | remove  smallest prefix pattern |
| ${var##prefix}               | remove  largest prefix pattern  |

List of file comparison operators in the conditional expression

| equation            | condition to  return logical true                            |
| ------------------- | ------------------------------------------------------------ |
| -e <file>           | <file>  exists                                               |
| -d <file>           | <file>  exists and is a directory                            |
| -f <file>           | <file>  exists and is a regular file                         |
| -w <file>           | <file>  exists and is writable                               |
| -x <file>           | <file>  exists and is executable                             |
| <file1> -nt <file2> | <file1>  is newer than <file2> (modification)                |
| <file1> -ot <file2> | <file1>  is older than <file2> (modification)                |
| <file1> -ef <file2> | <file1>  and <file2> are on the same device and the same inode number |

List of string comparison operators in the conditional expression

| equation         | condition to  return logical true              |
| ---------------- | ---------------------------------------------- |
| -z <str>         | the  length of <str> is zero                   |
| -n <str>         | the  length of <str> is non-zero               |
| <str1> = <str2>  | <str1>  and <str2> are equal                   |
| <str1> != <str2> | <str1>  and <str2> are not equal               |
| <str1> < <str2>  | <str1>  sorts before <str2> (locale dependent) |
| <str1> > <str2>  | <str1>  sorts after <str2> (locale dependent)  |



```bash
#!/bin/sh
echo "Your last name is $1"
echo "What is your name?"
read My_Name # interactively set variable names using the read command.
Variable1="Hello" # Note that there must be no spaces around the "=" sign, VAR=value works, VAR = value doesn't work.
echo "$Variable1, $My_Name. Nice to meet you!" # output variables.
touch "${My_Name}_10_April.txt" # enclose the variable itself in curly brackets to refer to the variable with a suffix.

$ ./my_script.sh Lee
Your last name is Lee
What is your name?
Levante # Typed in the shell.
Hello, Levante. Nice to meet you!
```

 

```bash
# Variables - Part 2

#!/bin/sh
echo "VAR is: $Variable2"
Variable2=Levante
echo "VAR is: $Variable2"

$ Variable2="Lee"
$ ./my_script.sh
VAR is:			# Ops, we got nothing here
VAR is: Levante
$ echo $Variable2
Lee
# 1.Variables in the Bourne shell do not have to be declared
# 2.If you try to use an undeclared variable, the result is the empty string and you get no warnings or errors. 
# 3.variable created in your shell won't be passed or inherited by your script, unless you "export" it. Because it will use another new shell environment.

$ Variable2="Lee"
$ export Variable2
$ ./my_script.sh
VAR is: Lee
VAR is: Levante
$ echo $Variable2
Lee
```

 

```bash
# Variables - Part 3

# 4.if you want to receive environment changes back from the script, you should "source" the script by add .(dot) ahead of your script path. And shell will use your current environment.
$ Variable2="Lee"
# $ export Variable2 So you don't need this command.
$ . ./my_script.sh
VAR is: Lee
VAR is: Levante
$ echo $Variable2
Levante

#--------------------------------------------------
# Wildcards
*
```

 

```bash
# Variables - Part 4
# Positional Parameters
#!/bin/sh
echo "I was called with $# parameters"
echo "My name is $0"
echo "My first parameter is $1"
echo "My second parameter is $2"
echo "All parameters are $@"

$ ./my_script.sh hello world earth
I was called with 3 parameters
My name is ./my_script.sh
My first parameter is hello
My second parameter is world
All parameters are hello world earth

# Return Codes
# $? contains the exit value of the last run command
#!/bin/sh
/usr/local/bin/my-command # run a command
if [ "$?" -ne "0" ]; then
  echo "Sorry, we had a problem there!" # return 0 means OK
fi

# Default values
# Way 1
#!/bin/sh
echo "What is your name [ `whoami` ] "
read myname
if [ -z "$myname" ]; then # -z means [ Var = 0 ]
  myname=`whoami`
fi
echo "Your name is : $myname"

Levante$ ./name.sh	# without user's input
What is your name [ Levante ]
Your name is : Levante

# Way 2
echo "What is your name [ `whoami` ] "
read myname
echo "Your name is : ${myname:-`whoami`}" # ":-" specify a default value to use if the variable is unset
```

### Loops

+ "`for x in foo1 foo2 … ; do command ; done`" loops by assigning items from the list "`foo1 foo2 …`" to variable "`x`" and executing "`command`".
+ "`while condition ; do command ; done`" repeats "`command`" while "`condition`" is true."
+ `until condition ; do command ; done`" repeats "`command`" while "`condition`" is not true.
+ "`break`" enables to exit from the loop.
+ "`continue`" enables to resume the next iteration of the loop.

Tip: The C language like numeric iteration can be realized by using seq(1) as the `"foo1 foo2 …"` generator.

```bash
# "for" loops iterate through a set of values until the list is exhausted:
#!/bin/sh
for i in hello 1 * 2 goodbye 
do
  echo "Looping ... i is set to $i"
done

$ ls
my_script.sh  my_script.txt
$ ./my_script.sh
Looping ... i is set to hello
Looping ... i is set to 1
Looping ... i is set to my_script.sh	# file in the dir
Looping ... i is set to my_script.txt	# file in the dir
Looping ... i is set to 2
Looping ... i is set to goodbye

# while loop
#!/bin/sh
INPUT_STRING=hello
while [ "$INPUT_STRING" != "bye" ]
do
  echo "Please type something in (bye to quit)"
  read INPUT_STRING
  echo "You typed: $INPUT_STRING"
done

# 1.The colon (:) always evaluates to true.
while :
do
	echo "Looping"
done

#!/bin/sh
X=0
while [ -n "$X" ]
do
  echo "Enter some text (RETURN to quit)"
  read X
  if [ -n "$X" ]; then
    echo "You said: $X"
  fi
done
```

### Conditionals

Each command returns an **exit status** which can be used for conditional expressions.

+ Success: 0 ("True")
+ Error: non 0 ("False")

Note : "`[`" is the equivalent of the `test` command, which evaluates its arguments up to "`]`" as a conditional expression.

Basic **conditional idioms** to remember are the following.

+ `<command> && <if_success_run_this_command_too> || true`

+ `<command> || <if_not_success_run_this_command_too> || true`

+ A multi-line script snippet as the following

  ```bash
  if [ <conditional_expression> ]; then
   <if_success_run_this_command>
  else
   <if_not_success_run_this_command>
  fi
  ```

+ Here trailing "`|| true`" was needed to ensure this shell script does not exit at this line accidentally when shell is invoked with "`-e`" flag.

List of file comparison operators in the conditional expression



```bash
# "[" is a symbolic link to "test", just to make shell programs more readable. Watch out the space around operators.
# if ["$var"="bar"] won't work
# if [ "$var" = "bar" ] worked

#!/bin/sh
echo "Please input a string in [bar, foo]"
read var
if [ "$var" = "bar" ]
then
	echo "bar"
elif [ "$var" = "foo" ]; then # semicolon (;) to join two lines together.
    echo "foo"
else
    echo "Undefined input"
fi

# Shorthand Syntax for if []
#!/bin/sh
if [ A = B ]
then
	echo C
	echo D
else
    echo E
    echo F
fi

#!/bin/sh
[ A = B ] && echo C && echo D || \
	echo E && echo F
# ackslash (\) is used to split the single-line command across two lines in the shell script file

#!/bin/sh
if [ "$X" -lt "0" ]
then
  echo "X is less than zero"
fi
if [ "$X" -gt "0" ]; then
  echo "X is more than zero"
fi
[ "$X" -le "0" ] && \
      echo "X is less than or equal to  zero"
[ "$X" -ge "0" ] && \
      echo "X is more than or equal to zero"
[ "$X" = "0" ] && \
      echo "X is the string or number \"0\""
[ "$X" = "hello" ] && \
      echo "X matches the string \"hello\""
[ "$X" != "hello" ] && \
      echo "X is not the string \"hello\""
[ -n "$X" ] && \
      echo "X is of nonzero length"
[ -f "$X" ] && \
      echo "X is the path of a real file" || \
      echo "No such file: $X"
[ -x "$X" ] && \
      echo "X is the path of an executable file"
[ "$X" -nt "/etc/passwd" ] && \
      echo "X is a file which is newer than /etc/passwd"
```

+ Brace Expansion

```bash
# Expanding Lists
$ echo {1,2,3}" is a num."
1 is a num. 2 is a num. 3 is a num.

$ echo {1,2,3} " is a num."
1 2 3  is a num.

$ echo {1,2,3} "is a num."
1 2 3 is a num.

$ mkdir -p ./{p1,p2}/{c1,c2} # will create ./p1/c1 ./p1/c2 ./p2/c1 ./p2/c2
```

### Case

```bash
#!/bin/sh
echo "Please talk to me ..."
while :
do
  read INPUT_STRING
  case $INPUT_STRING in
	hello)
		echo "Hello yourself!"
		;;
	bye)
		echo "See you again!"
		break # break out of the while loop
		;;
	*)
		echo "Sorry, I don't understand"
		;;
  esac
done
```

+ Select (bash only)

```bash
#!/bin/bash
# Define the menu list here
select v in data_list
do
    statement1
    ...
done
# you have to use ^C to termiate it because there is no "break" in the script


# select can be used with "case" and "if-then" and so on
#!/bin/bash
echo "Which Operating System do you like?"
# Operating system names are used here as a data source
select os in Ubuntu Debian Windows8 Windows7 WindowsXP "(Exit)"
do
    case $os in
        # Two case values are declared here for matching
        "Ubuntu"|"Debian")
            echo "I also use $os."
            ;;
        # Three case values are declared here for matching
        "Windows8" | "Windows10" | "WindowsXP")
            echo "Why don't you try Linux?"
            ;;
        "(Exit)")
            echo "Bye"
            break
            ;;
        # Matching with invalid data
        *)
            echo "Invalid entry."
            break
            ;;
    esac
done

$ ./my_script.sh
Which Operating System do you like?
1) Ubuntu
2) Debian
3) Windows8
4) Windows7
5) WindowsXP
6) \(Exit\)
#? 6
Bye
```

+ External programs

```bash
# The backtick (`) is used to indicate that the enclosed text is to be executed as a command

$ MYNAME=`grep "^${USER}:" /etc/passwd | cut -d: -f5`
$ echo MYNAME
levante

#!/bin/sh
HTML_FILES=`find / -name "*.html" -print`
echo "$HTML_FILES" | grep "/index.html$"
echo "$HTML_FILES" | grep "/contents.html$"
# Note: the quotes around $HTML_FILES are essential to preserve the newlines between each file listed.

# $(...) is same as '...'
```

### Functions

```bash
#!/bin/sh
# A simple script with a function...
add_a_user()
{
  USER=$1
  PASSWORD=$2
  shift; shift;
  # Having shifted twice, the rest is now comments ...
  COMMENTS=$@
  echo "Adding user $USER ..."
  echo useradd -c "$COMMENTS" $USER # Prefixed with "echo" can be a useful debugging technique to check that the right commands would be executed without being root
  echo passwd --stdin $USER $PASSWORD
  echo "Added user $USER ($COMMENTS) with pass $PASSWORD"
}
# Main body of scripts starts here
...
add_a_user Levante 123 Super man
...
# Main body of scripts ends here

$ ./my_script.sh
Adding user Levante ...
useradd -c Super man Levante
passwd --stdin Levante 123
Added user Levante (Super man) with pass 123
```



```bash
# Scope of variables
#!/bin/sh
myfunc()
{
  echo "myfunc was called as : $@"
  x=2
}

echo "Script was called with $@"
x=1
echo "x is $x"
myfunc 1 2 3
echo "x is $x"

$ ./my_script.sh a b c
Script was called with a b c
x is 1
myfunc was called as : 1 2 3	# means $@ will change between functions
x is 2	# means this variable is a global variable


#!/bin/sh
myfunc()
{
    echo "\$1 is $1"
    a=World # we canot change $1, $1=World is not a valid syntax, but we can change global variable
}
a=Hello
myfunc $a
echo "a is $a"

$ ./my_script.sh
$1 is Hello
a is World
```



```bash
# Recursion
#!/bin/sh

factorial()
{
  if [ "$1" -gt "1" ]; then
    i=`expr $1 - 1`
    j=`factorial $i`
    k=`expr $1 \* $j`
    echo $k
  else
    echo 1
  fi
}
```



```bash
# Libraries

In common.lib:
# common.lib
# Note no #!/bin/sh as this should not spawn 
# an extra shell.
STD_MSG="About to rename some files..."

rename()
{
  # expects to be called as: rename .txt .bak 
  FROM=$1
  TO=$2

  for i in *$FROM
  do
    j=`basename $i $FROM`
    mv $i ${j}$TO
  done
}

In function2.sh:
#!/bin/sh
# function2.sh
. ./common.lib
echo $STD_MSG
rename .txt .bak

In function3.sh:
#!/bin/sh
# function3.sh
. ./common.lib
echo $STD_MSG
rename .html .html-bak

# Here we see two user shell scripts, function2.sh and function3.sh, each "sourceing" the common library file common.lib, and using variables and functions declared in that file.
```



```bash
# Return codes
# Note: the return value from function is a single byte, so it can only have a value between 0 and 255.

#!/bin/sh
myfunc()
{
    if [ $1 = $2 ]; then
        return 0
    else
        return 1
    fi
}

myfunc 1 2
echo "\$? = $?"

$ ./my_script.sh
$? = 1
```

### Links

[Shell Mistakes](http://www.greenend.org.uk/rjk/tech/shellmistakes.html)

[Linux基础](https://linuxtools-rst.readthedocs.io/zh_CN/latest/base/index.html#id2)































