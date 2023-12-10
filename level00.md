# Challenge
Find a program with the SUID bit set.

Challenge Link: https://exploit.education/nebula/level-00/

# Solution
We can use the `find` command to find programs with the SUID bit set. 

```shell
level00@nebula:~$ find / -user flag00 -perm /u=s 2>/dev/null
/bin/.../flag00
/rofs/bin/.../flag00
```

In simple english, we're starting at the root directory (`/`) and looking for all files owned by the user flag00 (`-user flag00`) with the SUID/setuid permission (`-perm /u=s`) and redirecting all errors to `/dev/null` (`2>/dev/null`) to clean up the output.

If we wanted to use numeric permissions instead of symbolic permissions we could replace `-perm /u=s` with `-perm /4000`.

Let's try running that first binary:
```shell
level00@nebula:~$ /bin/.../flag00
Congrats, now run getflag to get your flag!
flag00@nebula:~$
```

Notice how we swapped from user level00 to flag00. Let's run `getflag`.
```shell
flag00@nebula:~$ getflag
You have successfully executed getflag on a target account
```

Success! We have solved level00.

# Further Reading
For a more detailed explanation of the command we used, we can use [explainshell.com](https://explainshell.com/explain?cmd=find+%2F+-user+flag00+-perm+%2Fu%3Ds+2%3E%2Fdev%2Fnull)

For more explanation on the numeric and symbolic representations of permissions as well as special permissions see this article from Red Hat: [Linux permissions: SUID, SGID, and sticky bit](https://www.redhat.com/sysadmin/suid-sgid-sticky-bit)
