
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
ctrl + r    # redo
e           # jump forwards to the end of a word
ge          # jump backward to the end of a word
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



R       W       X

2^2     2^1     2^0

chmod 724 file > user: RWX, group: W, Other: R



# umask denfine default permision for a file, umask apply to a directory and after that any file we create on that directory got the umask permision


R       W       X
0       0       0


R       -       -
0       2^1     2^0     >> 3

umask 113 dirctory_1   >>   RW-RW-R--

# for security reasons it cannot be set x permision with umask





# Special Permission: 
SUID
SGID
StickyBit


```

# modules 
```
lsmod                               # print all modules which already added to kernel
modprobe module_name                # add module and requiremnt modules to kernel
modprobe -r module_name             # remove a modlue and all dependencies
insmod module_name                  # only add module and can't load requirement modules
rmmod module_name                   # remove a module from a kernel
modinfo module_name                 # get info of modules

# if you want to permenent loaded modules, add modules to "/etc/modules-load.d"


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
# we have two option for finding a file in linux: 1-locate, 2-find
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

# comparing files

```

# we have three way to compare files in linux: 1-cmp, 2-diff, 3-sha256sum or other hash function

cmp file1 file2
echo $? # if return 1, it means not eqal
        # if return 0, it means eqal

sha256sum a b   

diff file1 file2            # diff only used for files
diff -B file1 file2         # ignore blank line
diff -w a b                 # ignore space
diff -i a b                 # case insensitive
diff -y a b                 # pretty output




```


## hard link and soft link

```
ln file1.txt file2.txt          # hard link

# hard link only used for files

ls -s file1.txt file2.txt       # soft link


```


# Process management

```

ps
ps -e
ps -f
ps -ef

ps -f -u root           # only show root process

ps aux --sort=%mem
ps aux --sort=-%cpu


pgrep python3
pgrep sshd                  # get process id

pgrep -l sshd               # get process id with service name





pidof sshd
pidof chrome



nohub ./app.sh                          # or we can use tmux


```

## how to secure ssh

```

1 - change default port from 22 to something else
2 - disable root login
3 - allowed only required user to connect to server via ssh
4 - enable iptables from specific IP
5 - use ssh version2
6 - ClientAliveInterval 300 and clientAliveCountMax 0


```
# tar and zip

```
tar -cvf backup.tar file1 file2 file3                   # tar the files
tar -tvf backup.tar                                     # only show which file is in the backup.tar
tar -xvf backup.tar                                     # untar the file


tar -zcvf backup.tar.gz file2 file3
tar -zxvf backup.tar.gz


```

# compression
[refrence](https://www.rootusers.com/gzip-vs-bzip2-vs-xz-performance-comparison/) for comparing these three method for compress files.
**tip**: before compres any file, first use tar and then use compres method
```
gzip -c file.tar > file.tar.gz                  # compression rate 60~70 %  > 100GB > 40 ~ 30 GB
gzip -d file.tar.gz                             # uncompress the file


sudo apt install bzip2
bzip2 file.tar
bzip2 -d file.tar.bz2


xz file.tar
xz -d file.xz

```

# SCP (Secure COPY)
```
scp -p 22 file.tar user@172.16.2.2:/home/user/backup/                           # for scp a file or tar file
scp -r -p 22 dirctory user@172.16.2.2:/home/user/backup/                        # for scp a dirctory

scp user@172.16.2.2:/home/user/backup/backup.tar user1@172.16.2.4:/home/user1/backup/   # scp from a server to another server

# you can use WinSCP for transfer file between linux and Windows 




sudo rsync -av /etc/ /home/user/etc-backup              # if target directory does not exist, rsync will be create for us



```


# Nmap
nmap is a network discovery and security auditing tool, there are a variety scans that can be perform nmap, TCP SYN scan is the default and most popular scan option for good reason

```
sudo apt install nmap  # if you want to install nmap in windows you can install zenmap which is a GUI for nmap

sudo nmap IP_address
sudo nmap -sS IP_address

# by default nmap scan most common 1000 port for each protocol
nmap -p 22,50000 -sV IP_address

# if you want to sacn all port
nmap -p- IP_address


# for UDP scan
namp -sU localhost


```
# lvm (logical volume management)
[ref](https://www.tecmint.com/create-lvm-storage-in-linux/)
```
fdisk -l
df -TH
# fdisk /dev/sdb
# note that change partion type to linux lvm(8e) 

pvcreate /dev/sdb1
pvs # list pv

vgcreate myvg /dev/sdb1
vgextend myvg /dev/sdc2
vgextend myvg /dev/sdc3

vgs # list vg


lvcreate -L +4G myvg --name mylv01
lvs  # list lv

mkfs.ext4 /dev/mapper/myvg-mylv01
mount /dev/mapper/myvg-mylv01 /test

lsblk
df -TH
blkid


partprobe   # 


--------------------------------
# for extend a lv
vgs
lvextend -L +3G /dev/mapper/myvg-mylv01

resize2fs /dev/mapper/myvg-mylv01	# if file system is ext2, ext3, ext4
xfs_growfs /dev/mapper/myvg-mylv01	# if file system is xfs


lvdisplay
vgdisplay


----------------------------
# remove lv
umount /LVM
lvremove /dev/mapper/myvg-lv01
vgremove myvg

pvremove /dev/sdb1
pvremove /dev/sdb2
pvremove /dev/sdc1
pvremove /dev/sdc2



-----------------------------
# full example1

pvcreate /dev/sdb1
pvcreate /dev/sdb2
pvcreate /dev/sdc1
pvcreate /dev/sdc2


vgcreate myvg /dev/sdb1
vgextend myvg /dev/sdb2
vgextend myvg /dev/sdc1
vgextend myvg /dev/sdc2
vgs


lvcreate -l 74%free myvg --name lv01

mkfs.ntfs /dev/mapper/myvg-lv01

mount /dev/mapper/myvg-lv01 /LVM



---------------------------------
# full example2

pvcreate /dev/sdb1
pvcreate /dev/sdb2
pvcreate /dev/sdc1
pvcreate /dev/sdc2

vgcreate -s 32M myvg /dev/sdb1 /dev/sdb2 /dev/sdc1 /dev/sdc2 # PE size 32MB


bc      # terminal calculator

lvcreate -L 6G -n mylv1 myvg
lvcreate -L 6G -n mylv2 myvg
lvcreate -L 6G -n mylv3 myvg

# format lvs

mkfs.ext4 /dev/myvg/mylv1
mkfs.ext4 /dev/myvg/mylv2
mkfs.ext4 /dev/myvg/mylv3


mount /dev/myvg/mylv1 /LVM1
mount /dev/myvg/mylv2 /LVM2
mount /dev/myvg/mylv3 /LVM3

df -TH


cat /etc/mtab

# copy and paste in /etc/fstab, change rw to defaults
/dev/mapper/myvg-mylv1 /LVM1 ext4 defaults 0 0
/dev/mapper/myvg-mylv2 /LVM2 ext4 defaults 0 0
/dev/mapper/myvg-mylv3 /LVM3 ext4 defaults 0 0


 mount -av

pvscan

vgdisplay
lvextend -l +331 /dev/myvg/mylv1  # +331 is PE size
resize2fs /dev/myvg/mylv1
lvdisplay
df -TH
lsblk
```

## Reducing Logical Volume (LVM)

* Before starting, it is always good to backup the data, so that it will not be a headache if something goes wrong.
* To Reduce a logical volume there are 5 steps needed to be done very carefully.
* While extending a volume we can extend it while the volume under mount status (online), but for reduce we must need to unmount the file system before reducing.

Let’s wee what are the 5 steps below: \
* unmount the file system for reducing
* Check the file system after unmount.
* Reduce the file system
* Reduce the Logical Volume size than Current size
* Recheck the file system for error
* Remount the file-system back to stage

```

# reduse mylv01

df -TH
umount -v /LVM1
e2fsck -ff /dev/myvg/mylv1
resize2fs /dev/myvg/mylv1 10G
lvreduce -L -6G /dev/myvg/mylv1
resize2fs /dev/myvg/mylv1
mount -a
lvs 
vgs




























swapon -s               # show list of swaps
mount -a                # apply changes in /etc/fstab



```