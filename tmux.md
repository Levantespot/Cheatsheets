## Tmux cheat sheet

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

Usage: `CTRL-B` + shortcuts

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
```

