Following the same steps, and investigating the files 
```bash
bandit30@bandit:/tmp/b30/repo$ cat README.md 
just an epmty file... muahaha
```
So funny :/
Also after searching for branches and logs, there was no results, to be honest It seemed to be hopeless case, but after searching across the internet I found that git offers an option for "tagging" changes, as far as I know it is used to tag a certain point in the history of the repo as important, anyway it's just a text data let's check
```bash
bandit30@bandit:/tmp/b30/repo$ git tag
secret
```
Hmmm, seems interesting, we have a tag called secret, let's see what does it contain
```bash
bandit30@bandit:/tmp/b30/sd/repo$ git show secret 
fb5S2xb7bRyFmAvQYQGEqsbhVyJqhnDy
```
success! the tag contains our password
