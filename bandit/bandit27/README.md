Now it's quite easy, we always clone girhub repos, but to specify a custom port you should add a colon ":" and the port number after the hostname/ip
```bash
bandit27@bandit:/tmp/b27$ git clone ssh://bandit27-git@localhost:2220/home/bandi
t27-git/repo //and enter the password (same as the ssh password)
```
Now we have the repo cloned it only contains a text file, the password obviously
```bash
bandit27@bandit:/tmp/b27$ cd repo/
bandit27@bandit:/tmp/b27/repo$ ls
README
bandit27@bandit:/tmp/b27/repo$ cat README 
The password to the next level is: Yz9IpL0sBcCeuG7m9uQFt8ZNpS4HZRcN
```
