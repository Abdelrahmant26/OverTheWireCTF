Because the '-' character is reserved for bash to make the executable read "Standard input",In
order to read a file called '-' u must specify it inside an absolute or cd to its parent directo>
```bash
cat /home/bandit1/- 
```
OR
```bash
cd /home/bandit1/ && cat ./-
```
