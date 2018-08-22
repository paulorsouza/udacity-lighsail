# Udacity Lighsail

**SSH -> ssh grader@18.219.166.229 -i <id_grader> -p 2200

**Catalog app -> http:18.219.166.229

# Installed

- make
- zip
- unzip
- postgresql
- python3-pip
- libapache2-mod-wsgi-py3
- apache2

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

- Pet Catalog App

```sh
$ git clone https://github.com/paulorsouza/udacity-catalog.git
$ sudo apt-get -qqy install make zip unzip postgresql
$ sudo apt-get -qqy install python3 python3-pip
$ sudo pip3 install --upgrade pip
$ sudo pip3 install --upgrade pyasn1
$ sudo pip3 install flask packaging oauth2client redis passlib flask-httpauth
$ sudo pip3 install sqlalchemy flask-sqlalchemy psycopg2-binary bleach requests
$ sudo pip3 install -U pytest
$ su postgres -c 'createuser -dRS ubuntu'
$ su ubuntu -c 'createdb catalog'
$ su ubuntu -c 'psql -d catalog sql/drop.sql'
$ su ubuntu -c 'psql -d catalog -sql/create.sql'
$ su ubuntu -c 'python3 catalog/pets.py'
$ sudo apt-get install apache2
$ sudo apt-get install libapache2-mod-wsgi-py3
$ sudo a2enmod wsgi
$ sudo mkdir /var/www/udacity-catalog
$ cd /var/www/udacity-catalog
$ ln -s /home/ubuntu/udacity-catalog/catalog
$ touch app.wsgi
$ nano app.wsgi

#!/usr/bin/python3
import sys
import logging
logging.basicConfig(stream=sys.stderr)
sys.path.insert(0,"/var/www/udacity-catalog/")

from catalog import app as application
application.secret_key = 'ngm-vai-saber'

$ sudo su
$ touch /etc/apache2/sites-available/udacity-catalog.conf
$ nano /etc/apache2/sites-available/udacity-catalog.conf
<VirtualHost *:80>
            ServerName prs.com
            ServerAdmin paulor1809l@gmail.com
            WSGIScriptAlias / /var/www/udacity-catalog/app.wsgi
            <Directory /var/www/udacity-catalog/catalog/>
                    Order allow,deny
                    Allow from all
            </Directory>
            Alias /static /var/www/udacity-catalog/catalog/static
            <Directory /var/www/udacity-catalog/catalog/static/>
                    Order allow,deny
                    Allow from all
            </Directory>
            ErrorLog ${APACHE_LOG_DIR}/error.log
            LogLevel warn
            CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
$ a2ensite udacity-catalog
$ service apache2 reload
$ service apacher2 restart
```

- Psql permitions to www-data

```sh
$ su postgres su -c 'psql'
psql=# grant all privileges on database catalog to "www-data";
$ su postgres su -c 'psql catalog'
catalog=# grant all privileges on table pet_family to "www-data";
catalog=# grant all privileges on table user_profile to "www-data";
catalog=# grant all privileges on table pet_type to "www-data";
```

- Pip locale error

```sh
$ export LC_ALL="en_US.UTF-8"
$ export LC_CTYPE="en_US.UTF-8"
$ sudo dpkg-reconfigure locales
```

# Links

https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys-on-ubuntu-1604

https://support.rackspace.com/how-to/logging-in-with-an-ssh-private-key-on-linuxmac/

https://stackoverflow.com/questions/14547631/python-locale-error-unsupported-locale-setting

http://leonwang.me/post/deploy-flask


