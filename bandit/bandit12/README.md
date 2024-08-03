For this challenge, we have to reverse a hexdump of a compressed file then decompress it
reversing the hexdump can be done using xxd command
```bash
bandit12@bandit:/tmp/midgeek$ xxd -r data.txt > out
```
here I redirected the output to a file called out, now Let's find out what type of compression this file use
```bash
bandit12@bandit:/tmp/midgeek$ file out
out: gzip compressed data, was "data2.bin", last modified: Wed Jul 17 15:57:06 2024, max compression, from Unix, original size modulo 2^32 577
```
It's a gzip format , so we have to rename it to have a proper suffix, then decompress it
```bash 
bandit12@bandit:/tmp/midgeek$ mv out out.gz && gzip -d out.gz
```
Again, check the output file's compression type
```bash
bandit12@bandit:/tmp/midgeek$ file out
out: bzip2 compressed data, block size = 900k
```
Now It's another type, a bzip, so we decompress also
```bash
bandit12@bandit:/tmp/midgeek$ bunzip2 -d out
bunzip2: Can't guess original name for out -- using out.out
```
And decompression goes on ...
```bash
bandit12@bandit:/tmp/midgeek$ file out.out 
out.out: gzip compressed data, was "data4.bin", last modified: Wed Jul 17 15:57:06 2024, max compression, from Unix, original size modulo 2^32 20480
bandit12@bandit:/tmp/midgeek$ mv out.out out.out.gz
bandit12@bandit:/tmp/midgeek$ gzip -d out.out.gz 
bandit12@bandit:/tmp/midgeek$ file out.out 
out.out: POSIX tar archive (GNU)
bandit12@bandit:/tmp/midgeek$ tar -xf out.out
bandit12@bandit:/tmp/midgeek$ ls
data5.bin  data.txt  out.out
bandit12@bandit:/tmp/midgeek$ file data5.bin 
data5.bin: POSIX tar archive (GNU)
bandit12@bandit:/tmp/midgeek$ tar -xf data5.bin
bandit12@bandit:/tmp/midgeek$ ls
data5.bin  data6.bin  data.txt  out.out
bandit12@bandit:/tmp/midgeek$ file data6.bin 
data6.bin: bzip2 compressed data, block size = 900k
bandit12@bandit:/tmp/midgeek$ bzip2 -d data6.bin
bzip2: Can\'t guess original name for data6.bin -- using data6.bin.out
bandit12@bandit:/tmp/midgeek$ file data6.bin.out 
data6.bin.out: POSIX tar archive (GNU)
bandit12@bandit:/tmp/midgeek$ tar -xf data6.bin.out
bandit12@bandit:/tmp/midgeek$ ls
data5.bin  data6.bin.out  data8.bin  data.txt  out.out
file data8.bin
data8.bin: gzip compressed data, was "data9.bin", last modified: Wed Jul 17 15:57:06 2024, max compression, from Unix, original size modulo 2^32 49
bandit12@bandit:/tmp/midgeek$ ls
data5.bin  data6.bin.out  data8.bin  data.txt  out.out
bandit12@bandit:/tmp/midgeek$ mv data8.bin data8.bin.gz
bandit12@bandit:/tmp/midgeek$ gzip -d data8.bin.gz 
bandit12@bandit:/tmp/midgeek$ ls
data5.bin  data6.bin.out  data8.bin  data.txt  out.out
bandit12@bandit:/tmp/midgeek$ file data8.bin 
data8.bin: ASCII text
bandit12@bandit:/tmp/midgeek$ cat data8.bin 
The password is FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn //finally!
```
