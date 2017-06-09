# Project overview

A linux server (Ubuntu 16.04 LTS - Xenial (HVM)) is installed on Amazon LightSail, and equipped to host a web application 'Tournament Result' which uses Postgresql to manage its data.

## Connect to the server:

i. The IP address and SSH port so your server can be accessed by the reviewer

	Public IP: 34.211.165.57
	Public DNS: ec2-34-211-165-57.us-west-2.compute.amazonaws.com
	Private IP: 172.31.22.2
	SSH port: 2200

To connect to the server, please use its public dns name:
ex:
	ssh -i udacity1.pem ubuntu@ec2-34-211-165-57.us-west-2.compute.amazonaws.com -p 2200

ii. The complete URL to your hosted web application.

TODO

Locate the SSH key you created for the grader user.
During the submission process, paste the contents of the grader user's SSH key into the "Notes to Reviewer" field.

## A summary of software installed and configuration changes made.

### Software installed

### Configuration made to the server

1. Update packages

	''sudo apt-get update''

2. Configure SSH port to 2200

	* Edit file ''/etc/ssh/sshd_config'' and changed ''Port 22'' to ''Port 2200''

	* Restart sshd service:
	  >sudo systemctl restart sshd
	
3. Configure Uncomplicated Firewall (UFW) to allow SSH(2200), HTTP(80) and NTP(123)

	Ran below commands as sudo:
	
	>sudo ufw allow from any to any port 80 proto tcp
	>sudo ufw allow from any to any port 2200 proto tcp
	>sudo ufw allow from any to any port 123 proto udp
	>sudo ufw enable

	ufw status on the server after configuration:
	ubuntu@ip-172-31-22-2:~$ sudo ufw status
	Status: active

	To                         Action      From
	--                         ------      ----
	80/tcp                     ALLOW       Anywhere                  
	22/tcp                     ALLOW       Anywhere                  
	2200/tcp                   ALLOW       Anywhere                  
	123/udp                    ALLOW       Anywhere                  
	80/tcp (v6)                ALLOW       Anywhere (v6)             
	22/tcp (v6)                ALLOW       Anywhere (v6)             
	2200/tcp (v6)              ALLOW       Anywhere (v6)             
	123/udp (v6)               ALLOW       Anywhere (v6)             

4. Create 'grader' account

	Ran command: `sudo adduser grader`

5. Give grader the permission of sudo

	ubuntu@ip-172-31-22-2:~$ sudo adduser grader sudo
	>Adding user `grader' to group `sudo' ...
	>Adding user grader to group sudo
	>Done.

6. Create an SSH key pair for grader using RSA

	Use `ssh-keygen` command to generate the public/private key pair

7. Configure the local timezone to UTC

	Run the command: `sudo timedatectl set-timezone Etc/UTC`
	Ex:
	---
	ubuntu@ip-172-31-22-2:~$ sudo timedatectl set-timezone Etc/UTC
	ubuntu@ip-172-31-22-2:~$ date
	Fri Jun  9 05:28:50 UTC 2017
	ubuntu@ip-172-31-22-2:~$ 

8. Install and configure Apache to serve a Python mod_wsgi application.

	sudo apt-get install apache2
	ubuntu@ip-172-31-22-2:~$ sudo apt-get install libapache2-mod-wsgi


9. Install and configure PostgreSQL:
   sudo apt-get install postgresql
	
	* Do not allow remote connections (default setting)

	Checked the file:
	/etc/postgresql/9.5/main/pg_hba.conf and only local connection is allowed.

    * Create a new database user named catalog that has limited permissions to your catalog application database.
	
	sudo su - postgres
	psql
	CREATE USER catalog WITH CREATEDB;

	* configure and start database server
	
	postgres=# initdb -D /usr/local/pgsql/data
	
	start the database server in background:
	postgres -D /usr/local/pgsql/data >logfile 2>&1 &
	
10. Install git.

Deploy the Item Catalog project.

11. Clone and setup your Item Catalog project from the Github repository you created earlier in this Nanodegree program.

12. Set it up in your server so that it functions correctly when visiting your server's IP address in a browser. Make sure that your .git directory is not publicly accessible via a browser!

### A list of any third-party resources you made use of to complete this project.

