---
title: Getting Started
template: article.jade
order: 1
----
###Getting to Know Git

Before we get started we need to introduce ourselves to Git.  This is important because beyond tracking what has changed, Git is also going to track who made the changes.

	git config

Git has a configuration where it tracks things like who you (the user) are.  You can modify this configuration using the <code>git config</code> commands.

	git config user.name Me Myself
	git config user.email me.myself@example.com

These commands will identify who you are to Git.  They will store this information in the current repository which may be fine.  If you don't want to have to keep entring this information for each repository however you can run the commands with the global flag:

	git config --global user.name Me Myself
	git config --global user.email me.myself@example.com

Now Git will remmeber these settings for all repositories for you.  This is probably how you want to edit most of the config settings (globally) in most circumstances.  While we are editing the configuration we should tell Git what editor we want to use with Git.  This command looks like this:

	git config --system core.editor C:\Windows\notepad.exe

Note in this case we have updated the system configuration which will apply to anyone using Git on this machine.  Git looks for the first place to find a configuration setting.  It looks first in the repository config (no command line switch), then your user config (--global) and finally in the system config (--system) file.  There are a couple more settings you should tend to.  Git does not have a tool for showing file differences.  You can use any tool that you like.  

	git config --global merge.tool vimdiff

  Finally, you may want to set how Git handles end of line (EOL), characters.  Anytime you are sharing text files across multiple computers there is the question of how lines are ended.  If you do not know there are differences to the characters that mark the end of a line on different computers.  The trick is that you don't want Git telling you that all the lines in the file are different because someone on a different system edited the file and their editor replaced all the end of line markers.  Git can handle this situation for you.  The autocrlf feature will convert Windows line ending to Unix line ending when the files are put back into the repository. 

	git config --system --core.autocrlf true

Now that Git is setup, let's add some files.

	Git init

To start working we need a repository.  Git creates the repository when you run the `get init` command.  Git will create a folder called `.git`.  There is nothing in this folder you need to look at.  It is importand that this folder is being backed up somehow as all of the change history that is being tracked is in this folder.

  So now that we have run `git init` we have a repository but there are no files in it.  Not very interesting.

	Git add

  Not surprisingly the command to add files in git is `git add`.  This adds the files to a staging area.  Since Git was designed around a command line interface (CLI) there needed to be a way to select the files you want to act on.  This staging area is that list.  People will often use `get add .` or `git add -A`.  The first command `git add .` will add files that are new or that have been modified to the staging area.  `Git add -A` will add new files, modified files and note any files that have been removed.  For this tutorial we will use a shared directory for our examples.  This directory will have a line for each person with their name, phone number and file name of their picture.  Let's say Fred Foyle is starting this directory, so he creates a file with his information and his picture.

Now that our list of files has been staged we can "look" at the list using the `git status` command.

	# On branch master
	#
	# Initial commit
	#
	# Changes to be committed:
	#   (use "git rm --cached <file>..." to unstage)
	#
	#	new file:   Directory.txt
	#	new file:   ffoyle.jpg
	#

  Note that Git shows that the files are new and have been added to the staging area.  It also notes that we are on branch master, which is the default main branch of a Git repository.

  Once we have our changes added to the staging area, we commit them to the repository.

    git commit -m "Initial commit"

  This command will add the changes we had staged to the repository with the comment message _Initial Commit_.  The output will look something like this:

	[master (root-commit) 7576855] Initial commit
	 2 files changed, 1 insertion(+)
	 create mode 100644 Directory.txt
	 create mode 100644 ffoyle.jpg

  A couple of things to note.  The `[master  7576855]` tells you that the commit was added to the master branch (more on branches later) and the commit has an ID of 7576855.  The ID will become important when we start [SharingWithOthers](/pages/sharing-with-others.html).  Note that the commit records all the changes, in this case both files.  Also note that though Git allows for adding binary files (like ffoyle.jpg) it will not track differences in binary files.

  Now that we have a start our man fred decides to add his son to the directory.  After adding his son's name to the directory Fred copies a picture also.  Then if you run git status you will see something like this:

	# On branch master
	# Changes not staged for commit:
	#   (use "git add <file>..." to update what will be committed)
	#   (use "git checkout -- <file>..." to discard changes in working directory)
	#
	#	modified:   Directory.txt
	#
	# Untracked files:
	#   (use "git add <file>..." to include in what will be committed)
	#
	#	MichealF.jpg
	no changes added to commit (use "git add" and/or "git commit -a")

  See that git reminds you that you will need to add the new file.  So let's do that:

	git add MichealF.jpg

  Now `git status` shows that the files are both in the index.

	# On branch master
	# Changes to be committed:
	#   (use "git reset HEAD <file>..." to unstage)
	#
	#	new file:   MichealF.jpg
	#
	# Changes not staged for commit:
	#   (use "git add <file>..." to update what will be committed)
	#   (use "git checkout -- <file>..." to discard changes in working directory)
	#
	#	modified:   Directory.txt
	#

  Now we just need to commit these changes.

	git commit -m "Added Micheal"

  The output looks something like this:

	[master f6d26b1] Added Micheal
	 1 file changed, 0 insertions(+), 0 deletions(-)
	 create mode 100644 MichealF.jpg

  So `git add .` will add all the files and `git add MichealF.jpg` adds that one file.  If you have several files to add it may become tedious to add them one at a time, so git allows you to add them interactively.

	git add -i

  Then follow the prompts.  Experience with other version control systems suggests that these are very small changes to be commiting.  With Git it is better to make many small commits.  We will learn later how these can be made more meaningful.  Also because these changes are committed to the repository on your machine no one else will see them until you are ready to share them.  Note that you do not have to plan for the files you wish to modify.  You just make changes and Git notices those changes, you stage them and then commit them.  If you are working on your own this is all the workflow that you need.  As with any endeavor you will make mistakes from time to time, so let's talk about [fixing mista<span style="text-decoration: line-through;">t</span>kes](/pages/fixing-mistakes.html).

