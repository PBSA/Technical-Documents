# EXEBlock Knowledge Base : How To Jenkins

1\) Steps to change your Jenkins password when you forgot it.  
  


* LAZY AND UNSAFE METHOD                               Login to the Virtual machine where Jenkins is installed                               Go to /var/lib/jenkins directory, there should be a file config.xml                               set useSecurity to false  **&lt;useSecurity&gt;false&lt;/useSecurity&gt;**                               Reboot Jenkins using sudo service jenkins restart                               The next time you login to your Jenkins console no password would be asked and you can configure the password from there and then re-enable to use security via the console or the Config.xml.
* PROPER SAFE METHOD                               Calculate the hash of your new password using                                htpasswd -bnBC 10 "" newpassword \| tr -d ':\n' \| sed 's/$2y/$2a/'                               goto /var/lib/jenkins/users/&lt;yourusername&gt;/ , there will be a file called config.xml                               Replace the contents of your &lt;passwordHash&gt;                                &lt;passwordHash&gt;\#jbcrypt:**NEWPASSWORDHASH**&lt;/passwordHash&gt;                                save the file and restart jenkins using sudo service jenkins restart    
*  Cli  for Jenkins on 44 testnet [http://10.20.10.44:8086/cli/](http://10.20.10.41:8080/cli/)  
* [Role-based Authorization Strateg](https://plugins.jenkins.io/role-strategy)y plugin to restrict user access based upon roles

## Related articles <a id="HowToJenkins-Relatedarticles"></a>

*  Page: [How to set up Linux testnet environment](/wiki/spaces/EKB/pages/197460035/How+to+set+up+Linux+testnet+environment)
*  Page: [How to Setup EXEchain](/wiki/spaces/EKB/pages/197460257/How+to+Setup+EXEchain)
*  Page: [How to setup Sidechain environment](/wiki/spaces/EKB/pages/197460072/How+to+setup+Sidechain+environment)
*  Page: [EVM Smart Contract testing](/wiki/spaces/EKB/pages/197460007/EVM+Smart+Contract+testing)
*  Page: [How to Create Smart Contract](/wiki/spaces/EKB/pages/197460297/How+to+Create+Smart+Contract)

