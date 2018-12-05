# ansible-java-app-deployment

Apache server is load balancing two tomcat instances all residing on the same server on different ports. 
The user will hit the apache and the request will be served via either of the two tomcat instances on which sample java application is deployed.

The architecture of application is:
User -> Apache httpd -> Tomcat1 -> Sample Java Application
                 |----> Tomcat2 -> Sample Java Application
MySQL server is also installed as a service to automatically start up on server start but sample java application isn't using it.

Instructions to use this code on Ubuntu:-

1. Install Ansible:
	
		sudo apt-get update
		sudo apt-get install software-properties-common
		sudo apt-add-repository ppa:ansible/ansible
		sudo apt-get update
		sudo apt-get install ansible
		
2. Create new user:

		adduser user1

3. On master and remote node give root permissions to user1 user:
	
		sudo vim /etc/sudoers
			user1 ALL=(ALL) NOPASSWD: ALL	
			
4. On master and remote node edit sshd_config file:
	
		sudo vim /etc/ssh/sshd_config
			PasswordAuthentication yes
			PermitRootLogin yes			
	
5. Switch user to user1 and create keys:
	
		su - user1
		ssh-keygen
		ssh-copy-id user1@localhost	
			
6. Setting worker nodes (hosts):
	
		Default ansible installation directory is /etc/ansible.	

		edit /etc/ansible/hosts file to add web server:
		sudo vim /etc/ansible/hosts
			[web]
			localhost
			
		check if the hosts are reachable via command:
		ansible web -m ping
			
7. Create Playbook:

		cd /etc/ansible;
    	sudo vim setup-playbook.yml (Available in folder apache-tomcat-app in the repository)

8. To Execute a playbook:

		ansible-playbook setup-playbook.yml
	
9. To access application hit the url http://<IP_address>/sample/ in browser.

		
			
		
	
