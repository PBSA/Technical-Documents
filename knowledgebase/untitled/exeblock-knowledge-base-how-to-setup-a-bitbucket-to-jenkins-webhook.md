# EXEBlock Knowledge Base : How to setup a BitBucket to Jenkins webhook

1. Verify the BitBucket plugin is installed on Jenkins. 
2. Configure the Source Code Management section to the repository and branch and make sure the "Build when a change is pushed to BitBucket" box is checked under Build Triggers.  
3. Go to the desired Bitbucket repository and click the "Add webhook" button in the webhooks section under settings. 
4. Enter in the desired settings. The URL should be in the form of http://&lt;public ip&gt;:&lt;jenkins port&gt;/bitbucket-hook/  For testnet 44, the URL is [http://208.66.16.201:8086/bitbucket-hook/](http://208.66.16.201:8086/bitbucket-hook/)  
5. Verify if requests are successful by clicking on "View requests" under the specified webhook actions. 

