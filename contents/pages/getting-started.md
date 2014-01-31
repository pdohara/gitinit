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

Now Git will remmeber these settings for all repositories where you have not set them.  This is probably how you want to edit the config (globally) in most circumstances.  While we are editing the configuration we should tell Git what editor we want to use with Git.  This command looks like this:

	git config --system core.editor C:\Windows\notepad.exe

Note in this case we have updated the system configuration which will apply to anyone using Git on this machine.  Git looks fir the first place to find a configuration setting.  It looks first in the repository config, then your user config and finally in the system config file.

  TODO 

  git config --core.autocrlf true

Now that Git know who we are let's add some files.

	Git init

To start working with our DVCS, the tool is going to need a place to do it's work.  This is both the scratch pad the tool uses and the location of the version information for the changes we are tracking.  Git creates this space when you run the `get init` command.  Git will create a folder called `.git`.  There is nothing in this folder you need to look at.  It is importand that this folder is being backed up somehow as all of the change history that is being tracked is in this folder.

  So now that we have run `get init` we have a repository but there are no files in it.  Not very interesting.

	Git add

Not surprisingly the command to add files in git is `git add`.  This adds the files to a staging area.  Since Git was designed around a command line interface (CLI) there needed to be a way to select the files you want to act on.  This staging area is that list.  People will often use `get add .` or `git add -A`.  The first command `git add .` will add files that are new or that have been modified to the staging area.  `Git add -A` will add new files, modified files and note any files that have been removed.

Now that our list of files has been staged we can "look" at the list using the `git status` command.

    # On branch master
    # Changes to be committed:
    #   (use "git reset HEAD &lt;file&gt;..." to unstage)
    #
    #       new file:   GettingStarted.html
    #       modified:   index.html
    #
    # Untracked files:
    #   (use "git add &lt;file&gt; to include in what will be committed)
    #
    #       FixingMistakes.html
    #       SharingWithOthers.html

Note that git show that the file GettingStarted.html is a new file that has been added to the staging area.  The index.html file is a file that was already in the repository and has been modified.  The files FixingMistakes.html and SharingWithOthers.html are untracked or not in the staging area.
  Once we have our changes added to the staging area, we commit them to the repository.

    git commit -m "Initial commit"

This command will add the changes we had staged to the repository with the comment message _Initial Commit_.  The output will look something like this:

    [master ca93a4c] Started working on Getting Started page
    2 files changed, 70 insertions(+)
    create mode 100644 GettingStarted.html

A couple of things to note.  The `[master  ca93a4c]` tells you that the commit was added to the master branch (more on branches later) and the commit has an ID of ca93a4c.  The ID will become important when we start [SharingWithOthers](/pages/sharing-with-others.html).

    git add [filename]

Although it is common to wish to add everything that has changed, sometimes we want to be a little more selective.  The command `git add GettingStarted.html` will add only the file specified.  Another way to be selective is to use the command `get add -i`.  This command will present a menu that will walk you through adding files and even changes (hunks) within files that you wish to stage.

TODO Talk about how a commit captures all the changes, not just the changes to a specific file.

Note that you do not have to plan for the files you wish to modify.  You just make changes and Git notices those changes, you stage them and then commit them.  If you are working on your own this is all the workflow that you need.  As with any endeavor you will make mistakes from time to time, so let's talk about [fixing mista<span style="text-decoration: line-through;">t</span>kes](/pages/fixing-mistakes.html).

Note that because this is all on your disk, you can (and should) commit files routinely.  In centralized version control systems commiting files is often avoided because they are immediately shared with everyone else on the team.  In Git sharing comes later, when you decide it is time too, so go ahead and commit changes as you go.

TODO Add another commit here and talk about Changesets as apposed to State.

