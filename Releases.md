# Release Checklist

The following steps should be taken to release a new version of the software.

1. Increment versions in the following projects:
	1. StatTag
	2. Core
	3. Stata
	4. SAS
	5. R
2. Reload the OfficeAddInSetup-x86 project
	1. Under Organize Your Setup > General Information
		1. Update the Product Version
		2. Regenerate the Product Code
		3. Change the upgrade path to include the previous version
	2. Change the build configuration to "Release"
	3. Change the platform to "x86"
	4. Clean and then rebuild the entire solution
	5. Copy the setup file to the shared releases folder
3. Unload the OfficeAddInSetup-x86 project and reload the OfficeAddInSetup-x64 project
	1. Repeat steps 2.1 and 2.2
	2. Change the platform to "Any CPU"
	3. Perform steps 2.4 and 2.5


## Notes
The x64 and x86 Upgrade Code (which should never be changed) is: {AEC2B59D-045C-4D6D-A4B4-1C87125EA095}

We have purposely kept the same upgrade code for all platforms.



## Additional information
Setting up InstallShield to allow upgrades was found at: [http://stackoverflow.com/a/13874942/5670646](http://stackoverflow.com/a/13874942/5670646)

Original setup instructions were:


> "In your InstallShield setup project, you should do the following:

> * select branch: Organize your setup -> Upgrade Paths
* add new upgrade path and than press the cancel button
* the default properties of the new upgrade path should not be changed if you do not plan to change the Product version from the following branch: Organize your setup -> General Information. If you plan to change the Product version, than you should play with the following upgrade path properties: Min Version/_Include Min Version_, Max Version/_Include Max Version_.
* every time you need to create a new setup, change the Product code from the following branch: Organize your setup -> General Information.
* please be aware that the Upgrade Code should NEVER be changed."