# EXEBlock Knowledge Base : How-to: Useful Docker & Linux Commands

## **Docker** <a id="How-to:UsefulDocker&amp;LinuxCommands-Docker"></a>

1. Delete all \(including running\) containers:  _docker container rm $\(docker container ls -aq\) -f_  
2. Delete all stopped containers:  _sudo docker container prune -f_  
3. Delete all orphan images:  _sudo docker image prune -f_  
4. Copy files to/from a docker container:  Run the command below to get the container id:  _docker inspect -f '{{.Id}}' sidechain\_merge_  Assuming the container id is '[e4f60e85f3e4415260947dc7543cb2f0ec0978ef0ef1965f9b648713e6101691](http://e4f60e85f3e4415260947dc7543cb2f0ec0978ef0ef1965f9b648713e6101691/home/sidechain_node/nohup.out)', copy nohup.out file to the VM home directory:  _sudo docker cp_ [_e4f60e85f3e4415260947dc7543cb2f0ec0978ef0ef1965f9b648713e6101691:/home/sidechain\_node/nohup.out_](http://e4f60e85f3e4415260947dc7543cb2f0ec0978ef0ef1965f9b648713e6101691/home/sidechain_node/nohup.out) _/home/_  
5. Inspect container ports:  _docker inspect --format='{{range $p, $conf := .NetworkSettings.Ports}} {{$p}} -&gt; {{\(index $conf 0\).HostPort}} {{end}}'_ [_e4f60e85f3e4415260947dc7543cb2f0ec0978ef0ef1965f9b648713e6101691_](http://e4f60e85f3e4415260947dc7543cb2f0ec0978ef0ef1965f9b648713e6101691/home/sidechain_node/nohup.out)  
6. List container commits:  _docker images -a --no-trunc \| head -n4 \| grep -v container\_name \| awk '{ print $3 }' \| xargs docker inspect \| grep Comment_  
7.  Export \(save and compress\) a docker container:  _docker save exeblock/sidechain\_node:latest &gt; sidechain\_node.tar_  
8.  Stop all containers by name:  _docker ps -a \| awk '{ print $1,$2 }' \| grep container\_name \| awk '{print $1 }' \| xargs -I {} docker stop {}_  
9. Remove all containers by name:  _docker ps -a \| awk '{ print $1,$2 }' \| grep container\_name\| awk '{print $1 }' \| xargs -I {} docker rm {}_  
10. Build a docker container in detached mode with TTY, and to bind ports:  _sudo docker run -dit --name peerplays-chain -p 9777:9777 -p 8090:8090 -p 3692:3692 exeblock/peerplays-chain  \*\*\* Note: when building or running a docker container from Jenkins, omit the -it flag to disable TTY and interactive mode._ 

## **Linux** <a id="How-to:UsefulDocker&amp;LinuxCommands-Linux"></a>

1.  Free memory:    _free -m_  
2. Watch RAM every 5 seconds:  watch '/usr/bin/top -b \| head -4 \| tail -5'  
3. Watch CPU bu user:  _watch 'ps auxk -gid,-%mem \| head -11 \| tail -5'_  
4.  View memory and cpu by process:  _ps -eo pid,ppid,cmd,%mem,%cpu --sort=-%mem \| head_  
5. Get public IP:  _dig +short_ [_myip.opendns.com_](http://myip.opendns.com) _@_[_resolver1.opendns.com_](http://resolver1.opendns.com)  
6. Get Ubuntu version:  _cat /etc/\*release_  or   _lsb\_release -a_  
7.  Increase swap file to 4GB:  _sudo nano /etc/fstab_ _cd /_ _sudo dd if=/dev/zero of=/swapfile bs=1M count=4096_ _sudo mkswap swapfile_ _sudo swapon swapfile_ _sudo nano etc/fstab_ _/swapfile none swap sw 0 0_ _cat /proc/meminfo_ _swapon -s_  
8.  Install Java 8 \(required by Jenkins\):  _sudo apt-get install software-properties-_ _sudo add-apt-repository ppa:webupd8team/java_ _sudo apt install oracle-java8-installer_  
9. Install Jenkins:  _wget -q -O -_ [_https://pkg.jenkins.io/debian-stable/jenkins.io.key_](https://pkg.jenkins.io/debian-stable/jenkins.io.key) _\| sudo apt-key add -_ _sudo apt-add-repository "deb_ [_https://pkg.jenkins.io/debian-stable_](https://pkg.jenkins.io/debian-stable) _binary/"_ _sudo apt install jenkins_

