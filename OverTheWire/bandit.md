# OverTheWire: Bandit Walkthrough
# My goal is to improve my Linux and CLI skills

# --------------------------------------------------------------------------------------------- #

## Level 0
*Goal:* connect to OTW's server using SSH

*Commands:* 
- `ssh <username@host>`: used to connect to a remote host.
    - `-port <port>`: designate a port
- `man <command>`: open the manual page of a command. Very important

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