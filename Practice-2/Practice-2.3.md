1. Create AWS EC2 REDHAT instance (Linux server)

	Systemx to connect to Linux server:
  
  ssh -i "private-key" user-name@server-name (login with ssh key and username)
  
  ssh user-name@server-name  (if user public key is already added to the target server)
  
	ssh user-name@server-public-ip
  
  ssh user-name@server-private-ip (if connecting to the serevr within the same network)

	ex: 
  ssh -i "rhel.pem" ec2-user@ec2-3-80-91-11.compute-1.amazonaws.com

	ssh -i "rhel.pem" ec2-user@3.80.91.11

> Note: 'ssh' full form - secure shell, ssh service running at port number 22. Make sure port number 22 is open if linux server is going to be connected using ssh.

2. setup/create a new user "devops" on RHEL Linux server

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

