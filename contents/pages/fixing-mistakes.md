---
title: Fixing Mistakes
template: article.jade
order: 2
----

## Taking Stock

  Let's take a look at what we have done so far:

	git log
	
	commit f6d26b1b816ff88a7544e3ac605fda59fae26413
	Author: Fred Foyle <fred.foyle@example.com>
	Date:   Sat Feb 1 20:44:51 2014 -0600
	
	    Added Micheal
	
	commit 7576855c65228c16cbf6151bcd68f167791e4958
	Author: Fred Foyle <fred.foyle@example.com>
	Date:   Sat Feb 1 20:23:13 2014 -0600
	
	    Initial commit
	
  Here git is telling us what commits it is tracking, when they occurred, who did them and what the comment was.  This is good information, but the display is a little verbose.  As the number of commits increases we are going to have a real issue looking at more than a little of the history at a time.  Fortunately the log command allows for us to change the output:

	git log --decorate --graph --oneline --date-order
	* f6d26b1 (HEAD, master) Added Micheal
	* 7576855 Initial commit

  This is very concise, but we have lost much of the information.  We can customise it further though:

	git log --graph --pretty=format:'%C(yellow)%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --date=short
	* f6d26b1 - (HEAD, master) Added Micheal (14 hours ago) <Fred Foyle>
	* 7576855 - Initial commit (14 hours ago) <Fred Foyle>

  We have all the information and it is pretty concise, but I really don't want to type in that command line ever again.  Git has a solution for this as well:

	git config --global alias.hist "log --graph --pretty=format:'%C(yellow)%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --date=short"

  Now we can just type:

	git hist

  And get that formated log output.  Make sure you include the double quotes to the whole line is entered for the alias.  If you would prefer to edit the config file directly, you just need to open ~/.gitconfig.  On Windows this will be in your home folder (which depends on the version of Windows you are running).

	[user]
	        name = Fred Foyle
	        email = fred.foyle@example.com
	[core]
	        excludesfile = /Users/mmyself/.gitignore_global
	        editor = vim
	        autocrlf = true
	[alias]
	        hist = log --graph --pretty=format:'%C(yellow)%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --date=short
	[merge]
	        tool = vimdiff

  This may be an easier way to edit your alias list.  You can build an alias of any command.  For instance some people prefer to type `co` rather than `checkout`.  The thing to remember is that you are entering the command line after git.  Some operating systems also allow you to create an alias for commands.  This is a case of doing what makes the most sense for your environment.

## Fixing Mistakes

  Part of the reason for tracking versions is because things do not always go as planned.  Consider that Micheal (Fred's Son) has run a script to put a bunch of names into our directory file.  After running it, he looks at the file and discovers that he forgot to have the script put end of line makes between the names.  Obviously he needs to fix the script, but he would also like to get the file back to where it was.  First let's look at what is different:

	git status
	# On branch master
	# Changes not staged for commit:
	#   (use "git add <file>..." to update what will be committed)
	#   (use "git checkout -- <file>..." to discard changes in working directory)
	#
	#	modified:   Directory.txt
	#
	no changes added to commit (use "git add" and/or "git commit -a")

  Here we see that only the Directory.txt file has changed.  It is always a good idea to look at the changes before we reset them so we know what we are losing.  If you are accustomed to other version control tools you might think we can get rid of the changes to this file by getting the master branch again.

	git checkout master
	M	Directory.txt
	Already on 'master'

  This doesn't work because checking out a branch in Git is simply moving the HEAD tag to the tip of the branch you specify.  This is why it is very quick, but Git doesn't change the modified files unless you tell Git to:

	git checkout --force master
	Already on 'master'

  Now we are back to where we wanted to be.  What if we had added that file to the staging area, but not committed it yet?  Git has a command for clearing the staging area (index):

	git reset
	Unstaged changes after reset:
	M	Directory.txt

  Notice that though Git removed the file from the staging area it did not reset the file.  To ask Git to do that also you add the hard command line option:

	git reset --hard
	HEAD is now at dfa5664 Added Micheal

  So far we have been talking about mistakes that we catch before the commit.  What can we do if we catch the error after the commit?  Let's say that Fred has added an entry for his sister Cheryl.  So he copies her picture into the folder (Cheryl.jpg) and edits the Directory.txt file.

	git status
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
	#	Cheryl.jpg
	no changes added to commit (use "git add" and/or "git commit -a")
	git add Directory.txt 
	git commit -m "Added Cheryl"
	[master f980833] Added Cheryl
	 1 file changed, 1 insertion(+)

  Oops, he just added the changes to the Directory.txt file, but not her picture.  Obviously, we could do another commit and add the picture, but we would like to have this be a single commit.  Git allows us to recover from this mistake by amending that commit:

	git add Cheryl.jpg 
	git commit --amend
	[master 10a326c] Added Cheryl
	 2 files changed, 1 insertion(+)
	 create mode 100644 Cheryl.jpg

  Now if we look at the history of our repository:

	* 10a326c - (HEAD, master) Added Cheryl (87 seconds ago) <Fred Foyle>
	* dfa5664 - Added Micheal (17 minutes ago) <Fred Foyle>
	* 7576855 - Initial commit (15 hours ago) <Fred Foyle>

  We have the three commits, and the last commit contains both the change to the Directory.txt file and the new file Cheryl.jpg.  But doesn't that mean we changed history?  Yes it does.  We'll talk about this a little more in a minute, but first let's get back to amending commits'.  Note that we did not specify which commit we wanted to amend.  That is because Git only allows you to amend that last commit.  This is good for those situations when you press enter on the commit command and then realize that you have forgotten that new file.  What do we do if we made a mistake a couple of commits ago?  If you made a change some time ago and it has become apparent that it was a mistake you can revert that change using:

	git revert dfa5664

  This command makes a new commit that undoes the changes that were made by commit dfa5664.  Note that since Git is storing the changes that were made (rather than the state of the files after the changes) it knows what actions were taken and therefore what actions are needed to undo those changes.  The number (dfa5664) is a commit identifier.  It is the first 7 characters from the SHA1 hash.  The commit identifiers are a little scary at first.  The good news is that you do not have to type the whole SHA1 ID, just enough to make it unique.  You can also use a Tag in place of a commit identifier in most commands.  You can also specify a commit relative to a tag.  Since this commit is one behind the HEAD tag (current pointer) you could use the command:

	git revert HEAD~1

  If you do this you will likely get a conflict.  Git shows you that like this:

	Fred Foyle,415-555-3467,ffoyle.jpg
	<<<<<<< HEAD
	Micheal Foyle,415-555-3467,MichealF.jpg
	Cheryl Motague,413-555-8725,Cheryl.jpg
	
	=======
	>>>>>>> parent of dfa5664... Added Micheal

  From the line that starts with `<<<<<<<` to the line that starts with `=======` is one side of the conflict.  The other side of the conflict is from the line that starts with `=======` to the line that starts with `>>>>>>>`.  You can edit the file directly, removing these lines and making the way you want.  Alternately you can use most Diff/Merge tools to graphically resolve this conflict.  Note that Git did not commit this change because of the conflict, but if there hadn't been a conflict then Git would have committed the change.  If you want to run the command but not commit the result you can add the no-commit command line option:

	git revert --no-commit HEAD~1

  Maybe we just want to look at the state of the files from that commit.  You can checkout any commit in the repository.

	git checkout dfa5664
	Note: checking out 'dfa5664'.
	
	You are in 'detached HEAD' state. You can look around, make experimental
	changes and commit them, and you can discard any commits you make in this
	state without impacting any branches by performing another checkout.
	
	If you want to create a new branch to retain commits you create, you may
	do so (now or later) by using -b with the checkout command again. Example:
	
	  git checkout -b new_branch_name

	HEAD is now at dfa5664... Added Micheal

  Although this changes the state of the working files to the commit specified, it also results in a warning.  Git is telling you that you are looking at a commit that isn't tagged.  You can work here, but it will be difficult to get back here unless you tag this commit.  If we just wanted to look at the state of the files for this commit this is fine. If we wanted to move back in history and get rid of that last commit we can use the reset command:

    git reset dfa5664

  Or

    git reset HEAD~1

After running this command the history will look like this:

	git hist
	* dfa5664 - (HEAD) Added Micheal (54 minutes ago) <Fred Foyle>
	* 7576855 - Initial commit (16 hours ago) <Fred Foyle>

### Changing History

  As we can see Git definitely let's us change the history in our repository.  So this leads to a philosophy question.  Why are we tracking all this history?  What is the purpose of having these different commits recorded?  As with many things there are different purposes depending on what you are doing.

  - **Developer** Keep track of where I am and be able to fix mistakes.
  - **Dev Lead** Track the state of different features.
  - **Release Manager** Track features that make up a release and releases.

In addition to ones responsibilities there is also a question of time.  We are much more likely to care about the individual commits made last week then we are the ones made last year.

Git allows a great deal of flexibility with history.  Both in deciding what eventually gets shared with others (See Working with Others later in this tutorial), as well as in correcting history.  The idea of changing history may seem odd or even dangerous.  There is a certain amount of risk in deciding to change the history in a local repository.  One should consider if this history has value.  To do this you must think more like a Dev Lead or Release Manager than a Developer.  If you are considering removing a set of commits because you are embarrassed by them you should seek a second opinion.  If you have tried something and it just did not work, then you may consider removing that history.  In the end Git is a tool and the decision is up to you.
  There is another consideration about making changes to history.  We are going to learn about sharing our repository with others.  If you have shared your repository then it is almost always a bad idea to change history in that repository.  If you amend a commit, or reset changes that someone else has already based new commits on you will create a mess that will be difficult to resolve.  Git will warn you about commands that may cause difficulty on public commits, but it is up to you to decide.  The rule of thumb is if you have shared the commits don't change them.

## Are They Really Gone?

  Since we are talking about editing the repository and fixing mistakes, what happens if we make a mistake while editing the repository?  Are those changes permanent? Not yet.

	git reflog

  Let's say that I had made a mistaken commit, then used reset to remove it.  My reflog would look something like this:

	git commit -m "It's a mistake"
	git reset HEAD~1
	git hist
	* 10a326c - (HEAD, master) Added Cheryl (3 hours ago) <Fred Foyle>
	* dfa5664 - Added Micheal (3 hours ago) <Fred Foyle>
	* 7576855 - Initial commit (18 hours ago) <Fred Foyle>
	git reflog
	10a326c HEAD@{0}: reset: moving to HEAD~1
	1b922e7 HEAD@{1}: commit: It's a mistake
	10a326c HEAD@{5}: commit (amend): Added Cheryl
	f980833 HEAD@{6}: commit: Added Cheryl
	dfa5664 HEAD@{7}: commit (amend): Added Micheal
	f6d26b1 HEAD@{8}: checkout: moving from master to master
	f6d26b1 HEAD@{9}: checkout: moving from master to master
	f6d26b1 HEAD@{10}: checkout: moving from master to master
	f6d26b1 HEAD@{11}: commit: Added Micheal

  If you were to checkout that commit you will get a warning:

	git checkout 1b922e7
	Note: checking out '1b922e7'.
	
	You are in 'detached HEAD' state. You can look around, make experimental
	changes and commit them, and you can discard any commits you make in this
	state without impacting any branches by performing another checkout.
	
	If you want to create a new branch to retain commits you create, you may
	do so (now or later) by using -b with the checkout command again. Example:
	
	  git checkout -b new_branch_name
	
	HEAD is now at 1b922e7... It's a mistake

  This is that warning again that we are pointing to a commit that has no tags.  If you want to start working here, then you can create a new branch from here just as Git suggested.

	git checkout -b new_branch_name

  So we see that even when you are editing the repository, Git is not really rewriting history, but adding new commits or moving branch tags around.  Git offers many options for correcting mistakes and oversights as we work.  These will become more clear as you use them.  The main take away is that it is essentially always possible to undo the things you do in Git.

Now that we understand working in Git, we will look at how we go about [sharing our changes with others](/pages/sharing-with-others.html).

