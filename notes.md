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

## history
- `hisory` will show you the last commands you used with a number in front.
- `!<number>` will run repeat the command with the given number.

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

## `find`
- `find / -name "host"` lists all files that are exactly named `host` in `/` and its sub directories.
- `find / -user amy` will find all files created my user "amy" in `/` and its sub directories.
- `find / -size +100M` finds all files bigger than 100 mb in `/` and its sub directories.
- `find / -name go -type d` finds all directories in `/` that are named "go".
- `find ~ -name "go.mod" -exec cp {} /tmp/ \;` find all files named "go.mod" and copies them to `/tmp/`. 
- Note that `exec` commands always have the structure `find <find instructions> -exec <command...> {} <...command> \;` where `{}` is a placeholder for all the results of `find`. 

# links
there are *hard* links and *symbolic* (sometimes called *soft*) links.

a file in linux consists of an *inode* (that contains basic data of the file that you will see when you run `ls -l`) and its *blocks* (the actual file data). A name of a file is a *hard link* to the file's inode and a file can have multiple *hard links* and thus names. Further, a *symbolic* link is a name that points to a *hard* link.

hard links come with two limitations:
- they must be on the same device as the inode.
- the can only point to files, not to directories.

## `ln`
"link" creates links.
- `ln myfile.txt myname.txt` creates a *hard link* to `myfile.txt` called `myname.txt`.
- if you write changes to `myfile.txt` or to `myname.txt` you will find the changes in the respective other file as well.
- if you delete `myfile.txt` the underlying inode would still exist and you could access it via `myname.txt` or vice versa.
- `ln -s myfile.txt mysl` would create a *symbolic link* to `myfile.txt` named mysl.
- BETTER: `ln -s /tmp/myfile.txt somesl`; best practice: use absolute names for *symolic links*. If you move `somesl` it will not break the link. 
- `ln -s mydir dirsymln` would create a link to the directory `mydir`.

## tar
"Tape ARchiver"
 
 - `tar -cf myarch.tar /myfile.txt`: `-c` will create a file `-f` lets you specify the name of the new archive file; it will create archive called `myarch.tar` containing the file `myfile.txt`.
 - `tar -xf myarch.tar` will e**x**tract the **f**ile `myarch.tar` to the current directory.
 - `tar -xfC myarch.tar ./somedir` will e**x**tract the **f**ile `myarch.tar` to `somedir`.
 - `tar -tf myarch.tar` will show the content of `myarch.tar`.
 - add `-Z` to use gzip compression.
 - add `-J` to use bzip compression.
 - add `-v` to work verbosely. 

## file
- `file myfile` will print out a files metadata witch will help you for example to know, what compression type an archive uses. 

