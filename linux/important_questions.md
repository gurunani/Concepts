
---

````markdown
# Linux Interview Questions for DevOps Engineers

## 🧠 Category 1: The Absolute Fundamentals (You MUST Know These)

1. **How do you see the last 10 lines of a file? And how do you follow the file as it updates (like a log file)?**  
   **Answer:**  
   ```bash
   tail -n 10 filename
   tail -f filename
````

The `-f` flag is critical for live log monitoring.

2. **What is the difference between `curl` and `wget`?**
   **Answer:**
   Both retrieve data from URLs.

   * `curl`: More feature-rich, supports more protocols, and better for APIs (outputs to stdout by default).
   * `wget`: Simpler and designed for recursive downloading of websites.

3. **How do you find all files containing a specific string/pattern recursively in a directory?**
   **Answer:**

   ```bash
   grep -r "pattern" /path/to/directory/
   ```

4. **What is the difference between `kill` and `kill -9`?**
   **Answer:**

   * `kill`: Sends a **TERM (15)** signal → graceful shutdown.
   * `kill -9`: Sends a **KILL (9)** signal → forceful, immediate termination.
     Always try normal `kill` first.

5. **How do you check the running processes and their resource usage?**
   **Answer:**
   `top` or `htop` (more user-friendly).
   Use `ps aux` for a snapshot.

6. **What is the purpose of `/etc/passwd`, `/etc/group`, and `/etc/shadow`?**
   **Answer:**

   * `/etc/passwd`: User account information.
   * `/etc/group`: Group information.
   * `/etc/shadow`: Encrypted passwords and aging info (more secure).

7. **How do you change file permissions? What does `chmod 755 filename` do?**
   **Answer:**
   `chmod` changes permissions.

   * Owner: `7` (read + write + execute)
   * Group: `5` (read + execute)
   * Others: `5` (read + execute)

---

## ⚙️ Category 2: Core System Administration & Troubleshooting

8. **A process is using a port (e.g., 8080). How do you find which process it is?**
   **Answer:**

   ```bash
   lsof -i :8080
   netstat -tulpn | grep 8080
   ```

9. **The system is running slow. How would you investigate?**
   **Answer:** Check the **Big Four**:

   * **CPU:** `top`, `vmstat`
   * **Memory:** `free -m`, `top`
   * **Disk I/O:** `iostat`, `iotop`
   * **Network:** `netstat`, `ss`, `iftop`
     Also check `/var/log/` for errors.

10. **What is the difference between a soft link and a hard link?**
    **Answer:**

    * **Soft Link (symlink):** Points to filename; breaks if original deleted.
    * **Hard Link:** Points directly to inode; survives file deletion.
      Cannot cross filesystems.

11. **How do you see disk usage of directories? How to find large files?**
    **Answer:**

    ```bash
    du -sh /path/to/dir/*
    find / -type f -size +100M
    ```

12. **Explain the Linux boot process.**
    **Answer:**
    BIOS/UEFI → Bootloader (GRUB) → Kernel → `systemd` → Runlevels/Targets → Login Prompt.

13. **What is load average? How to interpret `1.5`, `0.5`, `3.0`?**
    **Answer:**
    Load average = average number of active/runnable processes.
    For 1-core CPU → 1.0 = fully utilized.
    For multi-core → divide by cores.

---

## 🌐 Category 3: Networking & Security

14. **How do you check if a remote port is open?**
    **Answer:**

    ```bash
    telnet hostname port
    nc -zv hostname port
    nmap -p port hostname
    ```

15. **What is the difference between `iptables` and `firewalld`?**
    **Answer:**

    * `iptables`: Low-level packet filtering.
    * `firewalld`: Frontend daemon for managing iptables dynamically.

16. **What is SSH? How do you create password-less login?**
    **Answer:**
    Generate SSH key-pair:

    ```bash
    ssh-keygen
    ssh-copy-id user@remote-server
    ```

17. **What is the role of `/etc/hosts`?**
    **Answer:**
    Static mapping of hostnames to IPs. Bypasses DNS.

---

## 💻 Category 4: Shell Scripting & Automation

18. **Explain what `#!/bin/bash` does.**
    **Answer:**
    It tells the system to execute the script using the Bash shell.

19. **What is the difference between `$@` and `$*`?**
    **Answer:**

    * `"$@"`: Treats each argument separately (preferred).
    * `"$*"`: Treats all arguments as a single string.

20. **How do you check the exit status of the last command?**
    **Answer:**

    ```bash
    echo $?
    ```

21. **One-liner to find number of "error" occurrences in `.log` files:**
    **Answer:**

    ```bash
    grep -rch "error" *.log | awk '{sum+=$1} END {print sum}'
    ```

    or

    ```bash
    grep -roh "error" *.log | wc -w
    ```

---

## 🧩 Category 5: Advanced & Scenario-Based Questions

22. **A user can’t create a file in a directory, even with write permission. Why?**
    **Answer:**
    Missing **execute permission** on the parent directory.

23. **What is an `inode`? Why "No space left on device" when space is free?**
    **Answer:**
    Each file has an inode (metadata).
    The error can mean **inodes are full**, not storage.
    Check with:

    ```bash
    df -i
    ```

24. **Explain `strace` and `ltrace`.**
    **Answer:**

    * `strace`: Traces **system calls** and signals.
    * `ltrace`: Traces **library calls**.
      Useful for debugging unknown programs.

25. **How would you update a config file across 100 servers?**
    **Answer:**

    * Use **Ansible**, **Chef**, **Puppet**, or **Salt**.
    * As a fallback, use `pssh` or `ssh` loops with key-based auth.

---

## 💡 Pro Tips

* **Understand the “Why.”** Don’t memorize—know why commands are used.
* **Think aloud.** Walk through troubleshooting steps logically.
* **Be practical.** Mention tools you’ve actually used (`htop`, `journalctl`, etc.).
* **Practice hands-on.** Use a VM, break things, fix them.

---
