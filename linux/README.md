# Linux

## UDemy - Master Linux Administration

### Apps

|Name|Command|Comments|
|---|---|---|
|Terminator|sudo apt install terminator |Needs GUI, terminal with tabs in a single instance|
|ifconfig|sudo apt install net-tools||
|tree|sudo apt install tree|show folder/file in tree format in terminal|
|du||disk usage will show size of folder  <br> ``` du -sh /etc ``` |
|stat||show statistics of a file <br> ``` stat /etc/passwd ``` <br> atime = access time <br> mtime = modified time <br> ctime = metadata changed time|
|date||show current datetime|
|file||display file datatype and info <br> ``` file /etc/* ``` <br> ``` file /etc/xpto.jpg ```|
|wc|wc -l /etc/group|word count = display counts of line/word/byte count|
|cut|cut -d" " -f10|will cut the line based on a delimiter space and get the 10th field  <br> ``` ifconfig \| grep ether \| cut -d" " -f10 > mac.txt ```|
|tee|tee -a file.txt file2.txt |will output to the terminal and file ``` ifconfig \| grep ether \| tee m.txt ``` <br> -a will append|
|plocate|sudo apt install plocate|Search files based on index created previously <br> sudo updatedb <br> plocate nameoffile|
|find|||
|cmp|||
|sha256sum|sha256sum name_of_file.txt name_of_file2.txt|Will generate Checksum hash for the list of files|
|diff|diff file1 file2|Will compare and show differences of two files|
|patch|patch -w original.txt patched.txt|diff and apply differences to a file|
||||
||||


- Terminator 
  > sudo apt install terminator 
- ifconfig
  > sudo apt install net-tools
- tree
  > sudo apt install tree

### Commands

- ls = list files and folders
- ping
- man = manual / help of a command
  > space or ctrl f = prox pagina  
  > ctrl b = prev page  
  > g = go to begin  
  > G = go to end  
  > / = search
  
  > man -k "term to be searched"
- type = display path of a executable
- clear = ctrl l = clear screen
- history = show history of commands
  > history -c 
- id = tell user logged in
- sudo su
- sudo su -
- sudo -k = clear psw cache
- passwd = change user password
- pwd = current working directory
- grep =
- tty = see terminals
- watching the terminal = ifconfig > /dev/pts/0

### Filesystem

The Filesystem Hierarchy Standard - 1
 ● /bin contains binaries or user executable files which are available to all users.
 ● /sbin contains applications that only the superuser (hence the initial s) will need.
 ● /boot contains files required for starting your system.
 ● /home is where you will find your users’ home directories. Under this directory there is another 
directory for each user, if that particular user has a home directory. 
root has its home directory separated from the rest of the users’ home directories and is /root
 ● /dev contains device files. 
● /etc contains most, if not all system-wide configuration files.
 ● /lib contains shared library files used by different applications. 
● /media is used for external storage will be automatically mounted.
 ● /mnt is like /media but it’s not very often used these days

 The Filesystem Hierarchy Standard - 2
 ● /tmp contains temporary files, usually saved there by applications that are running. 
Non-privileged users may also store files here temporarily.
 ● /proc is a virtual directory. It contains information about your computer hardware, such as 
information about your CPU, RAM memory or Kernel. The files and directories are generated 
when your computer starts, or on the fly, as your system is running and things change.
 ● /sys contains information about devices, drivers, and some kernel features.
 ● /srv  contains data for servers. 
● /run is a temporary file system which runs in RAM.
 ● /usr contains many other subdirectories binaries files, shared libraries and so on. On some 
distributions like CentOS many commands are saved in /usr/bin and /usr/sbin instead of /bin 
and /sbin.
 ● /var typically contains variable-length files such as logs which are files that register events that 
happen on the system.

![Linux](https://lcom.static.linuxfound.org/sites/lcom/files/standard-unix-filesystem-hierarchy.png)

### Piping and Command Redirection

1. STDIN (0)
2. STDOUT (1)
3. STDERR (2)

Example to output error to a file: (no space between 2 and > , so 2>)
``` tail -n 3 /etc/shadow 2> err.txt ```

Example to concatenate the output to the file:
``` tail -n 2 /etc/passwd >> output.txt ```

Example to output both to different files:
``` tail -n 2 /etc/passwd /etc/shadow > output.txt 2> error.txt ```

Example to output both to same file (stderr to stdoutput):
``` tail -n 2 /etc/passwd /etc/shadow > output.txt 2>&1 ```

---

[Duck Duck Go](https://duckduckgo.com)  
<https://www.markdownguide.org>  
<fake@example.com>  


---


## Getting Help in Linux

 
### MAN Pages
man command     # => Ex: man ls
 
### The man page is displayed with the less command
#### SHORTCUTS:
 h         => getting help
 q         => quit
 enter     => show next line
 space     => show next screen
 /string   => search forward for a string
 ?string   => search backwards for a string
 n / N     => next/previous appearance
 
### checking if a command is shell built-in or executable file
type rm        # => rm is /usr/bin/rm
type cd        # => cd is a shell builtin
 
### getting help for shell built-in commands
help command    # => Ex: help cd
command --help  # => Ex: rm --help
 
### searching for a command, feature or keyword in all man Pages
man -k uname
man -k "copy files"
apropos passwd

---


## Keyboard Shortcuts

TAB  # autocompletes the command or the filename if its unique
TAB TAB (press twice)   # displays all commands or filenames that start with those letters
 
### clearing the terminal
CTRL + L
 
### closing the shell (exit)
CTRL + D
 
### cutting (removing) the current line 
CTRL + U
 
### moving the cursor to the start of the line
CTRL + A
 
### moving the cursor to the end of the line
Ctrl + E
 
### stopping the current command
CTRL + C
 
### sleeping a the running program
CTRL + Z
 
### opening a terminal 
CTRL + ALT + T

---


## Bash History

 
### showing the history
history
 
### removing a line (ex: 100) from the history
history -d 100
 
### removing the entire history
history -c
 
### printing the no. of commands saved in the history file (~/.bash_history)
echo $HISTFILESIZE
 
### printing the no. of history commands saved in the memory
echo $HISTSIZE
 
### rerunning the last command from the history
!!
 
### running  a specific command from the history (ex: the 20th command)
!20
 
### running the last nth (10th) command from the history
!-10
 
### running the last command starting with abc 
!abc
 
### printing the last command starting with abc 
!abc:p
 
### reverse searching into the history
CTRL + R
 
### recording the date and time of each command in the history
HISTTIMEFORMAT="%d/%m/%y %T"
 
### making it persistent after reboot
echo "HISTTIMEFORMAT=\"%d/%m/%y %T\"" >> ~/.bashrc
<br> or <br>
echo 'HISTTIMEFORMAT="%d/%m/%y %T"' >> ~/.bashrc

---


## Running commands as root (sudo, su)

 
### running a command as root (only users that belong to sudo group [Ubuntu] or wheel [CentOS])
sudo command
 
### becoming root temporarily in the terminal
sudo su      # => enter the user's password
 
### setting the root password
sudo passwd root
 
### changing a user's password
passwd username
 
### becoming root temporarily in the terminal
su     # => enter the root password

---


## Linux Paths

 
.       # => the current working directory
..      # => the parent directory
~       # => the user's home directory
 
cd      # => changing the current directory to user's home directory
cd ~    # => changing the current directory to user's home directory
cd -    # => changing the current directory to the last directory
cd /path_to_dir    # => changing the current directory to path_to_dir 
pwd     # => printing the current working directory
 
### installing tree
sudo apt install tree
 
tree directory/     # => Ex: tree .
tree -d .           # => prints only directories
tree -f .           # => prints absolute paths

---



## The ls Command

 ls [OPTIONS] [FILES]





### listing the current directory

 ~ => user's home directory

 . => current directory

 .. => parent directory

ls

ls .



### listing more directories

ls ~ /var /



### -l => long listing

ls -l ~



### -a => listing all files and directories including hidden ones

ls -la ~



### -1 => listing on a single column

ls -1 /etc



### -d => displaying information about the directory, not about its contents

ls -ld /etc



### -h => displaying the size in human readable format

ls -h /etc



### -S => displaying sorting by size

ls -Sh /var/log



<b>Note:<b> ls does not display the size of a directory and all its contents. Use du instead

du -sh ~



### -X => displaying sorting by extension

ls -lX /etc



### --hide => hiding some files

ls --hide=*.log /var/log



### -R => displaying a directory recursively

ls -lR ~



### -i => displaying the inode number

ls -li /etc

---


## File Timestamps and Date

 
### displaying atime
ls -lu
 
### displaying mtime
ls -l
ls -lt
 
### displaying ctime
ls -lc
 
### displaying all timestamps
stat file.txt
 
### displaying the full timestamp
ls -l --full-time /etc/
 
### creating an empty file if it does not exist, update the timestamps if the file exists
touch file.txt
 
### changing only the access time to current time
touch -a file
 
### changing only the modification time to current time
touch -m file
 
### changing the modification time to a specific date and time
touch -m -t 201812301530.45 a.txt
 
### changing both atime and mtime to a specific date and time
touch -d "2010-10-31 15:45:30" a.txt
 
### changing the timestamp of a.txt to those of b.txt
touch a.txt -r b.txt
 
### displaying the date and time
date
 
### showing this month's calendar
cal
 
### showing the calendar of a specific year
cal 2021
 
### showing the calendar of a specific month and year
cal 7 2021
 
### showing the calendar of previous, current and next month
cal -3
 
### setting the date and time
date --set="2 OCT 2020 18:00:00"
 
### displaying the modification time and sorting the output by name.
ls -l
 
### displaying the output sorted by modification time, newest files first
ls -lt
 
### displaying and sorting by atime
ls -ltu
 
### reversing the sorting order
ls -ltu --reverse


-----



## Viewing files (cat, less, more, head, tail, watch)

 
### displaying the contents of a file
cat filename
 
### displaying more files
cat filename1 filename2
 
### displaying the line numbers
can -n filename
 
### concatenating 2 files
cat filename1 filename2 > filename3
 
### viewing a file using less
less filename
 
### less shortcuts:
 h         => getting help
 q         => quit
 enter     => show next line
 space     => show next screen
 /string   => search forward for a string
 ?string   => search backwards for a string
 n / N     => next/previous appearance
 
 
### showing the last 10 lines of a file
tail filename
 
### showing the last 15 lines of a file
tail -n 15 filename
 
### showing the last lines of a file starting with line no. 5
tail -n +5 filename
 
### showing the last 10 lines of the file in real-time
tail -f filename
 
 
### showing the first 10 lines of a file
head filename
 
### showing the first 15 lines of a file
head -n 15 filename
 
### running repeatedly a command with refresh of 3 seconds
watch -n 3 ls -l

---


## Working with files and directory (touch, mkdir, cp, mv, rm, shred)

 
### creating a new file or updating the timestamps if the file already exists
touch filename
 
### creating a new directory
mkdir dir1
 
### creating a directory and its parents as well
mkdir -p mydir1/mydir2/mydir3
 

## The cp command

### copying file1 to file2 in the current directory
cp file1 file2
 
### copying file1 to dir1 as another name (file2)
cp file1 dir1/file2
 
### copying a file prompting the user if it overwrites the destination
cp -i file1 file2
 
### preserving the file permissions, group and ownership when copying
cp -p file1 file2
 
### being verbose
cp -v file1 file2
 
### recursively copying dir1 to dir2 in the current directory
cp -r dir1 dir2/
 
### copy more source files and directories to a destination directory
cp -r file1 file2 dir1 dir2 destination_directory/
 
 

## The mv command ###

### renaming file1 to file2
mv file1 file2
 
### moving file1 to dir1 
mv file1 dir1/
 
### moving a file prompting the user if it overwrites the destination file
mv -i file1 dir1/
 
### preventing a existing file from being overwritten
mv -n file1 dir1/
 
### moving only if the source file is newer than the destination file or when the destination file is missing
mv -u file1 dir1/
 
### moving file1 to dir1 as file2
mv file1 dir1/file2
 
### moving more source files and directories to a destination directory
mv file1 file2 dir1/ dir2/ destination_directory/
 

## The rm command 

### removing a file
rm file1
 
### being verbose when removing a file
rm -v file1
 
### removing a directory
rm -r dir1/
 
### removing a directory without prompting
rm -rf dir1/
 
### removing a file and a directory prompting the user for confirmation
rm -ri fil1 dir1/
 
### secure removal of a file (verbose with 100 rounds of overwriting)
shred -vu -n 100 file1

----


## Piping and Command Redirection

 
### Piping Examples:
 
ls -lSh /etc/ | head            # see the first 10 files by size
ps -ef | grep sshd              # checking if sshd is running
ps aux --sort=-%mem | head -n 3  # showing the first 3 process by memory consumption
 
### Command Redirection
 
### output redirection
ps aux > running_processes.txt
who -H > loggedin_users.txt
 
### appending to a file
id >> loggedin_users.txt
 
### output and error redirection
tail -n 10 /var/log/*.log > output.txt 2> errors.txt
 
### redirecting both the output and errors to the same file
tail -n 2 /etc/passwd /etc/shadow > output_errors.txt 2>&1
 
cat -n /var/log/auth.log | grep -ai "authentication failure" | wc -l
cat -n /var/log/auth.log | grep -ai "authentication failure" > auth.txt     # => piping and redirection

-----


## Finding Files (find, plocate)

 
## LOCATE 
locate is a symlink (shortcut) to plocate
 
### updating the plocate db
sudo updatedb
 
### finding file by name
locate filename # => filename is expanded to *filename*
locate -i filename # => the filename is case insensitive
locate -r '/filename$' # => finding by exact name
 
### finding using the basename
locate -b filename
 
### finding using regular expressions
locate -r 'regex'
 
### checking that the file exists
locate -e filename
 
### showing command path
which command
which -a command
 
 
## FIND 
find PATH OPTIONS
 
  Example: find ~ -type f -size +1M # => finding all files in ~ bigger than 1 MB
 
## Options:
- -type f, d, l, s, p
- -name filename
- -iname filename # => case-insensitive
- -size n, +n, -n
- -perm permissions
- -links n, +n, -n
- -atime n, -mtime n, ctime n
- -user owner
- -group group_owner

---

## Commands - VIM

## VIM

 
Modes of operation: Command, Insert, and Last Line Modes.
VIM Config File: ~/.vimrc
 
### Entering the Insert Mode from the Command Mode
i  => insert before the cursor
I  => insert at the beginning of the line
a  => insert after the cursor
A  => insert at the end of the line
o  => insert on the next line
 
### Entering the Last Line Mode from the Command Mode
:
 
### Returning to Command Mode from Insert or Last Line Mode 
ESC
 
### Shortcuts in Last Line Mode
w!  => write/save the file
q!  => quit the file without saving
wq! => save/write and quit
e!  => undo to the last saved version of the file
set nu => set line numbers
set nonu  => unset line numbers
syntax on|off
%s/search_string/replace_string/g
 
### Shortcuts in Command Mode
x   => remove char under the cursor
dd  => cut the current line
5dd => cut 5 lines
ZZ  => save and quit
u   => undo
G   => move to the end of file
$   => move to the end of line
0 or ^  => move to the beginning of file
:n (Ex :10) => move to line n
Shift+v     => select the current line
y           => yank/copy to clipboard
p           => paste after the cursor
P           => paste before the cursor
/string     => search for string forward
?string     => search for string backward
n           => next occurrence
N           => previous occurrence
 
### Opening more files in stacked windows
vim -o file1 file2
 
### Opening more files and highlighting the differences
vim -d file1 file2
Ctrl+w => move between files

---

## Archiving and compressing files 

### TAR

- c = creates
- x = extracts
- t = displays contents

- czvf output.gz input_in_list


---

## Hardlink and Symbolic Links

- Hardlink: both files point to the same file, changing one will change the other (same inode). You cannot hardlink directories or other disks.

  ln origin target

- Symlink (soft links): similar to hardlink but doesn't share the same inode. Symlinks can be broken (moving or deleting the origin file). You can create symlink to directories.

  ln -s origin target

---

## Menu

## Submenu