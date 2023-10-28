`bash` uses a library (a shared collection of routines
that different programs can use) called `Readline` to
implement command line editing.

Use `vi` mode: `set -o vi`.

`Completion`` occurs when we press the tab key while typing a command.
Completion works for the following:
- Pathname
- Variable (if the beginning is `$`)
- Command
- Username (if the beginning is `~`)
- Hostname (if the beginning is `@`)

Completion commands:
- `Tab-Tab`: Display a list of possible completions.
- `Alt-*`: Insert all possible completions.

`bash` maintains a history of commands that have been entered.
This list of commands is kept in our home directory
in a file called `.bash_history`.

To see the contents of the history: `history | less`.

To find the commands we used to list `/usr/bin`:
`history | grep /usr/bin`.

```
erkhes@rexes:~$ history | grep /usr/bin
...
 1384  ls -l /usr/bin > ls-output.txt
...
```
`1384` is the line number of the command in the history list.
We can use this immediately using another type of expansion
called `history expansion`: `!1384`.

The bash will expand `!1384` into the contents of the
1384th line in the history list.
```
erkhes@rexes:~$ !1384
ls -l /usr/bin > ls-output.txt
```

`bash` also provides the ability to search the history list
incrementally. To start incremental search press `Ctrl-r`.
When we find it, we can either press `Enter` to execute the command
or press `Ctrl-j` to copy the line from the history list to the
current command line. To find the next occurrence of the text,
press `Ctrl-r` again.

- `!!`: Repeat the last command.
- `!number`: Repeat history list item number
- `!string`: Repeat last history list item starting with string
- `!?string`: Repeat last history list item containing string

Use `script` to record an entire shell session
and store it in a file: `script [file]`.
If no file specified, the file `typescript` is used.


