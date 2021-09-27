##### 1. Create AWS EC2 REDHAT instance (Linux server)

	Systemx to connect to Linux server:

	ssh -i "private-key" user-name@server-name (login with ssh key and username)

	ssh user-name@server-name  (if user public key is already added to the target server or enter the password)

	ssh user-name@server-public-ip

	ssh user-name@server-private-ip (if connecting to the serevr within the same network)

	ex: 

	ssh -i "rhel.pem" ec2-user@ec2-3-80-91-11.compute-1.amazonaws.com

	ssh -i "rhel.pem" ec2-user@3.80.91.11

> Note: 'ssh' full form - secure shell, ssh service running at port number 22. Make sure port number 22 is open if linux server is going to be connected using ssh.


##### 2. setup/create a new user "devops" on RHEL Linux server


    sudo -i (login to root user)

    cat /etc/passwd (just observe this file before user setup)

    useradd <username>
    ex: useradd devops (create a new user)

    passwd <userName>
    ex: passwd devops (setup the password for the user 'devops')

    cat /etc/passwd (just observe this file after user setup)

    sudo vi /etc/ssh/sshd_config (uncomment below two lines)

      Port 22

      PasswordAuthentication yes

    sudo systemctl restart sshd.service #(REDHAT/Linux)
    sudo systemctl restart ssh (Ubuntu)

##### 3. Connect to the Linux server using new user "devops"

	ssh user-name@server-name/server-public-ip

	ssh devops@3.80.91.11

	whoami

	sudo bash

	sudo -i

	sudo su -

	su - <username>
	ex: su - devops

	#change the user home directory
	usermod -m -d /home/devops2 devops

	cd ~

##### 4. adding sudo permissions to new user

	usermod –aG sudo [username]   - Ubuntu

	usermod –aG wheel [username]  - RedHat

	usermod -aG wheel devops (adding to wheel group i.e, giving sudo permissions)

	(OR) 

	update the file "/etc/sudoers" with below content

	visudo (or) vi "/etc/sudoers"

	devops ALL=(ALL)       ALL

> Note: "sudo" stands for "SuperUser DO" and is used to access restricted files and operations. By default, Linux restricts access to certain parts of the system preventing sensitive files from being compromised. The sudo command temporarily elevates privileges allowing users to complete sensitive tasks without logging in as the root user.

	yum install git -y

	sudo yum install git -y

	git --version

	yum remove git

	sudo yum remove git -y

	git --version

> Ref: https://unix.stackexchange.com/questions/291454/difference-between-sudo-user-and-root-user

##### 5. User Groups and Permissions in Linux

> Ref-1: https://www.pluralsight.com/blog/it-ops/linux-file-permissions

> Ref-2: copying-files-from-one-user-to-another-in-a-single-machine: https://www.guru99.com/file-permissions.html#:~:text=There%20are%20three%20user%20types,into%20Absolute%20and%20Symbolic%20mode

> Ref-3: https://www.guru99.com/file-permissions.html

	-rw-rw-r--. 1 devops devops  0 May 12 16:07 new.txt
	drwxrwxr-x. 2 devops devops  6 May 12 16:11 dir

	-rw-rw-r--

	r = read permission
	w = write permission
	x = execute permission
	- = no permission


	- > file type
	d > directory type

	The first part of the code is 'rw-'. This suggests that the owner 'devops' can: read, write but no execute permissions

	second part "rw-" - group permissions

	third part "r--" - others


	useradd user1

	touch user1.txt

##### 6. Permissions

6.1. default permissions of the file

	ll user1.txt
	-rw-rw-r--. 1 user1 user1 0 May 12 16:24 user1.txt

6.2. update the permissions of user, group, others

	chmod u=rwx,g=r,o=r user1.txt

	ll user1.txt
	
	-rwxr--r--. 1 user1 user1 0 May 12 16:24 user1.txt

6.3. update the permissions of user, group, others (give all type of permissions to user, group, others)

	chmod u=rwx,g=rwx,o=rwx user1.txt

	ll user1.txt
	-rwxrwxrwx. 1 user1 user1 0 May 12 16:24 user1.txt

6.4. update permissions for "others" only to "read"

	chmod o=r user1.txt

	ll user1.txt
	-rwxrwxr--. 1 user1 user1 0 May 12 16:24 user1.txt

6.5. user should have only write access

	create file and default permissions
	
	touch user.sh
	ll user.sh
	-rw-rw-r--. 1 user1 user1  0 May 12 16:51 user.sh

	chmod u=w user.sh  - user should have only write access
	cat user.sh
	cat: user.sh: Permission denied

6.6. chmod u=r user.sh  - user should have only read access

	echo "sample" > user.sh
	-bash: user.sh: Permission denied

6.7. chmod -rwx user.sh or chmod 000 user.sh - remove all permissions

	----------. 1 user1 user1 23 May 12 17:01 user.sh

	read:
	cat user.sh
	cat: user.sh: Permission denied

	write:
	echo "writing content" > user.sh
	-bash: user.sh: Permission denied

	execute:
	./user.sh
	sh: user.sh: Permission denied

6.8. chmod +r user.sh or chmod 444 user.sh - giving read permission to all levels

	-r--r--r--. 1 user1 user1 23 May 12 17:01 user.sh

	echo "writing content" > user.sh
	-bash: user.sh: Permission denied

6.9. copy the files from one user to another

		method-1:

			As USER1:
				cp [filename] /tmp
				chmod 777 /tmp/[filename]
			As USER2:
				cp /tmp/[filename] .
			As USER1:
				rm /tmp/[filename]

		method-2:

			sudo -i (login to root user)
			cp /home/user1/file /home/user2/file

##### 7. SSH connection

7.1. Create two ec2 instances with Red Hat Linux AMI.

7.2. Connect server-1 and create a new user and setup password using Step-2 (above)

	username: server1
	Password: server1

7.3. Connect server2 and create a new user and setup password using Step-2 (above)

	username: server2
	Password: server2

7.4. Go to server-1 and login to new user 'server1'.

	su - server1

7.5. Generate ssh key using the command: ssh-keygen -t rsa

	ssh-keygen -t rsa (press enter for all options, don't give any value for any option)

	Sample output: 
	[server1@ip-172-31-42-239 ~]$ ssh-keygen -t rsa
	Generating public/private rsa key pair.
	Enter file in which to save the key (/home/server1/.ssh/id_rsa):
	Created directory '/home/server1/.ssh'.
	Enter passphrase (empty for no passphrase):
	Enter same passphrase again:
	Your identification has been saved in /home/server1/.ssh/id_rsa.
	Your public key has been saved in /home/server1/.ssh/id_rsa.pub.
	
7.6. Copy the pulic key from /home/server1/.ssh/id_rsa.pub

	cat /home/server1/.ssh/id_rsa.pub
	
7.7. Connect to other server-2, and login to 'server2' user.

	ssh server2@server2-ip-address
	
	vi .ssh/authorized_keys
	
	
