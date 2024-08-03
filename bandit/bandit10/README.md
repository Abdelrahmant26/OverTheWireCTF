Also quite simple, just a one command for decoding
```bash
bandit10@bandit:~$ base64 -d data.txt 
The password is dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr
```
Lvl 11 :-
For this challenge, we need some command to just translate characters in order and according to >
```bash
bandit11@bandit:~$ cat data.txt | tr "a-mn-zA-MN-Z" "n-za-mN-ZA-M"
The password is 7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4
```
