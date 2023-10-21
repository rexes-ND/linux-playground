Use `ls` to get a list of files and subdirectories contained in the current working directory.

```
# Besides current working directory, we can specify the directory
erkhes@rexes:~$ ls /usr
bin    include  lib32  libexec  local  share
games  lib      lib64  libx32   sbin   srcls /s
```

Home directory is symbolized by the `~` character.

```
erkhes@rexes:~$ ls ~ /usr
/home/erkhes:
'Calibre Library'   Documents   Music      Public   Templates
 Desktop            Downloads   Pictures   snap     Videos

/usr:
bin    include  lib32  libexec  local  share
games  lib      lib64  libx32   sbin   src
```

```
# To reveal more detail
erkhes@rexes:~$ ls -l
total 40
drwxrwxr-x  4 erkhes erkhes 4096 Sep 25 17:15 'Calibre Library'
drwxr-xr-x  6 erkhes erkhes 4096 Oct 11 09:29  Desktop
drwxr-xr-x  2 erkhes erkhes 4096 Sep 21 19:49  Documents
drwxr-xr-x  2 erkhes erkhes 4096 Oct 15 15:06  Downloads
drwxr-xr-x  2 erkhes erkhes 4096 Sep 21 19:49  Music
drwxr-xr-x  3 erkhes erkhes 4096 Sep 25 10:56  Pictures
drwxr-xr-x  2 erkhes erkhes 4096 Sep 21 19:49  Public
drwx------ 11 erkhes erkhes 4096 Sep 25 17:06  snap
drwxr-xr-x  2 erkhes erkhes 4096 Sep 21 19:49  Templates
drwxr-xr-x  3 erkhes erkhes 4096 Oct  3 01:47  Videos
```

By adding `-l` option, we changed the output to the long format.

Commands are often followed
by one or more `options` that modify their behavior, and further,
by one or more `arguments`, the items upon which the command acts.

Most commands follow this format:
`command -options arguments`

Most commands use options which consist of a single character preceded by a `-` (for example `-l`).

Many commands support `long options`, consisting of a word preceded by `--`.

Many commands allow multiple short options to strung together.

Using `ls -it` combines following 2 options:
- `-l` option produces long format output
- `-t` option sort the result by the file's modification time.

If we add the long option `--reverse`,
it will reverse the order of the sort:
`ls -lt --reverse`

Note: command options are case-sensitive.

Common `ls` options:
- `-a` (`--all`): List all files, including hidden files.
- `-A` (`--almost-all`): `-a` without `.` and `..`
- `-d` (`--directory`): List the contents of the directory, not the directory itself.
- `-F` (`--classify`): Append an indicator character to the end of each listed name.
- `-h` (`--human-readable`): Display the file sizes in human readable format
rather than in bytes. For example, `4.0K` instead of `4096`.
- `-l`: Display results in long format
- `-r` (`--reverse`): Display the results in reverse order.
- `-S`: Sort the results by file size. (big to small)
- `-t`: Sort by modification time. (recent to old)

```
erkhes@rexes:~/Desktop/personal/linux-playground$ ls -l
total 8
-rw-rw-r-- 1 erkhes erkhes   18 Oct 19 19:12 README.md
drwxrwxr-x 6 erkhes erkhes 4096 Oct 19 21:28 tlcl
```

`-rw-rw-r--`: Access rights to the file.
`1`: File's number of `hard links`. To be discussed later.
`erkhes`: The username of the file's owner.
`erkhes`: The name of the group that owns the file.
`18`: Size of the files in bytes.
`18 Oct 19 19:12`: Date and time of the file's last modification.
`README.md`: Name of the file.

The filenames in Linux are not required to reflect a file's contents.

Use `file filename` to determine a file's type.

```
erkhes@rexes:~/Desktop/meme$ file cheddar.jpg 
cheddar.jpg: JPEG image data, JFIF standard 1.01, aspect ratio, density 72x72, segment length 16, progressive, precision 8, 1440x1440, components 3
```

One of the common ideas in Unix-like OS such as Linux is that `everything is file`.

The `less` command is a program to view text files.

Text is simple one-to-one mapping of characters to numbers.

Many `configuration files`, which contains system settings, are stored in text format,
and reading them gives us insight about how the system works.

Moreover, `scripts`, which are actual programs that the system uses, are stored in this format.

Use `less filename` and it allows us to scroll forward and backward
through a text file.

To examine the file that defines all the system's user accounts,
`less /etc/passwd`. Use `q` to exit `less`.

- `b`: Scroll back one page
- `space`: Scroll forward one page
- `k`: Scroll up one line
- `j`: Scroll down one line
- `G`: Move to the end of the text file
- `g`: Move to the start of the text file
- `/characters`: Search forward to the next occurrence of `characters`
- `n`: Search for the next occurrence of the previous search
- `h`: Display help screen
- `q`: Quit `less`

The `less` program was designed as a improved replacement of an earlier Unix program called `more`.

- `/`: The root directory, where everything begins.
- `/bin`: Contains binary (or programs) that must be present
for the system to boot and run.
- `/boot`: Contains the Linux kernel,
initial RAM disk image (for drivers needed at boot time),
and the boot loader.
    - `/boot/grub/grub.conf` or `menu.lst`, which are used to configure the boot loader.
    - `/boot/vmlinuz`, the Linux kernel
- `dev`: Special directory that contains `device nodes`.
`Everything is a file` also applies to devices.
The kernel maintains a list of all devices it understands here.
- `/etc`: Contains all of the system-wide configurations files.
It also contains a collection of shell scripts
that start each of the system services at boot time.
Everything in this directory should be readable text.
    - `/etc/crontab`, a file that defines cronjobs.
    - `/etc/fstab`, a table of storage devices and their associated mount points.
    - `/etc/passwd`, a list of the user accounts.
- `/home`: Each user is given a directory in `/home`.
Ordinary users can only write files in their home directories.
- `/lib`: Contains shared library files used by the core system programs.
- `/lost+found`: Each formatted partition or device using a Linux file system,
such as ext4, will have this directory.
It is used in the case of a partial recovery
from a file system corruption event.
- `/media`: Contain the mount points for removable media
such as USB drives, CD-ROMs, etc.
that are mounted automatically at insertion.
- `/mnt`: Contains mount points for removable devices
that have been mounted manually.
- `/opt`: Used to install optional sofware.
- `/proc`: Virtual file system maintained by the Linux kernel.
- `/root`: Home directory for the `root` account.
- `/sbin`: Contains "system" binaries.
- `/tmp`: Intended for the storage of temporary files
created by various programs.
- `/usr`: Most likely to be largest and
contains all the programs and support files used by regular users.
- `/usr/bin`: Contains the executable programs
installed by Linux distribution.
- `/usr/lib`: The shared libraries for the programs in `/usr/bin`.
- `/usr/local`: Programs that are not included with
the distribution but are intended for system-wide use are installed.
Programs compiled from source code are normallly installed in `/usr/local/bin`.
- `/usr/sbin`: Contains more system administration programs.
- `/usr/share`: Contains all the shared data
used by programs in `/usr/bin`.
- `/usr/share/doc`: The documentation files of the installed packages on the system are here.
- `/var`: Contains the data that is likely to changes is stored.
Various databases, spool files, user mail, etc. are located here.
- `/var/log`: Contains `log files`,
records of various system activity. The most useful ones are
`/var/log/messages` and `/var/log/syslog`.

```
erkhes@rexes:/lib$ ls -l
total 712
...
lrwxrwxrwx  1 root root    28 Sep 25 23:45 ld-linux.so.2 -> i386-linux-gnu/ld-linux.so.2
...
```

First letter is  `l`.
This is a special kind of a file called a `symbolic link` (a.k.a `soft link` or `symlink`).

In most Unix-like systems it is possible to have a file
referenced by multiple names.

`Hard link` also allows files to have multiple names,
but they do it in a different way.
