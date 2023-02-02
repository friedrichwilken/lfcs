# Summary of O'Reilley
my vm:
severname: `ubuntu`
user: `user`
pw: `pass`

## basic commands
- `whoami` returns the current user
- `ctrl`+`l` cleans screen
- `hostname` returns the name of the host
- `less` makes input text scrollable with `up`/`down-arrow` or `j`/`k`. Press `q` to leave.
	- use `<somecommand> | less` to make the output of `<somecommand>` scrollable
- `date` returns date, time, timezone etc.
- `passwd` change password
- `su` switch user.
	- on CentOS `su -` changes to the **root** user. Note that the prompt will changes from `$` to `#`.
	- on Ubuntu `su -l` (`su --login`) does the same.
		- instead you can just run `sudo <somecommand>` directly.
- `exit` leaves the root shell.
- `touch` creates an empty file.
	- Use this to check if you have the right to write.
- `last` show the last logins.

## man
### how to read
- `[options]` in brackets are optional
- `{a | b}` in curly braces must be on of the given 

### sections
most important sections are
- `1` end-user commands
- `8` root commands (commands that need sudo)
- `5` configuration files

### search for commands
- `man -k <some search string>`  will search for a command. 
	- pipe some `grep` to filter further.
- this is equivalent to `aprops`

### update man
- if there are missing docs in `man`, run `sudo mandb` to update the docs.

### info
for historical reasons some software's docs is stored in `info` instead of `man`. `pinfo` is a modern alternative to it. e. g. :
`pinfo coreutils 'whoami invocation'
**(you are going to be fine to only use `man`)**

# linux file system
## most important parts
- `/usr` (unix system resources) is where you usually will find binaries
	- `/usr/bin` (binaries) for regular user commands
	- `/usr/sbin` (system binaries) for root commands
- `/boot` contains 
	- booting information
	- the linux kernel
- `/dev` (devices) contains the systems **hardware**
- `/etc` contains configuration files
- `/home` contains the user's home directory
- `/opt` is for big applications like databases
- `/proc` (process) is an interface to the linux kernel
- `/root` the home dir for the root user
- `/srv` (service) for services like web servers; usually empty on fresh linux systems
- `/sys` (system) is for hardware information
- `/tmp` (temporary) for temp files; gets cleaned on boot-up
- `/run` is a newer implementation of `/tmp` 
- `/var` various files; most important:
	- `/var/log` contains the log files

# `ls` and file properties
`ls -l` (long) will print out a detailed list of file information
```
  $ ls -l
total 80
-rw-r--r--   1 user  group   4275 Jan  2 23:20 file
lrw-r--r--   1 user  group   1116 Jan  2 23:20 link -> actual/file
drwxr-xr-x   6 user  group    192 Jan  2 23:20 dir
```
First, note that for linux directories are just files.
From right to left you will see:
- the `name` of the file
	- if it is a `link` you will see the `target`
- the creation `date` and `time`
- the `size` of the file
- the `group` that owns this file
- the `user` that owns this file
- the `link counter`
- the `permissions`:
	- left to right:
		- `user owner`
		- `group owner`
		- `others`
- on the very left the `type` of the file:
	- `-` for file
	- `d` for directory
	- `l` for a symbolic link

## useful ls flags
`-l` (long) detailed 
`-lrt` (long reverse time) shows the files with descending order of the last modification time
`-a` shows hidden files
`-ld /mydir` shows only the properties of the given dir

# globbing
"globbing" is the use of wildcards.
`*` wildcard, any character. Can be combined
	- `*.file` all files that end with ".file", for example *my.file* 
	- `file.*` all files that start with "file.", for example *file.txt*
- `?` single character
- `?[a-b]` single range from a to b

#  `cp` (copy)
 `cp` is used like this 
 ```
 cp source destination
``` 
e. g. to copy the content of `/tmp` to where ever you currently are
```
cp /tmp .
```
## important flags
- `-R` recursive copy

# Directories
"Directories" in linux is what in windows is called "Folders"
## `pwd` 
"print working directory"
- `pwd` prints the current directory

## `cd` 
"change directory"
- `cd somedir` to change to a dir called `somedir`
- `cd ..` to got to the parent directory
- `cd -` to go to the last directory

## `rmdir`
"remove directory"
this command is not very useful. use rm instead
- `rm somedir` removes a directory if it is empty

## `rm`
"remove"
- `rm myfile.txt` removes the file `myfile.txt`
- `rm mydir` removes the directory `mydir` if it is empty
- `rm -r mydir` removes the directory `mydir` "recursively" (even if it contains directories and/or files

