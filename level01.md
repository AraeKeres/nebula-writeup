# Challenge
Find and exploit the vulnerability in the given program.

# Solution
Looking at the provided source code, what immediately jumps out to me is that the call to the `env` command uses an absolute path but the call to `echo` uses a relative path. 

Why does this matter? When you try to run a command like `echo` the way the system finds the program is by checking the directories in the PATH environment variable. We control this enivronment variable meaning we can supply our own `echo` program by modifying the PATH.

So we can run arbitrary commands by altering the PATH, so what? Well, let's check the permissions of the provided program:
```shell
level01@nebula:~$ ls -la /home/flag01/flag01
-rwsr-x--- 1 flag01 level01 7322 2011-11-20 21:22 /home/flag01/flag01
```

See the 's' in `-rwsr-x---`? The setuid bit is set! Using what we learned from level00 we know this means the program runs under the flag01 user. So if we alter the PATH variable to run our own `echo` we can run the `getflag` program as flag01 and solve the challenge:
```shell
level01@nebula:~$ echo "/bin/getflag" > echo
level01@nebula:~$ chmod +x echo
level01@nebula:~$ tmp_path=$PATH
level01@nebula:~$ PATH=/home/level01
level01@nebula:~$ /home/flag01/flag01
You have successfully executed getflag on a target account
```

Success! We have solved level01. So what happened here?

First we set up our echo program by echoing "/bin/getflag" into a file named 'echo.' We then give it the execution permission using `chmod` to make it executable. The current contents of the PATH variable are backed up into `$tmp_path` just in case. PATH is set to be our home directory where we created our 'echo' script. Finally, we run the provided `flag01` program.

# Further Reading
For more information on environment variables (including the PATH variable), see this article from Red Hat: [Linux environment variable tips and tricks](https://www.redhat.com/sysadmin/linux-environment-variables)
