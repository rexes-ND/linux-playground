Unix is not only multitasking system but also multi-user system.
```
erkhes@rexes:~$ file /etc/shadow
/etc/shadow: regular file, no read permission
erkhes@rexes:~$ less /etc/shadow
/etc/shadow: Permission denied
```
In the Unix security model, a user may own files and directories.
When a user owns a file or directory,
the user has control over its access.
User can belong to a `group` consisting of one or more users
who are given access to files and directories by their owners.
In addition to granting access to a group, an owner may also
grant some set of access rights to everybody, which in Unix terms
is referred to as the `world`.

To find out information about your identity, use the `id` command:
```
erkhes@rexes:~$ id
uid=1000(erkhes) gid=1000(erkhes) groups=1000(erkhes),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),122(lpadmin),135(lxd),136(sambashare),999(docker)
```
When user accounts are created, users are assigned a number
called a user ID (`uid`) which is then mapped to a username.
The user is assigned a primary group ID (`gid`) and
may belong to additional groups.

User accounts are defined in the `/etc/passwd` file and
groups are defined in the `/etc/group` file.
When user accounts and group are created, these files are modified
along with `/etc/shadow` which holds information about the user's
password.

For each user account, the `/etc/passwd` file defines the
user (login) name, uid, gid, account's real name, home directory,
and login shell.
If we examine the contents of `/etc/passwd` and `/etc/group`,
we notice that besides the regular user accounts,
there are accounts for the superuser (uid 0)
and various other system users.

```
erkhes@rexes:~$ > foo.txt
erkhes@rexes:~$ ls -l foo.txt
-rw-rw-r-- 1 erkhes erkhes 0 Oct 30 08:45 foo.txt
```
Access rights to files and directories are defined in terms of
read access, write access, and execution access.

The first 10 characters of the listing are the
`file attributes`.

The first of these characters is the `file type`.

Common file types:
- `-`: A regular file
- `d`: A directory
- `l`: A symbolic link. Notice that with symbolic links,
the remaining file attributes are always `rwxrwxrwx` (dummy values).
The real file attributes are those of the file the symbolic link
points to.
- `c`: A character special file. This file type refers to a device
that handles data as a stream of bytes, such as a terminal or
`/dev/null`
- `b`: A block special file. This file type refers to a device that
handles data in blocks, such as a hard drive or DVD drive.

The remaining 9 characters of the file attributes,
called the `file mode`, represent the read, write, and execute
permissions of the file's owner, the file's group owner, and
everybody else.

Permission attribute (file):
- `r`: Allows a file to be opened and read.
- `w`: Allows a file to be written to or truncated,
however this attribute does not allow files to be renamed or deleted.
The ability to delete or rename files is determined by directory
attributes.
- `x`: Allows a file to be treated as a program and executed.
Program files written in scripting languages must also be set as
readable to be executed.

Permission attribute (directories):
- `r`: Allows a directory's contents to be listed if the execute
attribute is also set.
- `w`: Allows files within directory to be created, deleted, and
renamed if the execute attribute is also set.
- `x`: Allows a directory to be entered or `cd`.

To change the `file mode` of a file or directory,
the `chmod` command is used.

Note that only the file's owner or the superuser can change
the mode of the file or directory.

`chmod` supports 2 ways of specifying mode changes:
- octal number representation
- symbolic representation

Example (octal):
```
erkhes@rexes:~$ > foo.txt
erkhes@rexes:~$ ls -l foo.txt
-rw-rw-r-- 1 erkhes erkhes 0 Nov  1 17:31 foo.txt
erkhes@rexes:~$ chmod 600 foo.txt
erkhes@rexes:~$ ls -l foo.txt
-rw------- 1 erkhes erkhes 0 Nov  1 17:31 foo.txt
```

Symbolic notation is divided into 3 parts:
- Who the change will affect
- Which operation will be performed
- What permission will be set

Symbolic notation:
- `u`: Short for "user" but means the file or directory owner
- `g`: Group owner
- `o`: Short for "others" but means world
- `a`: Short for "all".

If no character is specified, "all" will be assumed.

The operation may be a `+` indicating that a permission is to be
added, a `-` indicating that a permission is to be taken away,
or a `=` indicating that only the specified permissions are to be
applied and that all others are to be removed.

Symbolic notation examples:
- `u+x`: Add execute permission for the owner.
- `u-x`: Remove execute permission from the owner.
- `+x`: Add execute permission for the owner, group, and world.
This is equivalent to `a+x`.
- `o-rw`: Remove the read and write permissions from anyone besides
the owner and group owner.
- `go=rw`: Set the group owner and anyone besides the owner to
have read and write permission. If either the group owner or the
world previously had execute permission, it is removed.
- `u+x,go=rx`: Add execute permission for the owner and set the
permissions for the group and others to read and execute. Multiple
specifications may be separated by commas.

Symbolic notation does offer the advantage of allowing us to set a
single attribute without disturbing any of the others.

Be extra careful when working with option `--recursive` in `chmod`:
it acts on both files an directories, which is rarely useful.

The `umask` command controls the default permissions given to a file
when it is created.
It uses octal notation to express a mask of bits to be removed
from a file's mode attributes.
```
erkhes@rexes:~$ umask
0002
erkhes@rexes:~$ > foo.txt
erkhes@rexes:~$ ls -l foo.txt
-rw-rw-r-- 1 erkhes erkhes 0 Nov  1 18:27 foo.txt
```
To understand how it works, we have to look at octal numbers.

- Original file mode: `--- rw- rw- rw-`
- Mask: `000 000 000 010` (0002 in octal representation)
- Result: `--- rw- rw- r--`

In additon to read, write, and execute permission, there are
some other less used, permission settings.

The first of these is the `setuid` bit (octal 4000).
When applied to an executable file, it sets the
`effective user ID` from that of the real user
(the user actually running the program) to that of program's owner.
Most often this is given to a few programs owned by the superuser.

The second less-used setting is the `setgid` bit (octal 2000),
which, like the setuid bit, changes the `effective group ID` from the
real group ID of the real user to that of the file owner.
If the setgid bit is set on directory, newly created files in the
directory will be given the group ownership of the directory
rather the group ownership of the file's creator.
This is useful in a shared directory when members of a common group
need access to all the files in the directory, regardless of the file
owner's primary group.

The third is called the sticky bit (octal 1000). On files, this bit
is ignored. On directory, it prevents users from deleting or
renaming files unless the user is either the owner of the directory,
the owner of the file, or the superuser.

- `chmod u+s program`: assigning setuid to a program
- `chmod g+s dir`: assigning setgid to a directory
- `chmod +t dir`: assigning the sticky bit to a directory

At various times, we may find it necessary to take on the identity
of another user.
There are 3 ways to take on an alternative identity:
1. Log out and log back in as the alternative user.
2. Use the `su` command.
3. Use the `sudo` command.

From within our own shell session, the `su` command allows us to
assume the identity of another user and either start a new shell
session with that user's ID, or to issue a single command as that
user.

The `sudo` command allows and administrator to set up a config file
called `/etc/sudoers` and define specific commands that particular
users are permitted to execute under an assumed identity.

The `su` command is used to start a shell as another user.
The command syntax looks like this: `su [-[l]] [user]`.

If the `-l` option is included, the resulting shell session
is a `login shell` for the specified user.
This means the user's environment is loaded and the working directory
is changed to the user's home directory.
If no user is specified, superuser is assumed.

```
erkhes@rexes:~$ su -
Password:

erkhes@rexes:~$ su -l erkhes
Password: 
erkhes@rexes:~$ exit
logout
```

It is possible to execute single command: `su -c 'command'`.
```
erkhes@rexes:~$ su -l erkhes -c "ls"
Password: 
'Calibre Library'   Downloads   Pictures   Templates
 Desktop	    foo.txt     Public	   Videos
 Documents	    Music       snap
```

The administrator can configure `sudo` to allow an ordinary user
to execute commands as a different user (usually the superuser)
in a controlled way. Another important difference is that the use of
`sudo` does not require access to the superuser's password.
To authenticating using `sudo`, requires the user's own password.

The one important difference between `su` and `sudo` is that
`sudo` does not start a new shell, nor does it load another user's
environment.

```
# Interactive shell in `sudo`
erkhes@rexes:~$ sudo -i
root@rexes:~# pwd
/root
```

To see what priviliges are granted by `sudo`, use the `-l` option
to list then:
```
erkhes@rexes:~$ sudo -l
Matching Defaults entries for erkhes on rexes:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin,
    use_pty

User erkhes may run the following commands on rexes:
    (ALL : ALL) ALL
```

Note on `Ubuntu`: By default, Ubuntu disables logins to the root
account (by failing to set a password for the account) and instead
uses sudo to grant superuser priviliges.

The `chown` command is used to change the owner and group owner
of a file and directory. Superuser priviliges are required to user
this command. The syntax of `chown` looks like this:
`chown [owner][:[group]] file...`.

`chown` Argument examples:
- `bob`: Changes the ownership of the file from its current owner
to user `bob`
- `bob:users`: Changes the ownership of the file from its current
owner to user `bob` and changes the file group owner to group `users`
- `:admins`: Changes the group owner to the group `admins`. The
file owner is unchanged.
- `bob:`: Changes the file owner from the current owner to user `bob`
and changes the group owner to the login group of user `bob`.

You can use `chgrp` to change file's group ownership.

To set or change a password, the `passwd` command is used:
`passwd [user]`.

If you have superuser privileges, you can specify a username
as an argument to be `passwd` command to set the password for
another user.
