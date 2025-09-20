Got it! You want a **Markdown file of real-life shell scripting interview questions** that are **practical and commonly asked in real scenarios**, not just theory. Here’s a ready-to-use `.md` version you can directly push to GitHub:

---

````markdown
# Real-Life Shell Scripting Interview Questions and Answers

---

## **1. How do you check if a process is running and restart it if it’s not?**
```bash
if ! pgrep nginx > /dev/null; then
    systemctl start nginx
    echo "Nginx restarted"
fi
````

---

## **2. How to find large files taking up space?**

```bash
find / -type f -size +100M -exec ls -lh {} \;
```

---

## **3. How do you monitor disk usage and alert if >80%?**

```bash
usage=$(df / | awk 'NR==2 {print $5}' | sed 's/%//')
if [ $usage -gt 80 ]; then
    echo "Disk usage is above 80%"
fi
```

---

## **4. How do you take a backup of a directory with timestamp?**

```bash
tar -czf /backup/dir_$(date +%F).tar.gz /var/www/html
```

---

## **5. How do you check last login of users?**

```bash
last | head -n 5
```

---

## **6. How to find the top 5 memory-consuming processes?**

```bash
ps aux --sort=-%mem | head -n 6
```

---

## **7. How to search for a specific error in log files?**

```bash
grep -i "error" /var/log/nginx/error.log
```

---

## **8. How to monitor CPU usage and alert if >90%?**

```bash
cpu=$(top -bn1 | grep "Cpu(s)" | awk '{print $2+$4}')
if (( $(echo "$cpu > 90" | bc -l) )); then
    echo "CPU usage above 90%"
fi
```

---

## **9. How to check if a service is enabled and running?**

```bash
systemctl is-enabled nginx
systemctl status nginx
```

---

## **10. How to delete files older than 30 days in a directory?**

```bash
find /var/log/nginx/ -type f -mtime +30 -exec rm -f {} \;
```

---

## **11. How to compare two files line by line?**

```bash
diff file1.txt file2.txt
```

---

## **12. How to count number of lines, words, and characters in a file?**

```bash
wc file.txt
```

---

## **13. How to check if a port is in use?**

```bash
netstat -tuln | grep 80
```

---

## **14. How to send email from shell script (Linux)?**

```bash
echo "Backup completed" | mail -s "Backup Status" admin@example.com
```

---

## **15. How to run a command every hour using cron?**

```bash
# Edit crontab
crontab -e
# Add the line
0 * * * * /home/user/script.sh
```

---

## **16. How to check disk space of all mounted partitions?**

```bash
df -h
```

---

## **17. How to append timestamped logs to a file?**

```bash
echo "$(date +%F_%T) - Task completed" >> /var/log/task.log
```

---

## **18. How to extract specific columns from a CSV file?**

```bash
awk -F',' '{print $1,$3}' file.csv
```

---

## **19. How to replace a string in multiple files recursively?**

```bash
find /var/www/html -type f -exec sed -i 's/old/new/g' {} \;
```

---

## **20. How to check system uptime?**

```bash
uptime
```

---

## **21. How to check number of files in a directory?**

```bash
ls -1 /var/log | wc -l
```

---

## **22. How to monitor a directory for new files?**

```bash
inotifywait -m /var/log
```

---

## **23. How to find all symbolic links in a directory?**

```bash
find /home/user -type l
```

---

## **24. How to sort a file numerically or alphabetically?**

```bash
sort -n numbers.txt   # numeric
sort names.txt        # alphabetic
```

---

## **25. How to extract lines between two patterns in a file?**

```bash
sed -n '/START/,/END/p' file.txt
```

---

## **26. How to check free memory?**

```bash
free -h
```

---

## **27. How to kill a process by PID or name?**

```bash
kill 1234          # by PID
pkill nginx        # by name
```

---

## **28. How to schedule a one-time job in Linux?**

```bash
at 23:00
# Inside prompt
/home/user/script.sh
Ctrl+D
```

---

## **29. How to check environmental variables?**

```bash
printenv
```

---

## **30. How to archive and compress multiple directories?**

```bash
tar -czf backup.tar.gz /dir1 /dir2 /dir3
```

---

## **31. How to check if a user exists?**

```bash
id -u username &>/dev/null && echo "User exists" || echo "User does not exist"
```

---

## **32. How to monitor a log file in real-time?**

```bash
tail -f /var/log/nginx/error.log
```

---

## **33. How to check last modified time of files?**

```bash
ls -lt /var/log
```

---

## **34. How to copy files with progress display?**

```bash
rsync -ah --progress /source/ /destination/
```

---

## **35. How to extract files from tar.gz archive?**

```bash
tar -xzf archive.tar.gz
```

---

## **36. How to get system hostname and IP?**

```bash
hostname
hostname -I
```

---

## **37. How to get top 5 CPU or memory consuming processes?**

```bash
ps aux --sort=-%cpu | head -n 6
ps aux --sort=-%mem | head -n 6
```

---

## **38. How to change file permissions recursively?**

```bash
chmod -R 755 /var/www/html
```

---

## **39. How to find all files modified in last 24 hours?**

```bash
find /var/log -type f -mtime -1
```

---

## **40. How to check network connectivity?**

```bash
ping -c 4 google.com
```

---

## **41. How to get disk usage of each subdirectory?**

```bash
du -h --max-depth=1 /var/www/html
```

---

## **42. How to check listening ports?**

```bash
ss -tuln
```

---

## **43. How to create a user and set password via script?**

```bash
useradd guru
echo "guru:password" | chpasswd
```

---

## **44. How to check current logged in users?**

```bash
who
```

---

## **45. How to find all executable files in a directory?**

```bash
find /usr/bin -type f -executable
```

---

## **46. How to extract last N lines from a log file?**

```bash
tail -n 20 /var/log/syslog
```

---

## **47. How to count occurrences of a word in a file?**

```bash
grep -o "error" /var/log/nginx/error.log | wc -l
```

---

## **48. How to create a symbolic link?**

```bash
ln -s /var/www/html /home/user/html_link
```

---

## **49. How to schedule a backup using cron?**

```bash
0 2 * * * /home/user/backup.sh
```

---

## **50. How to monitor CPU, memory, and disk usage together?**

```bash
top
vmstat 1
iostat -xz 1
```
