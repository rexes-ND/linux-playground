The shell provides special characters to help rapidly specifying group of filenames.
These special characters are called `wildcards`.

Wildcards:
- `*`: Matches any characters
- `?`: Matches any single character
- `[characters]`: Matches any character that is a member of the set `characters`
- `[!characters]`: Matches any character that is not a member of the set `characters`
- `[[:class:]]`: Matches any character that is a member of the specified class

Common character classes:
- `[:alnum:]`: Matches any alphanumeric character
- `[:alpha:]`: Matches any alphabetic character
- `[:digit:]`: Matches any numeral
- `[:lower:]`: Matches any lowercase letter
- `[:upper:]`: Matches any uppercase letter

Examples:
- `[[:upper:]]*`: Any file beginning with an uppercase letter
- `[![:digit:]]*`: Any file not beginning with a numeral
- `*[[:lower:]123]`: Any file ending with a lowercase letter or
the numerals 1, 2, 3.

Wildcards can be used with any command that accepts filenames as arguments.

Use character classes instead of character ranges notations like `[A-Z]` and `[a-z]`.

The `mkdir` command is used to create directories:
`mkdir directory...`

Note: `...` means that the argument can be repeated.

The `cp` command copies files and directories:
`cp item1 item2`.

`cp item... directory` copies multiple items into a directory.

- `-a` (`--archive`): Copy the files and directories and all of
their attributes, including ownerships and permissions.
Normally, copies take on the default attributes of the user performing the copy.
- `-i` (`--interactive`): Before overwriting an existing file,
prompt the user for confirmation.
- `-r` (`--recursive`): Recursively copy directories and their contents. This option is required when copying directories.
- `-u` (`--update`): Only copy files that either don't exist
or are newer than the existing corresponding files,
in the destination directory, and skips files that don't need to be copied
- `-v` (`--verbose`): Display informative messages

Example:
- `cp dir1/* dir2`: Copy all the files in dir1 into dir2. The directory dir2 must already exist.
```
erkhes@rexes:~/test$ cp dir1/* dir3
cp: target 'dir3' is not a directory
```
- `cp -r dir1 dir2`: Copy the contents of directory dir1 to directory dir2.
If directory dir2 does not exist, it will be created and contain the same contents as directory dir1.
If directory dir2 does exist, then directory dir1 will be copied into dir2.
```
erkhes@rexes:~/test$ cp -r dir1 dir2
erkhes@rexes:~/test$ ls dir2
dir1
erkhes@rexes:~/test$ cp -r dir1 dir3
erkhes@rexes:~/test$ ls dir1 dir3
dir1:
file3  file4

dir3:
file3  file4
```

The `mv` command performs both file moving and file renaming,
depending on how it is used.

`mv` is used in much the same way as `cp`:
- `mv item1 item2`
- `mv item... directory`

Option:
- `-i` (`--interactive`): Same as `cp`
- `-u` (`--update`): Same as `cp`
- `-v` (`--verbose`): Same as `cp`

Example:
- `mv file1 file2`: Move file1 to file2. If file2 exists,
it is overwritten with the contents of file1.
If file2 does not exist, it is created. In either case,
file1 ceases to exist.
- `mv file1 file2 dir1`: Move file1 and file2 into directory dir1.
The directory dir1 must already exist.
- `mv dir1 dir2`: If directory dir2 does not exist,
create directory dir2 and move the contents of directory dir1
into dir2 and delete directory dir1. If directory dir2 does exist,
move directory dir1 (and its contents) into directory dir2.

```
erkhes@rexes:~/test$ cat file1 file2 file3
file1 # file1 content
file2 # file2 content
file3 # file3 content
erkhes@rexes:~/test$ mv file3 file2
erkhes@rexes:~/test$ cat file2
file3
erkhes@rexes:~/test$ mv file1 file2 dir4
mv: target 'dir4' is not a directory
```

The `rm` command is used to remove (delete) files and directories:
`rm item...`

- `-i` (`--interactive`): Before deleting an existing file,
prompt the use for confirmation.
- `-r` (`--recursive`): Recursively delete directories.
To delete a directory, this option must be specified.
- `-f` (`--force`): Ignore nonexistent files and do not prompt.
Overrides the `--interactive` option.
- `-v` (`--verbose`): Display informative messages
as the deletion is performed.

Tip: Whenever you use wildcards with `rm`,
test the wildcard first with `ls`.

The `ln` command is used to create either hard or symbolic links:
`ln file link`

To create a hard link: `ln file link`.
To create a symbolic link: `ln -s item link`

Hard links are the original Unix way of creating links,
compared to symbolic links, which are more modern.

By default, every file has a single hard link that gives the file its name.
When we create a hard link, an additional directory entry for a file is created.
Hard links have 2 important limitations:
1. A hard link cannot reference a file outside its own file system.
This means a link cannot reference a file
that is not the same disk partition as the link itself.
2. A hard link may not reference a directory.

A hard link is indistinguishable from the file itself.
The file is not deleted as long as there is hard link to it.
Modern practice prefers symbolic links.

Symbolic links were created to overcome the limitations of hard links.
Symbolic link creates a special type of file that contains
a text pointer to the referenced file or directory.
The file pointed by symbolic link can be deleted,
creating `broken` link.

```
erkhes@rexes:~/playground$ mkdir dir1 dir2
erkhes@rexes:~/playground$ cp /etc/passwd .
erkhes@rexes:~/playground$ ls -l
total 12
drwxrwxr-x 2 erkhes erkhes 4096 Oct 21 19:26 dir1
drwxrwxr-x 2 erkhes erkhes 4096 Oct 21 19:26 dir2
-rw-r--r-- 1 erkhes erkhes 2890 Oct 21 19:30 passwd
erkhes@rexes:~/playground$ cp -v /etc/passwd . # Try for fun
'/etc/passwd' -> './passwd'
erkhes@rexes:~/playground$ cp -i /etc/passwd .
cp: overwrite './passwd'? y # No if you want to leave passwd alone
erkhes@rexes:~/playground$ mv passwd fun # rename passwd -> fun
erkhes@rexes:~/playground$ mv fun dir1 # dir1/fun
erkhes@rexes:~/playground$ mv dir1/fun dir2 # dir2/fun
erkhes@rexes:~/playground$ mv dir2/fun . # dir1 dir2 fun
erkhes@rexes:~/playground$ mv fun dir1
erkhes@rexes:~/playground$ mv dir1 dir2
erkhes@rexes:~/playground$ ls -l dir2
total 4
drwxrwxr-x 2 erkhes erkhes 4096 Oct 21 19:41 dir1
erkhes@rexes:~/playground$ ls -l dir2/dir1
total 4
-rw-r--r-- 1 erkhes erkhes 2890 Oct 21 19:37 fun
erkhes@rexes:~/playground$ mv dir2/dir1 .
erkhes@rexes:~/playground$ mv dir1/fun . # dir1 dir2 fun
erkhes@rexes:~/playground$ ln fun fun-hard
erkhes@rexes:~/playground$ ln fun dir1/fun-hard
erkhes@rexes:~/playground$ ln fun dir2/fun-hard
erkhes@rexes:~/playground$ ls -l
total 16
drwxrwxr-x 2 erkhes erkhes 4096 Oct 21 19:43 dir1
drwxrwxr-x 2 erkhes erkhes 4096 Oct 21 19:43 dir2
-rw-r--r-- 4 erkhes erkhes 2890 Oct 21 19:37 fun
-rw-r--r-- 4 erkhes erkhes 2890 Oct 21 19:37 fun-hard
# Notice the 4 in fun and fun-hard, the number of hard links
```

When thinking about hard links, it is helpful to imagine
that files are made up of two parts:
1. The data part containing the file's contents.
2. The name part that holds the file's name.

Creating hard links is same as creating additional name parts
that all refer to the same data part. The system assigns a chain of
disk blocks to what is called an `inode`, which is then associated
with the name part. Each hard link therefore refers to a specific
inode containing the file's contents.


```
erkhes@rexes:~/playground$ ls -li
total 16
10889608 drwxrwxr-x 2 erkhes erkhes 4096 Oct 21 19:43 dir1
10889609 drwxrwxr-x 2 erkhes erkhes 4096 Oct 21 19:43 dir2
10889598 -rw-r--r-- 4 erkhes erkhes 2890 Oct 21 19:37 fun
10889598 -rw-r--r-- 4 erkhes erkhes 2890 Oct 21 19:37 fun-hard
# The first field is inode number.
```

Symbolic links were created to overcome the two disadvantages of hard links:
1. Hard links cannot span physical devices.
2. Hard links cannot reference directories, only files.

Symbolic links are a special type of file that contains
a text pointer to the target file or directory.
Creating symbolic links is similar to creating hard links.
```
erkhes@rexes:~/playground$ ln -s fun fun-sym
erkhes@rexes:~/playground$ ln -s ../fun dir1/fun-sym
erkhes@rexes:~/playground$ ln -s ../fun dir2/fun-sym
```

When we are creating a symbolic link, we are creating a text
description of where the target file is relative to the symbolic link.

```
erkhes@rexes:~/playground$ ls -l dir1
total 4
-rw-r--r-- 4 erkhes erkhes 2890 Oct 21 19:37 fun-hard
lrwxrwxrwx 1 erkhes erkhes    6 Oct 21 20:01 fun-sym -> ../fun
# 6 is the length of `../fun`
```
```
erkhes@rexes:~/playground$ ln -s dir1 dir1-sym
erkhes@rexes:~/playground$ ls -l
total 16
drwxrwxr-x 2 erkhes erkhes 4096 Oct 21 22:28 dir1
lrwxrwxrwx 1 erkhes erkhes    4 Oct 21 22:29 dir1-sym -> dir1
drwxrwxr-x 2 erkhes erkhes 4096 Oct 21 22:31 dir2
-rw-r--r-- 4 erkhes erkhes 2890 Oct 21 19:37 fun
-rw-r--r-- 4 erkhes erkhes 2890 Oct 21 19:37 fun-hard
lrwxrwxrwx 1 erkhes erkhes    3 Oct 21 20:00 fun-sym -> fun
```
```
erkhes@rexes:~/playground$ ls -l
total 12
drwxrwxr-x 2 erkhes erkhes 4096 Oct 21 22:28 dir1
lrwxrwxrwx 1 erkhes erkhes    4 Oct 21 22:29 dir1-sym -> dir1
drwxrwxr-x 2 erkhes erkhes 4096 Oct 21 22:31 dir2
-rw-r--r-- 3 erkhes erkhes 2890 Oct 21 19:37 fun
lrwxrwxrwx 1 erkhes erkhes    3 Oct 21 20:00 fun-sym -> fun
```
```
erkhes@rexes:~/playground$ ls -l
total 8
drwxrwxr-x 2 erkhes erkhes 4096 Oct 21 22:28 dir1
lrwxrwxrwx 1 erkhes erkhes    4 Oct 21 22:29 dir1-sym -> dir1
drwxrwxr-x 2 erkhes erkhes 4096 Oct 21 22:31 dir2
lrwxrwxrwx 1 erkhes erkhes    3 Oct 21 20:00 fun-sym -> fun
erkhes@rexes:~/playground$ less fun-sym
fun-sym: No such file or directory
```
```
erkhes@rexes:~/playground$ rm fun-sym dir1-sym
erkhes@rexes:~/playground$ ls -l
total 8
drwxrwxr-x 2 erkhes erkhes 4096 Oct 21 22:28 dir1
drwxrwxr-x 2 erkhes erkhes 4096 Oct 21 22:31 dir2
```

Most file operations are carried out on symbolic link's target,
not the link itself.
