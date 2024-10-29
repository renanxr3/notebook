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

---

[Duck Duck Go](https://duckduckgo.com)  
<https://www.markdownguide.org>  
<fake@example.com>  

![The San Juan Mountains are beautiful!](/assets/images/san-juan-mountains.jpg "San Juan Mountains")

---

##########################
## Getting Help in Linux
##########################
 
# MAN Pages
man command     # => Ex: man ls
 
# The man page is displayed with the less command
# SHORTCUTS:
# h         => getting help
# q         => quit
# enter     => show next line
# space     => show next screen
# /string   => search forward for a string
# ?string   => search backwards for a string
# n / N     => next/previous appearance
 
# checking if a command is shell built-in or executable file
type rm        # => rm is /usr/bin/rm
type cd        # => cd is a shell builtin
 
# getting help for shell built-in commands
help command    # => Ex: help cd
command --help  # => Ex: rm --help
 
# searching for a command, feature or keyword in all man Pages
man -k uname
man -k "copy files"
apropos passwd

---

##########################
## Keyboard Shortcuts
##########################
TAB  # autocompletes the command or the filename if its unique
TAB TAB (press twice)   # displays all commands or filenames that start with those letters
 
# clearing the terminal
CTRL + L
 
# closing the shell (exit)
CTRL + D
 
# cutting (removing) the current line 
CTRL + U
 
# moving the cursor to the start of the line
CTRL + A
 
# moving the cursor to the end of the line
Ctrl + E
 
# stopping the current command
CTRL + C
 
# sleeping a the running program
CTRL + Z
 
# opening a terminal 
CTRL + ALT + T

---

##########################
## Bash History
##########################
 
# showing the history
history
 
# removing a line (ex: 100) from the history
history -d 100
 
# removing the entire history
history -c
 
# printing the no. of commands saved in the history file (~/.bash_history)
echo $HISTFILESIZE
 
# printing the no. of history commands saved in the memory
echo $HISTSIZE
 
# rerunning the last command from the history
!!
 
# running  a specific command from the history (ex: the 20th command)
!20
 
# running the last nth (10th) command from the history
!-10
 
# running the last command starting with abc 
!abc
 
# printing the last command starting with abc 
!abc:p
 
# reverse searching into the history
CTRL + R
 
# recording the date and time of each command in the history
HISTTIMEFORMAT="%d/%m/%y %T"
 
# making it persistent after reboot
echo "HISTTIMEFORMAT=\"%d/%m/%y %T\"" >> ~/.bashrc
# or
echo 'HISTTIMEFORMAT="%d/%m/%y %T"' >> ~/.bashrc

---

##########################
## Running commands as root (sudo, su)
##########################
 
# running a command as root (only users that belong to sudo group [Ubuntu] or wheel [CentOS])
sudo command
 
# becoming root temporarily in the terminal
sudo su      # => enter the user's password
 
# setting the root password
sudo passwd root
 
# changing a user's password
passwd username
 
# becoming root temporarily in the terminal
su     # => enter the root password

---

##########################
## Linux Paths
##########################
 
.       # => the current working directory
..      # => the parent directory
~       # => the user's home directory
 
cd      # => changing the current directory to user's home directory
cd ~    # => changing the current directory to user's home directory
cd -    # => changing the current directory to the last directory
cd /path_to_dir    # => changing the current directory to path_to_dir 
pwd     # => printing the current working directory
 
# installing tree
sudo apt install tree
 
tree directory/     # => Ex: tree .
tree -d .           # => prints only directories
tree -f .           # => prints absolute paths

---

##########################

## The ls Command

## ls [OPTIONS] [FILES]

##########################



# listing the current directory

# ~ => user's home directory

# . => current directory

# .. => parent directory

ls

ls .



# listing more directories

ls ~ /var /



# -l => long listing

ls -l ~



# -a => listing all files and directories including hidden ones

ls -la ~



# -1 => listing on a single column

ls -1 /etc



# -d => displaying information about the directory, not about its contents

ls -ld /etc



# -h => displaying the size in human readable format

ls -h /etc



# -S => displaying sorting by size

ls -Sh /var/log



# Note: ls does not display the size of a directory and all its contents. Use du instead

du -sh ~



# -X => displaying sorting by extension

ls -lX /etc



# --hide => hiding some files

ls --hide=*.log /var/log



# -R => displaying a directory recursively

ls -lR ~



# -i => displaying the inode number

ls -li /etc

