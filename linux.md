# 🛡️ Linux & Bash Cheat Sheet

[← Back to Main Index](./README.md)

A fast-reference guide for everyday Linux administration, file manipulation, user permissions, and network diagnostics.

---

## 📂 File & Directory Operations

| Command | Action |
| :--- | :--- |
| `ls -la` | List all files and directories in long format, including hidden files. |
| `ls -lhS` | List files sorted by size (largest first) with human-readable sizes. |
| `pwd` | Print the current working directory path. |
| `cd -` | Jump back to the previous directory. |
| `mkdir -p <path/to/dir>` | Create a directory and any missing parent directories recursively. |
| `cp -r <source> <dest>` | Copy a directory and its contents recursively. |
| `mv <source> <dest>` | Move or rename files and directories. |
| `rm -rf <path>` | Force-delete a file or directory recursively (**use with caution**). |
| `ln -s <target> <link_name>` | Create a symbolic link to a file or folder. |
| `touch <file>` | Create an empty file, or update the timestamp of an existing one. |
| `tree -L 2` | Show the directory structure two levels deep (may need installing). |

---

## 🗜️ Archives & Compression

| Command | Action |
| :--- | :--- |
| `tar -czvf <archive.tar.gz> <dir>` | Create a gzip-compressed archive of a directory. |
| `tar -xzvf <archive.tar.gz>` | Extract a gzip-compressed archive into the current directory. |
| `tar -xzvf <archive.tar.gz> -C <dir>` | Extract an archive into a specific directory. |
| `tar -tzvf <archive.tar.gz>` | List the contents of an archive without extracting it. |
| `zip -r <archive.zip> <dir>` | Create a zip archive of a directory. |
| `unzip <archive.zip> -d <dir>` | Extract a zip archive into a specific directory. |

---

## 🔍 Searching & Text Processing

| Command | Action |
| :--- | :--- |
| `grep -rn "pattern" <dir>` | Search recursively for a string, showing file names and line numbers. |
| `grep -rni "pattern" <dir>` | Same as above, but case-insensitive. |
| `grep -v "pattern" <file>` | Print only lines that do **not** match the pattern. |
| `find <dir> -name "*.log"` | Find files within a directory matching a specific name pattern. |
| `find <dir> -type f -mtime -1` | Find files modified in the last 24 hours. |
| `find <dir> -type f -size +100M` | Find files larger than 100 MB. |
| `tail -f <file>` | Print the last 10 lines of a file and keep following new output. |
| `tail -n 100 <file>` | Display the last 100 lines of a file. |
| `head -n 20 <file>` | Display the first 20 lines of a file. |
| `less <file>` | Page through a file (`/` to search, `q` to quit). |
| `wc -l <file>` | Count the number of lines in a file. |
| `awk '{print $1}' <file>` | Extract and print the first column of data from a text file. |
| `cut -d',' -f2 <file>` | Extract the second field from a comma-delimited file. |
| `sed -i 's/old/new/g' <file>` | Find and replace a string directly inside a file. |
| `sort <file> \| uniq -c \| sort -rn` | Count duplicate lines and rank them by frequency. |
| `<cmd> \| xargs <cmd2>` | Pass the output of one command as arguments to another. |

<details markdown="1">
<summary>🔍 Text-processing one-liners...</summary>

```bash
# Top 10 most-used shell commands
history | awk '{print $2}' | sort | uniq -c | sort -rn | head -10

# Delete all .log files older than 7 days
find /var/log/myapp -name "*.log" -mtime +7 -delete

# Replace a string across every file in a project
grep -rl "old_name" ./src | xargs sed -i 's/old_name/new_name/g'

# Top 10 IPs hitting a web server
awk '{print $1}' access.log | sort | uniq -c | sort -rn | head -10
```
</details>

---

## 🔒 Permissions & Ownership

| Command | Action |
| :--- | :--- |
| `chmod 755 <file>` | Owner: read/write/execute. Group & others: read/execute. |
| `chmod 644 <file>` | Owner: read/write. Group & others: read-only (typical for files). |
| `chmod 600 <file>` | Owner-only read/write (e.g. SSH private keys). |
| `chmod +x <script.sh>` | Make a script or file executable. |
| `chown <user>:<group> <file>` | Change both the owner and group ownership of a file. |
| `chown -R <user>:<group> <dir>` | Change the owner of a directory and its contents recursively. |
| `sudo -i` | Open a root shell. |
| `whoami` / `id` | Show the current user / its UID, GID and groups. |

> **Octal cheat:** `r=4`, `w=2`, `x=1` → add them per digit (owner, group, others).

---

## ⚡ System Performance & Process Management

| Command | Action |
| :--- | :--- |
| `top` or `htop` | Monitor real-time system resources and active processes. |
| `ps aux \| grep <name>` | Find the process ID (PID) of a specific running application. |
| `pgrep -a <name>` | Find PIDs by process name (cleaner than `ps \| grep`). |
| `kill <PID>` | Ask a process to terminate gracefully (SIGTERM). |
| `kill -9 <PID>` | Force-kill a process that ignores SIGTERM (SIGKILL). |
| `pkill <process_name>` | Kill all running instances of a process by its name. |
| `<cmd> &` / `jobs` / `fg` | Run a command in the background, list jobs, bring one back. |
| `nohup <cmd> &` | Keep a command running after you log out. |
| `df -h` | Display available and used disk space across mounted filesystems. |
| `du -sh * \| sort -h` | Show the size of each file and folder here, smallest to largest. |
| `free -h` | Display total, used, and available RAM on the system. |
| `uptime` | Show how long the system has been up and its load averages. |

---

## ⚙️ Services & Logs (systemd)

| Command | Action |
| :--- | :--- |
| `systemctl status <service>` | Show whether a service is running, plus its latest log lines. |
| `sudo systemctl start\|stop\|restart <service>` | Control a service. |
| `sudo systemctl enable --now <service>` | Start a service and enable it on boot. |
| `journalctl -u <service> -f` | Follow the logs of a specific service. |
| `journalctl -u <service> --since "1 hour ago"` | Show a service's logs from the last hour. |

---

## 🌐 Network Diagnostics

| Command | Action |
| :--- | :--- |
| `curl -I <URL>` | Fetch **only** the HTTP response headers of a URL. |
| `curl -i <URL>` | Fetch a URL, printing response headers followed by the body. |
| `curl -X POST -H "Content-Type: application/json" -d '{"k":"v"}' <URL>` | Send a JSON POST request. |
| `wget <URL>` | Download a file to the current directory. |
| `ping <host>` | Send ICMP echo requests to verify network connectivity. |
| `ss -tulpn` | List listening TCP/UDP ports and the owning processes (modern `netstat -tulpn`). |
| `lsof -i :<port>` | Find out which process or application is using a specific port. |
| `dig <domain>` or `nslookup <domain>` | Look up the DNS records of a domain. |
| `ip a` | Show network interfaces and their IP addresses. |

---

## 🔑 SSH & Remote Transfer

| Command | Action |
| :--- | :--- |
| `ssh <user>@<host>` | Connect to a remote machine. |
| `ssh -i <key> -p <port> <user>@<host>` | Connect with a specific private key and port. |
| `ssh-keygen -t ed25519 -C "<email>"` | Generate a new SSH key pair. |
| `ssh-copy-id <user>@<host>` | Install your public key on a remote host for password-less login. |
| `scp <file> <user>@<host>:<path>` | Copy a file to a remote machine. |
| `rsync -avz <src>/ <user>@<host>:<dest>` | Sync a directory to a remote machine, transferring only changes. |
| `ssh -L <local_port>:localhost:<remote_port> <user>@<host>` | Forward a local port to a port on the remote host. |
