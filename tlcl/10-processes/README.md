The Linux kernel manages `multitasking` through the use of `processes`.

When a system starts up, the kernel initiates a few of its own
activities as processes and launches a program called `init`.

`init` runs a series of shell scripts (in `/etc`) called
`init scripts`, which start all the system services.
Many of these services are implemented as `daemon programs`,
programs that just sit in the background and do their thing without
having any user interface.

The fact that a program can launch other programs is expressed
in the process scheme as a `parent process` producing a
`child process`.

The kernel maintians info about each process to help keep things
organized. For example, each process is assigned a number called
a process `PID`. PID(s) are assigned in ascending order, with
`init` always getting PID 1. The kernel keep track of 2 things:
- memory assigned to each process
- process' readiness to resume execution

Like files, processes also have owners and user IDs,
effective user IDs, etc.

The view processes, use `ps`.
```
erkhes@rexes:~$ ps
    PID TTY          TIME CMD
   9125 pts/2    00:00:00 bash
  93439 pts/2    00:00:00 ps
```
By default, `ps` doesn't show much, just the processes associated
with the current terminal session.

`TTY` is short for "teletype," and refers to the
`controlling terminal` for the process.

`TIME` is the amount of CPU time consumed by the process.

```
erkhes@rexes:~$ ps x
    PID TTY      STAT   TIME COMMAND
   2142 ?        Ss     0:01 /lib/systemd/systemd --user
   2143 ?        S      0:00 (sd-pam)
   2149 ?        S<sl   0:00 /usr/bin/pipewire
   2150 ?        Ssl    0:10 /usr/bin/pipewire-media-session
   2151 ?        S<sl   8:23 /usr/bin/pulseaudio --daemonize=no --log-target=journal
   2162 ?        Ss     0:06 /usr/bin/dbus-daemon --session --address=systemd: --nofork --nopidfile --systemd-activation --syslog-only
   2164 ?        Sl     0:01 /usr/bin/gnome-keyring-daemon --daemonize --login
   2173 ?        Ssl    0:00 /usr/libexec/gvfsd
   2178 ?        Sl     0:00 /usr/libexec/gvfsd-fuse /run/user/1000/gvfs -f
   2198 ?        Ssl    0:01 /usr/libexec/xdg-document-portal
   2201 ?        Ssl    0:00 /usr/libexec/xdg-permission-store
   2219 ?        SNsl   0:16 /usr/libexec/tracker-miner-fs-3
   2241 ?        Ssl    0:03 /usr/libexec/gvfs-udisks2-volume-monitor
   2247 ?        Ssl    0:00 /usr/libexec/gvfs-mtp-volume-monitor
   2251 ?        Ssl    0:00 /usr/libexec/gvfs-goa-volume-monitor
   2256 ?        Sl     0:00 /usr/libexec/goa-daemon
   2266 ?        Sl     0:00 /usr/libexec/goa-identity-service
   2271 ?        Ssl    0:00 /usr/libexec/gvfs-gphoto2-volume-monitor
   2276 ?        Ssl    0:02 /usr/libexec/gvfs-afc-volume-monitor
   2311 tty2     Ssl+   0:00 /usr/libexec/gdm-wayland-session env GNOME_SHELL_SESSION_MODE=ubuntu /usr/bin/gnome-session --session=ubun
   2314 tty2     Sl+    0:00 /usr/libexec/gnome-session-binary --session=ubuntu
```

Adding the `x` option tells `ps` to show all of our processes
regardless of what terminal (if any) they are controlled by.

The presence of a "?" in the `TTY` column indicates no controlling
terminal.

`STAT` is short for "state" and reveals the current status of the
process.

Process States:
- `R`: Running. This means that the process is running or ready to run.
- `S`: Sleeping. The process is not running; rather, it is waiting
for an event, such as a keystroke or network packet.
- `D`: Uninterruptiple sleep. The process is waiting for I/O such as
a disk drive.
- `T`: Stopped. The process has been instructed to stop.
- `Z`: A defunct or "zombie" process. This is a child process that
has terminated but has not been cleaned up by its parent.
- `<`: A high-priority process. It is possible to grant more
importance to a process, giving it more time on the CPU. This
property of a process is called `niceness`. A process with high
priority is said to be less `nice`.
- `N`: low-priority process. A.K.A nice process.

The process state may be followed by other characters.
These indicate various exotic process characteristics.

`aux` option displays the processes belonging to every user.
```
erkhes@rexes:~$ ps aux
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root           1  0.0  0.0 168392 13820 ?        Ss   09:14   0:04 /sbin/init 
root           2  0.0  0.0      0     0 ?        S    09:14   0:00 [kthreadd]
root           3  0.0  0.0      0     0 ?        I<   09:14   0:00 [rcu_gp]
root           4  0.0  0.0      0     0 ?        I<   09:14   0:00 [rcu_par_gp
root           5  0.0  0.0      0     0 ?        I<   09:14   0:00 [slub_flush
root           6  0.0  0.0      0     0 ?        I<   09:14   0:00 [netns]
```

- `USER`: User ID. This is the owner of the process.
- `%CPU`: CPU usage in percent.
- `%MEM`: Memory usage in percent.
- `VSZ`: Virtual memory size.
- `RSS`: Resident set size. This is the amount of physical memory
(RAM) the process is using in kilobytes.
- `START`: Time when the process started. For values over 24 hours, a date is used.

`ps` command provides only a snapshot of the machine's state
at the moment `ps` command is executed. To see the dynamic view of
the machine's activity, use `top`.

`top` program updates every three seconds (by default) display
of the system processes listed in order of process activity.

The `top` display consists of two parts: a system summary at the
top of the display, followed by a table of processes sorted by
CPU activity.
```
top - 21:54:41 up 12:39,  1 user,  load average: 1.02, 0.92, 0.95
Tasks: 394 total,   1 running, 393 sleeping,   0 stopped,   0 zombie
%Cpu(s):  6.8 us,  0.9 sy,  0.0 ni, 92.3 id,  0.0 wa,  0.0 hi,  0.1 si,  0.0 st
MiB Mem :  31762.3 total,  17393.2 free,   5833.2 used,   8535.9 buff/cache
MiB Swap:   2048.0 total,   2048.0 free,      0.0 used.  23904.2 avail Mem 

    PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND                          
  81262 erkhes    20   0 1132.4g 717492 170492 S  68.9   2.2  34:28.89 chrome                           
   2385 erkhes    20   0 6062236 379448 171128 S   8.6   1.2  56:57.65 gnome-shell                      
   3535 erkhes    20   0   33.3g 241636 136172 S   3.3   0.7  26:05.33 chrome                           
   7176 erkhes    20   0  665604 106132  63840 S   2.3   0.3   3:42.88 gnome-terminal-                  
  86066 erkhes    20   0 3719596 238396 148688 S   1.3   0.7   0:47.58 spotify                          
    788 root      20   0 2022016  45232  32136 S   0.7   0.1   4:45.43 containerd
```
|Row|Field|Meaning|
| ------------- |:-------------:| -----:|
|1|top|The name of the program|
||21:54:41|The current time of day|
||up 12:39|This is called `uptime`. It is the amount of time since the machine was booted.|
||1 user|There are 1 user logged in|
||load average:| `Load average` refers to the number of processes that are waiting to run, that is, the number of processes that are in a runnable state and are sharing the CPU. Three values are shown, each for a different period of time. The first is the average for the last min, the next the 5 mins, and finally the previous 15 minutes. Values less than 1.0 indicate that the machine is not busy.|
|2|Tasks:|Number of processes and their various process states.|
|3|Cpu(s):|This row describes the character of the activities that the CPU is performing|
||6.8 us|6.8 percent of the CPU is being used for user processes|
||0.9 sy|0.9 percent of the CPU is being used for system(kernel) processess|
||0.0 ni|0.0 percent of the CPU is being used for nice processes|
||92.3 id|92.3 percent of the CPU is idle|
||0.0 wa|0.0 percent of the CPU is waiting for I/O|
|4|Mem:|This shows how physical RAM is being used|
|5|Swap:|This shows how swap space(virtual memory) is being used|

In a terminal, pressing `Ctrl-c`, interrupts a program.

The terminal has a `foreground` (with stuff visible on the surface like the shell
prompt) and a `background` (with stuff hidden behind the surface).

To launch a program so that it is immediately placed in the background, we follow
the command with an ampersand (&) character.
```
erkhes@rexes:~$ xlogo &
[1] 57112
```
This message is part of a shell feature called `job control`. In this case,
job number (or `jobspec`) 1 and PID 57112.
```
erkhes@rexes:~$ ps
    PID TTY          TIME CMD
  35550 pts/6    00:00:00 bash
  57112 pts/6    00:00:00 xlogo
  57172 pts/6    00:00:00 ps
```
Use `jobs` command to list all jobs:
```
erkhes@rexes:~$ jobs
[1]+  Running                 xlogo &
```
To return a process to the foreground, use the `fg` command:
```
erkhes@rexes:~$ fg 1 # or fg %1
xlogo
```
Sometimes it is desirable to stop a process without terminating it.
This is often done to allow a foreground process to be moved to the background.
To stop a foreground process and place it in the background, press `Ctrl-z`.
```
erkhes@rexes:~$ xlogo
^Z
[1]+  Stopped                 xlogo
```
We can either continue the program's execution in the foreground, using the `fg`
command, or resume the program's execution in the background with the `bg` command:
```
erkhes@rexes:~$ bg 1
[1]+ xlogo &
erkhes@rexes:~$
```
There are 2 reasons for launching a graphical program from the command line:
- The program might be not visible elsewhere.
- By launching a program from the command line, error messages can be seen.
Also, some graphical programs have interesting and useful command line options.

To kill processes, the `kill` command is used.
```
erkhes@rexes:~$ xlogo &
[1] 58759
erkhes@rexes:~$ kill 58759
erkhes@rexes:~$ 
[1]+  Terminated              xlogo
```
The `kill` command send processes `signals`.
Signals are one of several ways that the OS communicates with programs.
When the terminal receives keystrokes such as `Ctrl-c` or `Ctrl-z`, it sends
a signal to the program in the foreground. `Ctrl-c` sends a signal called `INT`
(interrupt); `Ctrl-z` sends a signal called `TSTP` (terminal stop). The fact that
a program can listen and act upon signal allows a program to do things such as
save work in progress when it is sent a termination signal.

Syntax of `kill`: `kill [-signal] PID...`

If no signal is specified, the `TERM` (terminate) signal is sent.

|Number|Name|Meaning|
|-|-|-|
|1|HUP|Hangup. The signal is used to indicate to programs that the controlling terminal has "hung up." The effect of this signal can be demonstrated by closing a terminal session. This signal is also used by many daemon programs to cause a reinitilization. This means  that when a daemon is sent this signal, it will restart and reread its config file.|
|2|INT|Interrupt. Ctrl-c. It usually terminate a program.|
|9|KILL|Kill. This signal is special. The KILL signal is never actually sent to the target program. Rather, the kernel immediately terminates the process. It is given no opportunity to "clean up" or save its work. Should be used as a last resort.|
|15|TERM|Terminate. Default sent by `kill`.|
|18|CONT|Continue. This will restore a process after a `STOP` or `TSTP` signal. This signal is sent by the `bg` and `fg` commands.|
|19|STOP|Stop. This signal causes a process to pause without terminating. Like KILL, it cannot be ignored.|
|20|TSTP|Terminate stop. This is the signal sent by the terminal when Ctrl-z is pressed.|
```
erkhes@rexes:~$ xlogo &
[1] 73740
erkhes@rexes:~$ kill -1 73740
erkhes@rexes:~$ 
[1]+  Hangup                  xlogo
```
Note that signals may be specified either by number or by name, including the
name prefixed with the letters SIG.
```
erkhes@rexes:~$ xlogo &
[1] 73863
erkhes@rexes:~$ kill -INT 73863
erkhes@rexes:~$ 
[1]+  Interrupt               xlogo
erkhes@rexes:~$ xlogo &
[1] 73878
erkhes@rexes:~$ kill -SIGINT 73878
erkhes@rexes:~$ 
[1]+  Interrupt               xlogo
```

|Number|Name|Meaning|
|-|-|-|
|3|QUIT|Quit.|
|11|SEGV|Segmentation violation. This signal is sent if program makes illegal use of memory|
|28|WINCH|Window change. This is the signal sent by the system when a window changes size.|

Use `killall` command to send signals to multiple processes matching a specified
program or username. The syntax: `killall [-u user] [-signal] name...`
```
erkhes@rexes:~$ xlogo &
[1] 77618
erkhes@rexes:~$ xlogo &
[2] 77631
erkhes@rexes:~$ killall xlogo
erkhes@rexes:~$ 
[1]-  Terminated              xlogo
[2]+  Terminated              xlogo
```
Note that we must have superuser priviliges to send signals to processes
that do not belong to us.

The process of shutting down the system involves the orderly
termination of all processes on the system, as well as performing
some vital housekeeping chores
(such as syncing all of the mounted file systems) before the system
powers off.

There are 4 commands that does this:
- `halt`
- `poweroff`
- `reboot`
- `shutdown`

For monitoring processes:
- `pstree` - process list in tree-like pattern
- `vmstat` - system resource usage including memory, swap, disk I/O
- `xload` - A graphical program that draws a graph showing system load
- `tload` - `xload` but in terminal