This challenge is quite simple , just use the ssh key in the directory to log in as the next challenge user
```bash
bandit13@bandit:~$ ssh bandit14@localhost -p 2220 -i sshkey.private //and go through ssh stuff
```
So much easy to get the password 
```bash
bandit14@bandit:~$ cat /etc/bandit_pass/bandit14
MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS
```
