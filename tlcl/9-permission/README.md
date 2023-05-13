# id
Use `id` to find out info about your identity.

```
$ id
uid=1000(erkhes) gid=1000(erkhes) groups=1000(erkhes) ...
```

When user accounts are created, users are assigned a number called a user ID (uid), which is mapped to a username.

The user is assigned **primary** group ID (gid) and can belong to additional groups.

User accounts are defined in `/etc/passwd` and groups are defined in `/etc/group`.

User's password info is held in `/etc/shadow`.

`/etc/passwd` defines the user (login) name, uid, gid, account's real name, home dir, and login shell. For more info, go to https://www.cyberciti.biz/faq/understanding-etcpasswd-file-format/.

```
$ > foo.txt
$ ls -l foo.txt
-rw-rw-r-- 1 erkhes erkhes 0  5월 13 14:33 foo.txt
```

The first 10 characters are the file attributes.

The first one is the file type. `-` is a regular file, `d` is a directory, `l` is a symbolic link and remaining file attrs are always "rwxrwxrwx" (dummy values), `c` is a character special file, and `b` is a block special file.

The remaining 9 characters (called file mode) represents the read, write, and execute permissions for the file's owner, the file's group owner, and everybody else (a.k.a World).

`r` allows file to be opened and read.

`w` allows file to be written to or truncated but doesn't allow it to be deleted or renamed. The ability to delete or rename is determined by directory attrs.

`x` allows file to be treated as a program and executed.

`r` allows directory's content to be listed if the execute attr is also set.

`w` allows files within directory to be created, deleted, and renamed if the execute attr is also set.

`x` allows directory to be entered (`cd`).

# chmod
Use `chmod` to change the mode (permissions) of file or directory.

Only file's owner or the superuser can change the mode of a file or directory.

`chmod` supports 2 ways of specifying mode changes: octal number representation and symbolic representation.

```
$ chmod 600 foo.txt
$ ls -l foo.txt
-rw------- 1 erkhes erkhes 0  5월 13 14:33 foo.txt
```

To specify who is affected, a combination fo the characters `u`, `g`, `o`, and `a`: `u` is short for "user" but means the file or directory owner, `g` is group owner, `o` is short for others (a.k.a World), and `a` is short for all (combination of `u`, `g`, and `o`) and is default when no character is specified.

To specify an operation, `+` is for adding a permission, `-` is for taking a permission away, and `=` is for specifying a permission.

Permissions are specified with the `r`, `w`, and `x` characters.

# umask

Use `umask` to control the default permissions given to a file when it is created.

It uses octal notation to express a mask of bits to be removed from a file's mode attrs.

```
$ umask
0002
```

# su

Use `su` to start a shell as another user.

Use `su -c 'command'` to execute a single command rather than starting a new interactive command.

# sudo

`sudo` is similar to `su` but an administrator can configure `sudo` to allow an ordinary user to execute commands as a different user (most likely, superuser) in a controlled way.

Another important difference is that the use of `sudo` doesn't require access to the superuser's password. Instead, it requires user's own password.

`sudo` doesn't start another shell or load another user's environment.

# chown

`chown` is used to change the owner and group owner of a file or directory.

Syntax is the following: `chown [owner][:[group]] file...`

# chgrp

`chgrp` is old Unix feature when `chown` has some limitations.

# passwd

`passwd` is used to set passwords for yourself (and for others if you have superuser priviliges).

Syntax is the following: `passwd [user]`
