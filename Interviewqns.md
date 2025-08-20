1. Server is slow. How will you check?
- uptime
- top
- free -m
- df -h
- iostat

| Tool      | Checks for                |
| --------- | ------------------------- |
| `uptime`  | CPU load / system load    |
| `top`     | High resource processes   |
| `free -m` | Memory usage / swap usage |
| `df -h`   | Disk space usage          |
| `iostat`  | Disk I/O bottlenecks      |

2. Find .log files in /var/log modified in last 7 days

| Part            | Meaning                                                                            |
| --------------- | ---------------------------------------------------------------------------------- |
| `find`          | The Linux command used to search for files and directories.                        |
| `/var/log`      | The directory in which to search (commonly holds system log files).                |
| `-name "*.log"` | Search for files with names ending in `.log`. Wildcard `*` matches any filename.   |
| `-type f`       | Restrict search to **regular files** (ignores directories, symlinks, etc.).        |
| `-mtime -7`     | Find files **modified less than 7 days ago**. `-7` means "within the last 7 days". |

3. Restart a service in linux
- systemctl restart servicename
- servoce restart servicename

4. Find which process isn using port 8080?
- losf: list open files (including network connections)
- -i filters the process which is using port 8080

5. View logs in real time
- tail -f /var/log/nginx/access.log

- tail: Displays the last lines of a file
- -f "Follow" keeps the terminal open and continously shows new lines as they are added to the file
- /var/log/nginx/access.log: This is the file you are watching

6. Show first 20 lines of a large file
- head -n 20 filename

7. Change file permission so only owner can read/write
- chmod 600 filename
- chmod u=rw,go= filename

8. Search error in a file
- grep -i "error" filename

9. Check disk usage 
- df -h

10. Schedule a script every day at 5 PM.
- crontab -e 0 17 * * * path to script

11. Kill a process by name
- pkill -f processname

12. Find last 10 logged in users
- last -n 10

13. Find largest 5 files in /var/log
- du -ah /bar/log | sort -rh | head -n 5

14. Count numbers in a file 
- wc -l filename

15. Compress a folder
- tar -czvf backup.tar.gz foldername

16. Extract a tar file
- tar -xzvf backup.tar.gz

17. Check CPU usage of processes
- top

18. Find total memory in GB
- free -g

19. Find all open networks
- ss -tulwn

20. Find OS version
- cat /etc/os-release

21. Copy a file to a remote server
- scp filename username@path to dir

22. Check failed login attempts
- grep "Failed password" /var/log/auth.log

23. List all users in system
- cut -d: f1 /etc/passwd

24. Find and delete files older than 30 days
- find /tmp -type f -mtime +30 -delete

25. Monitor file chanegs in realtime
- inotofywait -m path to dir

26. Write a script to check if a services is running and reatart if stopped
- 
if ! systemctl is-active --quiet $SERVICE; then
    echo "$SERVICE is not running, restarting..."
    systemctl restart $SERVICE
fi

27. Check number files in in a directory
- ls -1 | wc -l

28. Find and replace text in multiple files
- grep -rl "oldtext" . | xargs sed -i 's/oldtext/newtext/g'

29. Create a user with specific home sir
- sudo useradd -m -d /custom home username

30. Set env variables permanently
- Add export VAR=value in ~/.bashrc
and run source ~/.bashrc





