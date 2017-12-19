# Configuring an Amazon Lightsail Instance to run a Flask Application and PostgreSQL Database

Using a baseline installation of a Linux server and preparing it to host my web application. This includes securing it against a number of attack vectors, installing and configuring a database server and deploying a handmade web application. The steps reflect the steps recorded in the Project Details section from Udacity.

## Getting Started (Setps 1 and 2)

I began by creating a new server instance through Amazon Lightsail.  I followed the instructions to SSH into my newly created server.  Until specified, all the commands are issued from the browser console on the AWS site.

### Step 3

I first updated all my currently installed packages by running the command:

```
$ sudo apt-get update
```

After it had fetched the details, I retrieved thos updates using:

```linux
$ sudo apt-get udgrade
```

### Step 4

I changed the SSH port on my instance from 22 to 2200 using the 'sudo nano' command. The SSH configuration is found in /etc/ssh/sshd_config.
```linux 
$ sudo nano /etc/ssh/sshd_config
```
Inside the file I simply changed the port number to reflect the necessary change.


### Step 5

Then I began to configure my UFW. First I verified that my firewall was inactive by running:

```linux
$ sudo ufw status
```
Then I blocked all incoming and allowed all outgoing connections using:

```linux
$ sudo ufw default deny incoming
```

and

```linux
$ sudo ufw allow outgoing
```

In order to configure the UFW to allow the necessary connections, I had to run the following series of commands. They made allowances for SSH on port 2200, HTTP on port 80, and NTP on port 123.

```linux
$ sudo ufw allow 2200/tcp
$ sudo ufw allow www
$ sudo ufw allow ntp
```

After words, I ran ```$ sudo ufw show added``` to check the rules I'd made, followed by ```$ sudo ufw enable``` turning on the firewall, and ```$ sudo ufw status``` to check the status of the enabled firewall.

### Step 6

I created a new user named grader.

```linux
$ sudo adduser grader
```

### Step 7

I had to edit the sudoers file in order to grant grader access to the ```sudo``` command. Add the line ```grader ALL=(ALL:ALL) ALL``` to the file /etc/sudoers.d/grader.

```linux
$ sudo nano /etc/sudoers.d/grader
```

### Step 8

In order to log into the grader user on the server instance from the command line, it is necessary to generate a key pair either locally or through the Lighthouse terminal.  According to the Udacity specifications, I generated them locally (IN MY TERMINAL NOW) by first creating a '.ssh' directory to store the keys. I had to make my vagrant user the owner and vagrant the group in order to make the files private enough to meet the Amazon security policy. Then after changing into that newly created directory, I generated a key pair using the keygen command. These commands were issued from inside a vagrant machine on my local command line within the '/home/vagrant' directory.

```linux
/home/vagrant $ mkdir .ssh
/home/vagrant $ chown vagrant:vagrant /home/vagrant/.ssh
/home/vagrant $ cd .ssh
/home/vagrant/.ssh $ ssh-keygen
```

Running the ```ssh-keygen``` command will prompt the user for a file in which to save the newly created keys.  I named the file 'grader'. This resulted in the creation of both 'grader' and 'grader.pub'. I renamed 'grader' to 'grader.pem' to remove any confusion about which file was the public and which was the private.

```linux
$ mv grader grader.pem
``` 

Then I read the contents of the 'grader.pub' file in order to copy them and then jumped back to my Lightsail instance.

```linux
$ sudo nano grader.pub
```

In my instance (accessed through the browser console) I then switched into the grader user-- ```$ sudo su - grader``` --and created a subdirectory called '.ssh' and set the owner to the grader user and set the permission to read write and execute only to the grader user.

```linux
$ mkdir .ssh
$ chown grader:grader /home/grader/.ssh
$ chmod 700 /home/grader/.ssh
```

I then 'cd'ed into the '.ssh' directory and created a file called 'authorized_keys', used ```sudo nano``` to edit the 'authorized_keys' file and inserted the copied 'grader.pub' contents.  I set the permissions of the file to 400.

```linux
$ sudo nano /.ssh
$ sudo chmod 400 /.ssh/authorized_keys
```

Back in my terminal I verified that I could now log in to the instance as grader from my local machine. I had to specify the port because of my firewall and ssh configuration from earlier.

```linux
$ ssh -i grader.pem grader@18.218.28.108 -p 2200
```

Once I was logged in, I completed the rest of the steps from my terminal while logged in to my instance as grader.

I needed to disable root login and force authentication using the key pair, so in the file '/etc/ssh/sshd_config' I changed "PermitRootLogin without-password" to "PermitRootLogin no" and uncommented the line that reads "PasswordAuthentication no".  I ran ```$ sudo service ssh restart```

### Step 9

The instance timezone was already set to UTC, but to verify I ran ```$ sudo dpkg-reconfigure tzdata```

### Step 10

Install Apache and the libapache2-mod-wsgi, and the python set up tools packages and then restarted the Apache service.

```linux
$ sudo apt-get install apache2
$ sudo apt-get install libapache2-mod-wsgi
$ sudo apt-get install python-setuptools
$ sudo service apache2 restart
```


### Step 11

I installed PostgreSQL and then opened the file '/etc/postgresql/9.5/main/pg_hba.conf' to ensure that remote connections weren't allowed.
```linux
$ sudo apt-get install postgresql
$ sudo nano /etc/postgresql/9.5/main/pg_hba.conf
```

I then switched to the postgres user and switched into the interactive postgres mode. Within the interactive prompt I created a new database and user both named catalog and set the password to catalog.  I then gave the catalog user permission to use the catalog database and used ctrl+z to exit the prompt.

```linux
$ sudo su - postgres
postgres $ psql
postgres=# CREATE DATABASE catalog;
# CREATE USER catalog;
# ALTER ROLE catalog WITH PASSWORD 'catalog'
# GRANT ALL PRIVILEGES ON DATABASE catalog TO catalog;
```

### Step 12

In order to clone my remote repository I needed to install git within my instance so I ran ```$ sudo apt-get install git```

### Step 13

### Step 14

## Acknowledgments

* https://digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps
* [Amazon Lightsail Documentation](https://aws.amazon.com/documentation/lightsail/)