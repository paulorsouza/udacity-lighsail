# Udacity Lighsail

## Step by Step

- Update system

```sh
$ sudo apt-get -qqy update
$ sudo apt-get upgrade -y
```

- Create and grant root to grader

```sh
$ sudo useradd grader
$ sudo passwd grader 
Enter new Unix password: tyty56yt
$ sudo su
$ cd /etc/sudoers.d
$ cp 90-cloud-init-users grader
$ nano grader
# grader ALL=(ALL) NOPASSWD:ALL
$ su grader
password tyty56yt
$ sudo mkhomedir_helper grader
$ cd home/grader
$ mkdir .ssh
$ touch .ssh/authorized_keys
$ nano .ssh/authorized_keys
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDA/moCf0oe4uZn8gigzj9zfLajCbr69Vyyw4cIc7TigWHjzKFufC7qhnLaCl+ObZmKFal3sykO4m00Lw1iFMXOMPjirflQEDg2COnzitKmt7J86hIXJtGTYORsuDYFqT9dwTyU88EH6ODxvSZQiiE0KWRk0J7o3ztgjXSU2kxYl2y1vVyFD9ejYKeY5gr2K/uVGnkcMLE7RisS4C5scbyWIGDwDpfgCNbIQ7tMaQ+3xAKYZY+wb9Ow4+qDjkd+8eRUO6165UM2+tKkv5Y3GwrpKjsTDbtxoB/Da1zukwYraXlAkdPDppuIj9uWwpWa0Rc9W5KVG2TRDhTW65MjLOSb prs@Inspiron-5557
$ chmod 700 .shh
$ chmod 644 .ssh/authorized_keys
```

- Login in local machine

```sh
$ ssh grader@18.219.166.229 -i id_grader 
```

- Firewall

Change lighsail firewall to permit all ports
All TCP+UDP	ALL	0 - 65535

```sh
$ sudo su
$ ufw default deny incoming
$ ufw default allow outgoing
$ ufw allow http
$ ufw allow 2200/tcp
$ ufw allow ntp
$ nano /etc/ssh/sshd_config
Port 2200
$ ufw enable
```
