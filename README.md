Introduction:
=============

descracker command permits to perform dictionary attacks ( brute force ) against legacy DES based Unix password hashes. It uses CUDA as acceleration layer. I wrote this program more than 10 years ago, recently I recovered it from a forgotten backup file. I decided to release it with some modernisations ( CUDA and C++ versions ).

![alt text](./screenshots/timeexex.png "Execution Time")

Testing:
=========

Tested on:
* Ubuntu 22.04.4 LTS
* nvcc / CUDA 12.4

Dependencies:
=============

* CUDA 12.4

Installation and Use:
=====================

- compile the program as follow :
```shell
  make clean all
```
- install it:
```shell
  sudo make install
```
- User must provide a text file with the dictionary to use for the brute force attack. The dictionary contains a single word for line, lines are '\n' terminated.

- Use program's help option to have information about required parameters:
```
$ ./descracker -h
./descracker [-H<hash>] [-d dict_file] [ -t group | -T group ] | [-h]

 -H  <hash>      hash to crack
 -d  <dict_file> dictionary file
 -t  <1|2|3>     enable cascading transformation groups
 -T  <1|2|3>     enable specific transformation group
 -b  <units>     cuda block size (optional)
 -h              print this synopsis
```

- This program, besides direct use of dictionary words, can employ some basic transformation on the same words. <BR>
- To activate the transformations  use -t flag. Groups 2 and 3 call the previous ones (i.e. if you specify 3 , also group 2 and 1 will be executed);<BR>
- To use only a specific group use -T (i.e. -T 1 will execute only group 1);
- The flag -H (capital 'h') specifies the hash to crack;<BR>
- The flag -d specifies the dictionary file;<BR>

Transformations:
================

- Group1:
```
initial capital letter
single digit, init / end + capital letter
2 digits init/end + capital letter
special character init/end
some 'camel' case
capital incremental
others
```
- Group2:
```
reverse
specular
repeated
leet
others
```
- Group3:
```
toggle case
duplicated initial
duplicated characters
others
```

Performances:
=============

- This program can be optimised in future (i.e. removing intermediate base64 representation of the hash);<BR>
- On my pc, full attack ( dictionary with 18M+ words , all groups) requires 20 minutes.

Examples:
========
- The following starts a plain dictionary attack:
```shell
descracker -H v.I4KLGWHOIsY -d ./seclist.lower.txt 
```
- The following starts a dictionary attack, then uses all the transformations available to get the password:
```shell
descracker -H v.I4KLGWHOIsY -d ./seclist.lower.txt -b 256 -t3
```

CREDITS:
========

- DES implementation was ripped off from OPENSSL. 
