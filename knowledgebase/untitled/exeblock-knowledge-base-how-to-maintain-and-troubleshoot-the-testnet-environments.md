# EXEBlock Knowledge Base : How To Maintain and Troubleshoot the Testnet Environments

### **A list of exeBlock VMs and Testnets** <a id="HowToMaintainandTroubleshoottheTestnetEnvironments-AlistofexeBlockVMsandTestnets"></a>

| Important VMs and Testnets |
| :--- |


<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>#</b>
      </th>
      <th style="text-align:left">Name</th>
      <th style="text-align:left">Private IP</th>
      <th style="text-align:left">Public IP</th>
      <th style="text-align:left">Main Folders</th>
      <th style="text-align:left">Docker</th>
      <th style="text-align:left">Auto recovery</th>
      <th style="text-align:left">CI/CD</th>
      <th style="text-align:left">Status</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">1</td>
      <td style="text-align:left">EOS Private Testnet</td>
      <td style="text-align:left">10.20.10.42</td>
      <td style="text-align:left">--</td>
      <td style="text-align:left">eos</td>
      <td style="text-align:left">--</td>
      <td style="text-align:left">--</td>
      <td style="text-align:left">--</td>
      <td style="text-align:left">No node is running</td>
    </tr>
    <tr>
      <td style="text-align:left">2</td>
      <td style="text-align:left">EOS Public Testnet</td>
      <td style="text-align:left">--</td>
      <td style="text-align:left">208.66.16.205</td>
      <td style="text-align:left">/opt</td>
      <td style="text-align:left">--</td>
      <td style="text-align:left">--</td>
      <td style="text-align:left">--</td>
      <td style="text-align:left">
        <p>All components are compiled and configured. What&apos;s left is to setup
          the node to target Jungle Testnet:</p>
        <p><a href="https://github.com/CryptoLions/EOS-Jungle-Testnet">https://github.com/CryptoLions/EOS-Jungle-Testnet</a>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">3</td>
      <td style="text-align:left">?</td>
      <td style="text-align:left">10.20.10.43</td>
      <td style="text-align:left">208.66.16.208</td>
      <td style="text-align:left"></td>
      <td style="text-align:left">-</td>
      <td style="text-align:left">--</td>
      <td style="text-align:left">--</td>
      <td style="text-align:left">No Node is Running</td>
    </tr>
    <tr>
      <td style="text-align:left">4</td>
      <td style="text-align:left">5050 QA Private Testnet</td>
      <td style="text-align:left">10.20.10.44</td>
      <td style="text-align:left">
        <p>208.66.16.201</p>
        <p>qa.5050labs.fun
          <br />
          <br />
        </p>
      </td>
      <td style="text-align:left">
        <p>jenkins</p>
        <p>peerplays-chain</p>
        <p>selenium</p>
      </td>
      <td style="text-align:left">--</td>
      <td style="text-align:left">
        <p>Yes</p>
        <p>start-testnet.sh</p>
      </td>
      <td style="text-align:left">Jenkins Server on: 10.20.10.44:8086</td>
      <td style="text-align:left">
        <p>The VM serves as a Jenkins CI Server</p>
        <p>Secured with SSL (ports 8091, 3001, 443)</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">5</td>
      <td style="text-align:left">5050 DEV Private Testnet</td>
      <td style="text-align:left">10.20.10.45</td>
      <td style="text-align:left">
        <p>208.66.16.202</p>
        <p>dev.5050labs.fun
          <br />
          <br />
        </p>
      </td>
      <td style="text-align:left">
        <p>Peerplays</p>
        <p>jenkins-remote-node</p>
      </td>
      <td style="text-align:left">--</td>
      <td style="text-align:left">
        <p>Yes</p>
        <p>start-testnet.sh</p>
      </td>
      <td style="text-align:left">Jenkins Remote Node</td>
      <td style="text-align:left">Secured with SSL (ports 8091, 3001, 443)</td>
    </tr>
    <tr>
      <td style="text-align:left">6</td>
      <td style="text-align:left">5050 Mobile Build Server</td>
      <td style="text-align:left">10.20.10.46</td>
      <td style="text-align:left">208.66.16.200</td>
      <td style="text-align:left">C:\Program Files (x86)\Jenkins</td>
      <td style="text-align:left">--</td>
      <td style="text-align:left">--</td>
      <td style="text-align:left">
        <p>Jenkins Server on:</p>
        <p>10.20.10.46:8086</p>
      </td>
      <td style="text-align:left">In case of unplanned shutdowns Jenkins Service might need to be restarted
        manually</td>
    </tr>
    <tr>
      <td style="text-align:left">7</td>
      <td style="text-align:left">Sidechain Testnet</td>
      <td style="text-align:left">10.20.10.47</td>
      <td style="text-align:left">208.66.16.211</td>
      <td style="text-align:left">
        <p>.bitcoin</p>
        <p>sidechain-latest</p>
        <p>tapin</p>
        <p>sidechain-gui-merge</p>
      </td>
      <td style="text-align:left">--</td>
      <td style="text-align:left">--</td>
      <td style="text-align:left">--</td>
      <td style="text-align:left">Up and running
        <br />Debugging Sidechain Deposit Issue</td>
    </tr>
    <tr>
      <td style="text-align:left">8</td>
      <td style="text-align:left">5050 Public Testnet</td>
      <td style="text-align:left">10.20.10.48</td>
      <td style="text-align:left">
        <p><a href="https://208.66.16.204/">https://208.66.16.204</a>
        </p>
        <p>5050labs.fun</p>
      </td>
      <td style="text-align:left">
        <p>5050dapp</p>
        <p>peerplays-chain</p>
        <p>tapin</p>
      </td>
      <td style="text-align:left">-</td>
      <td style="text-align:left">
        <p>Yes</p>
        <p>start-testnet.sh</p>
      </td>
      <td style="text-align:left">--</td>
      <td style="text-align:left">
        <p>Secured with SSL (ports 8091, 3001, 443)</p>
        <p>Public domain: 5050labs.fun</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">9</td>
      <td style="text-align:left">CI/CD Server</td>
      <td style="text-align:left">10.20.10.49</td>
      <td style="text-align:left">--</td>
      <td style="text-align:left">--</td>
      <td style="text-align:left">--</td>
      <td style="text-align:left">--</td>
      <td style="text-align:left">TODO: the new VM is intended for Jenkins CI/CD Server and should repalce
        Jenkins Servers installed on 44 and 46 machines.</td>
      <td style="text-align:left">TODO</td>
    </tr>
    <tr>
      <td style="text-align:left">10</td>
      <td style="text-align:left">Sidechain - Minsk</td>
      <td style="text-align:left">10.20.10.50</td>
      <td style="text-align:left"><a href="http://208.66.16.206">208.66.16.206</a>
      </td>
      <td style="text-align:left">
        <p>sidechain</p>
        <p>web</p>
        <p>bitcoin</p>
      </td>
      <td style="text-align:left">tapin</td>
      <td style="text-align:left">No</td>
      <td style="text-align:left">-</td>
      <td style="text-align:left">Multiples sidechain nodes running with bitcoin daemon in regtest mode.</td>
    </tr>
    <tr>
      <td style="text-align:left">11</td>
      <td style="text-align:left">Mobile Automation</td>
      <td style="text-align:left">192.168.252.147</td>
      <td style="text-align:left">51.38.111.78 ( SSH port 2226)</td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">12</td>
      <td style="text-align:left">Baxter Node / exeBlockChain</td>
      <td style="text-align:left">10.20.10.51</td>
      <td style="text-align:left">208.66.16.207</td>
      <td style="text-align:left">exeblock</td>
      <td style="text-align:left">-</td>
      <td style="text-align:left">No</td>
      <td style="text-align:left">-</td>
      <td style="text-align:left">exeblock blockchain node running</td>
    </tr>
  </tbody>
</table>### **Troubleshooting** <a id="HowToMaintainandTroubleshoottheTestnetEnvironments-Troubleshooting"></a>

### Problem <a id="HowToMaintainandTroubleshoottheTestnetEnvironments-Problem"></a>

### **Commits don't trigger a new build** <a id="HowToMaintainandTroubleshoottheTestnetEnvironments-Commitsdon&apos;ttriggeranewbuild"></a>

### Solution <a id="HowToMaintainandTroubleshoottheTestnetEnvironments-Solution"></a>

For 5050app:

* check if the Jenkins is running. Open [http://208.66.16.201:8086](http://208.66.16.201:8086), login and check the statuses. Select dev-5050-45 project and verify the project is enabled.
* Select the dev-5050-45 project and click 'build now'
* ping 208.66.16.201 when NOT connected with VPN
* Open 5050dapp in Bitbucket, go to Settings and Webhooks. Select the jenking-5050dapp and 'View Requests'. Then click 'Load New Requests'.
* When the new trigger is sent, switch to Jenkins and check whether a new build has been scheduled. If not, likely it is a problem with ports configuration in the ufw firewall. In that case, you have to find what ip address Bitbucket is using to send the webhook from. Confirm with Matt Hunter that the same IP is white-listed in the firewall.

For 5050Mobile:

* Jenkins server is on [http://208.66.16.200:8086](http://208.66.16.200:8086/). The project name in Bitbucket is 5050Mobile.
* The webhook is jenkins-5050-mobile-app.
*  The rest is quite the same, except the 208.66.16.200 is not currently secured with SSL and firewall.

### Problem <a id="HowToMaintainandTroubleshoottheTestnetEnvironments-Problem.1"></a>

### **Build fails** <a id="HowToMaintainandTroubleshoottheTestnetEnvironments-Buildfails"></a>

### Solution <a id="HowToMaintainandTroubleshoottheTestnetEnvironments-Solution.1"></a>

* Open Jenkins, select the project. Click on the latest build and open Console Output.
* Work with the developers to find the root cause.
* If you have physically opened the directories on the 46 machine where the source code of Mobile app is located, jenkins build will fail as it does not seem to clean up the directories. Sample error output: [http://10.20.10.46:8086/job/5050MobileAutomation/3/console](http://10.20.10.46:8086/job/5050MobileAutomation/3/console) Close the directories and rerun the build manually.

### Problem <a id="HowToMaintainandTroubleshoottheTestnetEnvironments-Problem.2"></a>

### **Services are not running after either restart or power outage** <a id="HowToMaintainandTroubleshoottheTestnetEnvironments-Servicesarenotrunningaftereitherrestartorpoweroutage"></a>

### Solution <a id="HowToMaintainandTroubleshoottheTestnetEnvironments-Solution.2"></a>

As for 5050 Web Project the QA build is not being triggered automatically.

* Open Jenkins on 44 Server. Select dev-5050-44 project and click 'Enable project button'.
* Then click 'Build Now'.
* When the build is complete, disable the project again to prevent Jenkins from kicking off new builds.

### Problem <a id="HowToMaintainandTroubleshoottheTestnetEnvironments-Problem.3"></a>

### **QA is not seeing a new build** <a id="HowToMaintainandTroubleshoottheTestnetEnvironments-QAisnotseeinganewbuild"></a>

### Solution <a id="HowToMaintainandTroubleshoottheTestnetEnvironments-Solution.3"></a>

* 42 /43/ 46 / 47 208.66.16.205 / - don't have an automatic recovery in place.
* All components have to be restarted manually.
* Follow the instructions on Confluence how to setup a private testnet \(or EOS github\) and start front and back-end in the nohup mode.
* 44/45/48 – will recover automatically. There is a script start-testnet.sh being executed by Ubuntu \(see start-testnet.service\).
* 46 – On Windows Jenkins is loaded by the Jenkins Windows service. Try to start or restart the service in case if something went wrong.

### Problem <a id="HowToMaintainandTroubleshoottheTestnetEnvironments-Problem.4"></a>

### **The chain stopped producing blocks** <a id="HowToMaintainandTroubleshoottheTestnetEnvironments-Thechainstoppedproducingblocks"></a>

### Solution <a id="HowToMaintainandTroubleshoottheTestnetEnvironments-Solution.4"></a>

It might happen when the server has been shutdown improperly. If this happened:

* stop all 'nohup' services, and reset the chain.
* To reset the chain, first remove the object-database, data/my-blockprod/blockchain folder, delete after\* and before\* files and my-wallet.json.
* Restart the chain and create a new wallet.
* Update tapin config.yml file with the new chain id.

### Problem <a id="HowToMaintainandTroubleshoottheTestnetEnvironments-Problem.5"></a>

### **All changes in the docker container are gone** <a id="HowToMaintainandTroubleshoottheTestnetEnvironments-Allchangesinthedockercontaineraregone"></a>

### Solution <a id="HowToMaintainandTroubleshoottheTestnetEnvironments-Solution.5"></a>

By default Docker doesn't save changes made in containers.

To save the changes:

* Open another terminal and connect to the VM hosting the container
* Run docker container commit \[container-name\] 

## **Daily check up of the Testnets**  <a id="HowToMaintainandTroubleshoottheTestnetEnvironments-DailycheckupoftheTestnets"></a>

**\*\*Opening 5050app in incognito should produce a message**

1. Public Testnet \(https://10.20.10.48 / [https://208.66.16.204](https://208.66.16.204/) / https://5050labs.fun\)   - Disconnect exeBlock VPN  - Open [https://5050labs.fun](https://5050labs.fun)     **Checkup routine**:            - Type user and password \(see the [Access to exeblock Testnets](https://peerplays.atlassian.net/wiki/spaces/EKB/pages/197460287/Access+to+exeblock+Testnets)  document\)            - Login with your existing account \(or use this one: tst / YdYDHb3F89AsoJM5YloaM0i8paEMcaZRQAGtX5Cs3oHyPT0Ty4cC \)            - Inspect the 5050app: History, Draws, My Account            - Log out            - Test the tapin faucet by opening the [https://5050labs.fun:3001/api/v1/accounts](https://208.66.16.204:3001/api/v1/accounts) link            -  By default the faucet returns 'Method not allowed' html page               - Repeat the Checkup routine after replacing [https://5050labs.fun](https://5050labs.fun) with [https://208.66.16.204](https://208.66.16.204/) \(test the faucet with [https://208.66.16.204](https://208.66.16.204/):[3001/api/v1/accounts](https://208.66.16.204:3001/api/v1/accounts)\)  - Connect to exeBlock VPN  - Open the [https://10.20.10.48](https://10.20.10.48)  - Repeat the Checkup routine \(test the faucet with [https://10.20.10.48](https://10.20.10.48):[3001/api/v1/accounts](https://208.66.16.204:3001/api/v1/accounts)\)  
2. Development Testnet \(https://10.20.10.45 / https://208.66.16.202\)   - Disconnect exeBlock VPN  - Open https://208.66.16.202  - Make sure 5050 web app is being loaded  - Login and inspect the application  - Log out  - Connect to the VPN  -  Open 10.20.10.45  - Make sure 5050 web app is being loaded  - Login and inspect the application  - Log out  
3. QC \(Staging\) Testnet \([https://10.20.10.44](https://10.20.10.44) / https://208.66.16.201 \)  - Disconnect exeBlock VPN  - Open https://208.66.16.201  - Make sure 5050 web app is being loaded  - Login and inspect the application  - Log out  - Connect to the VPN  -  Open [https://10.20.10.44/home](https://10.20.10.44/home)  - Make sure 5050 web app is being loaded  - Login and inspect the application  - Log out  
4. Jenkins Web Server   - Open 10.20.10.44:8086  - Type Jenkins credentials \(exeblock / Bunker$oct10!\)  - Make sure you can see the main page  - Select the dev-5050-45 build plan, click 'Configure'  - Go to the 'Source Code Management' section and check that the default Bitbucket credentials are ok and there is no error displayed  - Go to the 'Build Triggers' section and make sure that the 'Build when a change is pushed to BitBucket' is checked  - Check the status of the last build. If the build failed, open the 'Console Output' and investigate the issue.   - Go back to the main page  - Select qc-5050-44 build plan, click 'Configure'  - Go to the 'Source Code Management' section and check that the default Bitbucket credentials are ok and there is no error displayed  - Go to the 'Build Triggers' section and make sure that NONE of the options is checked. \*\*\* NOTE: The QC build should be triggered manually upon QC \(Alik\) request  - Check the status of the last build. If the build failed, open the 'Console Output' and investigate the issue.  
5. Jenkins Mobile \(Xamarin Server\)  - Open 10.20.10.46:8086  - Type Jenkins credentials \(exeblock / exeblock123\)  - Make sure you can see the main page  - Select the 5050mobile-46-v3 build plan, click 'Configure'  - Go to the 'Source Code Management' section and check that the default Bitbucket credentials are ok and there is no error displayed  - Go to the 'Build Triggers' section and make sure that the 'Build when a change is pushed to BitBucket' is checked  - Connect remotely to the VM \(using RDP\)  - Open Visual Studio 2017  - Check if there are updates available. Update if needed.  - Remove all files from Windows temporary folders \(Temp\\)   - Install available Windows updates  - Open Windows Services panel, select Jenkins service, check the service status \(Running\), make sure the service is running under 'exeblock' account  - Open Event Viewer  - Open 'Windows Logs', 'Application'. Click 'Filter Current Logs'  - Select 'Jenkins' in the 'Event sources' drop down list, click 'OK'  - Inspect events, and investigate any errors occurred.  
6. \(Temp Step \) Steps that needs to added to everyServer\( check available space, clean up log/temp files   
7. Jenkins Server on 49 \( Not part of daily checkup routine as of now \)  10.20.10.49:8086 Uname : devops password : exeblock123  
8. Exe Explorer - Currently only deployed in 45 testnet \( contents to be added when app is deployed in additional testnets/features added \)  dev.5050labs.fun:5000 Make sure the app is running/ Page loads 10.20.10.45:5000 Make sure the app is running/ Page loads

Added Checkup Scripts for our testnets/applications/webservices. Checks should be added as our apps/testnets grow  
/home/exeblock/checkup-scripts/checkTestnetup.sh \( Available on 44 and 45 \)

*  Page: [How To Maintain and Troubleshoot the Testnet Environments](/wiki/spaces/EKB/pages/197460129/How+To+Maintain+and+Troubleshoot+the+Testnet+Environments)

## Useful paths <a id="HowToMaintainandTroubleshoottheTestnetEnvironments-Usefulpaths"></a>

44: Jenkins folder

/home/exeblock/jenkins

/home/exeblock/.jenkins \(hidden folder\)

/home/exeblock/.jenkins/workspace

/home/exeblock/.jenkins/jobs  
  
Log files can be located in:  
/home/exeblock/.jenkins/jobs/&lt;build-name&gt;/builds/&lt;build-number&gt;/log

  
config files can be located in:  
/home/exeblock/.jenkins/jobs/&lt;build-name&gt;/config.xml

