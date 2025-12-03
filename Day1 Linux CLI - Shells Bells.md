# Linux CLI - Shells Bells

## Learning Objectives
- Understand the basics of the Linux command-line interface (CLI)
- Explore how the CLI is used for personal workflows and IT administration
- Apply this knowledge to uncover the Christmas mysteries

Linux is a command-line–driven operating system based on Unix. Many modern operating systems are built on top of Linux.  
Its CLI is extremely powerful, enabling system management and automation entirely through typed commands — no GUI required.  
In fact, most experienced IT and cybersecurity professionals work with the CLI daily.

---

## Running Your First Commands

- Run your first command with:  
  `echo "Hello World!"`  
  This prints the text back to you.

- List the contents of the current directory:  
  `ls`  
  This will show McSkidy’s files.

- Display a file’s contents:  
  `cat README.txt`  
  The output appears directly in the terminal.

---

# Navigating the Filesystem

- Move into a directory:  
  `cd Guides`  
  This changes your path to `/home/mcskidy/Guides`.

- View the directory contents again:  
  `ls`  
  (The example directory is empty.)

---

# Looking for the Hidden Guide

Linux hides files and directories whose names start with a dot (e.g., `.secret.txt`).  
This is useful for system configurations — or for attackers hiding malware — and in this case, for McSkidy hiding a secret guide.

- Show *all* files, including hidden ones:  
  `ls -la`  
  `-a` → show hidden items  
  `-l` → show file details (permissions, owner, size, etc.)

- Read the hidden guide:  
  `cat .guide.txt`  
  (Don't forget the leading dot.)

---

# Grepping the Logs

Large log files are hard to read with `cat`, so SOC analysts use **grep** to search for specific text inside files.

- Navigate to the logs directory:  
  `cd /var/log`

- Explore its contents:  
  `ls`

- Search `auth.log` for failed login attempts:  
  `grep "Failed password" auth.log`

This filters the log and shows only matching entries.

---

# Finding Files

The `find` command searches for files that match specific criteria (location, name, type, size, etc.).

- Search for files containing the word "egg" in their names:  
  `find /home/socmas -name "*egg*"`

`find` is incredibly powerful — more details here:  
https://man7.org/linux/man-pages/man1/find.1.html

---

# Analyzing the “Eggstrike” Script

Files ending in `.sh` are **shell scripts**. They contain CLI commands and are used by IT teams for automation — and by attackers to execute malicious actions quickly.

When reviewing a suspicious script:

- Lines beginning with `#` are comments.
- `cat wishlist.txt | sort | uniq`  
  → Reads wishlist.txt, sorts it, and prints unique items.
- The output is redirected to a file:  
  `> /tmp/dump.txt`
- `rm wishlist.txt`  
  → Deletes the original wishlist.
- `mv eastmas.txt wishlist.txt`  
  → Replaces the wishlist with a malicious file.

### Useful Special Symbols

| Symbol | Description | Example |
|--------|-------------|---------|
| '|' (pipe) | Send output from one command to the next | `cat list.txt | sort | uniq` |
| `>` / `>>` | `>` overwrites a file; `>>` appends to a file | `cmd > output.txt` |
| `&&` | Run the next command only if the first succeeds | `grep "secret" notes.txt && echo "Found!"` |

---

# Sir Carrotbane Attacks

The investigation confirms that the server was breached.  
Sir Carrotbane replaced the wishlist by accessing:
http://10.64.135.230:8080/ from VM's web browser.


---

# System Utilities

Linux provides hundreds of CLI commands to inspect and manage your system:

- `uptime` — how long the system has been running  
- `ip addr` — view network interfaces  
- `ps aux` — list all running processes  

System username and password hashes (including McSkidy’s) are located in:
/etc/shadow


⚠️ Access requires **root** privileges.

---

# Root User

The **root** user is the most powerful account in Linux — able to modify any file or setting.

- Switch to the root user:  
  `sudo su`

- Verify the current user:  
  `whoami`

- Return to the previous user:  
  `exit`

Because root has unrestricted access, it is a prime target for attackers.

---

# Bash History

Linux records all terminal commands in a hidden file called Bash history:

- McSkidy’s history: `/home/mcskidy/.bash_history`
- Root’s history: `/root/.bash_history`

You can view command history with:

- `history` — list past commands  
- `cat .bash_history` — read the log file directly  

This is extremely helpful for auditing activity.

---

# Quick Review Questions

**Which CLI command would you use to list a directory?**  
`ls`

**Which command helped you filter the logs for failed logins?**  
`grep`

**Which command would you run to switch to the root user?**  
`sudo su`
