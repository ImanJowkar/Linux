# install mail server on ubuntu 20.04(VERSION="20.04.5 LTS (Focal Fossa)")
[ref](https://www.youtube.com/watch?v=IzG_Rbq_LmY)
```
# add A and MX record in dns server

apt update
apt install apache2 apache2-utils
apt install mariadb-server mariadb-client

apt install mailutils



# apt -y install lsb-release apt-transport-https ca-certificates 

# apt -y install lsb-release apt-transport-https ca-certificates 

# wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg

# echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" | sudo tee /etc/apt/sources.list.d/php.list

# apt update



apt install php7.4 libapache2-mod-php7.4 php7.4-mysql php-net-ldap2 php-net-ldap3 php-imagick php7.4-common php7.4-gd php7.4-imap php7.4-json php7.4-curl php7.4-zip php7.4-xml php7.4-mbstring php7.4-bz2 php7.4-intl php7.4-gmp php-net-smtp php-mail-mime php-net-idna2


apt install postfix
systemctl status postfix


apt install dovecot-imapd dovecot-pop3d
systemctl status dovecot.service
systemctl restart dovecot.service

wget https://github.com/roundcube/roundcubemail/releases/download/1.4.8/roundcubemail-1.4.8-complete.tar.gz


tar -xvf roundcubemail-1.4.8-complete.tar.gz

mv roundcubemail-1.4.8 /var/www/html/roundcubemail
chown -R www-data:www-data /var/www/html/roundcubemail/
chmod 755 /var/www/html/roundcubemail/



# database configuration

mysql -u root
create database roundcube DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci;
CREATE USER roundcubeuser@localhost IDENTIFIED BY 'Pa$$w0rd';
GRANT ALL PRIVILEGES ON roundcube.* TO roundcubeuser@localhost;
flush privileges; 
quit;


mysql roundcube < /var/www/html/roundcubemail/SQL/mysql.initial.sql


vim /etc/apache2/sites-available/roundcube.conf
# add
======================
<VirtualHost *:80>
ServerName mail.kuber.local
DocumentRoot /var/www/html/roundcubemail/
ErrorLog ${APACHE_LOG_DIR}/roundcube_error.log
CustomLog ${APACHE_LOG_DIR}/roundcube_access.log combined

<Directory />
Options FollowSymLinks
AllowOverride All
</Directory>

<Directory /var/www/html/roundcubemail/>
Options FollowSymLinks MultiViews
AllowOverride All
Order allow,deny
allow from all

</Directory>

</VirtualHost>

=======================
systemctl reload apache2
a2ensite roundcube.conf

go to the browser: 
http://mail.kuber.local/roundcubemail/installer/


# after setting compeleted go to the: 
http://mail.kuber.local/roundcubemail

# after configuration compeleted, remove the installer for security purpses.



rm -rf /var/www/html/roundcubemail/installer/


```





# Apache Hardening

## Hide Apache Version and Operating System
```

vim /etc/apache2/conf-enabled/security.conf
# add
=======
ServerSignature Off 
ServerTokens Prod

=======
sudo systemctl reload apache2

```



## Disable Directory Listing and FollowSymLinks
```
# change the following line in /etc/apache2/apache2.conf
vim /etc/apache2/apache2.conf

<Directory /var/www/>
        Options -Indexes -FollowSymLinks
        AllowOverride None
        Require all granted
</Directory>

sudo systemctl reload apache2

```











## Secure Apache using mod_security and mod_evasive Modules

### Mod_security
```
# Acts as a firewall for web servers and applications, providing protection against brute force attacks
sudo apt install libapache2-mod-security2 -y
sudo systemctl restart apache2

```


### Mod_evasive
```
# Detects and provides protection against DDOS and HTTP brute force attacks
sudo apt install libapache2-mod-evasive -y
sudo systemctl restart apache2

```

## Limit Request Size

```


```
## Disable TRACE HTTP Request

```
vim /etc/apache2/apache2.conf

# add
TraceEnable off

systemctl restart apache2.service



```

# DNS
```

apt-get install resolvconf
vim /etc/resolvconf/resolv.conf.d/head

### add
nameserver 1.1.1.2
nameserver 1.0.0.2
systemctl enable --now resolvconf.service
systemctl restart resolvconf.service

```
