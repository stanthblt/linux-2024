# TP3 Sécu : Linux Hardening

**Dans ce TP, vous allez renforcer la sécurité d'un OS Linux.**

## Sommaire

- [TP3 Sécu : Linux Hardening](#tp3-sécu--linux-hardening)
  - [Sommaire](#sommaire)
  - [0. Setup](#0-setup)
  - [1. Guides CIS](#1-guides-cis)
  - [2. Conf SSH](#2-conf-ssh)
  - [4. DoT](#4-dot)
  - [5. AIDE](#5-aide)

## 0. Setup

Vous utiliserez une VM Rocky Linux pour dérouler ce TP.

## 1. Guides CIS

🌞 **Suivre un guide CIS**

```bash
[et0@TP3 ~]$ rpm -q autofs
package autofs is not installed
```
```bash
[et0@TP3 ~]$  rpm -q avahi
package avahi is not installed
```
```bash
[et0@TP3 ~]$ rpm -q dhcp-server
package dhcp-server is not installed
```
```bash
[et0@TP3 ~]$ rpm -q bind
package bind is not installed
```
```bash
[et0@TP3 ~]$ rpm -q dnsmasq
package dnsmasq is not installed
```
```bash
[et0@TP3 ~]$ rpm -q samba
package samba is not installed
```
```bash
[et0@TP3 ~]$ rpm -q vsftpd
package vsftpd is not installed
```
```bash
[et0@TP3 ~]$ rpm -q dovecot cyrus-imapd
package dovecot is not installed
package cyrus-imapd is not installed
```
```bash
[et0@TP3 ~]$ rpm -q nfs-utils
package nfs-utils is not installed
```
```bash
[et0@TP3 ~]$ rpm -q ypserv
package ypserv is not installed
```
```bash
[et0@TP3 ~]$ rpm -q cups
package cups is not installed
```
```bash
[et0@TP3 ~]$ rpm -q rpcbind
package rpcbind is not installed
```
```bash
[et0@TP3 ~]$ rpm -q rsync-daemon
package rsync-daemon is not installed
```
```bash
[et0@TP3 ~]$ rpm -q net-snmp
package net-snmp is not installed
```
```bash
[et0@TP3 ~]$ rpm -q tftp-server
package tftp-server is not installed
```
```bash
[et0@TP3 ~]$ rpm -q squid
package squid is not installed
```
```bash
[et0@TP3 ~]$ rpm -q httpd nginx
package httpd is not installed
package nginx is not installed
```
```bash
[et0@TP3 ~]$ rpm -q xinetd
package xinetd is not installed
```
```bash
[et0@TP3 ~]$ rpm -q xorg-x11-server-common
package xorg-x11-server-common is not installed
```
```bash
[et0@TP3 ~]$ ./script-2-1-21.sh 

- Audit Result:
 ** PASS **

 - Port "25" is not listening on a non-loopback network interface
 - Port "465" is not listening on a non-loopback network interface
 - Port "587" is not listening on a non-loopback network interface

```
```bash
[et0@TP3 ~]$ ss -plntu
Netid  State   Recv-Q  Send-Q   Local Address:Port   Peer Address:Port Process  
udp    UNCONN  0       0           127.0.0.54:53          0.0.0.0:*             
udp    UNCONN  0       0        127.0.0.53%lo:53          0.0.0.0:*             
udp    UNCONN  0       0              0.0.0.0:5355        0.0.0.0:*             
udp    UNCONN  0       0            127.0.0.1:323         0.0.0.0:*             
udp    UNCONN  0       0                 [::]:5355           [::]:*             
udp    UNCONN  0       0                [::1]:323            [::]:*             
tcp    LISTEN  0       4096     127.0.0.53%lo:53          0.0.0.0:*             
tcp    LISTEN  0       128            0.0.0.0:22          0.0.0.0:*             
tcp    LISTEN  0       4096        127.0.0.54:53          0.0.0.0:*             
tcp    LISTEN  0       4096           0.0.0.0:5355        0.0.0.0:*             
tcp    LISTEN  0       128               [::]:22             [::]:*             
tcp    LISTEN  0       4096              [::]:5355           [::]:*   
```
```bash
[et0@TP3 ~]$ ./script-3-1-1.sh 

- IPv6 is enabled

```
```bash
[et0@TP3 ~]$ ./script-3-1-2.sh 

- Audit Result:
 ** PASS **

 - System has no wireless NICs installed

```
```bash
[et0@TP3 ~]$  rpm -q bluez
package bluez is not installed
```
```bash
[et0@TP3 ~]$ ./script-3-2-1.sh

- Audit Result:
 ** PASS **
 - kernel module: "dccp" doesn't exist in "/usr/lib/modules/5.14.0-427.13.1.el9_4.aarch64/kernel/net"
 - kernel module: "dccp" doesn't exist in "/usr/lib/modules/5.14.0-503.21.1.el9_5.aarch64/kernel/net"

``` 
```bash
[et0@TP3 ~]$ ./script-3-2-2.sh 
./script-3-2-2.sh: line 34: /usr/lib/modules/5.14.0-427.13.1.el9_4.aarch64/kernel/net/tipc: Is a directory
./script-3-2-2.sh: line 34: /usr/lib/modules/5.14.0-503.21.1.el9_5.aarch64/kernel/net/tipc: Is a directory


 -- INFO --
 - module: "tipc" exists in:
 - "/usr/lib/modules/5.14.0-427.13.1.el9_4.aarch64/kernel/net"
 - "/usr/lib/modules/5.14.0-503.21.1.el9_5.aarch64/kernel/net"

- Audit Result:
 ** FAIL **
 - Reason(s) for audit failure:
 - kernel module: "tipc" is loadable
 - kernel module: "tipc" is not deny listed
- Correctly set:
 - kernel module: "tipc" is not loaded
```
```bash
[et0@TP3 ~]$ ./script-3-2-3.sh 

- Audit Result:
 ** PASS **
 - kernel module: "rds" doesn't exist in "/usr/lib/modules/5.14.0-427.13.1.el9_4.aarch64/kernel/net"
 - kernel module: "rds" doesn't exist in "/usr/lib/modules/5.14.0-503.21.1.el9_5.aarch64/kernel/net"
```
```bash
[et0@TP3 ~]$ ./script-3-2-4.sh 
./script-3-2-4.sh: line 34: /usr/lib/modules/5.14.0-427.13.1.el9_4.aarch64/kernel/net/sctp: Is a directory
./script-3-2-4.sh: line 34: /usr/lib/modules/5.14.0-503.21.1.el9_5.aarch64/kernel/net/sctp: Is a directory


 -- INFO --
 - module: "sctp" exists in:
 - "/usr/lib/modules/5.14.0-427.13.1.el9_4.aarch64/kernel/net"
 - "/usr/lib/modules/5.14.0-503.21.1.el9_5.aarch64/kernel/net"

- Audit Result:
 ** FAIL **
 - Reason(s) for audit failure:
 - kernel module: "sctp" is loadable
 - kernel module: "sctp" is not deny listed
- Correctly set:
 - kernel module: "sctp" is not loaded
```
```bash
[et0@TP3 ~]$ ./script-3-3-2.sh 

- Audit Result:
 ** FAIL **
 - Reason(s) for audit failure:

 - "net.ipv4.conf.all.send_redirects" is incorrectly set to "1" in the running configuration and should have a value of: "0"
 - "net.ipv4.conf.all.send_redirects" is not set in an included file
 ** Note: "net.ipv4.conf.all.send_redirects" May be set in a file that's ignored by load procedure **
 - "net.ipv4.conf.default.send_redirects" is incorrectly set to "1" in the running configuration and should have a value of: "0"
 - "net.ipv4.conf.default.send_redirects" is not set in an included file
 ** Note: "net.ipv4.conf.default.send_redirects" May be set in a file that's ignored by load procedure **

```
```bash
[et0@TP3 ~]$ sudo ./script-active.sh 
net.ipv4.conf.all.send_redirects = 0
net.ipv4.conf.default.send_redirects = 0
net.ipv4.route.flush = 1
```
```bash
[et0@TP3 ~]$ sudo ./script-3-3-3.sh 

- Audit Result:
 ** PASS **

 - "net.ipv4.icmp_ignore_bogus_error_responses" is correctly set to "1" in the running configuration

```
[et0@TP3 ~]$ ./script-3-3-4.sh 

- Audit Result:
 ** FAIL **
 - Reason(s) for audit failure:

 - "net.ipv4.icmp_echo_ignore_broadcasts" is not set in an included file
 ** Note: "net.ipv4.icmp_echo_ignore_broadcasts" May be set in a file that's ignored by load procedure **



- Correctly set:

 - "net.ipv4.icmp_echo_ignore_broadcasts" is correctly set to "1" in the running configuration

```
```bash
[et0@TP3 ~]$ ./script-3-3-5.sh 

- Audit Result:
 ** FAIL **
 - Reason(s) for audit failure:

 - "net.ipv4.conf.all.accept_redirects" is incorrectly set to "1" in the running configuration and should have a value of: "0"
 - "net.ipv4.conf.all.accept_redirects" is not set in an included file
 ** Note: "net.ipv4.conf.all.accept_redirects" May be set in a file that's ignored by load procedure **

 - "net.ipv4.conf.default.accept_redirects" is incorrectly set to "1" in the running configuration and should have a value of: "0"
 - "net.ipv4.conf.default.accept_redirects" is not set in an included file
 ** Note: "net.ipv4.conf.default.accept_redirects" May be set in a file that's ignored by load procedure **

 - "net.ipv6.conf.all.accept_redirects" is incorrectly set to "1" in the running configuration and should have a value of: "0"
 - "net.ipv6.conf.all.accept_redirects" is not set in an included file
 ** Note: "net.ipv6.conf.all.accept_redirects" May be set in a file that's ignored by load procedure **

 - "net.ipv6.conf.default.accept_redirects" is incorrectly set to "1" in the running configuration and should have a value of: "0"
 - "net.ipv6.conf.default.accept_redirects" is not set in an included file
 ** Note: "net.ipv6.conf.default.accept_redirects" May be set in a file that's ignored by load procedure **


```
```bash
[et0@TP3 ~]$ ./script-3-3-6.sh 

- Audit Result:
 ** FAIL **
 - Reason(s) for audit failure:

 - "net.ipv4.conf.all.secure_redirects" is incorrectly set to "1" in the running configuration and should have a value of: "0"
 - "net.ipv4.conf.all.secure_redirects" is not set in an included file
 ** Note: "net.ipv4.conf.all.secure_redirects" May be set in a file that's ignored by load procedure **

 - "net.ipv4.conf.default.secure_redirects" is incorrectly set to "1" in the running configuration and should have a value of: "0"
 - "net.ipv4.conf.default.secure_redirects" is not set in an included file
 ** Note: "net.ipv4.conf.default.secure_redirects" May be set in a file that's ignored by load procedure **


```
```bash
[et0@TP3 ~]$ ./script-3-3-7.sh 

- Audit Result:
 ** FAIL **
 - Reason(s) for audit failure:

 - "net.ipv4.conf.all.rp_filter" is incorrectly set to "0" in the running configuration and should have a value of: "1"
 - "net.ipv4.conf.all.rp_filter" is not set in an included file
 ** Note: "net.ipv4.conf.all.rp_filter" may be set in a file that's ignored by load procedure **



- Correctly set:

 - "net.ipv4.conf.default.rp_filter" is correctly set to "1" in the running configuration
 - "net.ipv4.conf.default.rp_filter" is correctly set to "1" in "/usr/lib/sysctl.d/50-redhat.conf"


```
```bash
[et0@TP3 ~]$ ./script-3-3-8.sh 

- Audit Result:
 ** FAIL **
 - Reason(s) for audit failure:

 - "net.ipv4.conf.all.accept_source_route" is not set in an included file
 ** Note: "net.ipv4.conf.all.accept_source_route" may be set in a file that's ignored by the load procedure **

 - "net.ipv4.conf.default.accept_source_route" is not set in an included file
 ** Note: "net.ipv4.conf.default.accept_source_route" may be set in a file that's ignored by the load procedure **

 - "net.ipv6.conf.all.accept_source_route" is not set in an included file
 ** Note: "net.ipv6.conf.all.accept_source_route" may be set in a file that's ignored by the load procedure **

 - "net.ipv6.conf.default.accept_source_route" is not set in an included file
 ** Note: "net.ipv6.conf.default.accept_source_route" may be set in a file that's ignored by the load procedure **



- Correctly set:

 - "net.ipv4.conf.all.accept_source_route" is correctly set to "0" in the running configuration
 - "net.ipv4.conf.default.accept_source_route" is correctly set to "0" in the running configuration
 - "net.ipv6.conf.all.accept_source_route" is correctly set to "0" in the running configuration
 - "net.ipv6.conf.default.accept_source_route" is correctly set to "0" in the running configuration

```
```bash
[et0@TP3 ~]$ ./script-3-3-9.sh 

- Audit Result:
 ** FAIL **
 - Reason(s) for audit failure:

 - "net.ipv4.conf.all.log_martians" is incorrectly set to "0" in the running configuration and should have a value of: "1"
 - "net.ipv4.conf.all.log_martians" is not set in an included file
 ** Note: "net.ipv4.conf.all.log_martians" may be set in a file that's ignored by the load procedure **

 - "net.ipv4.conf.default.log_martians" is incorrectly set to "0" in the running configuration and should have a value of: "1"
 - "net.ipv4.conf.default.log_martians" is not set in an included file
 ** Note: "net.ipv4.conf.default.log_martians" may be set in a file that's ignored by the load procedure **


```
```bash
[et0@TP3 ~]$ ./script-3-3-10.sh 

- Audit Result:
 ** FAIL **
 - Reason(s) for audit failure:

 - "net.ipv4.tcp_syncookies" is not set in an included file
 ** Note: "net.ipv4.tcp_syncookies" May be set in a file that's ignored by load procedure **



- Correctly set:

 - "net.ipv4.tcp_syncookies" is correctly set to "1" in the running configuration

```
```bash
et0@TP3 ~]$ ./script-3-3-11.sh 

- Audit Result:
 ** FAIL **
 - Reason(s) for audit failure:

 - "net.ipv6.conf.all.accept_ra" is incorrectly set to "1" in the running configuration and should have a value of: "0"
 - "net.ipv6.conf.all.accept_ra" is not set in an included file
 ** Note: "net.ipv6.conf.all.accept_ra" May be set in a file that's ignored by load procedure **

 - "net.ipv6.conf.default.accept_ra" is incorrectly set to "1" in the running configuration and should have a value of: "0"
 - "net.ipv6.conf.default.accept_ra" is not set in an included file
 ** Note: "net.ipv6.conf.default.accept_ra" May be set in a file that's ignored by load procedure **


```
```bash
[et0@TP3 ~]$ sudo ./script-5-5-1.sh 

- Audit Result:
 *** PASS ***
- * Correctly set * :

 - File: "/etc/ssh/sshd_config":
 - Correct: mode (0600), owner (root), and group
owner (root) configured

```
```bash
[et0@TP3 ~]$ sudo ./script-5-1-2.sh 

- Audit Result:
 ** PASS **
 - * Correctly configured * :
 - File: "/etc/ssh/ssh_host_ed25519_key"
 - Correct: mode: "0640", owner:
"root", and group owner: "ssh_keys" configured
 - File: "/etc/ssh/ssh_host_ecdsa_key"
 - Correct: mode: "0640", owner:
"root", and group owner: "ssh_keys" configured
 - File: "/etc/ssh/ssh_host_rsa_key"
 - Correct: mode: "0640", owner:
"root", and group owner: "ssh_keys" configured
```
```bash
[et0@TP3 ~]$ sudo ./script-5-1-3.sh 

- Audit Result:
 ** PASS **
 - * Correctly configured * :
 - File: "/etc/ssh/ssh_host_ed25519_key.pub"
 - Correct: mode: "0644", owner:
"root", and group owner: "root" configured
 - File: "/etc/ssh/ssh_host_ecdsa_key.pub"
 - Correct: mode: "0644", owner:
"root", and group owner: "root" configured
 - File: "/etc/ssh/ssh_host_rsa_key.pub"
 - Correct: mode: "0644", owner:
"root", and group owner: "root" configured
```
```bash
[et0@TP3 ~]$ sudo sshd -T | grep -P -- '^ciphers\h+"?([^#\n\r]+,)?' | grep -P '3des|blowfish|cast128|aes(128|192|256)-cbc|arcfour(128|256)?|rijndael-cbc@lysator\.liu\.se|chacha20-poly1305@openssh\.com'
ciphers aes256-gcm@openssh.com,chacha20-poly1305@openssh.com,aes256-ctr,aes128-gcm@openssh.com,aes128-ctr
```
```bash
[et0@TP3 ~]$ sudo sshd -T | grep -P -- 'kexalgorithms\h+"?([^#\n\r]+,)?' | grep -P 'diffie-hellman-group1-sha1|diffie-hellman-group14-sha1|diffie-hellman-group-exchange-sha1'
```
```bash
[et0@TP3 ~]$ sudo sshd -T | grep -P -- 'macs\h+"?([^#\n\r]+,)?' | grep -P 'hmac-md5|hmac-md5-96|hmac-ripemd160|hmac-sha1-96|umac64@openssh\.com|hmac-md5-etm@openssh\.com|hmac-md5-96-etm@openssh\.com|hmac-ripemd160-etm@openssh\.com|hmacsha1-96-etm@openssh\.com|umac-64-etm@openssh\.com|umac-128-etm@openssh\.com'
macs hmac-sha2-256-etm@openssh.com,hmac-sha1-etm@openssh.com,umac-128-etm@openssh.com,hmac-sha2-512-etm@openssh.com,hmac-sha2-256,hmac-sha1,umac-128@openssh.com,hmac-sha2-512
```
```bash
[et0@TP3 ~]$ sudo sshd -T | grep -P -- '^\h*(allow|deny)(users|groups)\h+' | grep -P '\H+'
```
```bash
[et0@TP3 ~]$ sudo sshd -T | grep -P -- '^banner\h+' | grep -P '/\H+'
```
```bash
[et0@TP3 ~]$ sudo sshd -T | grep -P -- 'clientaliveinterval' | grep -P 'clientalivecountmax'
```
```bash
[et0@TP3 ~]$ sudo sshd -T | grep -i '^disableforwarding'
disableforwarding yes
```
```bash
[et0@TP3 ~]$ sudo  sshd -T | grep gssapiauthentication
gssapiauthentication no
```
```bash
[et0@TP3 ~]$ sudo sshd -T | grep hostbasedauthentication
hostbasedauthentication no
```
```bash
[et0@TP3 ~]$ sudo sshd -T | grep ignorerhosts
ignorerhosts yes
```
```bash
[et0@TP3 ~]$ sudo sshd -T | grep logingracetime
logingracetime 60
```
```bash
[et0@TP3 ~]$ sudo sshd -T | grep loglevel
loglevel INFO
```
```bash
[et0@TP3 ~]$ sudo sshd -T | grep maxauthtries
maxauthtries 4
```
```bash
[et0@TP3 ~]$ sudo sshd -T | awk '$1 ~ /^\s*maxstartups/{split($2, a, ":");{if(a[1] > 10 || a[2] > 30 || a[3] > 60) print $0}}'
maxstartups 10:30:100
```
```bash
[et0@TP3 ~]$ sudo  sshd -T | grep -i maxsessions
maxsessions 10
```
```bash
[et0@TP3 ~]$ sudo sshd -T | grep permitemptypasswords
permitemptypasswords no
```
```bash
[et0@TP3 ~]$ sudo sshd -T | grep permitrootlogin
permitrootlogin no
```
```bash
[et0@TP3 ~]$ sudo sshd -T | grep permituserenvironment
permituserenvironment no
```
```bash
[et0@TP3 ~]$ sudo sshd -T | grep -i usepam
usepam yes
```
7.1 System File Permissions
```bash
[et0@TP3 ~]$  stat -Lc 'Access: (%#a/%A) Uid: ( %u/ %U) Gid: ( %g/ %G)' /etc/passwd
Access: (0644/-rw-r--r--) Uid: ( 0/ root) Gid: ( 0/ root)
```
```bash
[et0@TP3 ~]$ stat -Lc 'Access: (%#a/%A) Uid: ( %u/ %U) Gid: ( %g/ %G)' /etc/passwd-
Access: (0644/-rw-r--r--) Uid: ( 0/ root) Gid: ( 0/ root)
```
```bash
[et0@TP3 ~]$ stat -Lc 'Access: (%#a/%A) Uid: ( %u/ %U) Gid: ( %g/ %G)' /etc/group
Access: (0644/-rw-r--r--) Uid: ( 0/ root) Gid: ( 0/ root)
```
```bash
[et0@TP3 ~]$ stat -Lc 'Access: (%#a/%A) Uid: ( %u/ %U) Gid: ( %g/ %G)' /etc/group-
Access: (0644/-rw-r--r--) Uid: ( 0/ root) Gid: ( 0/ root)
```
```bash
[et0@TP3 ~]$ stat -Lc 'Access: (%#a/%A) Uid: ( %u/ %U) Gid: ( %g/ %G)' /etc/shadow
Access: (0/----------) Uid: ( 0/ root) Gid: ( 0/ root)
```
```bash
[et0@TP3 ~]$ stat -Lc 'Access: (%#a/%A) Uid: ( %u/ %U) Gid: ( %g/ %G)' /etc/shadow-
Access: (0/----------) Uid: ( 0/ root) Gid: ( 0/ root)
```
```bash
[et0@TP3 ~]$ stat -Lc 'Access: (%#a/%A) Uid: ( %u/ %U) Gid: ( %g/ %G)' /etc/gshadow
Access: (0/----------) Uid: ( 0/ root) Gid: ( 0/ root)
```
```bash
[et0@TP3 ~]$ stat -Lc 'Access: (%#a/%A) Uid: ( %u/ %U) Gid: ( %g/ %G)' /etc/gshadow-
Access: (0/----------) Uid: ( 0/ root) Gid: ( 0/ root)
```
```bash
[et0@TP3 ~]$ stat -Lc 'Access: (%#a/%A) Uid: ( %u/ %U) Gid: ( %g/ %G)' /etc/shells
Access: (0644/-rw-r--r--) Uid: ( 0/ root) Gid: ( 0/ root)
```
```bash
[et0@TP3 ~]$ sudo [ -e "/etc/security/opasswd" ] && stat -Lc '%n Access: (%#a/%A) Uid: ( %u/ %U) Gid: ( %g/ %G)' /etc/security/opasswd
/etc/security/opasswd Access: (0600/-rw-------) Uid: ( 0/ root) Gid: ( 0/ root)
```
```bash
[et0@TP3 ~]$ sudo [ -e "/etc/security/opasswd.old" ] && stat -Lc '%n Access: (%#a/%A) Uid: ( %u/ %U) Gid: ( %g/ %G)' /etc/security/opasswd.old
```
10 points :
```bash
[et0@TP3 ~]$  stat -Lc 'Access: (%#a/%A) Uid: ( %u/ %U) Gid: ( %g/ %G)' /etc/shells
Access: (0644/-rw-r--r--) Uid: ( 0/ root) Gid: ( 0/ root)
```
```bash
[et0@TP3 ~]$ sudo awk -F: '($2 != "x" ) { print "User: \"" $1 "\" is not set to shadowed passwords "}' /etc/passwd
```
```bash
[et0@TP3 ~]$ sudo awk -F: '($2 == "" ) { print $1 " does not have a password "}' /etc/shadow
```
```bash
[et0@TP3 ~]$ ./script-7-2-3.sh 
```
```bash
[et0@TP3 ~]$ ./script-7-2-4.sh 
```
```bash
[et0@TP3 ~]$ ./script-7-2-5.sh 
```
```bash
[et0@TP3 ~]$ ./script-7-2-6.sh 
```
```bash
[et0@TP3 ~]$ ./script-7-2-7.sh
```
```bash
[et0@TP3 ~]$ ./script-7-2-8.sh 

- Audit Result:
 ** PASS **
 - * Correctly configured * :
 - All local interactive users:
 - home directories exist
 - own their home directory
 - home directories are mode: "750" or more restrictive
```
```bash
[et0@TP3 ~]$ sudo ./script-7-2-9.sh 
- Audit Result:
 ** FAIL **
 - * Reasons for audit failure * :
 - User: "et0" Home Directory: "/home/patron"
 - File: "/home/patron/.vscode-server/cli/servers/Stable-42b266171e51a016313f47d0c48aca9295b9cbb2/server/extensions/json-language-features/server/.npmrc" is mode: "0664" and should be mode: "644" or more restrictive
 - File: "/home/patron/.vscode-server/cli/servers/Stable-42b266171e51a016313f47d0c48aca9295b9cbb2/server/extensions/terminal-suggest/testWorkspace/parent/home/child/.keep" is mode: "0664" and should be mode: "644" or more restrictive
 - File: "/home/patron/.vscode-server/cli/servers/Stable-42b266171e51a016313f47d0c48aca9295b9cbb2/server/extensions/css-language-features/server/.npmrc" is mode: "0664" and should be mode: "644" or more restrictive
 - File: "/home/patron/.vscode-server/cli/servers/Stable-42b266171e51a016313f47d0c48aca9295b9cbb2/server/extensions/html-language-features/server/.npmrc" is mode: "0664" and should be mode: "644" or more restrictive
```

## 2. Conf SSH

🌞 **Chiffrement fort côté serveur**

https://infosec.mozilla.org/guidelines/openssh.html

```bash
[et0@localhost .ssh]$ sudo cat /etc/ssh/sshd_config

Include /etc/ssh/sshd_config.d/*.conf

KexAlgorithms curve25519-sha256

Ciphers aes256-gcm@openssh.com,chacha20-poly1305@openssh.com

MACs hmac-sha2-512,hmac-sha2-256

PermitRootLogin no

PubkeyAuthentication yes

AuthorizedKeysFile	.ssh/authorized_keys

PasswordAuthentication yes

Subsystem	sftp	/usr/libexec/openssh/sftp-server
```

🌞 **Clés de chiffrement fortes pour le client**

```bash
ssh-keygen -t ed25519
```

```bash
ssh-copy-id -i ~/.ssh/id_ed25519 et0@10.1.1.11
```

🌞 **Connectez-vous en SSH à votre VM avec cette paire de clés**

```bash
stan@Stanislass-MacBook-Pro-2 ~ % ssh -i ~/.ssh/id_ed25519 -vvvv et0@10.1.1.11     
OpenSSH_9.8p1, LibreSSL 3.3.6
debug1: Reading configuration data /Users/stan/.ssh/config
debug1: Reading configuration data /etc/ssh/ssh_config
debug1: /etc/ssh/ssh_config line 21: include /etc/ssh/ssh_config.d/* matched no files
debug1: /etc/ssh/ssh_config line 54: Applying options for *
debug2: resolve_canonicalize: hostname 10.1.1.11 is address
debug3: expanded UserKnownHostsFile '~/.ssh/known_hosts' -> '/Users/stan/.ssh/known_hosts'
debug3: expanded UserKnownHostsFile '~/.ssh/known_hosts2' -> '/Users/stan/.ssh/known_hosts2'
debug1: Authenticator provider $SSH_SK_PROVIDER did not resolve; disabling
debug3: channel_clear_timeouts: clearing
debug3: ssh_connect_direct: entering
debug1: Connecting to 10.1.1.11 [10.1.1.11] port 22.
debug3: set_sock_tos: set socket 3 IP_TOS 0x48
debug1: Connection established.
debug1: identity file /Users/stan/.ssh/id_ed25519 type 3
debug1: identity file /Users/stan/.ssh/id_ed25519-cert type -1
debug1: Local version string SSH-2.0-OpenSSH_9.8
debug1: Remote protocol version 2.0, remote software version OpenSSH_8.7
debug1: compat_banner: match: OpenSSH_8.7 pat OpenSSH* compat 0x04000000
debug2: fd 3 setting O_NONBLOCK
debug1: Authenticating to 10.1.1.11:22 as 'et0'
debug3: record_hostkey: found key type ED25519 in file /Users/stan/.ssh/known_hosts:35
debug3: load_hostkeys_file: loaded 1 keys from 10.1.1.11
debug1: load_hostkeys: fopen /Users/stan/.ssh/known_hosts2: No such file or directory
debug1: load_hostkeys: fopen /etc/ssh/ssh_known_hosts: No such file or directory
debug1: load_hostkeys: fopen /etc/ssh/ssh_known_hosts2: No such file or directory
debug3: order_hostkeyalgs: have matching best-preference key type ssh-ed25519-cert-v01@openssh.com, using HostkeyAlgorithms verbatim
debug3: send packet: type 20
debug1: SSH2_MSG_KEXINIT sent
debug3: receive packet: type 20
debug1: SSH2_MSG_KEXINIT received
debug2: local client KEXINIT proposal
debug2: KEX algorithms: sntrup761x25519-sha512@openssh.com,curve25519-sha256,curve25519-sha256@libssh.org,ecdh-sha2-nistp256,ecdh-sha2-nistp384,ecdh-sha2-nistp521,diffie-hellman-group-exchange-sha256,diffie-hellman-group16-sha512,diffie-hellman-group18-sha512,diffie-hellman-group14-sha256,ext-info-c,kex-strict-c-v00@openssh.com
debug2: host key algorithms: ssh-ed25519-cert-v01@openssh.com,ecdsa-sha2-nistp256-cert-v01@openssh.com,ecdsa-sha2-nistp384-cert-v01@openssh.com,ecdsa-sha2-nistp521-cert-v01@openssh.com,sk-ssh-ed25519-cert-v01@openssh.com,sk-ecdsa-sha2-nistp256-cert-v01@openssh.com,rsa-sha2-512-cert-v01@openssh.com,rsa-sha2-256-cert-v01@openssh.com,ssh-ed25519,ecdsa-sha2-nistp256,ecdsa-sha2-nistp384,ecdsa-sha2-nistp521,sk-ssh-ed25519@openssh.com,sk-ecdsa-sha2-nistp256@openssh.com,rsa-sha2-512,rsa-sha2-256
debug2: ciphers ctos: chacha20-poly1305@openssh.com,aes128-ctr,aes192-ctr,aes256-ctr,aes128-gcm@openssh.com,aes256-gcm@openssh.com
debug2: ciphers stoc: chacha20-poly1305@openssh.com,aes128-ctr,aes192-ctr,aes256-ctr,aes128-gcm@openssh.com,aes256-gcm@openssh.com
debug2: MACs ctos: umac-64-etm@openssh.com,umac-128-etm@openssh.com,hmac-sha2-256-etm@openssh.com,hmac-sha2-512-etm@openssh.com,hmac-sha1-etm@openssh.com,umac-64@openssh.com,umac-128@openssh.com,hmac-sha2-256,hmac-sha2-512,hmac-sha1
debug2: MACs stoc: umac-64-etm@openssh.com,umac-128-etm@openssh.com,hmac-sha2-256-etm@openssh.com,hmac-sha2-512-etm@openssh.com,hmac-sha1-etm@openssh.com,umac-64@openssh.com,umac-128@openssh.com,hmac-sha2-256,hmac-sha2-512,hmac-sha1
debug2: compression ctos: none,zlib@openssh.com,zlib
debug2: compression stoc: none,zlib@openssh.com,zlib
debug2: languages ctos: 
debug2: languages stoc: 
debug2: first_kex_follows 0 
debug2: reserved 0 
debug2: peer server KEXINIT proposal
debug2: KEX algorithms: curve25519-sha256,curve25519-sha256@libssh.org,ecdh-sha2-nistp256,ecdh-sha2-nistp384,ecdh-sha2-nistp521,diffie-hellman-group-exchange-sha256,diffie-hellman-group14-sha256,diffie-hellman-group16-sha512,diffie-hellman-group18-sha512,kex-strict-s-v00@openssh.com
debug2: host key algorithms: rsa-sha2-512,rsa-sha2-256,ecdsa-sha2-nistp256,ssh-ed25519
debug2: ciphers ctos: aes256-gcm@openssh.com,chacha20-poly1305@openssh.com,aes256-ctr,aes128-gcm@openssh.com,aes128-ctr
debug2: ciphers stoc: aes256-gcm@openssh.com,chacha20-poly1305@openssh.com,aes256-ctr,aes128-gcm@openssh.com,aes128-ctr
debug2: MACs ctos: hmac-sha2-256-etm@openssh.com,hmac-sha1-etm@openssh.com,umac-128-etm@openssh.com,hmac-sha2-512-etm@openssh.com,hmac-sha2-256,hmac-sha1,umac-128@openssh.com,hmac-sha2-512
debug2: MACs stoc: hmac-sha2-256-etm@openssh.com,hmac-sha1-etm@openssh.com,umac-128-etm@openssh.com,hmac-sha2-512-etm@openssh.com,hmac-sha2-256,hmac-sha1,umac-128@openssh.com,hmac-sha2-512
debug2: compression ctos: none,zlib@openssh.com
debug2: compression stoc: none,zlib@openssh.com
debug2: languages ctos: 
debug2: languages stoc: 
debug2: first_kex_follows 0 
debug2: reserved 0 
debug3: kex_choose_conf: will use strict KEX ordering
debug1: kex: algorithm: curve25519-sha256
debug1: kex: host key algorithm: ssh-ed25519
debug1: kex: server->client cipher: chacha20-poly1305@openssh.com MAC: <implicit> compression: none
debug1: kex: client->server cipher: chacha20-poly1305@openssh.com MAC: <implicit> compression: none
debug3: send packet: type 30
debug1: expecting SSH2_MSG_KEX_ECDH_REPLY
debug3: receive packet: type 31
debug1: SSH2_MSG_KEX_ECDH_REPLY received
debug1: Server host key: ssh-ed25519 SHA256:vdGGo+jSeXSU6PK2GdRf0U6zlx3iqfjliZNSvIOiAfk
debug3: record_hostkey: found key type ED25519 in file /Users/stan/.ssh/known_hosts:35
debug3: load_hostkeys_file: loaded 1 keys from 10.1.1.11
debug1: load_hostkeys: fopen /Users/stan/.ssh/known_hosts2: No such file or directory
debug1: load_hostkeys: fopen /etc/ssh/ssh_known_hosts: No such file or directory
debug1: load_hostkeys: fopen /etc/ssh/ssh_known_hosts2: No such file or directory
debug1: Host '10.1.1.11' is known and matches the ED25519 host key.
debug1: Found key in /Users/stan/.ssh/known_hosts:35
debug3: send packet: type 21
debug1: ssh_packet_send2_wrapped: resetting send seqnr 3
debug2: ssh_set_newkeys: mode 1
debug1: rekey out after 134217728 blocks
debug1: SSH2_MSG_NEWKEYS sent
debug1: expecting SSH2_MSG_NEWKEYS
debug3: receive packet: type 21
debug1: ssh_packet_read_poll2: resetting read seqnr 3
debug1: SSH2_MSG_NEWKEYS received
debug2: ssh_set_newkeys: mode 0
debug1: rekey in after 134217728 blocks
debug2: KEX algorithms: sntrup761x25519-sha512@openssh.com,curve25519-sha256,curve25519-sha256@libssh.org,ecdh-sha2-nistp256,ecdh-sha2-nistp384,ecdh-sha2-nistp521,diffie-hellman-group-exchange-sha256,diffie-hellman-group16-sha512,diffie-hellman-group18-sha512,diffie-hellman-group14-sha256,ext-info-c,kex-strict-c-v00@openssh.com
debug2: host key algorithms: ssh-ed25519-cert-v01@openssh.com,ecdsa-sha2-nistp256-cert-v01@openssh.com,ecdsa-sha2-nistp384-cert-v01@openssh.com,ecdsa-sha2-nistp521-cert-v01@openssh.com,sk-ssh-ed25519-cert-v01@openssh.com,sk-ecdsa-sha2-nistp256-cert-v01@openssh.com,rsa-sha2-512-cert-v01@openssh.com,rsa-sha2-256-cert-v01@openssh.com,ssh-ed25519,ecdsa-sha2-nistp256,ecdsa-sha2-nistp384,ecdsa-sha2-nistp521,sk-ssh-ed25519@openssh.com,sk-ecdsa-sha2-nistp256@openssh.com,rsa-sha2-512,rsa-sha2-256
debug2: ciphers ctos: chacha20-poly1305@openssh.com,aes128-ctr,aes192-ctr,aes256-ctr,aes128-gcm@openssh.com,aes256-gcm@openssh.com
debug2: ciphers stoc: chacha20-poly1305@openssh.com,aes128-ctr,aes192-ctr,aes256-ctr,aes128-gcm@openssh.com,aes256-gcm@openssh.com
debug2: MACs ctos: umac-64-etm@openssh.com,umac-128-etm@openssh.com,hmac-sha2-256-etm@openssh.com,hmac-sha2-512-etm@openssh.com,hmac-sha1-etm@openssh.com,umac-64@openssh.com,umac-128@openssh.com,hmac-sha2-256,hmac-sha2-512,hmac-sha1
debug2: MACs stoc: umac-64-etm@openssh.com,umac-128-etm@openssh.com,hmac-sha2-256-etm@openssh.com,hmac-sha2-512-etm@openssh.com,hmac-sha1-etm@openssh.com,umac-64@openssh.com,umac-128@openssh.com,hmac-sha2-256,hmac-sha2-512,hmac-sha1
debug2: compression ctos: none,zlib@openssh.com,zlib
debug2: compression stoc: none,zlib@openssh.com,zlib
debug2: languages ctos: 
debug2: languages stoc: 
debug2: first_kex_follows 0 
debug2: reserved 0 
debug3: send packet: type 5
debug3: receive packet: type 7
debug1: SSH2_MSG_EXT_INFO received
debug3: kex_input_ext_info: extension server-sig-algs
debug1: kex_ext_info_client_parse: server-sig-algs=<ssh-ed25519,sk-ssh-ed25519@openssh.com,ssh-rsa,rsa-sha2-256,rsa-sha2-512,ssh-dss,ecdsa-sha2-nistp256,ecdsa-sha2-nistp384,ecdsa-sha2-nistp521,sk-ecdsa-sha2-nistp256@openssh.com,webauthn-sk-ecdsa-sha2-nistp256@openssh.com>
debug3: receive packet: type 6
debug2: service_accept: ssh-userauth
debug1: SSH2_MSG_SERVICE_ACCEPT received
debug3: send packet: type 50
debug3: receive packet: type 51
debug1: Authentications that can continue: publickey,gssapi-keyex,gssapi-with-mic
debug3: start over, passed a different list publickey,gssapi-keyex,gssapi-with-mic
debug3: preferred publickey,keyboard-interactive,password
debug3: authmethod_lookup publickey
debug3: remaining preferred: keyboard-interactive,password
debug3: authmethod_is_enabled publickey
debug1: Next authentication method: publickey
debug3: ssh_get_authentication_socket_path: path '/private/tmp/com.apple.launchd.agHCsnyCGG/Listeners'
debug1: get_agent_identities: bound agent to hostkey
debug1: get_agent_identities: ssh_fetch_identitylist: agent contains no identities
debug1: Will attempt key: /Users/stan/.ssh/id_ed25519 ED25519 SHA256:1G8r0xB2cAJ2QpuZlWQ/VnJW3djhzj8MK9UepQURBt4 explicit
debug2: pubkey_prepare: done
debug1: Offering public key: /Users/stan/.ssh/id_ed25519 ED25519 SHA256:1G8r0xB2cAJ2QpuZlWQ/VnJW3djhzj8MK9UepQURBt4 explicit
debug3: send packet: type 50
debug2: we sent a publickey packet, wait for reply
debug3: receive packet: type 60
debug1: Server accepts key: /Users/stan/.ssh/id_ed25519 ED25519 SHA256:1G8r0xB2cAJ2QpuZlWQ/VnJW3djhzj8MK9UepQURBt4 explicit
debug3: sign_and_send_pubkey: using publickey with ED25519 SHA256:1G8r0xB2cAJ2QpuZlWQ/VnJW3djhzj8MK9UepQURBt4
debug3: sign_and_send_pubkey: signing using ssh-ed25519 SHA256:1G8r0xB2cAJ2QpuZlWQ/VnJW3djhzj8MK9UepQURBt4
debug3: send packet: type 50
debug3: receive packet: type 52
Authenticated to 10.1.1.11 ([10.1.1.11]:22) using "publickey".
debug1: channel 0: new session [client-session] (inactive timeout: 0)
debug3: ssh_session2_open: channel_new: 0
debug2: channel 0: send open
debug3: send packet: type 90
debug1: Requesting no-more-sessions@openssh.com
debug3: send packet: type 80
debug1: Entering interactive session.
debug1: pledge: filesystem
debug3: client_repledge: enter
debug3: receive packet: type 80
debug1: client_input_global_request: rtype hostkeys-00@openssh.com want_reply 0
debug3: client_input_hostkeys: received RSA key SHA256:nMuaNQwvSFVKmV5NfaRp+pFasTcc9SLF72EY9xD8YgQ
debug3: client_input_hostkeys: received ECDSA key SHA256:9q21Ilz8Xtux3JLzibyLogL7CG0sBvpWVXUvLZYi0f4
debug3: client_input_hostkeys: received ED25519 key SHA256:vdGGo+jSeXSU6PK2GdRf0U6zlx3iqfjliZNSvIOiAfk
debug1: client_input_hostkeys: searching /Users/stan/.ssh/known_hosts for 10.1.1.11 / (none)
debug3: hostkeys_foreach: reading file "/Users/stan/.ssh/known_hosts"
debug3: hostkeys_find: found ssh-ed25519 key under different name/addr at /Users/stan/.ssh/known_hosts:27
debug3: hostkeys_find: found ssh-rsa key under different name/addr at /Users/stan/.ssh/known_hosts:28
debug3: hostkeys_find: found ecdsa-sha2-nistp256 key under different name/addr at /Users/stan/.ssh/known_hosts:29
debug3: hostkeys_find: found ssh-ed25519 key under different name/addr at /Users/stan/.ssh/known_hosts:30
debug3: hostkeys_find: found ssh-ed25519 key under different name/addr at /Users/stan/.ssh/known_hosts:31
debug3: hostkeys_find: found ssh-ed25519 key under different name/addr at /Users/stan/.ssh/known_hosts:32
debug3: hostkeys_find: found ssh-ed25519 key under different name/addr at /Users/stan/.ssh/known_hosts:33
debug3: hostkeys_find: found ssh-ed25519 key under different name/addr at /Users/stan/.ssh/known_hosts:34
debug3: hostkeys_find: found ssh-ed25519 key at /Users/stan/.ssh/known_hosts:35
debug1: client_input_hostkeys: searching /Users/stan/.ssh/known_hosts2 for 10.1.1.11 / (none)
debug1: client_input_hostkeys: hostkeys file /Users/stan/.ssh/known_hosts2 does not exist
debug3: client_input_hostkeys: 3 server keys: 2 new, 18446744073709551615 retained, 2 incomplete match. 0 to remove
debug1: client_input_hostkeys: host key found matching a different name/address, skipping UserKnownHostsFile update
debug3: client_repledge: enter
debug3: receive packet: type 4
debug1: Remote: /home/patron/.ssh/authorized_keys:1: key options: agent-forwarding port-forwarding pty user-rc x11-forwarding
debug3: receive packet: type 4
debug1: Remote: /home/patron/.ssh/authorized_keys:1: key options: agent-forwarding port-forwarding pty user-rc x11-forwarding
debug3: receive packet: type 91
debug2: channel_input_open_confirmation: channel 0: callback start
debug2: fd 3 setting TCP_NODELAY
debug3: set_sock_tos: set socket 3 IP_TOS 0x48
debug2: client_session2_setup: id 0
debug2: channel 0: request pty-req confirm 1
debug3: send packet: type 98
debug1: Sending environment.
debug3: Ignored env __CFBundleIdentifier
debug3: Ignored env TMPDIR
debug3: Ignored env XPC_FLAGS
debug3: Ignored env TERM
debug3: Ignored env SSH_AUTH_SOCK
debug3: Ignored env XPC_SERVICE_NAME
debug3: Ignored env TERM_PROGRAM
debug3: Ignored env TERM_PROGRAM_VERSION
debug3: Ignored env TERM_SESSION_ID
debug3: Ignored env SHELL
debug3: Ignored env HOME
debug3: Ignored env LOGNAME
debug3: Ignored env USER
debug3: Ignored env PATH
debug3: Ignored env SHLVL
debug3: Ignored env PWD
debug3: Ignored env OLDPWD
debug3: Ignored env HOMEBREW_PREFIX
debug3: Ignored env HOMEBREW_CELLAR
debug3: Ignored env HOMEBREW_REPOSITORY
debug3: Ignored env INFOPATH
debug1: channel 0: setting env LC_CTYPE = "UTF-8"
debug2: channel 0: request env confirm 0
debug3: send packet: type 98
debug3: Ignored env _
debug3: Ignored env __CF_USER_TEXT_ENCODING
debug2: channel 0: request shell confirm 1
debug3: send packet: type 98
debug3: client_repledge: enter
debug1: pledge: fork
debug2: channel_input_open_confirmation: channel 0: callback done
debug2: channel 0: open confirm rwindow 0 rmax 32768
debug3: receive packet: type 99
debug2: channel_input_status_confirm: type 99 id 0
debug2: PTY allocation request accepted on channel 0
debug2: channel 0: rcvd adjust 2097152
debug3: receive packet: type 99
debug2: channel_input_status_confirm: type 99 id 0
debug2: shell request accepted on channel 0
Last login: Fri Dec 20 10:11:26 2024 from 10.1.1.1
[et0@TP3TP3TP3 ~]$ 
```

## 4. DoT

🌞 **Configurer la machine pour qu'elle fasse du DoT**

```bash
[et0@TP3TP3TP3 ~]$ sudo dnf install systemd-resolved
[et0@TP3TP3TP3 ~]$ sudo systemctl enable --now systemd-resolved
```

```bash
[et0@TP3TP3TP3 systemd]$ cat /etc/systemd/resolved.conf
[Resolve]
DNS=1.1.1.1
DNSSEC=yes
DNSOverTLS=yes
```

```bash
[et0@TP3TP3TP3 systemd]$ sudo systemctl restart systemd-resolved
```

🌞 **Prouvez que les requêtes DNS effectuées par la machine...**


## 5. AIDE


🌞 **Installer et configurer AIDE**



🌞 **Scénario de modification**



🌞 **Timer et service systemd**

