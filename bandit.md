# Bandit
Below you can see my solutions for [Bandit](https://overthewire.org/wargames/bandit/) game created by [OverTheWire](https://overthewire.org/wargames/)
## Bandit 0
All you have to do to get to the next level is just logging to bandit0 acc
```bash
:~$ ssh bandit0@bandit.labs.overthewire.org -p 2220
bandit0@bandit.labs.overthewire.org s password: bandit0
```
Password to level 1 is stored in readme file.
```
Password: boJ9jbbUNNfktd78OOpsqOltutMc3MY1
```

## Bandit 1
Password to level 2 is stored in file named -. To acces it, we have to enter name with "./"
```bash
:~$ cat ./-
CV1DtqXWVFXTvM2F0k09SHz0YwRINYA9
:~$
```

## Bandit 2
Password to level 3 is stored in file with spaces in it's name. Bash is not clever enough to handle spaces, so we have to write them with "\\".
```bash
:~$ cat ./spaces\ in\ this\ filename
UmHadQclWmgdLOKQ3YNgjWxGoRMb5luK
```

## Bandit 3
Password to level 4 is stored in directory with hidden file.
```bash
:~$ cat /home/bandit3/inhere/.hidden
pIwrPrtPN36QITSp3EQaw936yaFoFgAB
```

## Bandit 4
Password to level 5 is stored in only human-readable file in inhere directory. We can find it easily with file command.
```bash
:~$ cd inhere
:~/inhere$ file ./*
./-file00: data
./-file01: data
./-file02: data
./-file03: data
./-file04: data
./-file05: data
./-file06: data
./-file07: ASCII text
./-file08: data
./-file09: data
:~/inhere$ cat ./-file07
koReBOKuIDDepwhWk7jZC0RTdopnAYKh
```

## Bandit 5
The password for the next level is stored in a file somewhere under the inhere directory and has all of the following properties:

 - human-readable
 - 1033 bytes in size
 - not executable

```bash
:~$ cd inhere
:~/inhere$ find ./ ! -executable -type f -size 1033c
./maybehere07/.file2
```
```bash
:~/inhere$ cat ./maybehere07/.file2
DXjZPULLxYr17uwoI01bNLQbtFemEgo7
```

## Bandit 6
The password for the next level is stored somewhere on the server and has all of the following properties:

- owned by user bandit7
- owned by group bandit6
- 33 bytes in size

```bash
:~$ find / -size 33c -user bandit7 -group bandit6 2>/dev/null
/var/lib/dpkg/info/bandit7.password
:~$ cat /var/lib/dpkg/info/bandit7.password
HKBPTKQnIay4Fw76bEy8PVxKEDQRKTzs
```

## Bandit 7
The password for the next level is stored in the file data.txt next to the word millionth

```bash
:~$ grep millionth data.txt
millionth       cvX2JJa4CFALtqS87jk27qwqGhBM9plV
```

## Bandit 8
The password for the next level is stored in the file data.txt and is the only line of text that occurs only once

```bash
:~$ sort data.txt | uniq -u
UsvVyFSfZZWbi6wgC7dAFyFuR6jQQUhR
```

## Bandit 9
The password for the next level is stored in the file data.txt in one of the few human-readable strings, preceded by several ‘=’ characters.

```bash
:~$ strings data.txt | grep ==
========== the*2i"4
========== password
Z)========== is
&========== truKLdjsbJ5g7yyJ2X2R0o3a5HQJFuLk
```

## Bandit 10
The password for the next level is stored in the file data.txt, which contains base64 encoded data

```bash
:~$ base64 -d data.txt
The password is IFukwKGsFW8MOq3IRFqrxE1hxTNEbUPR
```

## Bandit 11
The password for the next level is stored in the file data.txt, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions

```bash
:~$ cat data.txt | tr a-zA-Z n-za-mN-ZA-M
The password is 5Te8Y4drgCRfCx8ugdwuEX8KFC6k2EUu
```

## Bandit 12
The password for the next level is stored in the file data.txt, which is a hexdump of a file that has been repeatedly compressed. For this level it may be useful to create a directory under /tmp in which you can work using mkdir. For example: mkdir /tmp/myname123. Then copy the datafile using cp, and rename it using mv (read the manpages!)

```bash
:~$ mkdir /tmp/asdfg
:~$ cp data.txt /tmp/asdfg
:~$ cd /tmp/asdfg
:/tmp/asdfg$ xxd -r data.txt > qwer
```

And now we have to revert all compressions.

```bash
:/tmp/asdfg$ file qwer
qwer: gzip compressed data, was "data2.bin", last modified: Thu May  7 18:14:30 2020, max compression, from Unix
:/tmp/asdfg$ mv qwer qwer.gz
:/tmp/asdfg$ gunzip qwer.gz
:/tmp/asdfg$ file qwer
qwer: bzip2 compressed data, block size = 900k
:/tmp/asdfg$ bunzip2 qwer
qwer.out: gzip compressed data, was "data4.bin", last modified: Thu May  7 18:14:30 2020, max compression, from Unix
:/tmp/asdfg$ mv qwer.out qwer.gz
:/tmp/asdfg$ gunzip qwer.gz
:/tmp/asdfg$ file qwer
qwer: POSIX tar archive (GNU)
:/tmp/asdfg$ tar -xf qwer
:/tmp/asdfg$ file data5.bin
data5.bin: POSIX tar archive (GNU)
:/tmp/asdfg$ tar -xf data5.bin
:/tmp/asdfg$ file data6.bin
data6.bin: bzip2 compressed data, block size = 900k
:/tmp/asdfg$ bunzip2 data6.bin
:/tmp/asdfg$ file data6.bin.out
data6.bin.out: POSIX tar archive (GNU)
:/tmp/asdfg$ tar -xf data6.bin.out
:/tmp/asdfg$ file data8.bin
data8.bin: gzip compressed data, was "data9.bin", last modified: Thu May  7 18:14:30 2020, max compression, from Unix
:/tmp/asdfg$ mv data8.bin qw.gz
:/tmp/asdfg$ gunzip qw.gz
:/tmp/asdfg$ file qw
qw: ASCII text
:/tmp/asdfg$ cat qw
The password is 8ZjyCRiBWFYkneahHwxCv3wb2a1ORpYL
```