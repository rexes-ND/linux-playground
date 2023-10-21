Unix-like OS such as Linux organizes its files in what is called a `hierarchical directory structure`.
The means tree-like pattern of `directories`, which may contain files and other directories.

The first directory in the filesystem is called the `root directory`.

Windows has a separate file system tree for each storage device.

Unix-like systems such as Linux always have a single file system tree,
regardless of how many drives or storage devices are attached to the computer.

Storage devices are attached (or `mounted`) at various points.

The directory we are standing in is called the `current working directory`.
The directory above us is called the `parent directory`.

```
# To display current working directory
erkhes@rexes:~/Desktop/personal$ pwd # print working directory
/home/erkhes/Desktop/personal
```

When first logged in to the system (or start terminal emulator session)
our current working directory is set to our `home directory`.

```
erkhes@rexes:~$
```

Each user is given its own home directory and
it is the only place a regular user is allowed to write files.

```
# To list the files and directories in the current working directory, use `ls`
erkhes@rexes:~$ ls
'Calibre Library'   Documents   Music      Public   Templates
 Desktop            Downloads   Pictures   snap     Videos
```

```
# To change working directory, use `cd`
erkhes@rexes:~$ cd /usr/bin
erkhes@rexes:/usr/bin$ pwd
/usr/bin
erkhes@rexes:/usr/bin$
```

`cd` is followed by the `pathname` of the desired working directory.
A pathname is the route we take along the branches of the tree
to get to the directory we want.

There are 2 different way to specify pathnames:
1. `absolute pathnames`
2. `relative pathnames`

An absolute pathname begins with the root directory
and follows the tree branch by branch
until the path to the desired directory or file is completed.
For example, there is a directory on our system in which most of our system's programs are installed.
The directory's pathname is `/usr/bin`.

`/` (root) -> `usr` (directory) -> `bin` (directory)

A relative pathname starts from the working directory.
It uses a couple of special notations to represent relative positions in the file system tree: `.` and `..`

From working directory `/usr/bin` to directory `/usr`:
- `cd /usr`
- `cd ..`

In almost all cases, we can omit the `./`.
Instead of `cd ./bin`, use `cd bin`.

Common `cd` shortcuts:
- `cd`: Changes the working directory to your home directory
- `cd -`: Changes the working directory to the previous working directory
- `cd ~user_name`: Changes the working directory to the home directory of
user_name. For example, `cd ~bob` will change the directory to the
home directory of user bob.

Important facts about filenames:
1. Filenames that begin with a `.` are hidden.
`ls` will not list them unless you `ls -a`.
When your account was created,
several hidden files were placed in your home directory
to configure things for your account.
Moreover, some applications place their configs and setting files
in your home directory as hidden files.
2. Filenames and commands in Linux are case sensitive.
(`File1` and `file1` are different)
3. Linux has no concept of a `file extension`.
The contents and/or purpose of a file is determined by other means.
4. It is best if the punctuation characters in the filenames you create
to `.`, `-`, and `_`.
Most importantly, do not embed spaces in filenames.
Use `_` instead of space.
