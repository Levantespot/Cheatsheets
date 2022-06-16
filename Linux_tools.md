# Tools for Linux

## Ref

- https://linuxtools-rst.readthedocs.io/zh_CN/latest/tool/index.html

## ps

Information about running processes. More information: https://manned.org/ps.

- List all running processes:
  - `ps -aux`: USER, PID, %CPU, %MEM, VSZ, RSS, TTY, STAT, START, TIME, COMMAND
  - `ps -ef`: UID, PID, PPID, C, STIME, TTY, TIME, CMD
  - 上面任一指令的 options 中增加 `ww`，如 `ps -efww`，会显示完整的 command。
    - Search for a process that matches a string:
  - `ps aux | grep {{string}}`
    - List all processes of the current user in extra full format:
  - `ps --user $(id -u) -F`
    - Get the parent PID of a process:
  - `ps -o ppid, pid, wchan -p {{pid}}`

## pstree

pstree
A convenient tool to show running processes as a tree. More information: https://manned.org/pstree.

- Display a tree of processes:
  `pstree`

- Display a tree of processes with PIDs:
  `pstree -p`

## netstat

Displays network-related information such as open connections, open socket ports, etc. Root privileges (or sudo) is required to list information created by others. More information: https://man7.org/linux/man-pages/man8/netstat.8.html.

- List all ports(a) and display PID and program names(p) and do not resolve IP addresses to hostnames(n): `netstat -apn`
- List listening TCP ports:
   `netstat --tcp`
- List routes and do not resolve IP addresses to hostnames:
  - `netstat --route --numeric`
  - `netstat -rn`

## lsof

Lists open files and the corresponding processes.Note: Root privileges (or sudo) is required to list files opened by others. More information: https://manned.org/lsof.

- Find the processes that have a given file open:
  `lsof {{path/to/file}}`
  - Find the process that opened a local internet port:
    `lsof -i :{{port}}`
  - List files opened by the given user:
    `lsof -u {{username}}`
  - List files opened by the given command or process:
    `lsof -c {{process_or_command_name}}`
  - List files opened by a specific process, given its PID:
    `lsof -p {{PID}}`

Head: COMMAND, PID, TID, TASKCMD, USER, FD, TYPE, DEVICE, SIZE/OFF, NODE NAME

- COMMAND：进程的名称

- PID：进程标识符

- PPID：父进程标识符（需要指定-R参数）

- USER：进程所有者

- PGID：进程所属组

- FD：文件描述符，应用程序通过文件描述符识别该文件。如cwd、txt等：
  
  ```
  （1）cwd：表示current work dirctory，即：应用程序的当前工作目录，这是该应用程序启动的目录，除非它本身对这个目录进行更改
  （2）txt ：该类型的文件是程序代码，如应用程序二进制文件本身或共享库，如上列表中显示的 /sbin/init 程序
  （3）lnn：library references (AIX);
  （4）er：FD information error (see NAME column);
  （5）jld：jail directory (FreeBSD);
  （6）ltx：shared library text (code and data);
  （7）mxx ：hex memory-mapped type number xx.
  （8）m86：DOS Merge mapped file;
  （9）mem：memory-mapped file;
  （10）mmap：memory-mapped device;
  （11）pd：parent directory;
  （12）rtd：root directory;
  （13）tr：kernel trace file (OpenBSD);
  （14）v86  VP/ix mapped file;
  （15）0：表示标准输入
  （16）1：表示标准输出
  （17）2：表示标准错误
  ...
  ```

- TYPE：文件类型，如DIR、REG等：
  
  ```
  （1）DIR：表示目录
  （2）CHR：表示字符类型
  （3）BLK：块设备类型
  （4）UNIX： UNIX 域套接字
  （5）FIFO：先进先出 (FIFO) 队列
  （6）IPv4：网际协议 (IP) 套接字
  ```

- DEVICE：指定磁盘的名称

- SIZE：文件的大小

- NODE：索引节点（文件在磁盘上的标识）

- NAME：打开文件的确切名称

## tmux

Terminal multiplexer. It allows multiple sessions with windows, panes, and more.See also zellij and screen.More information: https://github.com/tmux/tmux.

### Ref

- [Tmux Cheat Sheet & Quick Reference](https://tmuxcheatsheet.com/)
- [tmux-cheatsheet.markdown](https://gist.github.com/MohamedAlaa/2961058)

### Cheatsheet

start new:

```shell
$ tmux
```

start new with session name (and window name):

```shell
$ tmux new -s <mysession>
$ tmux new -s <mysession> -n <mywindow>
```

attach:

```shell
$ tmux a(or at, or attach, or attach-session) <session_number> 
```

attach to named:

```shell
$ tmux a -t <myname>
```

list sessions:

```shell
$ tmux ls
```

kill session:

```shell
$ tmux kill-session -t myname
```

### Shortcuts

Usage: `CTRL-B` + shortcuts

**Sessions**

```
:new<CR>      # New session
s              # List sessions
w            # Session and Window Preview
$              # Rename session
d            # Detach from session
(            # Move to previous session
)            # Move to next session
```

**Windows**

```
c            # Create window
,            # Rename current window
&            # Close current window
p            # Previous window
n            # Next window
<#>            # Switch/select window by number
l            # Toggle last active window
```

**Panes**

```
%            # 
"            # Split pane with vertical layout
↑/↓/←/→        # Switch to pane to the direction
z            # Toggle pane zoom
x            # Close current pane
!           # Convert pane into a window
Ctrl+↑/↓/←/→    # Resize current pane
[            # enter scroll mode (Use PageUp & PageDown, enter `q` to quit scroll mode)
```
