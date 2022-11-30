# Linux Hardening

* Ensure physical Security.
* BIOS protection.
* Disable Booting from external media devices(such as CD/DVD and USB).
* Boot Loader protection
* Keep the OS update(only from trusted source)
* check the installed packages and remove the unnecessary packages
* Check the open ports and stop unnessary services (netstat)
* Enforce Password policy
* Do not use root account on regular basis

## secure ssh

```
vim /etc/ssh/sshd_config

# add or uncomment these lines
Port 2285

PermitRootLogin no

# before this add public-key
PasswordAuthentication no

AllowUsers kuber ali jack
----

iptables -A INPUT -p tcp --dport 2278 -s IP_Allowd -j ACCEPT
iptables -A INPUT -p tcp --dport 2278 -j DROP



```
* use non standard port
* disable direct root loging
* disable password authuntication and enable public key authuntication
* enable firewall
* user ssh version 2

## Secure bootloader(Grub2)
