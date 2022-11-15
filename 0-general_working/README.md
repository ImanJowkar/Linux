
# vim
```
dd          # delete a line 
d5          # delete line 5
u           # undo change
gg          # go to the begining of the file
G           # go to the end of the file
shift+A     # go to the end of a specefic line
0           # go to the begining of a specific line
5G          # go to line 5
```

#### vim option

```
:set number         # enable line number
:%s/foo/xxx         # find "foo" and replace with "xxx"

```


# User Permissions

```
adduser         # more intractive for create a user
deluser         # more intractive for delete a user
addgroup
delgroup



useradd         # low-level utilities for create user
userdel
groupadd
groupdel


usermod -aG group_name user_name    # for adding a user to a group
groups                              # show the list of groups which user are join to it





chown user:group file.txt   # change ownership of a file
chgrp group_name file.txt   # change group of a file


chmod u-x file.txt
chmod g-x file.txt
chmod o-x file.txt
chmod a-x file.txt


chmod u-rx file.txt
chmod g-rwx file.txt
chmod o+rw file.txt
chmod a-x file.txt

chmod g=rx file.txt








```


# file system

```
du -hs /etc/                         # how to size of /etc/ directory
stat /etc/passwd                     # give an information about the /etc/passwd file 


# tee
who | tee who.txt                      # show in terminal and save to a file
who | tee -a who.txt                   # show in terminal and append to a file



```

# finding file and directories

```
# we have to option for finding a file in linux: 1- locate, 2-find
# 'locate' is faster than a 'find', because it use a database which we need update it constantly


# locate
apt install mlocate                             # install locate
sudo updatedb                                   # update database

locate file_name
locate admin

locate -i file_name                             # it isn't case sensetive



# find ----> find search in realtime, therefor it is more slower than locate

find . -name file.txt                           # search "file.txt" in . directory
find . -iname file.txt                          # case insensetive
find . -name "file.*" -delete                   # find "file.*" and delete all of them
find /etc/ -name shadow                         # search for "shadow" in /etc/ directory


find /etc/ -type d                              # show all directory in /etc
find /etc/ -type d -maxdepth 2                  # show directory in /etc/ which have depth=2
find /etc/ -type d -maxdepth 2 -perm 755        # find by permision
find /etc/ -type d -size +100K -ls              # find the dirctory more than 100K size
find /etc/ -type d -size +10M -ls
find /etc/ -type f -size +5M -size -10M

find /var/ -type f -mtime 0 -ls                 # show file which modified in one day past
find /var/ -type f -mtime 1 -ls                 # show file which modified in two day past
find /var/ -type f -mmin -60 -ls                 # show file which modified in 60 minute past
find /var/ -type f -user iman -ls
find /etc/ -type f -not -group root -ls


```

# grep (text configurations)

```
grep iman /etc/passwd
grep "pattern" /etc/shadow
grep -i "SSH" /etc/ssh/ssh_config           # case insensitive

grep -in "ssh" /etc/..                      # case insensitive and show line number too

grep -v "pattern" /etc/..                   # inverted result
grep -c "pattern" /etc/...                  # sum of the result that match the condition

grep -A 3 -B 4 "pattern" /etc/..            # show 3 line after match and 4 line before match


```