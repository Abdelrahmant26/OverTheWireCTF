Lvl 4 :-
here we have several files to test, we have to loop over them using the find command
```bash
find . -type f -exec file {} +
./-file00: data
./-file03: data
./-file08: data
./-file02: data
./-file04: data
./-file01: data
./-file07: ASCII text
./-file06: data
./-file05: data
./-file09: data
```
we got it! The file07 is the one, simply cat it,but since the naming is not normal as it has the>
```bash
cd inhere && cat ./-file07
```
