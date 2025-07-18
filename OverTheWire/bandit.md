# OverTheWire: Bandit Walkthrough
### My goal is to improve my Linux and CLI skills

# ------------------------------------------------------------------------------ #

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

# ------------------------------------------------------------------------------ #

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

# ------------------------------------------------------------------------------ #

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

# ------------------------------------------------------------------------------ #

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

# ------------------------------------------------------------------------------ #

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

# ------------------------------------------------------------------------------ #

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

# ------------------------------------------------------------------------------ #

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

# ------------------------------------------------------------------------------ #

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

# ------------------------------------------------------------------------------ #

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

# ------------------------------------------------------------------------------ #

## Level 9
*Goal:* The password for the next level is stored in the file data.txt and is the only line of text that occurs only once

*Commands:*
- `uniq`: detects duplicates (REQUIRES sorted input)
    - `-u`: prints unique lines ONLY
- `sort`: sorts alphabetically
    - `-n`: sorts numerically ascending
    - `-u`: remove duplicates; still prints one of every line.
- `|`: takes output of command on the left and uses it as input for command on the right. **VERY IMPORTANT**
- `>`: redirects the output of a command to a file. If the file has text, it is overwritten. **VERY IMPORTANT**
- `>>`:  redirects the output of a command to a file. If the file has text, it is **NOT** overwritten. **VERY IMPORTANT**

*Steps:*
1. `cat data.txt`. Printed many lines of letters
2. `man sort` and `man uniq`. 
3. `sort data.txt | uniq -u`. Success.

*Answer:*
4CKMh1JI91bUIZZPXDqGanal4xvAg0JM

# ------------------------------------------------------------------------------ #

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

# ------------------------------------------------------------------------------ #

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

# ------------------------------------------------------------------------------ #

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

# ------------------------------------------------------------------------------ #

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

# ------------------------------------------------------------------------------ #

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

# ------------------------------------------------------------------------------ #

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

# ------------------------------------------------------------------------------ #

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

# ------------------------------------------------------------------------------ #

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

# ------------------------------------------------------------------------------ #

## Level 18

*Goal*: There are 2 files in the homedirectory: passwords.old and passwords.new. The password for the next level is in passwords.new and is the only line that has been changed between passwords.old and passwords.new

*Commands:*
- `diff`: Compare files line by line.

*Steps:*
1. `man diff`
2. `diff passwords.old passwords.new`

*Answer:*
x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO

# ------------------------------------------------------------------------------ #

## Level 19

*Goal:* The password for the next level is stored in a file readme in the homedirectory. Unfortunately, someone has modified .bashrc to log you out when you log in with SSH.

*Steps:*
1. `ssh bandit18@bandit.labs.overthewire.org -p 2220 'cat ~/readme'`. Password is previous answer

*Answer:*
cGWpMaKXVwDUNgPAVJbWYuGHVn9zl3j8

# ------------------------------------------------------------------------------ #

## Level 20

*Goal:* To gain access to the next level, you should use the setuid binary in the homedirectory. Execute it without arguments to find out how to use it. The password for this level can be found in the usual place (/etc/bandit_pass), after you have used the setuid binary.

*Steps:*
1. `./bandit20-do cat /etc/bandit_pass/bandit20`. ./bandit20-do is similar to sudo; programmed to do a specific action with root privileges.
2. `man ./bandit20-do` returns mostly binary gibbersh.

*Answer:*
0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO

# ------------------------------------------------------------------------------ #

## Level 21

*Goal:* There is a setuid binary in the homedirectory that does the following: it makes a connection to localhost on the port you specify as a commandline argument. It then reads a line of text from the connection and compares it to the password in the previous level (bandit20). If the password is correct, it will transmit the password for the next level (bandit21).

*Commands:*
- `nc`: Netcat is used for just about anything involving TCP, UDP, or Unix-domain sockets.
    - `-l`: listen rather than intiate connection to remote host
    - `-p`: port
- `&`: send process to the background

*Steps:*
1. `echo 0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO | nc -l -p 1234 &`. The password is sent to nc. nc is listening for a connection
2. `./suconnect 1234`. Makes the connection that nc listens to.

*Answer:*
EeoULMCra2q0dSkYj561DX7s1CpBuOBt

# ------------------------------------------------------------------------------ #

## Level 22

*Goal:* A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.

*Commands:*
- `cron`: daemon to execute scheduled commands
- `crontab`: A crontab file contains instructions to the cron(8) daemon of the general form: 'run this command at this time on this date'. 

*Steps:*
1. `cd /etc/cron.d`
2. `ls -a`. 
3. `cat cronjob_bandit22`. Output: 
```bash
@reboot bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
* * * * * bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
```
4. `cat /usr/bin/cronjob_bandit22.sh`. Output: 
```bash
#!/bin/bash
chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
```
So the .sh script allows my to read the file.
5. `cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv`. Success

*Answer:*
tRae0UfB9v0UzbCdn9cY0gQnds9GF58Q

# ------------------------------------------------------------------------------ #

## Level 23

*Goal:* A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.

*Steps:*
1. `cd /etc/cron.d`
2. `ls -a`. 
3. `cat cronjob_bandit23`. Output: 
```bash
@reboot bandit23 /usr/bin/cronjob_bandit23.sh &> /dev/null
* * * * * bandit23 /usr/bin/cronjob_bandit23.sh &> /dev/null
```
4. `cat /usr/bin/cronjob_bandit23.sh`. To see what the script does. Output:
```bash
#!/bin/bash

myname=$(whoami)
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)

echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"

cat /etc/bandit_pass/$myname > /tmp/$mytarget
```
5. Problem is I am bandit22 currently, so the password that appears after running the script is the password for bandit22. Run the code myself, and switch any [myname] variable with bandit23.
6. I tried using `nano` to edit the bash script, but it says I don't have the permissions.
7. `echo I am user bandit23 | md5sum | cut -d ' ' -f 1`. Output: 8ca319486bfbbc3663ea0fbe81326349
8. `cat /tmp/8ca319486bfbbc3663ea0fbe81326349`. Success.

*Answer:*
0Zf11ioIjMVN551jX3CmStKLYqjk54Ga

# ------------------------------------------------------------------------------ #

## Level 24

*Goal:* A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.

*Commands*:
- `nano <filename>`: text editor
- `touch <filename>`: create a file

*Steps:*
1. `mktemp -d`
2. `cd /tmp/tmp.9O8cvA27Ht`
3. `touch s1.sh`
4. `nano s1.sh`. I then wrote this script:
```bash
#!/bin/bash
target=$(echo I am user bandit24 | md5sum | cut -d ' ' -f 1)
cat /etc/bandit_pass/bandit24 > /tmp/$target
cat /tmp/$target
```
5. `bash s1.sh`. To run script. Success.

*Answer:*
gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8

# ------------------------------------------------------------------------------ #

## Level 25

*Goal:* A daemon is listening on port 30002 and will give you the password for bandit25 if given the password for bandit24 and a secret numeric 4-digit pincode. There is no way to retrieve the pincode except by going through all of the 10000 combinations, called brute-forcing.
You do not need to create new connections each time

*Commands:*
- `for` loop syntax:
```bash
for var in (<number range>)
do
    #process
done
```
- `$`: represents a command I write into the cli.
- `grep -v <pattern> <file>`: It shows all lines that do **NOT** match a given pattern.
    - `-n`: prints line numbers: to find

*Steps:*
1. Test response first. 
```bash
$ echo "gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8 0000" | nc localhost 30002
I am the pincode checker for user bandit25. Please enter the password for user bandit24 and the secret pincode on a single line, separated by a space.
Wrong! Please enter the correct current password and pincode. Try again.
```
2. So, I have to write a script using a for loop that goes from 0000 to 9999. The response of the daemon will be different when given the correct pin. So, I have to write all responses into a file, and then use the `uniq -u` and `sort` (level 9).
3. However, this would require only recording responses, and I will not know the pin. Therefore, I will use `grep` to detect any line with [Wrong!] and remove it. Then, print with line number. PIN = line number - 1. HOWEVER, when you first connect using nc, there is the server's intial prompt, so it is PIN = line number - 2.
3. `mktemp -d` then `touch pinBruteForce.sh`
4. `nano pinBruteForce.sh` 
```bash
#!/bin/bash
for i in {0000..9999}
do
    echo "gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8 $i" >> possibilities.txt
done
cat possibilities.txt | nc localhost 30002 > result.txt
grep -v -n "Wrong" result.txt
```
5. Output:
```bash
2221:Correct!
2222:The password of user bandit25 is iCi86ttT4KSNe1armKiwbQNmB3YJP3q4
```
PIN number is therefore 2219. 

*Answer:*
iCi86ttT4KSNe1armKiwbQNmB3YJP3q4

# ------------------------------------------------------------------------------ #

## Level 26

*Goal:* Logging in to bandit26 from bandit25 should be fairly easy… The shell for user bandit26 is not /bin/bash, but something else. Find out what it is, how it works and how to break out of it.

*Concepts:*
- `shell`: The 'window' a user uses to talk to the computer. The computer remembers which shell each user uses in a directory called `/etc/passwd`.

*Commands:*
- `more`: shows big files one page at a time. q = quit. space = go to next page. if file is smaller than a page, shows whole file.
- `Vim`: a text editor. Can be entered using the more command then pressing `v`
    - `:`: used to enter command-line mode in vim
    - `i`: insert before cursor
    - `:w`: save file (write)
    - `:q`: quit


*Steps:*
1. `cat /etc/passwd | grep bandit26` Check bandit26's shell. Output:
`bandit26:x:11026:11026:bandit level 26:/home/bandit26:/usr/bin/showtext`
2. `cat /usr/bin/showtext`. See the script that opens the shell. Output:
```bash
#!/bin/sh

export TERM=linux

exec more ~/text.txt
exit 0
```
3. `ls` in home directory shows a private ssh key. Tried `ssh -i bandit26.sshkey bandit26@localhost -p 2220`. Disconnects instantly. It is likely that the text in 'text.txt' is short, and more displays all of it on one page. 
4. In vim, `:shell` uses the users default shell. The default shell for user bandit26 is /usr/bin/showtext, which doesn't work. Therefore, I use the command `:set shell=/bin/bash` to set the default shell to /bin/bash
5. Resize windown so that it is very small vertically. `ssh -i bandit26.sshkey bandit26@localhost -p 2220`. Press `v` to enter vim.
6. `:e /etc/bandit_pass/bandit26` for password
7. `:set shell=/bin/bash` then `:shell`. I am now bandit26

*Answer:*
s0773xxkk0MXfdqOfPRVr9L3jJBUOgCZ

# ------------------------------------------------------------------------------ #

## Level 27

*Goal:* Good job getting a shell! Now hurry and grab the password for bandit27!

*Steps:*
1. `ls`. bandit27-do
2. `cat bandit27-do`. A bunch of unreadable characters
3. `file bandit27-do`. Output: bandit27-do: setuid ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux.so.2, BuildID[sha1]=35d353cf6d732f515a73f50ed205265fe1e68f90, for GNU/Linux 3.2.0, not stripped
4. `./bandit27-do cat /etc/bandit_pass/bandit27`. Success. (Level 20)

*Answer:*
upsNCc7vzaRDx6oZC6GiR6ERwe1MowGB

# ------------------------------------------------------------------------------ #

## Level 28

*Goal:* There is a git repository at ssh://bandit27-git@localhost/home/bandit27-git/repo via the port 2220. The password for the user bandit27-git is the same as for the user bandit27. Clone the repository and find the password for the next level.

*Commands:*
- `git`: 
    - `clone`: copy a repository from a link.

*Steps:*
1. `ssh bandit27-git@localhost -p 2220`. Then prompted for password upsNCc7vzaRDx6oZC6GiR6ERwe1MowGB. However, it gives an error.
2. `git clone ssh://bandit27-git@localhost:2220/home/bandit27-git/repo`. Permission denied.
3. `cd $(mktemp -d)`. Then run above command again. Enter level 27 password. It works
4. `ls -a`. `cd repo/`. `cat README`.

*Answer:*
Yz9IpL0sBcCeuG7m9uQFt8ZNpS4HZRcN

# ------------------------------------------------------------------------------ #

## Level 29

*Goal:* There is a git repository at ssh://bandit28-git@localhost/home/bandit28-git/repo via the port 2220. The password for the user bandit28-git is the same as for the user bandit28. Clone the repository and find the password for the next level.

*Steps:*
1. `cd $(mktemp -d)`
2. `git clone ssh://bandit28-git@localhost:2220/home/bandit28-git/repo`. Enter level 28 password
3. `cd repo/`
4. `cat README`. Output:
```bash
# Bandit Notes
Some notes for level29 of bandit.

## credentials

- username: bandit29
- password: xxxxxxxxxx
```
5. `git log`. View previous commits. Output:
```bash
commit 674690a00a0056ab96048f7317b9ec20c057c06b (HEAD -> master, origin/master, origin/HEAD)
Author: Morla Porla <morla@overthewire.org>
Date:   Thu Apr 10 14:23:19 2025 +0000

    fix info leak

commit fb0df1358b1ff146f581651a84bae622353a71c0
Author: Morla Porla <morla@overthewire.org>
Date:   Thu Apr 10 14:23:19 2025 +0000

    add missing data

commit a5fdc97aae2c6f0e6c1e722877a100f24bcaaa46
Author: Ben Dover <noone@overthewire.org>
Date:   Thu Apr 10 14:23:19 2025 +0000

    initial commit of README.md
```
6. `git show fb0df1358b1ff146f581651a84bae622353a71c0`
```bash
commit fb0df1358b1ff146f581651a84bae622353a71c0
Author: Morla Porla <morla@overthewire.org>
Date:   Thu Apr 10 14:23:19 2025 +0000

    add missing data

diff --git a/README.md b/README.md
index 7ba2d2f..d4e3b74 100644
--- a/README.md
+++ b/README.md
@@ -4,5 +4,5 @@ Some notes for level29 of bandit.
 ## credentials

 - username: bandit29
-- password: <TBD>
+- password: 4pT1t5DENaYuqnqvadYs1oE4QLCdjmJ7
```

*Answer:*
4pT1t5DENaYuqnqvadYs1oE4QLCdjmJ7

# ------------------------------------------------------------------------------ #

## Level 30

*Goal:* There is a git repository at ssh://bandit29-git@localhost/home/bandit29-git/repo via the port 2220. The password for the user bandit29-git is the same as for the user bandit29. Clone the repository and find the password for the next level.

*Commands:*
- `git checkout <branch>`: enter a branch. A branch is another version that 'splits' at some point from main/origin.
    - `git branch -a`: list all branches

*Steps:*
1. `cd $(mktemp -d)`
2. `git clone ssh://bandit29-git@localhost:2220/home/bandit29-git/repo`. Enter level 29 password
3. `cd repo/`
4. `cat README`. Output:
```bash
# Bandit Notes
Some notes for bandit30 of bandit.

## credentials

- username: bandit30
- password: <no passwords in production!>
```
5. `git log` doesn't contain the answer 
6. `git branch -a`. Output:
```bash
 master
  remotes/origin/HEAD -> origin/master
  remotes/origin/dev
  remotes/origin/master
  remotes/origin/sploits-dev
```
7. `git checkout master` and `git checkout HEAD`. Output: Your branch is up to date with 'origin/master'. No difference from main.
8. `git checkout sploits-dev`. The branch isn't up to date, but it also doesn't have the password. 
9. `git checkout dev` then `cat README`

*Answer:*
qp30ex3VLz5MDG1n91YowTv4Q8l7CDZL

# ------------------------------------------------------------------------------ #

## Level 31

*Goal:* There is a git repository at ssh://bandit30-git@localhost/home/bandit30-git/repo via the port 2220. The password for the user bandit30-git is the same as for the user bandit30. Clone the repository and find the password for the next level.

*Steps:*
1. `cd $(mktemp -d)`
2. `git clone ssh://bandit30-git@localhost:2220/home/bandit30-git/repo`. Enter level 30 password
3. `cd repo/`
4. `cat README`. Output:
```bash
just an epmty file... muahaha
```
5. `git log` and `git branch -a` don't contain the answer.
6. `git tag`. lists all tags. Output: `secret`
7. `git show secret`

*Answer:*
fb5S2xb7bRyFmAvQYQGEqsbhVyJqhnDy

# ------------------------------------------------------------------------------ #

## Level 32

*Goal:* There is a git repository at ssh://bandit31-git@localhost/home/bandit31-git/repo via the port 2220. The password for the user bandit31-git is the same as for the user bandit31. Clone the repository and find the password for the next level.

*Steps:*
1. `cd $(mktemp -d)`
2. `git clone ssh://bandit31-git@localhost:2220/home/bandit31-git/repo`. Enter level 31 password
3. `cd repo/`
4. `cat README`. Output:
```bash
This time your task is to push a file to the remote repository.

Details:
    File name: key.txt
    Content: 'May I come in?'
    Branch: master
```
5. `echo "May I come in?" > key.txt`
6. `git add key.txt -f`. -f to force
7. `git commit -m "key.txt added"`.

*Answer:*
3O9RfhqyAlVBEZpVb6LYStshZoqoSx5K

# ------------------------------------------------------------------------------ #

## Level 33

*Goal:* After all this git stuff, it’s time for another escape. Good luck!

*Concepts:*
- `Local variables:` You can only use them when you create them
- `Shell variables:` Made by the shell for you to use
- `Environment variables:` You and any tool you run can use them. Usually in uppercase.
- `echo $0`: tells you what shell you are using

*Steps:*
1. WELCOME TO THE UPPERCASE SHELL. Any command I write it converted to uppercase and says permission denied.
2. 
```bash
>> ls -la
sh: 1: LS: Permission denied
>> man
sh: 1: MAN: Permission denied
>> sh
sh: 1: SH: Permission denied
>> echo 'a'
sh: 1: ECHO: Permission denied
```
3. `$0` escapes into normal shell.
4. `whoami`. bandit33
5. `cat /etc/bandit_pass/bandit33`

*Answer:*
tQdtbs5D5i2vJwkO8mEyYEyTL8izoeJ0


