The one of the coolest feature of the command line is `I/O redirection`.
The `I/O` stands for `input/output` and with this facility we can
redirect the input and output of commands to and from files,
as well as connect multiple commands together into
powerful command `pipelines`.

The output from a command consists of 2 types:
- The program's results, the data the program is designed to produce
- Status and error messages that tell us how the prog is getting along

Keeping with Unix philosophy of 'everything is a file,' programs such as `ls` actually send their results to a special file called
`standard output` (or `stdout`) and their status messages to
another file called `standard error` (or `stderr`).

By default, both `stdout` and `stderr` are linked to the screen
and not saved into a disk file.

Moreover, many programs take input from a facility called
`standard input` (or `stdin`), which is, by default,
attached to the keyboard.

`I/O redirection` allows us to change where output goes
and where input comes from. Normally, output goes to the screen
and input comes from the keyboard.

To redirect `stdout` to another file instead of the screen,
we use the `>` redirection operator followed by the name of the file.

To send the output of the `ls` command
to the file `ls-output.txt` instead of the screen:
```
erkhes@rexes:~$ ls -l /usr/bin > ls-output.txt
erkhes@rexes:~$ ls -l ls-output.txt 
-rw-rw-r-- 1 erkhes erkhes 94902 Oct 23 18:29 ls-output.txt
erkhes@rexes:~$ ls -l /bin/usr > ls-output.txt
ls: cannot access '/bin/usr': No such file or directory
erkhes@rexes:~$ ls -l ls-output.txt 
-rw-rw-r-- 1 erkhes erkhes 0 Oct 23 18:38 ls-output.txt
```
When we redirect output with the `>` redirection operator,
the destination file is always rewritten from the beginning.

If we want to truncate a file (or create a new, empty file),
we can use: `> ls-output.txt`

To append the output instead of overwritting the file,
we use `>>` redirection operator:
```
ls -l /usr/bin >> ls-output.txt
```

Redirecting standard error lacks the ease fo a dedicated redirection
operator. To redirect standard error we must refer to its
`file descriptor`. A program can produce output to any of
several numbered file streams. While we've refered to the first
three of these file streams as `stdin`, `stdout`, and `stderr`, the
shell references them internally as file descriptors 0, 1, and 2.

The shell provides a notation for redirecting files using
the file descriptor number. Since `stderr` is same as 2,
we can redirect `stderr`:
```
erkhes@rexes:~$ ls -l /bin/usr 2> ls-error.txt
erkhes@rexes:~$ ls -l ls-error.txt
-rw-rw-r-- 1 erkhes erkhes 56 Oct 23 19:20 ls-error.txt
```

To capture all of the output of a command to a single file, we
must redirect both `stdout` and `stderr` at the same time.
There are 2 ways to do this:
```
erkhes@rexes:~$ ls -l /bin/usr > ls-output.txt 2>&1
```
There are 2 redirections involved:
1. redirect `stdout` to the file `ls-output.txt`
2. redirect file descriptor 2 (`stderr`) to
file descriptor 1 (`stdout`) using the notation 2>&1.

The order of the redirection is significant.
The redirection of `stderr` must always occur after
redirecting standard output or it doesn't work.
If the order is changed (`2>&1 >ls-output`),
```
erkhes@rexes:~$ ls -l /bin/usr 2>&1 >ls-output.txt
ls: cannot access '/bin/usr': No such file or directory
```
Second way:
```
erkhes@rexes:~$ ls -l /bin/usr &> ls-output.txt
erkhes@rexes:~$ ls -l ls-output.txt
-rw-rw-r-- 1 erkhes erkhes 56 Oct 23 19:38 ls-output.txt
# To append both stdout and stderr
erkhes@rexes:~$ ls -l /bin/usr &>> ls-output.txt
-rw-rw-r-- 1 erkhes erkhes 112 Oct 23 19:39 ls-output.txt
```

To ignore output of command, redirect to `/dev/null`.
This file is a system device often referred to as a `bit bucket`,
which accepts input and does nothing with it.
To suppress error messages from a command:
```
erkhes@rexes:~$ ls -l /bin/usr 2> /dev/null
```

The `cat` command reads one or more files and copies them
to `stdout` like so: `cat [file...]`

Since `cat` can accept more than one file as an argument,
it can also be used to join files together.

If `cat` is not given any arguments, it reads from `stdin` and
since `stdin` is, by default, attached to the keyboard,
it's waiting for us to type something.
```
erkhes@rexes:~$ cat
The quick brown fox jumped over the lazy dog.
The quick brown fox jumped over the lazy dog.
erkhes@rexes:~$ 
# Ctrl-d to trigger `EOF` (or `end of file`) on `stdin`
```
We can use this behavior to create short text files:
```
erkhes@rexes:~$ cat > lazy_dog.txt
The quick brown fox jumped over the lazy dog.
erkhes@rexes:~$ cat < lazy_dog.txt
The quick brown fox jumped over the lazy dog.
```

The capability of commands to read data from `stdin` and
send to `stdout` is utilized by a shell feature called `pipelines`.

Using the pipe operator `|`, the `stdout` of one command can be `piped` into the `stdin` of another:
`command1 | command2`

To display the output of any command using `less`,
```
erkhes@rexes:~$ ls -l /usr/bin | less
```

The redirection operator connects a command with a file,
while the pipeline operator connects the output of one command
with the input of a second command.
- `command1 > file1`
- `command1 | command2`

Pipelines are often used to perform complex operations on data.
It is possible to put several commands together into a pipeline.
The commands used this way are referred to as `filters`.

`sort` command combine two sorted lists into one sorted list.
```
erkhes@rexes:~$ ls /bin /usr/bin | sort | less
```

The `uniq` command is often used with `sort`. It accepts a
sorted list of data from either `stdin` or a single filename
argument and, by default, removes any duplicates from the list.
```
ls /bin /usr/bin | sort | uniq | less
```

The `wc` (word count) command is used to display the
number of lines, words, and bytes contained in files.
```
erkhes@rexes:~$ wc ls-output.txt
 1529 14306 94902 ls-output.txt
# lines, words, bytes
```

`grep` is a powerful program used to find text patterns within
files: `grep pattern [file...]`.
When `grep` encounters a `pattern` in the file,
it prints out the lines containing it.
```
erkhes@rexes:~$ ls /bin /usr/bin | sort | uniq | grep zip
bunzip2
bzip2
bzip2recover
funzip
gpg-zip
gunzip
gzip
preunzip
prezip
prezip-bin
streamzip
unzip
unzipsfx
zip
zipcloak
zipdetails
zipgrep
zipinfo
zipnote
zipsplit
```
Handy options:
- `-i`: Causes `grep` to ignore case when performing the search
- `-v`: which tells `grep` to print only those lines
that do not match the pattern

The `head` command prints the first ten lines of a file,
and the `tail` command prints the last ten lines.
To adjust the number of lines, use `-n` option:
```
erkhes@rexes:~$ head -n 5 ls-output.txt 
total 499916
-rwxr-xr-x 1 root root       51632 Feb  8  2022 [
-rwxr-xr-x 1 root root          96 Aug 25 22:20 2to3-3.8
-rwxr-xr-x 1 root root       35344 Oct 19  2022 aa-enabled
-rwxr-xr-x 1 root root       35344 Oct 19  2022 aa-exec
erkhes@rexes:~$ tail -n 5 ls-output.txt 
-rwxr-xr-x 1 root root      875096 Mar 25  2022 zstd
lrwxrwxrwx 1 root root           4 Sep 21 19:41 zstdcat -> zstd
-rwxr-xr-x 1 root root        3869 Mar 25  2022 zstdgrep
-rwxr-xr-x 1 root root          30 Mar 25  2022 zstdless
lrwxrwxrwx 1 root root           4 Sep 21 19:41 zstdmt -> zstd
```
`tail` has an option which allows us to view files in real time.
This is useful for watching the progress of log files
as they are being written.
```
erkhes@rexes:~$ tail -f /var/log/syslog
```
The `tee` program reads `stdin` and copies it to both `stdout` and to one or more files. This is useful for capturing a pipeline's contents at
and intermediate stage of processing.
```
# Captures the output of `ls /usr/bin` in ls.txt
erkhes@rexes:~$ ls /usr/bin | tee ls.txt | grep zip
```
