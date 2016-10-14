newFileWithPrompt
=======================
2016-10-14


On mac, create a new file in one click and prompting you for the filename.

The code comes from here: http://www.codium.co.nz/touch_here_app/

(I simply put it my repo because it will be easier to find the next time I'll look for it)




How to
-------------

1. Launch script editor 
2. Copy paste the code below in it:

```applescript
tell application "Finder"
	try
		set selectedItem to selection
		set currentPath to ((the first item of the selectedItem) as alias)
		set parentPath to ""
		
		if (currentPath as string) ends with ":" then
			-- it is a folder
			set the parentPath to currentPath
		else
			-- it is a file
			set {od, AppleScript's text item delimiters} to {AppleScript's text item delimiters, ":"}
			set the parentPath to (text items 1 thru -2 of (currentPath as string)) as string
			set AppleScript's text item delimiters to od
		end if
	on error -- no folder or file is selected
		set the currentPath to (folder of the front window as alias)
		set parentPath to currentPath
		
		
	end try
	set newFolder to (my createFile(parentPath))
	
end tell
quit

on createFile(folderLocation)
	set myNewFileName to text returned of (display dialog "Enter the name of your new file" default answer "")
	
	tell application "Finder"
		set thisFolder to make new file at folderLocation with properties {name:myNewFileName}
		
		
		set selection to thisFolder
		--tell application "System Events"
		--	keystroke return
		--	quit -- if i don't do this system events seems to hang???
		--end tell
		return thisFolder
	end tell
	
end createFile

on createFolder(folderLocation)
	tell application "Finder"
		set thisFolder to make new folder at folderLocation
		
		set selection to thisFolder
		tell application "System Events"
			keystroke return
			quit -- if i don't do this system events seems to hang???
		end tell
		return thisFolder
	end tell
end createFolder
```

3. Save as an app in a location (you cannot trash the file after, it has to be somewhere on your machine)
4. Now right click on any finder window's tool bar > Customize toolbar
5. Drop your app inside of the toolbar (you may need to open two windows to do that)

6. Congrats! You can now create a new file in one click.








