Simply using diff command with the 2 password files as arguments, It showed the line with difference between them
```bash
bandit17@bandit:~$ diff passwords.old passwords.new
42c42
< bSrACvJvvBSxEM2SGsV5sn09vc3xgqyp //here it shows that .new has this line
---
> x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO //here it shows that .old has this line
```
