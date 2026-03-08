# Linux Administration Cheat Sheet

## Table of Contents

| Section | Description |
|---|---|
| [1. Essential Apps & Commands](#1-essential-apps--commands) | Common apps and install commands |
| [2. TMUX Shortcuts](#2-tmux-shortcuts) | TMUX command reference |
| [3. systemctl Commands](#3-systemctl-commands) | Service management commands |
| [4. Common Linux Commands](#4-common-linux-commands) | Frequently used commands |
| [5. Filesystem Hierarchy](#5-filesystem-hierarchy) | Linux directory structure |
| [6. Piping & Redirection](#6-piping--redirection) | Streams and redirection examples |
| [7. Getting Help in Linux](#7-getting-help-in-linux) | Help and manual commands |
| [8. Keyboard Shortcuts](#8-keyboard-shortcuts) | Terminal shortcuts |
| [9. Bash History](#9-bash-history) | History commands |
| [10. Root Access (sudo, su)](#10-root-access-sudo-su) | Root and sudo commands |
| [11. Linux Paths & Navigation](#11-linux-paths--navigation) | Path and navigation commands |
| [12. The ls Command](#12-the-ls-command) | ls command options |
| [13. File Timestamps & Date](#13-file-timestamps--date) | File timestamps and date commands |
| [14. Viewing Files](#14-viewing-files-cat-less-head-tail-watch) | File viewing commands |
| [15. Working with Files & Directories](#15-working-with-files--directories) | File and directory operations |
| [16. Piping & Redirection Examples](#16-piping--redirection-examples) | Piping and redirection examples |
| [17. Finding Files](#17-finding-files-find-plocate) | File search commands |
| [18. VIM Editor](#18-vim-editor) | VIM shortcuts and commands |
| [19. Archiving & Compression](#19-archiving--compression) | Archive and compression commands |
| [20. Hardlink & Symbolic Links](#20-hardlink--symbolic-links) | Link commands |

## 1. Essential Apps & Commands

| Name         | Command                        | Description/Comments |
|--------------|-------------------------------|----------------------|
| Terminator   | sudo apt install terminator   | Terminal with tabs (GUI required) |
| ifconfig     | sudo apt install net-tools    | Network interface info |
| tree         | sudo apt install tree         | Show folder/file tree in terminal |
| du           | du -sh /etc                   | Disk usage of folder |
| stat         | stat /etc/passwd              | File statistics (atime, mtime, ctime) |
| date         | date                         | Show current datetime |
| file         | file /etc/*                   | Display file type info |
| wc           | wc -l /etc/group              | Line/word/byte count |
| cut          | cut -d" " -f10                | Cut by delimiter, get 10th field |
| tee          | tee -a file.txt file2.txt     | Output to terminal and file (append with -a) |
| plocate      | sudo apt install plocate      | Search files by index (sudo updatedb first) |
| sha256sum    | sha256sum file.txt            | Generate checksum hash |
| diff         | diff file1 file2              | Compare files |
| patch        | patch -w original.txt patched.txt | Apply differences to file |

## 2. TMUX Shortcuts

| Command | Description |
|---------|-------------|
| tmux | Start a new tmux session |
| tmux new -s [name] | Start a new session with name |
| tmux attach -t [name] | Attach to session |
| tmux ls | List sessions |
| Ctrl+b , | Rename session |
| Ctrl+b d | Detach session |
| Ctrl+b c | New window |
| Ctrl+b w | List windows |
| Ctrl+b n | Next window |
| Ctrl+b p | Previous window |
| Ctrl+b & | Kill window |
| Ctrl+b % | Split vertically |
| Ctrl+b " | Split horizontally |
| Ctrl+b arrow | Navigate panes |
| Ctrl+b x | Kill pane |
| tmux kill-session -t [name] | Kill session |
| tmux list-sessions | List all sessions |

## 3. systemctl Commands

| Command | Description |
|---------|-------------|
| systemctl | List subcommands and syntax |
| systemctl start [service] | Start service |
| systemctl stop [service] | Stop service |
| systemctl restart [service] | Restart service |
| systemctl reload [service] | Reload config |
| systemctl status [service] | Show status |
| systemctl enable [service] | Enable on boot |
| systemctl disable [service] | Disable on boot |
| systemctl is-enabled [service] | Check enabled |
| systemctl is-active [service] | Check active |
| systemctl list-units | List active units |
| systemctl list-units --all | List all units |
| systemctl list-unit-files | List unit files |
| systemctl daemon-reload | Reload manager config |
| systemctl mask [service] | Mask service |
| systemctl unmask [service] | Unmask service |
| systemctl show [service] | Show properties |

### Creating a Custom Service

1. `sudo nano /etc/systemd/system/my_service.service`
2. Edit file:
   ```ini
   [Unit]
   Description=My Custom Service
   After=network.target

   [Service]
   ExecStart=/path/to/your/script.sh
   Restart=always
   User=your_username
   Group=your_groupname

   [Install]
   WantedBy=multi-user.target
   ```
3. Reload config: `sudo systemctl daemon-reload`
4. Start: `sudo systemctl start my_service`
5. Enable: `sudo systemctl enable my_service`
6. Status: `sudo systemctl status my_service`

## 4. Common Linux Commands

| Command | Description |
|---------|-------------|
| ls | List files and folders |
| ping | Test network connectivity |
| man | Manual/help for command |
| type | Display path of executable |
| clear | Clear screen (Ctrl+L) |
| history | Show command history |
| id | Show user info |
| sudo su | Become root temporarily |
| sudo su - | Become root (login shell) |
| sudo -k | Clear sudo password cache |
| passwd | Change user password |
| pwd | Print working directory |
| grep | Search text |
| tty | Show terminals |
| ifconfig > /dev/pts/0 | Watch terminal |

## 5. Filesystem Hierarchy

| Directory | Description |
|-----------|-------------|
| /bin | User binaries |
| /sbin | Superuser binaries |
| /boot | Boot files |
| /home | User home directories |
| /root | Root's home directory |
| /dev | Device files |
| /etc | System-wide config files |
| /lib | Shared libraries |
| /media | External storage mount |
| /mnt | Temporary mount |
| /tmp | Temporary files |
| /proc | Hardware info (virtual) |
| /sys | Device/kernel info |
| /srv | Server data |
| /run | RAM temp files |
| /usr | User binaries/libs |
| /var | Variable files/logs |

![Linux Filesystem](https://lcom.static.linuxfound.org/sites/lcom/files/standard-unix-filesystem-hierarchy.png)

## 6. Piping & Redirection

| Stream | Number |
|--------|--------|
| STDIN  | 0 |
| STDOUT | 1 |
| STDERR | 2 |

| Example | Description |
|---------|-------------|
| `tail -n 3 /etc/shadow 2> err.txt` | Output error to file |
| `tail -n 2 /etc/passwd >> output.txt` | Append output |
| `tail -n 2 /etc/passwd /etc/shadow > output.txt 2> error.txt` | Output and error to different files |
| `tail -n 2 /etc/passwd /etc/shadow > output.txt 2>&1` | Output and error to same file |

## 7. Getting Help in Linux

| Command | Description |
|---------|-------------|
| man command | Show manual page |
| type rm | Check if command is built-in or executable |
| help command | Help for shell built-in |
| command --help | Help for executable |
| man -k uname | Search man pages |
| apropos passwd | Search man pages |

**MAN Page Shortcuts:**

| Shortcut | Action |
|----------|-------|
| h | Help |
| q | Quit |
| enter | Next line |
| space | Next screen |
| /string | Search forward |
| ?string | Search backward |
| n/N | Next/previous occurrence |

## 8. Keyboard Shortcuts

| Shortcut | Action |
|----------|-------|
| TAB | Autocomplete command/filename |
| TAB TAB | List all matches |
| Ctrl+L | Clear terminal |
| Ctrl+D | Exit shell |
| Ctrl+U | Cut current line |
| Ctrl+A | Move to start of line |
| Ctrl+E | Move to end of line |
| Ctrl+C | Stop command |
| Ctrl+Z | Sleep running program |
| Ctrl+Alt+T | Open terminal |

## 9. Bash History

| Command | Description |
|---------|-------------|
| history | Show history |
| history -d 100 | Delete line 100 |
| history -c | Clear history |
| echo $HISTFILESIZE | Commands in history file |
| echo $HISTSIZE | Commands in memory |
| !! | Rerun last command |
| !20 | Run 20th command |
| !-10 | Run last 10th command |
| !abc | Run last command starting with abc |
| !abc:p | Print last command starting with abc |
| Ctrl+R | Reverse search |
| HISTTIMEFORMAT="%d/%m/%y %T" | Record date/time |

Make persistent:
```bash
echo 'HISTTIMEFORMAT="%d/%m/%y %T"' >> ~/.bashrc
```

## 10. Root Access (sudo, su)

| Command | Description |
|---------|-------------|
| sudo command | Run as root |
| sudo su | Become root (user password) |
| sudo passwd root | Set root password |
| passwd username | Change user password |
| su | Become root (root password) |

## 11. Linux Paths & Navigation

| Symbol | Meaning |
|--------|--------|
| . | Current directory |
| .. | Parent directory |
| ~ | User's home directory |

| Command | Description |
|---------|-------------|
| cd | Go to home directory |
| cd ~ | Go to home directory |
| cd - | Go to last directory |
| cd /path_to_dir | Go to specific directory |
| pwd | Print working directory |

Install tree:
`sudo apt install tree`

| Command | Description |
|---------|-------------|
| tree directory/ | Show tree |
| tree -d . | Show directories only |
| tree -f . | Show absolute paths |

## 12. The ls Command

| Option | Description |
|--------|-------------|
| ls | List current directory |
| ls . | List current directory |
| ls ~ /var / | List multiple directories |
| ls -l ~ | Long listing |
| ls -la ~ | List all (including hidden) |
| ls -1 /etc | Single column |
| ls -ld /etc | Info about directory |
| ls -h /etc | Human readable size |
| ls -Sh /var/log | Sort by size |
| ls -lX /etc | Sort by extension |
| ls --hide=*.log /var/log | Hide files |
| ls -lR ~ | Recursive listing |
| ls -li /etc | Show inode number |

**Note:** ls does not show directory size. Use `du -sh ~`.

## 13. File Timestamps & Date

| Command | Description |
|---------|-------------|
| ls -lu | Show atime |
| ls -l | Show mtime |
| ls -lt | Sort by mtime |
| ls -lc | Show ctime |
| stat file.txt | All timestamps |
| ls -l --full-time /etc/ | Full timestamp |
| touch file.txt | Create/update file |
| touch -a file | Update access time |
| touch -m file | Update modification time |
| touch -m -t 201812301530.45 a.txt | Set mtime |
| touch -d "2010-10-31 15:45:30" a.txt | Set atime/mtime |
| touch a.txt -r b.txt | Set timestamp from another file |
| date | Show date/time |
| cal | Show calendar |
| cal 2021 | Calendar for year |
| cal 7 2021 | Calendar for month/year |
| cal -3 | Previous/current/next month |
| date --set="2 OCT 2020 18:00:00" | Set date/time |

## 14. Viewing Files (cat, less, head, tail, watch)

| Command | Description |
|---------|-------------|
| cat filename | Show file contents |
| cat filename1 filename2 | Show multiple files |
| cat -n filename | Show line numbers |
| cat filename1 filename2 > filename3 | Concatenate files |
| less filename | View file with less |
| tail filename | Last 10 lines |
| tail -n 15 filename | Last 15 lines |
| tail -n +5 filename | From line 5 |
| tail -f filename | Real-time tail |
| head filename | First 10 lines |
| head -n 15 filename | First 15 lines |
| watch -n 3 ls -l | Repeat command every 3s |

**less shortcuts:**

| Shortcut | Action |
|----------|-------|
| h | Help |
| q | Quit |
| enter | Next line |
| space | Next screen |
| /string | Search forward |
| ?string | Search backward |
| n/N | Next/previous occurrence |

## 15. Working with Files & Directories

| Command | Description |
|---------|-------------|
| touch filename | Create/update file |
| mkdir dir1 | Create directory |
| mkdir -p mydir1/mydir2/mydir3 | Create nested directories |

### cp Command

| Command | Description |
|---------|-------------|
| cp file1 file2 | Copy file |
| cp file1 dir1/file2 | Copy as another name |
| cp -i file1 file2 | Prompt before overwrite |
| cp -p file1 file2 | Preserve permissions |
| cp -v file1 file2 | Verbose copy |
| cp -r dir1 dir2/ | Recursive copy |
| cp -r file1 file2 dir1 dir2 destination_directory/ | Copy multiple |

### mv Command

| Command | Description |
|---------|-------------|
| mv file1 file2 | Rename file |
| mv file1 dir1/ | Move file |
| mv -i file1 dir1/ | Prompt before overwrite |
| mv -n file1 dir1/ | Prevent overwrite |
| mv -u file1 dir1/ | Move if newer |
| mv file1 dir1/file2 | Move as another name |
| mv file1 file2 dir1/ dir2/ destination_directory/ | Move multiple |

### rm Command

| Command | Description |
|---------|-------------|
| rm file1 | Remove file |
| rm -v file1 | Verbose remove |
| rm -r dir1/ | Remove directory |
| rm -rf dir1/ | Remove directory (no prompt) |
| rm -ri file1 dir1/ | Prompt before remove |
| shred -vu -n 100 file1 | Secure remove |

## 16. Piping & Redirection Examples

| Command | Description |
|---------|-------------|
| ls -lSh /etc/ | head | First 10 files by size |
| ps -ef | grep sshd | Check if sshd running |
| ps aux --sort=-%mem | head -n 3 | Top 3 by memory |
| ps aux > running_processes.txt | Output redirection |
| who -H > loggedin_users.txt | Output redirection |
| id >> loggedin_users.txt | Append to file |
| tail -n 10 /var/log/*.log > output.txt 2> errors.txt | Output and error |
| tail -n 2 /etc/passwd /etc/shadow > output_errors.txt 2>&1 | Output and error to same file |
| cat -n /var/log/auth.log | grep -ai "authentication failure" | wc -l | Count failures |
| cat -n /var/log/auth.log | grep -ai "authentication failure" > auth.txt | Save failures |

## 17. Finding Files (find, plocate)

| Command | Description |
|---------|-------------|
| locate filename | Find by name (wildcard) |
| locate -i filename | Case-insensitive |
| locate -r '/filename$' | Exact name |
| locate -b filename | Basename |
| locate -r 'regex' | Regex search |
| locate -e filename | Check existence |
| which command | Show command path |
| which -a command | Show all paths |
| sudo updatedb | Update plocate db |

### find Command

| Option | Description |
|--------|-------------|
| -type f, d, l, s, p | File, dir, link, etc. |
| -name filename | Name match |
| -iname filename | Case-insensitive name |
| -size n, +n, -n | Size |
| -perm permissions | Permissions |
| -links n, +n, -n | Links |
| -atime n, -mtime n, ctime n | Time |
| -user owner | Owner |
| -group group_owner | Group |

Example:
`find ~ -type f -size +1M` (files > 1MB)

## 18. VIM Editor

| Mode | Description |
|------|-------------|
| Command | Default mode |
| Insert | Insert text |
| Last Line | Command line |

VIM config: `~/.vimrc`

| Shortcut | Action |
|----------|-------|
| i | Insert before cursor |
| I | Insert at line start |
| a | Insert after cursor |
| A | Insert at line end |
| o | Insert on next line |
| : | Enter Last Line mode |
| ESC | Return to Command mode |
| w! | Save file |
| q! | Quit without saving |
| wq! | Save and quit |
| e! | Undo to last saved |
| set nu | Show line numbers |
| set nonu | Hide line numbers |
| syntax on/off | Toggle syntax |
| %s/search/replace/g | Replace |
| x | Remove char under cursor |
| dd | Cut current line |
| 5dd | Cut 5 lines |
| ZZ | Save and quit |
| u | Undo |
| G | End of file |
| $ | End of line |
| 0 or ^ | Start of line |
| :n | Go to line n |
| Shift+v | Select line |
| y | Yank/copy |
| p | Paste after cursor |
| P | Paste before cursor |
| /string | Search forward |
| ?string | Search backward |
| n/N | Next/previous occurrence |

| Command | Description |
|---------|-------------|
| vim -o file1 file2 | Open files in stacked windows |
| vim -d file1 file2 | Diff files |
| Ctrl+w | Move between files |

## 19. Archiving & Compression

| Option | Description |
|--------|-------------|
| c | Create |
| x | Extract |
| t | List contents |
| czvf output.gz input | Create compressed archive |

## 20. Hardlink & Symbolic Links

| Type | Description |
|------|-------------|
| Hardlink | Both files point to same inode; changes affect both; cannot hardlink dirs/disks |
| Symlink | Points to original file; can be broken; can link dirs |

| Command | Description |
|---------|-------------|
| ln origin target | Create hardlink |
| ln -s origin target | Create symlink |

