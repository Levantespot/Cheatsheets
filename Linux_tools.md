# Tools for Linux

[TOC]

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
 - List all processes of the current user as a tree:
   - `ps --user $(id -u) f`
 - Get the parent PID of a process:
   - `ps -o ppid, pid, wchan -p {{pid}}`

## netstat

Displays network-related information such as open connections, open socket ports, etc. More information: https://man7.org/linux/man-pages/man8/netstat.8.html.

- List all ports(a) and display PID and program names(p) and do not resolve IP addresses to hostnames(n):
   - `netstat -apn`
- List listening TCP ports:
   `netstat --tcp`
- List routes and do not resolve IP addresses to hostnames:
   - `netstat --route --numeric`
   - `netstat -rn`
- 

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
:new<CR>  	# New session
s  			# List sessions
w			# Session and Window Preview
$  			# Rename session
d			# Detach from session
(			# Move to previous session
)			# Move to next session
```

**Windows**

```
c			# Create window
,			# Rename current window
&			# Close current window
p			# Previous window
n			# Next window
<#>			# Switch/select window by number
l			# Toggle last active window
```

**Panes**

```
%			# 
"			# Split pane with vertical layout
↑/↓/←/→		# Switch to pane to the direction
z			# Toggle pane zoom
x			# Close current pane
!           # Convert pane into a window
Ctrl+↑/↓/←/→	# Resize current pane
[			# enter scroll mode (Use PageUp & PageDown, enter `q` to quit scroll mode)
```

