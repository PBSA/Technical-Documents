# EXEBlock Knowledge Base : How to update Jenkins on a Testnet

1. If Jenkins runs as :  

```text
   java -jar jenkins-cli.jar -s http://localhost:8080/
```

       The quickest way is to download jenkins.war file and to replace the current one.

2. The other option is to update the entire Ubuntu server:

```text
   apt-get update
   apt-get upgrade
```

3. If Jenkins has been installed into system folders the preferable way is to run the script below:  

```text
   cd /usr/share/jenkins:
   sudo service jenkins stop
   sudo mv jenkins.war jenkins.war.old
   sudo wget https://updates.jenkins-ci.org/latest/jenkins.war
   sudo service jenkins start
```

 Thoroughly identify the right way of updating Jenkins. A wrong setup may cause a deleting all build and deployment jobs, and a disrupting the whole environment !!!

