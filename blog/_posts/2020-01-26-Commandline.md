---
layout: post
title: "Command Line"
date: 2020-01-26
---

__Table of Contents__
 - [Navigating](#navigate)
 - [Modifying](#modify)
 - [Git](#git)

<a id="navigate"></a>
### Navigating

`pwd` - print working directory. prints where you are currently at
`ls` - lists contents in current folder
`cd` - brings you back to home folder
`cd ~/World` - brings you to a specific folder in the stated path. 

`clear` - clears all scrollback (all the text above). does not clear history. press `command + up-arrow` to scroll back up.


<a id="modify"></a>
### Modifying

`mkdir` - makes a new directory (folder)
`rm` - removes a directory. including `-r` deletes all subfolders as well. permanently deletes the file. if you would like a prompt before you actually delete it, include an `-i` (aka press `-ri` instead).
`mv filename location` - moves a directory. if there is already a file with the same name in the folder, it will automatically overwrite it (!) 
`mv filename newfilename` - renames a file. also automatically overwrites anything with the same name (!)


<a id="git"></a>
### Git

create a file, and make a change.

`git init` - initialises git
`git status` - checks on the status. highlights untracked files
`git add scene-1.txt` - adds files to be tracked

make another change in the file.

`git diff scene-1.txt`- tracks and shows you the changes made to the file
`git commit -m "Completed Scene 1"` - commits (saves) the changes to the repository with the comment

`git log` - returns the log of the changes made