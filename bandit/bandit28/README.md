As usual, start with cloning the repo after adding the custom port number and then try reading the file
```bash
bandit28@bandit:/tmp/b28$ cd repo/
bandit28@bandit:/tmp/b28/repo$ ls
README.md
bandit28@bandit:/tmp/b28/repo$ cat README.md 
# Bandit Notes
Some notes for level29 of bandit.

## credentials

- username: bandit29
- password: xxxxxxxxxx
```
Hmmmm, he is trolling us, but ..... did anyone edit that file to make the password like this? Let's see the logs
```bash
bandit28@bandit:/tmp/b28/repo$ git log
commit 8cbd1e08d1879415541ba19ddee3579e80e3f61a (HEAD -> master, origin/master, origin/HEAD)
Author: Morla Porla <morla@overthewire.org>
Date:   Wed Jul 17 15:57:30 2024 +0000

    fix info leak

commit 73f5d0435070c8922da12177dc93f40b2285e22a
Author: Morla Porla <morla@overthewire.org>
Date:   Wed Jul 17 15:57:30 2024 +0000

    add missing data

commit 5f7265568c7b503b276ec20f677b68c92b43b712
Author: Ben Dover <noone@overthewire.org>
Date:   Wed Jul 17 15:57:30 2024 +0000

    initial commit of README.md
```
Success! The file has been edited but we could see the logs and find the password!
