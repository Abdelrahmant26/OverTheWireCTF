Sadly, We cannot login to this level, We will be disconnected every time we try to connect, What a bad luck!
But!!
We can use scp which is used to securely copy files between devices over ssh, we just need to grab that readme file to the local machine 
```bash
$ scp -P 2220 bandit18@bandit.labs.overthewire.org:/home/bandit18/readme . && cat readme
```
