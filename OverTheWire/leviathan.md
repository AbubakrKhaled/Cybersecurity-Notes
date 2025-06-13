# OverTheWire: Bandit Walkthrough
### My goal is to improve my Linux and CLI skills

# ------------------------------------------------------------------------------ #

## Level 0

*Goal:* SSH into leviathan server

*Steps:*
1. `ssh leviathan0@leviathan.labs.overthewire.org -p 2223`. password is leviathan0.

*Answer:*
leviathan0

# ------------------------------------------------------------------------------ #

## Level 1

*Steps:*
1. `ls -la`
2. `cd ./backup`
3. `ls -a`
4. `cat bookmarks.html | grep leviathan`

*Answer:*
3QJ3TgzHDq

# ------------------------------------------------------------------------------ #

## Level 2

*Steps:*
1. `ls -la`
2. `file check`. it is a setuid file
3. `./check`.
```bash
password: 3QJ3TgzHDq
Wrong password, Good Bye ...
```
I tried the password of the previous level and it is wrong.
4. `strings check` gives all the strings in the file
5. `ltrace ./check`. input test as password. Output:
```bash
__libc_start_main(0x80490ed, 1, 0xffffd494, 0 <unfinished ...>
printf("password: ")                                                      = 10
getchar(0, 0, 0x786573, 0x646f67password: test
)                                         = 116
getchar(0, 116, 0x786573, 0x646f67)                                       = 101
getchar(0, 0x6574, 0x786573, 0x646f67)                                    = 115
strcmp("tes", "sex")                                                      = 1
puts("Wrong password, Good Bye ..."Wrong password, Good Bye ...
)                                      = 29
+++ exited (status 0) +++
```
6. password is `sex`. 
7. I am now in a shell as leviathan2. `cat /etc/leviathan_pass/leviathan2`.
8. `r2 ./check`
9. `VV` open graphical view
10. `sexsecrretgodlove` is also a password.

*Answer:*
NsN1HwFoyN

# ------------------------------------------------------------------------------ #

## Level 3

*Steps:*
1. `./printfile`. Output:
```bash
*** File Printer ***
Usage: ./printfile filename
```
2. tried `r2`. didn't find anything
3. `./printfile /etc/leviathan_pass/leviathan3`. Output: You cant have that file...
