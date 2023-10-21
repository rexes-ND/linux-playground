A command can be one of 4 different things:
1. <b>An executable program</b>, including compiled binaries written in C/C++
or writtten in scripting languages such as shell and Python.
2. <b>A command built into the shell itself</b>. Example is `cd`.
3. <b>A shell function</b> Shell functions are miniature
shell scripts incorporated into the environment.
4. <b>An alias</b> Aliases are commands that we can defined,
built from other commands.

The `type` command is a shell builtin that displays
the kind of command the shell will execute,
given particular command name: `type command`

```
erkhes@rexes:~$ type type
type is a shell builtin
erkhes@rexes:~$ type ls
ls is aliased to `ls --color=auto'
erkhes@rexes:~$ type cp
cp is /usr/bin/cp
```
The `which` command is used to determine the exact location of
a given executable.
```
erkhes@rexes:~$ which ls
/usr/bin/ls
```
`which` only works for executable programs.

`bash` has a built-in `help` command for each of the shell builtins.
```
erkhes@rexes:~$ help cd
cd: cd [-L|[-P [-e]] [-@]] [dir]
    Change the shell working directory.
...
```
Note: When `[]` appear in the description of the
command syntax, they indicate optional items.
A `|` indicates mutually exclusive items.

Many executable programs support a `--help` option
that displays a description of the commands's
supported syntax and options.
```
erkhes@rexes:~$ mkdir --help
Usage: mkdir [OPTION]... DIRECTORY...
Create the DIRECTORY(ies), if they do not already exist.
...
```

Most executable programs intended for command line use provide
a formal piece of documentation called a `manual` or `man page`.
A special paging program called `man` is used to view them:
`man program`.

Man pages vary somewhat in format
but generally contain the following:
- A title
- A syntax
- A purpose
- A listing and description of each of the command's options

Man pages do not include examples and intended as a reference.

On most Linux systems, `man` use `less` to display the manual page.

`man page` sections:
1. User commands
2. Programming interfaces for kernel system calls
3. Programming interfaces to the C library
4. Special files such as device nodes and drivers
5. File formats
6. Games and amusements such as screen savers
7. Miscellaneous
8. System administration commands

Sometimes we need to refer to a specific section of the manual
to find what we are looking for.
Without specifying a section number, we will always
get the first instance of a match, probably in section 1.

To specify section: `man section search_term`

It is also possible to search the list of man pages
for possible matches based on a search term.

```
erkhes@rexes:~/Desktop/personal/codecrafters-sqlite-cpp$ apropos passwd
chgpasswd (8)        - update group passwords in batch mode
chpasswd (8)         - update passwords in batch mode
fgetpwent_r (3)      - get passwd file entry reentrantly
getpwent_r (3)       - get passwd file entry reentrantly
gpasswd (1)          - administer /etc/group and /etc/gshadow
grub-mkpasswd-pbkdf2 (1) - generate hashed password for GRUB
openssl-passwd (1ssl) - compute password hashes
pam_localuser (8)    - require users to be listed in /etc/passwd
passwd (1)           - change user password
passwd (1ssl)        - OpenSSL application commands
passwd (5)           - the password file
passwd2des (3)       - RFS password encryption
update-passwd (8)    - safely update /etc/passwd, /etc/shadow and /etc/group
```
The first field in each line of output is the name of the man page
and second field shows the section.

`man -k` performs same function as `apropos`.

The `whatis` program displays the name
and a one-line description of
a man page matching a specified keyword:

```
erkhes@rexes:~$ whatis ls
ls (1)               - list directory contents
```

The `GNU Project` provides an alternative to man pages
for their programs, called `info`.

Info manuals are displayed with a reader program named `info`.
Info pages are hyperlinked much like web pages.

The `info` program reads `info files`,
which are tree structured into individual `nodes`,
each containing a single topic.
Info files contain hyperlinks that can move the reader from `node` to `node`.
A hyperlink can be identified by its leading asterisk and
is activated by placing the cursor upon it and pressing `Enter`.

- `?`: Display help command
- `PgUp` or `Backspace`: Display prev page
- `PgDn` or `Space`: Display next page
- `n`: Display the next node
- `p`: Display the prev node
- `u`: Display the parent node
- `Enter`: Follow the hyperlink at the cursor location
- `q`: Quit

Perform multiple commands in a line: `command1; command2; command3...`
```
erkhes@rexes:~$ cd /usr; ls; cd -
bin    include  lib32  libexec  local  share
games  lib      lib64  libx32   sbin   src
/home/erkhes
erkhes@rexes:~$
```

To use `alias`: `alias name='string'`
```
erkhes@rexes:~$ type foo
foo is aliased to `cd /usr; ls; cd -'
erkhes@rexes:~$ alias foo='cd /usr; ls; cd -'
erkhes@rexes:~$ foo
bin    include  lib32  libexec  local  share
games  lib      lib64  libx32   sbin   src
/home/erkhes
erkhes@rexes:~$ type foo
foo is aliased to `cd /usr; ls; cd -'
```
To remove an alias, the `unalias` command is used:
```
erkhes@rexes:~$ unalias foo
erkhes@rexes:~$ type foo
bash: type: foo: not found
```
To list all the aliases defined in the environment:
```
erkhes@rexes:~$ alias
alias alert='notify-send --urgency=low -i "$([ $? = 0 ] && echo terminal || echo error)" "$(history|tail -n1|sed -e '\''s/^\s*[0-9]\+\s*//;s/[;&|]\s*alert$//'\'')"'
alias egrep='egrep --color=auto'
alias erkhes='cd ~/Desktop/personal'
alias fgrep='fgrep --color=auto'
alias grep='grep --color=auto'
alias l='ls -CF'
alias la='ls -A'
alias ll='ls -alF'
alias ls='ls --color=auto'
alias work='cd ~/Desktop/work'alias
```
The aliases defined on the command line vanish when the shell session ends.
