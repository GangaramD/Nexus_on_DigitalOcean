# Installing Nexus On Digital ocean Ubuntu Droplet:

# Minimum requiremnt:
  1. Java 8 and above
  2. A minimum of 4GB of RAM on the VM or the Droplet

# Steps:

Always first create a User for the droplet, never use the root user for the Droplet.

1. Create A admin user and assign Sudo permission to it:

2. Minimum requirment Java 8: Installing JAVA 8

	    sudo apt-get update
	    sudo apt-get install
	    sudo apt-get install -y openjdk-8-jre-headless
	    java -version

 (if you have two java installed then we can use below cmd to   
   select which version of java needs to be used )

   	  sudo update-alternatives --config java


3. Download the Nexus package:

   Download the latest package from the Official website
   	
            $ cd /opt/

            $ wget https://download.sonatype.com/nexus/3/latest-unix.tar.gz
            $ tar -xvf latest-unix.tar.gz
            $ sudo rm latest-unix.tar.gz
            $ mv nexus-3.14.0-04 nexus

4. Create a Nexus user for best practice

      		$ sudo adduser nexus    (pwd: nexus)
             	$ sudo visudo  ( Give the sudo access)
                  nexus   ALL=(ALL)       NOPASSWD: ALL
            	$ sudo chown -R nexus:nexus /opt/

5. Open /opt/nexus/bin/nexus.rc file, uncomment run_as_user parameter and set it as following.

              $ su - nexus
              $ vi /opt/nexus/bin/nexus.rc
              $ run_as_user="nexus" (file shold have only this line)


	Now You can start the nexus by "$ ./opt/nexus/bin/nexus start"

To check if the Repo is up we can check in the VM as,

		    curl http://localhost:8081

			        or
		In browser hit to httP://{Public_IP}:8081

By default Nexus login are:

	            user: admin
	            pwd: admin123 


6. Add nexus as a service at boot time

		$ vi ~/.bashrc

              		NEXUS_HOME=/opt/nexus

            $ sudo ln -s $NEXUS_HOME/bin/nexus /etc/init.d/nexus
            $ cd /etc/init.d
            $ sudo update-rc.d nexus defaults
            $ sudo service nexus start
