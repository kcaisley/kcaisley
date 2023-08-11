```bash
for i in *.pdf
do
    mv "$i" "`echo $i | sed 's/ Circuit Intuitions//'`"
done
```

## Navigation

ls - list directory contents
pwd - print name of current/working directory
cd - change working directory
pushd/popd - put working directory on a stack
file - determine file type
find - file search by name, size, location, etc
locate - find files by name
updatedb - update database for locate
which - locate a command
history - display bash command history

#GETTING HELP

whatis - display the on-line manual descriptions
apropos - search the manual page names and descriptions
man - an interface to the on-line reference manuals

## WORKING W/ FILES

mkdir - create a directory/make directories
touch - change file timestamps/create empty files
cp - copy files and directories
mv - move (rename) files
rm - remove files or directories
rmdir - remove empty directories

## TEXT FILES

cat - concatenate files and print on the standard output
more/less - file perusal filter for crt viewing
nano - command line text editor

## USERS

sudo - execute a command as superuser
su - change user ID or become another user
users - print the user names of users currently logged in
id - print real and effective user and group IDs

## CHANGING FILE PERMISSIONS

chmod - change permissions of a file

## KILLING PROGRAMS AND LOGGING OUT

Ctrl+C - kill a running command
killall - kill processes by name
exit - log out of bash

## USEFUL SHORTCUTS

Ctrl+D - signal bash that there is no more input
Ctrl+L - redraw the screen
Ctrl++ - make text bigger in terminal emulator
Ctrl+- - make text smaller in terminal emulator

## MORE

pidof
top
ps acx
df
du
pgrep
gotop
htop
uname -a or -s -o
ip addr
ip address show
ip address
pinging website gives ip address
date
cal
df
du
grep
find
lsblk
dd
lspci
file
UTF-8 vs ASCII
cat
less
locate
updatedb
grok
alias cp to cp -iv
ldd

nohup (prevents shell exit from 'hang up' the processes aka killing the processes it started)