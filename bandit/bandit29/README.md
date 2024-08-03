gain, clone the repo and check the file
```bash
bandit29@bandit:/tmp/b29$ cd repo
bandit29@bandit:/tmp/b29/repo$ ls
README.md
bandit29@bandit:/tmp/b29/repo$ cat README.md 
# Bandit Notes
Some notes for bandit30 of bandit.

## credentials

- username: bandit30
- password: <no passwords in production!>
```
interesting, seems to be wrong branch, let's see the available branches
```bash
bandit29@bandit:/tmp/b29/repo$ git branch
  dev
* master
  origin/HEAD
  origin/dev
  origin/master
```
there is few of them, after trial and error, the dev branch was the right one
```bash
bandit29@bandit:/tmp/b29/repo$ git checkout dev
Switched to branch 'dev'
Your branch is up to date with 'remotes/origin/dev'.
bandit29@bandit:/tmp/b29/repo$ cat README.md 
# Bandit Notes
Some notes for bandit30 of bandit.

## credentials

- username: bandit30
- password: qp30ex3VLz5MDG1n91YowTv4Q8l7CDZL
```
