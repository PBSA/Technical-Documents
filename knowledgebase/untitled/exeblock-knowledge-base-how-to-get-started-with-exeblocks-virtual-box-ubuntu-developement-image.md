# EXEBlock Knowledge Base : How to get started with EXEBlock's Virtual Box Ubuntu Developement image

This Virtual Box appliance gets you a copy of Ubuntu Desktop, 16.04 with all of the tools necessary to begin debugging the C++ chain code installed and configured, and all of the bloat from the typical Ubuntu Desktop removed.

If you run into any issues getting this going, or you think the way I've explained anything is too confusing - feel free to ping me \(Hunter\) for assistance.

## Step 1 - Start downloading the appliance image <a id="HowtogetstartedwithEXEBlock&apos;sVirtualBoxUbuntuDevelopementimage-Step1-Startdownloadingtheapplianceimage"></a>

Grab a copy of the image - it's 4.7GB, so do it on a good connection.  It's currently in Dropbox but will be put in google drive soon:

[https://www.dropbox.com/sh/ygma0ggsebo99lu/AACdMCYLTztyDbYc2psADZSRa?dl=0](https://www.dropbox.com/sh/ygma0ggsebo99lu/AACdMCYLTztyDbYc2psADZSRa?dl=0)

## Step 2 - Install Virtual Box <a id="HowtogetstartedwithEXEBlock&apos;sVirtualBoxUbuntuDevelopementimage-Step2-InstallVirtualBox"></a>

Download a copy of Virtual Box from [https://www.virtualbox.org/](https://www.virtualbox.org/) and install it on your development system.

_**Important information for Mac users:**_ 

Apple tries to keep users safe by allowing only software obtained from it's App Store or 'identified developers' to be installed by default.  This **WILL** cause us some issues installing Virtual Box, but with a few quick tips and tricks we can get through this quickly.  The first time you try to install Virtual Box on your Mac you'll get an error and the end of the install process.  While the error is anything but informative as to what went wrong - we know that it's because MacOS blocked the install.  So the next step is to fix this.

Getting MacOS to let us install Virtual Box **should** be as easy as clicking a button in System Preferences.  This, however, won't work for everyone - it didn't work for me and cost over two hours of time trying to find a solution.  With the following guide, you should be past it in a matter of minutes.

**Solution 1 - Try to click the 'Allow' button**

* Open up System Preferences \(the gear wheel on your dock\)
* Go to Security and Privacy
* You'll see a note there that a software install by Oracle was blocked - and there's a button to Allow it. \(If you don't see this, then you likely haven't tried to install Virtual Box yet - you have to try to install it and let it fail for this to show up\).
* Click 'Allow'
* If you're lucky, you'll be prompted for your password and you'll now be able to go back to the Virtual Box installer and re-run it successfully.
* If, however, you click Allow elevnty-five times and nothing happens, move on to solution 2.

**Solution 2 - I swear to god - my trackpad IS attached to my Mac - come on Apple!**

* Just blocking instals from unidentified developers wasn't enough for Apple.  Those clever bastards also thought of the scenario where a rogue user manages to access your MacOS desktop remotely, and then installs all the nefarious software they want by remotely clicking that Allow button.  So fear not - the wise wizards of Apple have magically enchanted the Allow button so that it will only respond to clicks that come from a device that is physically connected to the machine!  And just to keep that evil software-installing villain guessing, they don't display any error message explaining this when you click 'Allow' over and over.  And over.  And over.
* But wait... my trackpad if built right into my Mac!  How can it possibly think that's NOT part of my physical device?  Well, it turns out there's a lot of software out there that installs middleware between your mouse/trackpad and the OS - to facilitate smart gestures and other fun tricks.  You might not even realize you have anything like that installed.  But this middleware PROXIES your clicks between your trackpad and your OS...  which - you guessed it - makes the OS think they came from somewhere other than the physical device!
* So, you could go on a hunt to find and uninstall any piece of software that might be doing this - but there's an easier way:
* Go to the root of System Preferences again
* Click on Keyboard
* Click on the tab 'Shortcuts'
* There's a radio button near the bottom for 'Full Keyboard Access' - change this to 'All Controls'
* Now go back to the root of System Preferences, and click on Security and Privacy again.
* Using the TAB key, tab until you see the Allow button get highlighted.  If you get a little tab-happy and tab to far, just keep tabbing until you get back there.
* With the Allow button highlighted, press the SPACE bar to 'click' it.
* Go back and re-run the Virtual Box installer.

## Step 3 - Import the appliance from the image <a id="HowtogetstartedwithEXEBlock&apos;sVirtualBoxUbuntuDevelopementimage-Step3-Importtheappliancefromtheimage"></a>

Once your image file from Step 1 is downloaded:

* Open Virtual Box
* From the File menu, select Import Appliance
* Locate the image file and continue.
* You should be able to accept all the default values that are set for the appliance and continue

## Step 4 - Using the Virtual Box image <a id="HowtogetstartedwithEXEBlock&apos;sVirtualBoxUbuntuDevelopementimage-Step4-UsingtheVirtualBoximage"></a>

**Username: exeblock**

**Password: pw4exeblock**  _\(you should change the password to something of your choosing upon logging in\)_

_**Some Quick Notes on the VM's setup:**_

* VS Code is installed, along with extensions for debugging and C++.  You'll see it's icon on the dock on the left-hand side of the desktop
* If you open Terminal and look in the folder ~/development there are two projects checked out.  peerplays-graphene \(our copy of the peerplays repo\) and peerplays-alice \(where we're doing the 5050 merge\).  These were cloned here to facilitate testing the building and debugging environment.  The project peerplays-graphene will contains an important example of configuring the project for debugging.  More on that below.
* I've removed most of the bloat that's included with the 16.04 ubuntu desktop - all the office software, and a lot of other crap in an attempt to make a lean copy for this purpose

**Getting started:**

* If you're setup to use SSH Keys for bitbucket, you will want to copy them over from your machine to this VM to allow seamless access into the exeblock repos.
* Open up VS Code.  By default it will launch the last project opened, so you should be looking at the launch.json file for the peerplays-graphene project.
* launch.json and tasks.json are two new files I've created here using VS Code that define build and debug tasks.  When you checkout a new project, you can use these files as a jumping-off point to enable debugging in those projects.
* Whenever you run cmake to configure a new project for building, make sure to include the flag: -DCMAKE\_BUILD\_TYPE=Debug

**tasks.json**

The tasks file can be used to create external tasks for VS Code - allowing it to work with external software.  Here we're using it to create the build process.  VS Code has a special task group 'build' where we can set 'isDefault' so that when we tell it to build the project this is the task that gets run.

There are a couple of important elements to note:

'args' :  your 'command' element can only contain the name \(or path/name\) of the executable.  You must put any arguments to pass in here.

'identifier' : This lets you call this task from other tasks and must be unique inside this project.  I've called our build task make\_witness\_node

**launch.json**

The launch file is used by our debugging setup.  Some important things to note:

args : You can see an example here of passing in multiple arguments.  You might find it odd that "--data-dir" and "data/my-blockprod" are passed in as two separate args - but this is the correct way to do it, and trying to pass them in as a single argument won't work.  Essentially, whenever there's a space, it's a new argument.

preLaunchTask: Here we're telling it that before we start debugging, the task make\_witness\_node should be run.  This ensures your latest changes are built before the debugging starts.  If you've already built, this process only takes a few seconds - so it's not going to get in your way.

**Starting Debugging**

This should be pretty straightforward at this point.  Just like any other IDE, you can add breakpoints by clicking to the left of the line numbers in your editor.  When you're ready to begin, simply choose 'Start Debugging' from the Debug menu.  This will automatically run the build process, then will launch the node using the args in launch.json.  If you need to launch with different arguments, simply change launch.json

The debugger will spawn a terminal window \(might be hidden behind VS Code, but if you look for it - it's there\) which will display the STDOUT/STDERR of the program, and in VS Code the debugging interface will load with a debug console, and your typical pause/step-into/step-over/etc commands on a toolbar.

Happy Debugging! 

## Related articles <a id="HowtogetstartedwithEXEBlock&apos;sVirtualBoxUbuntuDevelopementimage-Relatedarticles"></a>

*  Page: [How to set up Linux testnet environment](/wiki/spaces/EKB/pages/197460035/How+to+set+up+Linux+testnet+environment)
*  Page: [How to Setup EXEchain](/wiki/spaces/EKB/pages/197460257/How+to+Setup+EXEchain)
*  Page: [How to setup Sidechain environment](/wiki/spaces/EKB/pages/197460072/How+to+setup+Sidechain+environment)
*  Page: [EVM Smart Contract testing](/wiki/spaces/EKB/pages/197460007/EVM+Smart+Contract+testing)
*  Page: [How to Create Smart Contract](/wiki/spaces/EKB/pages/197460297/How+to+Create+Smart+Contract)

