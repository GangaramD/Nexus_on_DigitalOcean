# Nexus configuration with Docker:

Sign-in to Nexus:
  	default user:
    
   	user:admin
	pwd": admin123

Goto Settings--->Repository--->create_repository--->docker(hosted)

	name: docker-private
	
	Repository connectors:

		Http:
			Tick: 8083 ( Give an port to communicate to docker daemon)
	
		Force Basic authentication:
	  	
		  	Tick: Disable to allow ananyomus pull

	Tick: Enable Docker V1 API:


Leave all other default 


Now Goto the VM or the droplet where Docker is installed

Create a login (daemon.json) json file to authenticate to our nexus Repo.(If this file isnt present then create it)
	Enter the Public_IP of your instamce with the Port which was opned for docker communication in the Nexus while creating the Docker Repo.

         $ sudo vi /etc/docker/daemon.json

			{
 		  		"insecure-registries":["Your__Public_IP:8083"],
   		  		"disable-legacy-registry":true
			} 

Restart The Docker Daemon

Now try to login to the nexus repo:

		$ sudo docker login -u admin -p admin123 {your_nexus_IP}:8083


  login successfull


Now Lets create an Image and Push to the Nexus repo

	$ sudo docker pull ubuntu:14.04
   	$ sudo tag ubuntu:14.04 {your_Nexus_ip_}:port/mytest:1.1
      		ex: sudo tag ubuntu:14.04 13.34.123.23:8083/mytest:1.1
   	$ sudo push {your_Nexus_ip_}:port/mytest:1.1
     		ex: sudo push 13.34.123.23:8083/mytest:1.1


Now lets make some changes in the images and let push its 2nd version

   	$ sudo docker run -it --name mytest ubuntu:14.04 /bin/bash 
Install any new package which isnt in the ubuntu ( Ex: Lets install docker in it)
 
 	$ sudo docker commit mytest
   	$ sudo tag mytest {your_Nexus_ip_}:port/mytest:1.1
      		ex: sudo tag mytest 13.34.123.23:8083/mytest:1.1
   	$ sudo push {your_Nexus_ip_}:port/mytest:1.1
      		ex: sudo push 13.34.123.23:8083/mytest:1.1

