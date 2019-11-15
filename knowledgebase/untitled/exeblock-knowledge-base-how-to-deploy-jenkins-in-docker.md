# EXEBlock Knowledge Base : How to deploy Jenkins in Docker

1. First install the docker:
   1. ```text
      sudo apt-get update
      ```
   2. ```text
      sudo apt-get install apt-transport-https ca-certificates curl software-properties-common
      ```
   3. ```text
      curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
      ```
   4. ```text
      sudo apt-key fingerprint 0EBFCD88
      ```
   5. ```text
      sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
      ```
   6. ```text
      sudo apt-get update
      ```
   7. ```text
      sudo apt-get install docker-ce
      ```
   8. Verify the installation: _docker version_  
2. Install the latest version of Jenkins: 
   1. create jenking\_home folder: _mkdir /var/jenkins\_home_
   2. chown the folder: _chown 1000:1000 /var/jenkins\_home_
   3. Run the command below to install, configure and run Jenkins:  
  
      _docker run -d -e JENKINS\_OPTS="--httpPort=8086" -p 8086:8086 -v /var/_[_jenkins\_home:/var/jenkins\_home:z_](http://jenkins_home/var/jenkins_home:z) _--name jenkins jenkins/jenkins:lts_Run

                where:

                 _-e JENKINS\_OPTS="--httpPort=8086"  - configure Jenkins to run on port 8086_

                _-p 8086:8086 - redirect container port 8086 to sever's port 8086_

                _-v /var/_[_jenkins\_home:/var/jenkins\_home:z_](http://jenkins_home/var/jenkins_home:z) _- links /var/jenkins\_home folder of the container to the server. Which means the folder will be accessible from outside the container._  
  
3. Run : _sudo docker ps -a_ to make sure the container is up and running. You should see something like on the screenshot below:    
4. Open docker's bash: _docker exec -i -t jenkins /bin/bash_
5. If you want to make changes in the Jenkins configuration file_: sudo nano /var/jenkins\_home/config.xml_
6. Stop Jenkins: _sudo systemctl stop jenkins_
7. Start Jenking: _sudo systemctl start jenkins_
8. Check status of the service: _systemctl status jenkins_
9. Exit the docker bash_: exit_

