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
$ ssh-keygen
```
