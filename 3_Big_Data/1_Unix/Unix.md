# Data processing at the command line

# Data processing at the command line

## The UNIX operating system
UNIX is a true multiuser operating system. Users have their own private space on the machine’s harddisk and are identified by an _id_ number.
*   All users have a user name and a password and are required to enter them correctly before using the computer.
*   The user with id **0** is the system’s superuser or administrator and its traditional name is **root**.
*   UNIX processes act on behalf of the initiating user.
*   Multiple users can be running multiple processes at the same time.
*   Users are organised in groups. A group is a set of users that share the same class of permissions.


## Everything is a file
Everything in UNIX is represented by files. 

Everything can be:
*   **Configuration files.** Both user profile and system/server configuration files are plain text files. This allows to easily backup/restore/compare configuration files and remote administration using low-bandwidth text consoles.
*   **Devices.** Access is done using regular file I/O operations on filesystem objects (under `/dev`) that represent the real devices. For example `cat file.wav >/dev/sndcard` plays an audio file directly to the soundcard or `cat file.txt|lpr` prints it to a printer.

## The UNIX filesystem

```bash
|--bin          Binaries required before mounting /usr
|--etc          System wide configuration files
|--home         Users’ home directories
|--lib          Libraries required by the system
|--tmp          Temporary files. Everyone has RW access.
|--usr          Programs
| |--bin        Programs’ executables
| |--lib        Programs’ libraries
| |--local      Programs that are install locally
| |   |-bin,lib,share
| |--share Programs’ required files (e.g. docs, icons)
| |--sbin  System administration programs
| |--src   Source files for the kernel and programs
|--var          Temporary space for running programs
```

## Filesystem permissions

Files need to be protected against intentional or unintentional access. Filesystem permissions provide the system with information about who can access a file or directory and what can he do with it.

```
$ ls -la /bin/ls
-rwxr-xr-x 1 root wheel   38624   Jul 15 06:29     /bin/ls
\        /     |      |       |   \          /     |
Permissions  Owner  Owning  Size     Date of     Filename
                    group          last modification
```

|  | read(4) | write(2) | execute(1) |
| :-- | :-- | --- | :-- |
| owner | ×× |  | ×× |
| group | ×× |  | ×× |
| other | ×× |  | ×× |

The same permission set can be expressed with the number `0755`

## Filesystem paths

A path is a sequence of directories to reach a certain file, i.e. `/home/gousiosg/foo.txt`

Paths can be:
*   _Absolute_ - starting from the root directory “/” e.g. `/var/log/messages` - The system log file
*   _Relative_ to the current directory `.` e.g. if the current directory is `/var`, the relative path to the system log is `./log/messages` or `log/messages`

## File listing commands

*   **ls**: list files in a directory
    *   `-l`: list details
    *   `-a`: list hidden files (files that start with .)
    * 
*   **find** `<dir>`: walk through a file hierarchy starting from `<dir>`
    *   `-type [dfl]`: Only display _d_irectories, _f_iles or _l_inks
    *   `-name str`: Only display entries that start with `str`
    *   `-{max|min}depth d`

## File manipulation commands
*   **touch** `<file>`: Create and empty file named `<file>` or update the modification time for the existing file `<file>`
*   **cp** `<from> <to>`: copy file or directory `<from>` to the location specified by `<to>`
    *   `-R`: copy directories recursively
    *   `-p`: preserve filesystem permissions and attributes
*   **mv** `<from_1> · · · <from_n> <to>`: move files or directories `<from*>` to directory `<to>`
    *   `-n`: do not overwrite existing files
*   **mkdir **: create an empty directory in the path specified by the . The path to the directory must be pre-existing.
    *   `-p`: also create intermediate directories as required




## Text file processing

*   **cat** `file_1 · · · file_n`: concatenate and print files to standard output
*   **less** `file`: displays a file on the screen allowing to browse it on both directions.
    *   `q` exit
    *   `/pattern` search for pattern in text, pressing / repeatedly moves through all occurrences.
*   **echo** `<string>` write arguments to standard output
*   **head**, **tail** `<file>`: display first/last lines of `<file>`
    *   `-n` Number of lines to display
    *   `-f` Display newly appended lines

## Process management

*   UNIX can do many jobs at once, dividing the processor’s time between the tasks so quickly that it looks as if everything is running at the same time. This is called multitasking.

*   The UNIX shell has process management capabilities. When running a process, pressing `Ctrl+Z` will suspend it.

*   A process can be killed with `Ctrl+C`

*   A process can be started at the background by appending a & after the command. i.e. `find / |sort &`


## Manipulating running processes

*   **ps** See which processes are running
```
 UID   PID  PPID   C STIME   TTY           TIME CMD
    0     1     0   0 28Nov17 ??        21:20.80 /sbin/launchd
    0    51     1   0 28Nov17 ??         0:41.63 /usr/sbin/syslogd
    0    52     1   0 28Nov17 ??         2:19.32 /usr/libexec/UserEventAgent (System)
```

*   **kill** `-<singalno> <pid>`: Send a signal to a process Important signals:
    *   `TERM`: informs the process that it should terminate.
    *   `KILL`: directly kill a process

## Advanced text flow control
A process in Unix can output to 2 data streams: `STDOUT` or `1` and `STDERR` or `2`. Unix supports 2 times of text flow control:

*   **Redirects:** send a program’s output to a file (`>`) or make a program read from a file (`<`)
    *   `ps -ef > processes`
    *   `>` Overwrites an existing file; to append, we use `>>`

    ```
    $ echo "-What a nice day" > file
    $ echo "-Indeed" >> file
    $ cat file
    -What a nice day
    -Indeed
    ```

*   **Pipes:** Forward a program’s STDOUT line-by-line to the input of another program.


## Pipelines

What makes Unix such a joy to use is that most commands read and write easy to process text. This allows us to combine commands in surprising ways, using the pipe (`|`) operator.

*   `cat file |wc -l`: Count lines of file

*   `find / -type d | sort`: View all directories sorted

*   `cat /var/log/access_log |grep foo|tail -n 10`: See the last 10 accesses from the host “foo” to our system’s web server.

*   `cat /etc/passwd |cut -f1 -d':'|sort`: Get a sorted list of the system’s users.


# The UNIX programming environment
## Running a command per input line

**xargs** `cmd` will run `cmd` on each line in `STDIN`

```
# Get file size statistics for the current directory
$ find . -type f -maxdepth 1 |xargs wc
```

`xargs` by default appends each line at the end of `cmd`. Some times, it may be necessary to append it in the middle. We use the `-I {}` option and

```
$ find . -type f -maxdepth 1|xargs -I {} echo File {} is in `pwd`
File ./labcontents.doc is in /Users/gousiosg/Documents/course-material/isrm
File ./Makefile is in /Users/gousiosg/Documents/course-material/isrm
[...]
```

`xargs` can process things in parallel with `-P` option.

In terms of data processing, `xargs` is the equivalent of `map`

## Filtering lines with patterns

`grep` prints lines matching a pattern

*   `-v`: invert search result (only print those that DO NOT match the pattern)
*   `-i`: make matching case insensitive
*   `-n`: print the line number of the match
*   `-R`: recurse a directory structure

```
# Find all processes run by user 501
$ ps -ef | egrep "^ +501"

# Find all files that extend class Foo
$ grep -Rn "(Foo)" * | grep *.py

# Same, more efficient
$ find . -type f | grep .py$ | xargs grep -n "(Foo)"
```

## Regular expressions in 1 min

- **.** Match any character once
- `*` Match the previous pattern 0 or more times
- **+** Match the previous pattern 1 or more times
- **[e-fF-M]** Match any character in the (ASCII) range `F-M` or `e-f`
- **[^e]** Match all characters _except_ e
- **^** and **$** Match the beginning or the end of the line, respectively
- **()** Group together items for future reference
- **|** Match either the left or the right group

## Simple text processing

`tr` character translator; can convert or delete specific characters

*   `-s`: replace repeating characters into
*   `-d`: delete a character

```
$ echo "foo bar" | tr 'o' 'a'
faa bar
# Replace tabs with spaces
$ tr '\t' ' ' < file.txt
# Remove all instances of #
$ tr -d '#' < file.txt
```

`cut` allows us to split a line into columns, given a character, and extract specific fields.

```
# Get a list of users and home directories
$ cut -f1,6 -d: /etc/passwd
# Get details for all users that are running java
$ ps -ef|tr -s ' '|grep "java"|cut -f3,11  -d' '
```

## Joining data

`join` joins lines of two _sorted_ files on a common field

*   `-1`, `-2` specify fields in files 1 (first argument) and 2 (second argument) that represent keys

```
$ cat foodtypes.txt
3 Fat
1 Protein
2 Carbohydrate

$ cat foods.txt
Potato 2
Cheese 1
Butter 3

join -1 1  -2 2 <(sort foodtypes.txt) <(sort -k 2 foods.txt)
```

Practically, `join` performs a join operation on KV pairs.

## Orchestrating pipelines

`make` is a dependency-based command executor. It reads a rule file that specifies dependencies between files on disk along with production rules for those.

```
target: depend1 depend2 ... dependn
  commands to build the target given the dependencies
```

Make topologically sorts the specified dependency graph and executes commands (in parallel, if `-j` is specified) to generate all output files. If some of those already exist, make skips them.

```
result : file.csv

file.csv : file.txt /usr/bin/tr
  tr ' ' ',' file.txt > file.csv

file.txt :
  curl "http://a/web/page/file.txt" > file.txt
```

# Task-based tools

## Execute a command on a remote host
`ssh` provides a way to securely** login to a remote server and get a prompt. **In addition, it enables us to remotely execute a command and capture its output

```
# List of files on host dutihr
ssh dutihr ls
```

Firewall piercing / tunneling: We can use ssh to access ports on machines where a firewall blocks them

```
# Connect to port
$ ssh -L 27017:mongoserver:27017 mongoserver

# On another terminal
$ mongo localhost:27017
```

## Retrieve contents from URLs

**curl** queries a URL and prints the raw contents on the terminal

*   `-H` Set an HTTP header, e.g. “Authorization: token OAUTH-TOKEN”
*   `-i` Display all headers received
*   `-s` Don’t anything except from the response

```
curl -i "https://api.github.com/repos/vmg/redcarpet/issues"
```

We can then process contents with a pipeline

```
# Get all magnet links from a page
curl -s https://thepiratebay.org/browse/101 | # Get contents
tidy 2>/dev/null |                            # Tidy up HTML
grep magnet\:\? |                             # Only get links
tr -d '"'                                     # Remove quotes
```

## Querying JSON data

**json_pp** pretty-prints JSON files

**jq** uses a Domain Specific Language (DSL) to query tree structures in JSON files.

```
# Extract information for a Cargo package descriptor
curl -s "https://crates.io/api/v1/crates/libc" |
jq -M '[.crate .id, .crate .repository, .crate .downloads|tostring]|join(", ")'

"libc, https://github.com/rust-lang/libc, 7267424"
```

## Syncronizing files across hosts

**rsync** can be used to sync files between directories

*   `-a` archive mode, preserve permissions and access times
*   `-v` display files changed
*   `--delete`


## Run a command when a directory changes

**inotifywait** watches a directory for changes and prints a log of the changes

*   `-m` enables monitor mode (run forever)
*   `-r` watch directories recursively

```
## See changes to your database files
inotifywait -mr --timefmt '%d/%m/%y %H:%M' --format '%T %w %f' /mongo

## Copy all new files in the current directory to another location
inotifywait -mr . |
grep CLOSE_WRITE |
cut -f1 -d' ' |
xargs -I {} cp {} /tmp
```


# Writing programs
## The `bash` language

Bash (Bourne-again shell) is the name of the default command interpreter on most Unix environments. Bash is an almost complete programming language, with an interesting caveat: Many of its operators are programs that can be run individually!

Bash supports most

## Variables

Variables in `bash` are strings followed by `=`, e.g. `cwd="foo"` and are dereferenced with `$`, e.g. `echo $cwd`.

```
# Store the results of running ls in a variable
listing=`ls -la`
echo $listing
```

An interesting set of variables are called _environment_ variables. Those are declared by the operating system and can be read by all programs. The user can modify them with the `export` program.

```
$ export |grep PATH
$ export PATH=$PATH:/home/gousiosg/bin
$ export |grep PATH
```

## Conditionals

Bash supports if / else blocks

```
if [ -e 'test' ]; then
  echo "File exists"
else
  echo "File does not exist"
fi
```

`[` is an alias to the program `test`.

*   `[ $foo = 'test' ]`: Tests string equality
*   `[ $num -eq 3 ]`: Tests number equality
*   `[ ! expression ]`: Negates the expression

## Loops

The `for` loop iterates over all items in the list provided as argument:

```
# Print 1 2 3 4...
for i in `seq 1 10`; do
  echo $i
done

# Iterate over all files in a directory
for i in $(ls); do
  echo `file --mime $i`
done
```

`while` executes a piece of code if the control expression is true

```
ls -fa |tr -s ' '|cut -f9 -d' '|
while read file; do
  echo `file --mime $file`
done
```

## Command line input

`bash` maps special variables on command line inputs: `$0` is the program name, `$1` is the first argument, `$2` the second etc. More complex command lines (e.g. with switches) can be done with `getopt`.

```
#!/usr/bin/env bash

argA="defaultvalue"
while getopts ":a" opt; do
  case $opt in
    a)
      echo "-a was triggered!" >&2
      argA=$OPTARG
      ;;
    \?)
      echo "Invalid option: -$OPTARG" >&2
      ;;
  esac
done
```

The program above also illustrates the use of `case`



