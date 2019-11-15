# EXEBlock Knowledge Base : How to set up 50-50 Development Environment

  
 **exeBlock 50/50 dapp**  
 **Setting up development environment** \(Windows\)  
 The purpose of this document is to provide step by step installation of all required software components, a configuration of these components and illustrating how to debug a code using Chrome Browser.  
 Installation on Windows 10 \(installation on Mac has not been verified yet\):

1. Create a work folder \(will be referred as a \[Work Folder\]\). For example: [C:\Work](null/pages/createpage.action?spaceKey=EKB&title=..%2F..%2F..%2FWork&linkCreation=true&fromPageId=197460043)     
\*Note: The created folder can be setup as Windows Work Folder if you want to synchronize this folder across multiple PCs \(see the link for more information: [https://docs.microsoft.com/en-us/windows-server/storage/work-folders/work-folders-overview](https://docs.microsoft.com/en-us/windows-server/storage/work-folders/work-folders-overview)\).

2.  Download and install Git\([https://git-scm.com/downloads](https://git-scm.com/downloads)\)

3.  Download and install Git GUI Client \([https://git-scm.com/downloads/guis](https://git-scm.com/downloads/guis)\)

4.  Open GitHub Desktop

5.  Click 'File' and 'Clone Repository'

6.  Select 'URL'

7.  In a web browser, connect to the Exeblock github repository for 5050 at [https://bitbucket.org/exeblock/5050dapp](https://bitbucket.org/exeblock/5050dapp) and retrieve your download link \([https://\[your user\]@bitbucket.org/exeblock/5050dapp.git](https://exeblock-cpower@bitbucket.org/exeblock/5050dapp.git)\). Copy this URL and paste it into the GitHub GUI URL input. 

8.  Select \[Work Folder\]\5050-stage\' as a local path \(Example: C:\Work\5050-stage\)

9.  Click 'Clone'

10. Open [https://bitbucket.org/exeblock/5050dapp/src/stage/](https://bitbucket.org/exeblock/5050dapp/src/stage/) in your web browser.

11. Click 'Clone or download' button

12. Click 'Download ZIP' button

13. Extract the downloaded zip file to the \[Work Folder\].

After extracting the files, the folders tree should look like this:  


14. Download the latest version of VSCode \([https://code.visualstudio.com/](https://code.visualstudio.com/)\)

15. Install VSCode using default settings

16. Download and install Node.js LTS from: [https://nodejs.org/en/](https://nodejs.org/en/)

17. Open VSCode

18. Click 'File' and 'Open Folder'

19. Select \[Work Folder\]\5050-stage folder

20. Click 'View' and 'Integrated Terminal'

21. Execute 'npm install ' command to install all required packages.

22. Click 'View' and 'Extensions'

23. Install 'Debugger for Chrome' extension.

24. Click 'Debug' and 'Start Debug' – make sure the launch.json file is created.

25. Open launch.json file and replace its content with:

{  
    "version": "0.2.0",  
    "configurations": \[  
    {  
       "name": "Launch Chrome against local host",  
       "type": "chrome",  
       "request": "launch",  
       "url": "[http://localhost:5000/dashboard](http://localhost:5000/dashboard)",  
       "webRoot": "${workspaceRoot}",  
       "sourceMaps": true,  
       "trace": true,  
       "sourceMapPathOverrides": {  
       "/**": "${webRoot}/**",  
       "webpack:///**": "**",  
       "webpack:///./**": "${webRoot}/**",  
       "[webpack:///src/](exeblock-knowledge-base-how-to-set-up-50-50-development-environment.md)**": "${webRoot}/**",  
       "webpack:///./~/**": "${webRoot}/node\_modules/**"  
       }  
    },  
  \]  
 }

26. Open package.json file in \[Work Folder\]\5050-stage folder

27. After "precommit": "lint-staged" add comma and click enter

28. Paste these two rows:

```text
      "build": "SET NODE_ENV=development&&SET NODE_CONFIG=dev&&webpack --config webpack/webpack.config.js", 
      "start": "serve ./dist -s"
      (***Note: to run the build on Ubuntu it is required to:
         1. Update node and npm packages.
           curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
           sudo apt-get install -y nodejs
         2. Replace "build": "SET NODE_ENV=development&&SET NODE_CONFIG=dev&&webpack --config webpack/webpack.config.js"' 
            with    "build": "export NODE_ENV=development&&export NODE_CONFIG=dev&&webpack --config webpack/webpack.config.js"
```

29. Open Chrome and Extension

30. Install 'React Developer Tools' extension.

31. Enable SorceMap in Chrome:

* Press Ctrl+Shift+J \(or CMD-Option-I in Mac\)
* Switch to 'Sources'
* Select 'Customize and control DevTools' on the right
* Click 'Settings'
* Under 'Sources' check 'Enable JavaScript source maps' option.
* Now open up the Chrome flags settings by entering "chrome://flags/" into the URL bar.
* Click "Enable" under the "Enable Developer Tools experiments" flag.
* Restart Chrome.

32. Configure 5050 blockchain and faucet:

1. Open up VSCode
2. Open webpack/config.dev.json file
3. Configure Blockchain and Faucet URLs \(The exeBlockTestnet URLs to be determined, the shown URL's are of Minsk's Testnet\):

{ "BLOCKCHAIN\_URL": "[wss://595-dev-blockchain.pixelplex.by/ws](exeblock-knowledge-base-how-to-set-up-50-50-development-environment.md)", "FAUCET\_URLS": \[ "[https://595-dev-faucet.pixelplex.by/api/v1/accounts](https://595-dev-faucet.pixelplex.by/api/v1/accounts)" \], "BITSHARES\_WS": "[wss://bitshares.openledger.info/ws](exeblock-knowledge-base-how-to-set-up-50-50-development-environment.md)", "SOFTWARE\_UPDATE\_REFERENCE\_ACCOUNT\_NAME": "ppcoreupdates", "ELECTRON": false}

33. In the Terminal: run 'npm run-script build' command

34. When the build is complete, press Ctrl+C to close the build session \(Terminate batch job \(Y/N\)? Yes

35. In the Terminal: run 'npm start' command.

36. Click 'Debug' and 'Start Debugging'.

37. In Chrome window that just opened, navigate to localhost:5000/

38. Make sure the Sign-in page is open

39. Click F12 or open Developer Tools from the menu

40. Navigate to 'Sources', select main.\[number\].js file, and make sure that source maps are detected:

41. Set a breat point:

* In the main.\[number\].js java script file find a SignUp method:

  ```text
        navigateToSignUp() {
           this.props.navigateToSignUp();
         }
  ```

* Set a breakpoint at the this.props.navigateToSignUp\(\):
* Click 'Sign Up' button
* The code is suppsed to stop at the breakpoint in both Chrome and VSCode
* In Chrome:
* in VSCode:

  
 In an nutshell, the VSCode 'Debugger for Chrome' extension intercepts Chrome events for debugging sessions, and fires them over to VSScode IDE.

That is why VSCode debugger's behaviour is literally the same as of Chrome. It is just a matter of convenience of being able to modify code, build, run and debug without leaving VSCode IDE.  
  
 That should be all! Happy debugging!  
  


