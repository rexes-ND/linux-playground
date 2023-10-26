Each time we type a command and press the `Enter` key,
`bash` performs several substitutions upon the text
before it carries out our command. We call it `expansion`.

The `echo` is a shell builtin that prints its text arguments on `stdout`.

```
erkhes@rexes:~$ echo *
Calibre Library Desktop Documents Downloads lazy_dog.txt ls-output.txt ls.txt Music Pictures Public snap Templates Videos
```

The mechanism by which wildcards work is called `pathname expansion`.

`echo *` doesn't reveal the hidden files.

`echo .*` will include `.` and `..`.

`echo .[!.]*` doesn't include files with multiple `.` at the start.

Use `ls -A` to list hidden files.
```
erkhes@rexes:~$ echo ~ # home directory of current user
The tilde character (`~`) has a special meaning.
/home/erkhes
erkhes@rexes:~$ echo ~erkhes # home directory of named user
/home/erkhes
```

The shell allows arithmetic to be performed by expansion.
Arithmetic expansion uses the following form: `$((expression))`.
```
erkhes@rexes:~$ echo $((2+2))
4
```
- `+`: Addition
- `-`: Subtraction
- `*`: Multiplication
- `/`: Division
- `%`: Modulo
- `**`: Exponentiation

Spaces are not significant in arithmetic expressions
and expression may be nested.
```
erkhes@rexes:~$ echo $(((5**2) * 3))
75
```

With `brace expansion`, we can create multiple text strings
from a pattern containing braces.
```
erkhes@rexes:~$ echo Front-{A,B,C}-Back
Front-A-Back Front-B-Back Front-C-Back
```

Patterns to be brace expanded may contain
a leading portion called a `preamble` and 
a trailing portion called a `postscript`.

The brace expression itself may contain either of the following:
- comma separated list of strings
- a range of integers
- single characters
```
erkhes@rexes:~$ echo Number_{1..5}
Number_1 Number_2 Number_3 Number_4 Number_5
```
Integers may also be zero-padded:
```
erkhes@rexes:~$ echo {01..15}
01 02 03 04 05 06 07 08 09 10 11 12 13 14 15
erkhes@rexes:~$ echo {001..15}
001 002 003 004 005 006 007 008 009 010 011 012 013 014 015
erkhes@rexes:~$ echo {Z..A}
Z Y X W V U T S R Q P O N M L K J I H G F E D C B A
# Brace can be nested
erkhes@rexes:~$ echo a{A{1,2},B{3,4}}b
aA1b aA2b aB3b aB4b
```

Many of `parameter expansion`'s capabilities have to do with
the system's ability to store small chunks of data
and to give each chunk a name.

Many such chunks, called `variable`, are available for our examination. 
For example, the variable named `USER` contains the username.
```
erkhes@rexes:~$ echo $USER
erkhes
```
To list available variables:
```
erkhes@rexes:~$ printenv | less
```
Mistyping a pattern will result in empty string:
```
echo $SUER
```
`Command substitution` allows us to use the output
of a command as an expansion.
```
erkhes@rexes:~$ echo $(ls)
Calibre Library Desktop Documents Downloads ls-output.txt Music Pictures Public snap Templates Videos
erkhes@rexes:~$ ls -l $(which cp)
-rwxr-xr-x 1 root root 141824 Feb  8  2022 /usr/bin/cp
erkhes@rexes:~$ file $(ls -d /usr/bin/* | grep zip)
/usr/bin/bunzip2:      ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=04942293e732cd520714440dfeee0087129ea3ac, for GNU/Linux 3.2.0, stripped
...
```
Without using `quotes`,
```
# Shell remove extra spaces
erkhes@rexes:~$ echo this is a          test
this is a test

# Parameter expansion for $1
erkhes@rexes:~$ echo The total is $100.00
The total is 00.00
```

To suppress unwanted expansions, the shell provides
a mechanism called `quoting` to selectively
suppress unwanted expansions.

If we place text inside `double quotes`, all the special
characters used by the shell lose their special meaning and
are treated as ordinary characters.

The exceptions are `$`, `\`, and `` ` ``.
```
erkhes@rexes:~$ ls 
'Calibre Library'   Documents   ls-output.txt   Pictures   snap       'two words.txt'
 Desktop            Downloads   Music           Public     Templates   Videos
erkhes@rexes:~$ ls -l two words.txt
ls: cannot access 'two': No such file or directory
ls: cannot access 'words.txt': No such file or directory
erkhes@rexes:~$ ls -l "two words.txt"
-rw-rw-r-- 1 erkhes erkhes 0 Oct 26 11:38 'two words.txt'
erkhes@rexes:~$ echo "$USER $((2+2)) $(cal)"
erkhes 4     October 2023      
Su Mo Tu We Th Fr Sa  
 1  2  3  4  5  6  7  
 8  9 10 11 12 13 14  
15 16 17 18 19 20 21  
22 23 24 25 26 27 28  
29 30 31
```
By default, `word-splitting` looks for the presence of spaces,
tabs, and newlines and treats them as `delimiters` between words.
```
erkhes@rexes:~$ echo "echo this is a          test"
echo this is a          test
erkhes@rexes:~$ echo $(cal) # 38 arguments
October 2023 Su Mo Tu We Th Fr Sa 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30 31
erkhes@rexes:~$ echo "$(cal)" # 1 argument
    October 2023      
Su Mo Tu We Th Fr Sa  
 1  2  3  4  5  6  7  
 8  9 10 11 12 13 14  
15 16 17 18 19 20 21  
22 23 24 25 26 27 28  
29 30 31              
 
```
Use `single quote` to disable all expansions.
```
erkhes@rexes:~$ echo text ~/*.txt {a,b} $(echo foo) $((2+2)) $USER
text /home/erkhes/ls-output.txt a b foo 4 erkhes
erkhes@rexes:~$ echo "text ~/*.txt {a,b} $(echo foo) $((2+2)) $USER"
text ~/*.txt {a,b} foo 4 erkhes
erkhes@rexes:~$ echo 'text ~/*.txt {a,b} $(echo foo) $((2+2)) $USER'
text ~/*.txt {a,b} $(echo foo) $((2+2)) $USER
```

Sometimes we want to quote only a single character.
To do this, we can precede a character with a `\`,
which in this context is called the `escape character`.
```
# `\` prevents expansion inside `""`.
erkhes@rexes:~$ echo "The balance for user $USER is: \$5.00"
The balance for user erkhes is: $5.00
```
In addition to its role as the escape character,
the backlash is also used as part of a notation
to represent certain special characters called
`control codes`.
- `\a`: Bell (an alert that causes the computer to beep)
- `\b`: Backspace
- `\n`: Newline
- `\r`: Carriage return
- `\t`: Tab

Adding the `-e` option to `echo` will enable
interpretation of escape sequences.
