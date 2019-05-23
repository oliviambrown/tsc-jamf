#!/bin/bash

#version 2: updated to point to separate policies depending on the devices current OS.

loggedInUser=`python -c 'from SystemConfiguration import SCDynamicStoreCopyConsoleUser; import sys; username = (SCDynamicStoreCopyConsoleUser(None, None, None) or [None])[0]; username = [username,""][username in [u"loginwindow", None, u""]]; sys.stdout.write(username + "\n");'`

jhpath="/Library/Application Support/JAMF/bin/jamfHelper.app/Contents/MacOS/jamfHelper"

#Grab the OS version
userOS=$( sw_vers -productVersion )

#From the userOS variable, get the OS major version, 10 for yosemite, 11 for el cap, 12 for sierra, etc
osMajor=$( /bin/echo "$userOS" | /usr/bin/awk -F. '{print $2}' )

Title="$4"
Heading="$5"
Msg="$6"
Icon="$7"
ssLink="$8"

Butt1="Update!"
Butt2="Later On.."

#jamf helper pop-up. The button click determines the action taken. 0=update by going to SS, 1=later on.
Action=`"$jhpath" -windowType hud -title "$Title" -icon "$Icon" -iconSize 125 -heading "$Heading" -description "$Msg" -button1 "$Butt1" -button2 "$Butt2" -defaultButton 1 -cancelButton 2`

if [[ $Action == 0 ]]; then 
	#Here use the osMajor to determine which policy to point to.
	echo "Current OS Version: $userOS"
    echo "Button1 is pressed"
	if [[ $osMajor -eq 11 ]]; then
		echo "I am 11!"
		#point to the 10.11 only SS policy
		su -l $loggedInUser -c "open /Applications/Summit\ Self\ Service.app/ 'jamfselfservice://content?entity=policy&id=47&action=view'"
	else
		#point to the everyone else SS policy
		su -l $loggedInUser -c "open /Applications/Summit\ Self\ Service.app/ 'jamfselfservice://content?entity=policy&id=38&action=view'"
	fi
fi 

#Wishlist: after a certain time, the nag level escalates to level 2 automatically
