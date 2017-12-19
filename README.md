# Project Title

Using a baseline installation of a Linux server and preparing it to host my web application. This includes securing it against a number of attack vectors, installing and configuring a database server and deploying a handmade web application. The steps reflect the steps recorded in the Project Details section from Udacity.

## Getting Started (Setps 1 and 2)

I began by creating a new server instance through Amazon Lightsail.  I followed the instructions to SSH into my newly created server.

### Step 3

I first updated all my currently installed packages by running the command:

```
$ sudo apt-get update
```

After it had fetched the details, I retrieved thos updates using:

'''
$ sude apt-get udgrade
'''

### Step 4

I changed the SSH port on my instance from 22 to 2200 using the 'sudo nano' command. The SSH configuration is found in /etc/ssh/sshd_config.
'''
sudo nano /etc/ssh/sshd_config
'''
Inside the file I simply changed the port number to reflect the necessary change.


### Step 5

Then I began to configure my UFW. First I verified that my firewall was inactive by running:

'''
$ sudo ufw status
'''
Then I blocked all incoming and allowed all outgoing connections using:

'''
$ sudo ufw default deny incoming
'''

and

'''
$ sudo ufw allow outgoing
'''

### Step 6



### Step 7

### Step 8

### Step 9

### Step 10

### Step 11

### Step 12

## Acknowledgments

* Hat tip to anyone who's code was used
* Inspiration
* etc
