# OverTheWire: Bandit Walkthrough
### My goal is to improve my Linux and CLI skills

# --------------------------------------------------------------------------------------------- #

## Level 0
*Goal:* connect to OTW's server using SSH

*Commands:* 
- `ssh <username@host>`: used to connect to a remote host.
    - `-port <port>`: designate a port
- `man <command>`: open the manual page of a command. **This is very important.**

*Givens:*
- Username = bandit0
- Password = bandit0
- Host     = bandit.labs.overthewire.org
- Port     = 2220

*Answer:*
```bash
ssh bandit0@bandit.labs.overthewire.org -p 2220
```

# --------------------------------------------------------------------------------------------- #

## Level 1
*Goal:* Read file called 'readme' located in the home directory

*Commands:*
- `pwd`: print current directory path
- `ls`: list visible contents of a directory
- `cat`: display contents of a text file
- `head`: display first 10 lines of a text file
    - `-n <int>`: Specify number of lines
- `tail`: display last 10 lines of a text file
    - `-n <int>`: Specify number of lines
- `exit`: stop SSH connection

*Steps:*
1. `pwd` to make sure I am in home directory
2. `ls` to list contents
3. `cat readme` to display the password. Success

*Answer:*
ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If

# --------------------------------------------------------------------------------------------- #

## Level 2
*Goal:* Read file called '-' located in the home directory

*Commands:*
- `<`: input redirector (opens a file and connects it to the command's standard input)
- `/home/bandit1/`: absolute path; it starts from root
- `./`: relative path; starts from current directory

*Steps:*
1. `pwd` to print directory
2. `ls` to list contents
3. `cat -` failed. CLI didn't respond because it was waiting for input. `-` in linux is a convention that means read from standard input/keyboard. I terminated using ctrl+C
4. `cat '-'` and `cat "-"` also failed
5. `cat ./-` and `cat /home/bandit1/-`. Success. Lists the path of the file so cat doesn't get confused.
6. `cat < -`. Success. Redirects the file into cat as input.

*Answer:*
263JGJPfgU6LtdEvgfWU1XP5yac29mFx

# --------------------------------------------------------------------------------------------- #

## Level 3
*Goal:* Read file called 'spaces in this filename' in the home directory

*Commands:*

*Steps:*
1. `pwd` to print directory
2. `ls` to list contents
3. `cat spaces in this filename` failed. It says no such file or directory for spaces, in, this, filename. Cat is for concatenation. It regards each word as a separate file.
4. `cat "spaces in this filename"` and `cat 'spaces in this filename'`. Success
5. `cat spaces\ in\ this\ filename`. Success. Can be done by writing `cat s` then pressing TAB, which completes the filename.

*Answer:*
MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx

# --------------------------------------------------------------------------------------------- #

## Level 4
*Goal:* Read hidden file in the 'inhere' directory

*Commands:*
- `cd`: change directory
- `ls -a`: list visible and hidden files

*Steps:*
1. `pwd` to print directory
2. `ls` to list contents
3. `cd inhere/` to change directory
4. `ls` lists nothing. Failed
5. `ls -a` lists hidden file '...Hiding-From-You'
6. `cat ...Hiding-From-You`. Success

*Answer:*
2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ

# --------------------------------------------------------------------------------------------- #

## Level 5
*Goal:* Read the only human-readable file when given 10 files.

*Commands:*
- `file`: shows file type
- `--`: signals end of options. 
- `*`: all

*Steps:*
1. `pwd` to print directory.
2. `ls` to list contents.
3. `cd inhere/` to change directory.
4. `ls` to list contents.
5. `cat` for every file would work, but very slow.
6. `cat -- -file*` failed. It sent a bunch of gibberish output, and the password was included.
7. `file ./*` to show the type of all files in the `inhere/` directory
8. Only -file07 has ASCII text as its type; others are data and one is PGP Secret Sub-key, which is a diversion
9. `cat < -file07` or `cat ./-file07`. Success. (Return to level 2 for explanation)

*Answer:*
4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw

# --------------------------------------------------------------------------------------------- #

## Level 6
*Goal:* Read a file under the `inhere/` directory with the following properties:
human-readable
1033 bytes in size
not executable

*Commands:*
- `find`: search
    - `-size n(ckMG)`: File uses less than, more than or exactly n units of space, rounding up. bytes, KiB (1024 bytes), 
    MiB (1024 KiB, 1048576 bytes), GiB (1024 Mib, 1073741824 bytes)
    - `-executable`: checks if file is executable.
    - `-name <pattern>` and `-iname <pattern>`: -i is case insensitive
    - `-readable`: checks if file is readable
    - `-type`: type. `f` for regular files
    - `!`: not
    - `.`: current directory

*Steps:*
1. `pwd` to print directory.
2. `ls` to list contents.
3. `cd inhere/` to change directory.
4. `ls` to list contents. 20 directories
6. `man find` to know how to use the command.
5. `find . -type f -readable -size 1033c !-executable`. Success. Output is ./maybehere07/.file2
6. `cat ./maybehere07/.file2`.

*Answer:*
HWasnPhtq9AVKe0dmk45nxy20cvUa6EG

# --------------------------------------------------------------------------------------------- #

## Level 7
*Goal:* password is somewhere on the server and has the following properties:
owned by user bandit7
owned by group bandit6
33 bytes in size

*Commands:*
- `find`: search
    - `-user <user>`: owned by user
    - `-group <group>`: owned by group
    - `/`: root directory
    - `2`: error output
    - `/dev/null`: discards all input (called black hole)

*Steps:*
1. `find / -user bandit7 -group bandit6 -size 33c`. Gave me a long list of permission denied, but the file was found. 
/var/lib/dpkg/info/bandit7.password
2. `find / -user bandit7 -group bandit6 -size 33c 2>/dev/null`. This redirected all error messages to the black hole. Easier to spot the needed file
3. `cat /var/lib/dpkg/info/bandit7.password`. Success

*Answer:*
morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj

# --------------------------------------------------------------------------------------------- #

## Level 8
*Goal:* The password for the next level is stored in the file data.txt next to the word millionth

*Commands:*
- `grep "<pattern>" <file>`: Searches for pattern in the file. Prints the line.

*Steps:*
1. `ls`. Found data.txt
2. `cat data.txt`. Started printing a very large file. Terminated using ctrl+c
3. `grep "millionth" data.txt`. Success

*Answer:*
dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc

# --------------------------------------------------------------------------------------------- #

## Level 9
*Goal:* The password for the next level is stored in the file data.txt and is the only line of text that occurs only once

*Commands:*
- `uniq`: detects duplicates (REQUIRES sorted input)
    - `-u`: prints unique lines ONLY
- `sort`: sorts alphabetically
    - `-n`: sorts numerically ascending
    - `-u`: remove duplicates; still prints one of every line.
- `|`: takes output of command on the left and uses it as input for command on the right.

*Steps:*
1. `cat data.txt`. Printed many lines of letters
2. `man sort` and `man uniq`. 
3. `sort data.txt | uniq -u`. Success.

*Answer:*
4CKMh1JI91bUIZZPXDqGanal4xvAg0JM

# --------------------------------------------------------------------------------------------- #

## Level 10
*Goal:* The password for the next level is stored in the file data.txt in one of the few human-readable strings, c

*Commands:*
- `grep`: search
    - `-a`: forces grep to treat binary files as text. Because if you try to print a file that contains binary it will output `grep: data.txt: binary file matches`
- `string <file>`: extracts all human-readable ASCII characters from a file


*Steps:*
1. `grep "=" data.txt`. Output is `grep: data.txt: binary file matches`
2. `grep "=" data.txt -a`. Output is a lot of gibberish from binary, but the password is there.
3. `strings data.txt | grep "="`. I can see the password much more clearly, but there is still gibberish. After analyzing the ouput and re-reading the goal, I realized it says "preceded by several ‘=’ characters.", and I did only 1.
4. `strings data.txt | grep "=="`. Success.

*Answer:*
FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey

# --------------------------------------------------------------------------------------------- #

## Level 11
*Goal:* The password for the next level is stored in the file data.txt, which contains base64 encoded data

*Commands:*
- `base64`: use base64
    - `-d`: decode base64 encoding
    - `-i`: ignore garbage 

*Steps:*
1. `man base64`
2. `base64 -di data.txt`. Success. (I also tried without `-i` and it worked)

*Answer:*
dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr

# --------------------------------------------------------------------------------------------- #

## Level 12
*Goal:* The password for the next level is stored in the file data.txt, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions

*Commands:*
- `tr`: transform characters. Doesn't take file as input, have to use `|` or `<`

*Steps:*
1. `tr 'a-m' 'n-z' < data.txt`. Output: Gur pnssworq vs 7x16JArUVv5LxVuJssSVqootnHGyw9D4
2. `tr 'n-z' 'a-m' < data.txt`. Output: Ghe caffjbed if 7k16JAeUVi5LkVhJffSVdbbgaHGlj9D4
3. `tr 'A-M' 'N-Z' < data.txt`. Output: Gur pnssworq vs 7x16JArUVv5LxVuJssSVqootnHGyw9D4
4. `tr 'N-Z' 'A-M' < data.txt`. Output: Ghe caffjbed if 7k16JAeUVi5LkVhJffSVdbbgaHGlj9D4
5. I realize I am doing only half. I facepalm
6. `cat data.txt | tr 'a-m' 'n-z' | tr 'n-z' 'a-m' | tr 'A-M' 'N-Z' | tr 'N-Z' 'A-M'`. Correct idea, but still wrong. Lowercase is correct, but then the decoded is sent to be 'decoded' again but in uppercase, so it messes it up. 
Output: Ghe caffjbed if 7k16JAeHIi5LkIhJffFIdbbgaHGlj9D4
7. `tr 'A-Za-z' 'N-ZA-Mn-za-m' < data.txt`. Success. A-Z is transformed to N-Z and A-M. a-z is tranformed to n-z and a-m.
8. `cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'` also works.

*Answer:*
7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4 

# --------------------------------------------------------------------------------------------- #

## Level 13
*Goal:* The password for the next level is stored in the file data.txt, which is a hexdump of a file that has been repeatedly compressed. 

*Commands:*
- `xxd`: creates or reverses hex dumps from or to binary
    - `r`: reverse
- `touch`: create file
- `gzip` and `bzip2`: compress
    - `-d` decompress
    - `-S .bin` forces on files ending with .bin; usually it gives error. 
- `mv`: move or rename
- `tar`: create, manipulate, and extract archive files
    - `-x`: extracts from archive
    - `-f`: specify filename
    - `t`: shows what can be extracted. useful in step 28.

*Steps:*
1. `gzip -d data.txt`. Error, can't decompress data.txt because it isn't .gz and isn't decompressible.
2. `mv data.txt data.txt.gz | gzip -d data.txt`. Error. Data not in gzip format.
3. Need to convert hex dump to binary first then decompress.
4. `touch data.bin`. Create a binary file
5. `xxd -r data.txt > data.bin`. Convert hex dump to binary and put it into data.bin. Can overwrite data.txt, but this is cleaner for me.
6. `file data.txt.gz`. Output: data.txt.gz: ASCII text
7. `file data.bin`. Output: data.bin: gzip compressed data.
8. `gzip -d -S .txt data.txt`. Output: gzip: data.txt: not in gzip format
9. `gzip -d -S .bin data.bin`. There was no output. data.bin becomes data, and has to be renamed to data.bin again.
10. `mv data data.bin`.
11. Have to do it several times because it has been repeatedly compressed.
12. Error. Can no longer decomrpess using gzip. 
13. `file data.bin`. Output: data.bin: bzip2 compressed data, block size = 900k. In step 7, it was gzip.
14. `bzip2 -d data.bin`. Output: data.bin.out
15. `mv data.bin.out data.bin`.
16. `file data.bin`. gzip again
17. `gzip -d -S .bin data.bin`.
18. `file data`. Output: data: POSIX tar archive (GNU). We are done decompressing.
19. `tar -xf data`. New file called 'data5.bin' appeared in directory.
20. `file data5.bin`. Output: data: POSIX tar archive (GNU). So I do `tar -xf data` again, which results in a new file called 'data6.bin'.
21. `file data6.bin`. Output: data6.bin: bzip2 compressed data, block size = 900k.
22. `bzip2 -d data6.bin`. Became data6.bin.out
23. `file data6.bin.out`. Output: data6.bin.out: POSIX tar archive (GNU)
24. It created data8.bin.
25. `file data8.bin`. gzip
26. `gzip -d -S .bin data8.bin`. Step 9. Makes a new file called data.
27. `file data`. Output: data: POSIX tar archive (GNU)
28. `tar -xf data`. Made a new file named data5.bin; which has the same name as a previous file. To make sure, use -tf, which doesn't extract but shows the files that can be extracted.
29. `file data5.bin`. Output: data5.bin: POSIX tar archive (GNU).
30. `tar -xf data5.bin`. data6.bin
31. `file data6.bin`. data6.bin: bzip2 compressed data, block size = 900k
32. `bzip2 -d data6.bin`. 
33. `file data6.bin.out`. Output: data6.bin.out: POSIX tar archive (GNU)
34. `tar -xf data6.bin.out`. data8.bin
35. `file data8.bin`. gzip
36. `gzip -d -S .bin data8.bin`. produces data8
37. `file data8`. Output: data8: ASCII text

*Answer:*
FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn

# --------------------------------------------------------------------------------------------- #

## Level 14

*Goal:* The password for the next level is stored in /etc/bandit_pass/bandit14 and can only be read by user bandit14. For this level, you don’t get the next password, but you get a private SSH key that can be used to log into the next level. Note: localhost is a hostname that refers to the machine you are working on

*Commands:*
- `ssh <username@host>`: securely connect to a remote host (also in level 0)
    - `-i <filename>`: specify a file for a private SSH key.
    - `-port <port>`: designate a port


*Steps:*
1. `ls`. file name is sshkey.private
2. `cat sshkey.private`. I was curious. Just a bunch of letters and numbers in seemingly random order.
2. `man ssh`. used this to discover `-i`
3. `ssh -i sshkey.private -p 2220 bandit14@locahost`. bandit14 is obvious from the goal. localhost was a guess based on the note

*Answer:*
```bash
ssh -i sshkey.private bandit14@localhost -p 2220
```

# --------------------------------------------------------------------------------------------- #

## Level 15

*Goal:* The password for the next level can be retrieved by submitting the password of the current level to port 30000 on localhost.

*Commands:*
- `telnet <host> <port>`: The  telnet  command is used to communicate with another host using the TELNET protocol.

*Steps:*
1. `cat /etc/bandit_pass/bandit14`. This is from the goal of the previous level. MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS
2. `man telnet`. I used this to learn the telnet syntax.
3. `telnet localhost 30000`.
4. Telnet connection opens. Input previous password

*Answer:*
8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo

# --------------------------------------------------------------------------------------------- #

## Level 16

*Goal:* The password for the next level can be retrieved by submitting the password of the current level to port 30001 on localhost using SSL/TLS encryption.

*Commands:*
- `openssl`: OpenSSL is a cryptography toolkit implementing the Secure Sockets Layer and Transport Layer Security network protocols
    - `s_client`: Initiate a raw SSL/TLS client connection to a remote host and port. Like telnet, but for encrypted connections.
    - `-connect`: connect

*Steps:*
1. `man ssl`. Says to see individual pages for detail.
2. `man tls`. Command doesn't exist
3. `man openssl`. 
4. `openssl s_client -connect localhost:30001`
5. Input previous password

*Answer:*
kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx

# --------------------------------------------------------------------------------------------- #

## Level 17

*Goal:* The credentials for the next level can be retrieved by submitting the password of the current level to a port on localhost in the range 31000 to 32000. First find out which of these ports have a server listening on them. Then find out which of those speak SSL/TLS and which don’t. There is only 1 server that will give the next credentials, the others will simply send back to you whatever you send to it.

*Commands:*
- `nmap <port> <hostname>`: Network exploration tool and security / port scanner
    - `-p <port ranges>`: port range
- `ncat <hostname> <port>`: Concatenate and redirect sockets
    - `--ssl`: Connect or listen with SSL
- `openssl`: go to previous level
    - `-quiet`: suppresses most diagnostic output during SSL/TLS handshake.
- `chmod`: change mode; change permissions
    - `600`: only owner can read (4) + write (2). Group and others = 0. 600
    - **Read, write, execute. 4, 2, 1. User, group, others.**
    - `chmod u+rw, u-x, g-rwx, o-rwx` should also have the same function.

*Steps:*
1. `man nmap`
2. `nmap -p31000-32000 localhost`. Output is 5 ports. 31046, 31518, 31691, 31790, 31960
3. `man ncat`
4. `ncat -ssl localhost <port>`. I tried the 5 open ports. Nothing happened, and I terminated using ctrl+c. From what I understand after some research, it is waiting for input.
5. `echo "kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx" | ncat -ssl localhost <port>`. I thought that if the output was the same as the previous password, then according to the goal, it isn't the target port. Nothing happened, and I terminated using ctrl+c. It also needs some kind of input. Also, apparently ncat -ssl isn't very reliable in terms of ssl.
6. `echo "kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx" | openssl s_client -connect localhost:<port>`. Lots of words as output. No password. However, only 31518 and 31790 have a certificate.
7. `echo "kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx" | openssl s_client -connect localhost:<port> -quiet`>. Tried previous 2 ports. 31790 returned 'correct!' and an RSA private key.
8. `echo "[answer]" > rsakey.pem`.
9. `ssh -i rsakey.pem bandit17@bandit.labs.overthewire.org -p 2220`. Tells me permissions are too open
10. `chmod 600 rsakey.pem`. Now it works.

*Answer:*
-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAvmOkuifmMg6HL2YPIOjon6iWfbp7c3jx34YkYWqUH57SUdyJ
imZzeyGC0gtZPGujUSxiJSWI/oTqexh+cAMTSMlOJf7+BrJObArnxd9Y7YT2bRPQ
Ja6Lzb558YW3FZl87ORiO+rW4LCDCNd2lUvLE/GL2GWyuKN0K5iCd5TbtJzEkQTu
DSt2mcNn4rhAL+JFr56o4T6z8WWAW18BR6yGrMq7Q/kALHYW3OekePQAzL0VUYbW
JGTi65CxbCnzc/w4+mqQyvmzpWtMAzJTzAzQxNbkR2MBGySxDLrjg0LWN6sK7wNX
x0YVztz/zbIkPjfkU1jHS+9EbVNj+D1XFOJuaQIDAQABAoIBABagpxpM1aoLWfvD
KHcj10nqcoBc4oE11aFYQwik7xfW+24pRNuDE6SFthOar69jp5RlLwD1NhPx3iBl
J9nOM8OJ0VToum43UOS8YxF8WwhXriYGnc1sskbwpXOUDc9uX4+UESzH22P29ovd
d8WErY0gPxun8pbJLmxkAtWNhpMvfe0050vk9TL5wqbu9AlbssgTcCXkMQnPw9nC
YNN6DDP2lbcBrvgT9YCNL6C+ZKufD52yOQ9qOkwFTEQpjtF4uNtJom+asvlpmS8A
vLY9r60wYSvmZhNqBUrj7lyCtXMIu1kkd4w7F77k+DjHoAXyxcUp1DGL51sOmama
+TOWWgECgYEA8JtPxP0GRJ+IQkX262jM3dEIkza8ky5moIwUqYdsx0NxHgRRhORT
8c8hAuRBb2G82so8vUHk/fur85OEfc9TncnCY2crpoqsghifKLxrLgtT+qDpfZnx
SatLdt8GfQ85yA7hnWWJ2MxF3NaeSDm75Lsm+tBbAiyc9P2jGRNtMSkCgYEAypHd
HCctNi/FwjulhttFx/rHYKhLidZDFYeiE/v45bN4yFm8x7R/b0iE7KaszX+Exdvt
SghaTdcG0Knyw1bpJVyusavPzpaJMjdJ6tcFhVAbAjm7enCIvGCSx+X3l5SiWg0A
R57hJglezIiVjv3aGwHwvlZvtszK6zV6oXFAu0ECgYAbjo46T4hyP5tJi93V5HDi
Ttiek7xRVxUl+iU7rWkGAXFpMLFteQEsRr7PJ/lemmEY5eTDAFMLy9FL2m9oQWCg
R8VdwSk8r9FGLS+9aKcV5PI/WEKlwgXinB3OhYimtiG2Cg5JCqIZFHxD6MjEGOiu
L8ktHMPvodBwNsSBULpG0QKBgBAplTfC1HOnWiMGOU3KPwYWt0O6CdTkmJOmL8Ni
blh9elyZ9FsGxsgtRBXRsqXuz7wtsQAgLHxbdLq/ZJQ7YfzOKU4ZxEnabvXnvWkU
YOdjHdSOoKvDQNWu6ucyLRAWFuISeXw9a/9p7ftpxm0TSgyvmfLF2MIAEwyzRqaM
77pBAoGAMmjmIJdjp+Ez8duyn3ieo36yrttF5NSsJLAbxFpdlc1gvtGCWW+9Cq0b
dxviW8+TFVEBl1O4f7HVm6EpTscdDxU+bCXWkfjuRb7Dy9GOtt9JPsX8MBTakzh3
vBgsyi/sN3RqRBcGU40fOoZyfAMT8s1m/uYv52O6IgeuZ/ujbjY=
-----END RSA PRIVATE KEY-----

# --------------------------------------------------------------------------------------------- #

## Level 18

*Goal*: There are 2 files in the homedirectory: passwords.old and passwords.new. The password for the next level is in passwords.new and is the only line that has been changed between passwords.old and passwords.new

*Commands:*
- `diff`: Compare files line by line.

*Steps:*
1. `man diff`
2. `diff passwords.old passwords.new`

*Answer:*
x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO

# --------------------------------------------------------------------------------------------- #

## Level 19

*Goal:* The password for the next level is stored in a file readme in the homedirectory. Unfortunately, someone has modified .bashrc to log you out when you log in with SSH.

*Steps:*
1. `ssh bandit18@bandit.labs.overthewire.org -p 2220 'cat ~/readme'`. Password is previous answer

*Answer:*
cGWpMaKXVwDUNgPAVJbWYuGHVn9zl3j8

# --------------------------------------------------------------------------------------------- #

## Level 20

*Goal:* To gain access to the next level, you should use the setuid binary in the homedirectory. Execute it without arguments to find out how to use it. The password for this level can be found in the usual place (/etc/bandit_pass), after you have used the setuid binary.

*Steps:*
1. `./bandit20-do cat /etc/bandit_pass/bandit20`. ./bandit20-do is similar sudo; programmed to do one thing with root privileges.
2. `man ./bandit20-do` returns mostly binary gibbersh.

*Answer:*
0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO
