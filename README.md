This file describes steps to configure a linux server (Ubuntu 16.04 LTS - Xenial (HVM)) which is hosted on Amazon EC2, and equipped to host a web application with Postgres database server.

# Connect to the server:
------------------------

i. The IP address and SSH port

	Public IP: 34.211.165.57
	Public DNS: ec2-34-211-165-57.us-west-2.compute.amazonaws.com
	Private IP: 172.31.22.2
	SSH port: 2200

To connect to the server using ssh, use its public dns name and provide 2200 as port number:

ex:

	ssh -i udacity1.pem ubuntu@ec2-34-211-165-57.us-west-2.compute.amazonaws.com -p 2200

ii. The complete URL to web application 'item catalog' hosted on this server.

	http://catalog.moniph.al

# A summary of software installed and configuration changes made.
-----------------------------------------------------------------

## Software installed

1. Apache
2. Postgresql
3. git

## Configuration made to the server

1. Update packages

	`sudo apt-get update`

2. Configure SSH port to 2200

	* Edit file `/etc/ssh/sshd_config` and changed `Port 22` to `Port 2200`

	* Restart sshd service:
	`sudo systemctl restart sshd`
	
3. Configure Uncomplicated Firewall (UFW) to allow SSH(2200), HTTP(80) and NTP(123)

	Ran below commands as sudo:
	
        sudo ufw allow from any to any port 80 proto tcp
	    sudo ufw allow from any to any port 2200 proto tcp
	    sudo ufw allow from any to any port 123 proto udp
	    sudo ufw enable

4. Create 'grader' account

	Ran command: `sudo adduser grader`

5. Give grader the permission of sudo

	`sudo adduser grader sudo`

6. Create an SSH key pair for grader using RSA

	Use `ssh-keygen` command to generate the public/private key pair

7. Configure the local timezone to UTC

	Command: `sudo timedatectl set-timezone Etc/UTC`

8. Install and configure Apache to serve a Python mod_wsgi application.

	`sudo apt-get install apache2 libapache2-mod-wsgi`

9. Install and configure PostgreSQL:

    sudo apt-get install postgresql
	
	* Do not allow remote connections (default setting)

	Checked the file:
	/etc/postgresql/9.5/main/pg_hba.conf and only local connection is allowed.

    * Create a new database user named catalog that has limited permissions to your catalog application database.
	
	`sudo su - postgres
	psql
	CREATE USER catalog WITH CREATEDB;`

	* configure and start database server
	
	`postgres=# initdb -D /usr/local/pgsql/data`
	
	start the database server in background:
	`postgres -D /usr/local/pgsql/data >logfile 2>&1 &`

10. Configured and hosted item-catalog web application
	
## A list of any third-party resources made use of to complete this project.

	Amazon Elastic Compute Cloud (Amazon EC2)
	url: https://aws.amazon.com/ec2/
