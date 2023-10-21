The `shell` is a program that takes keyboard commands and passes them to the operating system to carry out.

Another program called a `terminal emulator` to interact with the shell.

When shell is ready to accept input, it will show the `shell prompt`:
```
# username@machinename followed by current working directory
erkhes@rexes:~$ 
```

Note:
If the last char of the prompt is a `#` rather than `$`, the terminal session has superuser privileges.

It is possible to see `command history` using up-arrow key.

Trick: highlight some text and clicking on the middle mouse will trigger pasting at the cursor location.

```
erkhes@rexes:~$ date
Thu Oct 19 07:50:45 PM KST 2023
```

```
# Ubuntu 22.04
erkhes@rexes:~$ sudo apt install ncal
erkhes@rexes:~$ cal
    October 2023      
Su Mo Tu We Th Fr Sa  
 1  2  3  4  5  6  7  
 8  9 10 11 12 13 14  
15 16 17 18 19 20 21  
22 23 24 25 26 27 28  
29 30 31

```

```
# To see the current amount of free space on our disk drives, use `df`
erkhes@rexes:~$ df
Filesystem     1K-blocks     Used Available Use% Mounted on
tmpfs            3252080     3044   3249036   1% /run
/dev/nvme0n1p2 982862268 50054740 882807196   6% /
tmpfs           16260392   204616  16055776   2% /dev/shm
tmpfs               5120        4      5116   1% /run/lock
/dev/nvme0n1p1    523248    35672    487576   7% /boot/efi
tmpfs            3252076      140   3251936   1% /run/user/1000
```

```
# To display the amount of free memory, use `free`
erkhes@rexes:~$ free
               total        used        free      shared  buff/cache   available
Mem:        32520784     3995980    21884244     1241072     6640560    26817636
Swap:        2097148       20480     2076668
```

```
# To exit a terminal session
$ exit # CTRL + D or Ctrl-d
```

`virtual terminals` or `virtual consoles`: terminal sessions run behind the scenes, which can be accessed by pressing `Ctrl-Alt-F1` through `Ctrl-Alt-F6`.
To switch from one `virtual console` to another, press `Alt-F1` through `Alt-F6`. Return back using `Alt-F7`.


