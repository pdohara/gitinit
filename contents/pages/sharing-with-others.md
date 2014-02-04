---
title: Sharing With the Team
template: article.jade
order: 3
----

### Distributed Storage
 
 Git allows for more than a single repository.  There can be a central machine that everyone shares their changes with, or you can setup a machine for each team to work with. Then team lead decides what changes should go to the project repository and be built with all the other teams work.  In this case a team can choose to pull changes from another team repositories (directly, not through the project server).  Perhaps to ensure that a difficult merge is given the time required to get it right.  There are many organizations or repositories that are possible with a DVCS.  This also means that there are new commands used by a DVCS and that it takes some time to become accustom to how this new tool works.

  Before you can share with others, you need to decide where you will be sharing.  Everything that has been done up to this point in the tutorial has happened on a single machine.  The commits that we have been doing have been recorded to the repository on the local machine.  The good part of this is that commits are very fast and we can do them when we do not have access to a "server".  This means that you can commit changes at your local coffee shop, or on an airplane.  The other side of it is that a commit does not save the information to another machine.  To share the information and therefore back it up we need a remote to work with.  Git needs to be able to accomplish file actions with the remote.

### Git Server Options

Git offers many [options for setting up a server](http://git-scm.com/book/en/Git-on-the-Server-The-Protocols).  As with any server technology these options can be complex and involve decisions relating to security and network performance.  As such, I would recommend using a service like [GitHub](https://github.com/).  This frees you from having to learn about deployment options and server setup.  Alternately you may have a Git server that has already been setup by your employer or someone else on your team.  Obviously in this case you will use that server.  If you are determined to setup your own server, I recommend getting a package like [GitHub for Windows](http://windows.github.com/) or [Bonobo](http://bonobogitserver.com/).  For these examples I will work with a file system based remote.

### Working with Remotes

  After the mess that Micheal made when changing the Directory.txt file with a script, Fred has decided he needs to work on it from Micheal's own machine.  Since the repository is on a network file share Micheal can get his own copy of it by cloning it:

	git clone file://HomeServer/Directory

  Note that the clone command takes a URL.  This can be http based like for GitHub.  It can be file based as in our example.  It may use ssh if you are using ssh for authentication to the shared repository.  Who ever setup the repository for you should be able to give you the appropriate URL to use to clone the repository.
       
  Now that Micheal has a clone of the repository he has decided to do something easy to make sure things are working.  So he adds his mother (Sara Jane) to the repository and adds a picture of her.

	git status
	# On branch master
	# Changes to be committed:
	#   (use "git reset HEAD <file>..." to unstage)
	#
	#	modified:   Directory.txt
	#	new file:   SaraJane.jpg
	#
	git commit -m "Added Mom"
	[master efa1565] Added Mom
	 2 files changed, 1 insertion(+)
	 create mode 100644 SaraJane.jpg
        
	git hist
	* efa1565 - (HEAD, master) Added Mom (38 seconds ago) <Micheal Foyle>
	* 10a326c - (origin/master) Added Cheryl (3 hours ago) <Fred Foyle>
	* dfa5664 - Added Micheal (4 hours ago) <Fred Foyle>
	* 7576855 - Initial commit (19 hours ago) <Fred Foyle>

  Meanwhile Fred has added his college buddy Jorge Ringenbach to the directory:

	git add Directory.txt 
	git add ringenbach.jpg 
	git commit -m "Added Jorge"
	[detached HEAD 9a41638] Added Jorge
	 2 files changed, 1 insertion(+)
	 create mode 100644 ringenbach.jpg
	git hist
	* 9a41638 - (HEAD) Added Jorge (2 minutes ago) <Fred Foyle>
	* 1b922e7 - It's a mistake (59 minutes ago) <Fred Foyle>
	* 10a326c - (master) Added Cheryl (4 hours ago) <Fred Foyle>
	* dfa5664 - Added Micheal (4 hours ago) <Fred Foyle>
	* 7576855 - Initial commit (19 hours ago) <Fred Foyle>

  Now before Micheal can share his changes with his dad, he needs to make sure he is up to date.  To do that he needs to get any new changes his dad has made.  So he fetches them with:

	git fetch
	remote: Counting objects: 4, done.
	remote: Compressing objects: 100% (3/3), done.
	remote: Total 3 (delta 1), reused 0 (delta 0)
	Unpacking objects: 100% (3/3), done.
	From /Users/pdohara/Source/mDir/../Directory
	   10a326c..7fe443a  master     -> origin/master

  This command requests any changes from the origin repository.  Though this retrieves any commits that are on the remote server, but not on yours, it does not resolve the two different lines of development nor does it update any of your working files.  Before we can get around to that we need to understand the options for merging these changes.
        
### Distributed Development Reality

  When you are working on the `master` branch on your local machine, it is likely that other people are pushing changes to the remote repository `master` branch.  One way to think of this is as if there were two branches.  Development on two different branches needs be merged.  This results in a history graph that looks like this:
        
![Merge Example](./Merge.png "Merge Example")

  If we actually had multiple branches, that would be fine, but we are working on the same branch.  So we need a new method of getting two different lines of development aligned.  For Git the command is `rebase`.  This takes the commits you have made to your local `master` branch and apply them to the remote `master` branch.  This leads to a linear history like this:
        
![Rebase Example](./Rebase.png "Rebase Example")

  Before we rebase these changes we want to have a look at them.  So we try our hist alias:

	git hist
	* efa1565 - (HEAD, master) Added Mom (2 hours ago) <Micheal Foyle>
	* 10a326c - Added Cheryl (5 hours ago) <Fred Foyle>
	* dfa5664 - Added Micheal (5 hours ago) <Fred Foyle>
	* 7576855 - Initial commit (20 hours ago) <Fred Foyle>

  So where are the changes we just fetched?  They don't show up in this history. Git has them in the FETCH_HEAD.  You can see them by pulling the log on that:

	git log FETCH_HEAD --not master
	commit 68ca42e8c0a23b746c1d67bcca46acc70d6c129f
	Author: Patrick O'Hara <pdohara@smallwarz.org>
	Date:   Sun Feb 2 15:24:00 2014 -0600
	
	    Added Jorge
	 

  Now that we know what we are going to rebase, we can go ahead.  

	git rebase
	First, rewinding head to replay your work on top of it...
	Applying: Added Mom
	Using index info to reconstruct a base tree...
	M	Directory.txt
	Falling back to patching base and 3-way merge...
	Auto-merging Directory.txt
	CONFLICT (content): Merge conflict in Directory.txt
	Failed to merge in the changes.
	Patch failed at 0001 Added Mom
	The copy of the patch that failed is found in:
	   /Users/pdohara/Source/mDir/Directory/.git/rebase-apply/patch
	
	When you have resolved this problem, run "git rebase --continue".
	If you prefer to skip this patch, run "git rebase --skip" instead.
	To check out the original branch and stop rebasing, run "git rebase --abort".

  So Git has identified that there are two changes to the Directory.txt file and we need to sort it out.  The decorated version of the file looks like this:

	Fred Foyle,415-555-3467,ffoyle.jpg
	Micheal Foyle,415-555-3467,MichealF.jpg
	Cheryl Motague,413-555-8725,Cheryl.jpg
	<<<<<<< HEAD
	Jorge Ringenbach,414-555-0251,ringenbach.jpg
	=======
	Sara Jane Foyle,415-555-3476,SaraJane.jpg
	>>>>>>> Added Mom

  After the merge the file will look like this:

	Fred Foyle,415-555-3467,ffoyle.jpg
	Micheal Foyle,415-555-3467,MichealF.jpg
	Cheryl Motague,413-555-8725,Cheryl.jpg
	Sara Jane Foyle,415-555-3476,SaraJane.jpg
	Jorge Ringenbach,414-555-0251,ringenbach.jpg

  Now we can continue the rebase.

	git add Directory.txt 
	git rebase --continue
	Applying: Added Mom

  As we can see, Git has committed the changes from the origin/master branch.  We can see this in the history:

	git hist
	* 2f2f89b - (HEAD, master) Added Mom (6 minutes ago) <Micheal Foyle>
	* 68ca42e - (origin/master) Added Jorge (71 minutes ago) <Fred Foyle>
	* 10a326c - Added Cheryl (6 hours ago) <Fred Foyle>
	* dfa5664 - Added Micheal (6 hours ago) <Fred Foyle>
	* 7576855 - Initial commit (21 hours ago) <Fred Foyle>

  Note that the origin/master branch is one commit behind our local master branch.  That is because we have yet to push our changes.

	git push 
	Counting objects: 6, done.
	Delta compression using up to 4 threads.
	Compressing objects: 100% (4/4), done.
	Writing objects: 100% (4/4), 170.00 KiB | 0 bytes/s, done.
	Total 4 (delta 2), reused 0 (delta 0)
	To /Users/pdohara/Source/mDir/../Directory
	   68ca42e..2f2f89b  master -> master

  Now the origin repository also has the changes.  Git provides a command to do both the fetch and rebase (or merge) at once.

	git pull --rebase

  `git pull` without the --rebase option is like issuing a `git fetch` followed by a `git merge`.  Though this will work you will find most shops use rebase for work on the same branch.  Merging is used for bringing branches back together.  This command is convenient, but I have found that I prefer doing it in two steps.  I don't have a compelling reason why, I just prefer it that way.
        
### Don't Cross the Streams
        
Merging and Rebasing work fine together as long as you don't use both commands on the same commits.  If you look back through our history listing you will see that the "Added Mom" commit is different after the rebase.  Before the rebase:


	git hist
	* efa1565 - (HEAD, master) Added Mom (2 hours ago) <Micheal Foyle>
	* 10a326c - Added Cheryl (5 hours ago) <Fred Foyle>
	* dfa5664 - Added Micheal (5 hours ago) <Fred Foyle>
	* 7576855 - Initial commit (20 hours ago) <Fred Foyle>


  And After:

	git hist
	* 2f2f89b - (HEAD, master) Added Mom (6 minutes ago) <Micheal Foyle>
	* 68ca42e - (origin/master) Added Jorge (71 minutes ago) <Fred Foyle>
	* 10a326c - Added Cheryl (6 hours ago) <Fred Foyle>
	* dfa5664 - Added Micheal (6 hours ago) <Fred Foyle>
	* 7576855 - Initial commit (21 hours ago) <Fred Foyle>

  This is another case of Git changing history.  Since the commit SHA1 has changed, Git has to assume that it is a different change.As a result someone will get another conflict.  If a few changes are both rebased and merged it can result in quite a mess.

### Who's the Master?

  `master` is the name of the trunk or main branch in a repository.  If no other branches are created, then all work will be on the `master` branch.  We'll talk more about branches in the next section.  The names of the branches (master) and (origin/master) represent two different branches in your local repository.  The master branch is the local trunk of your repository.  The origin/master branch is a **tracking branch** of the remote (origin) master branch.  So when you fetch changes from the remote they go on this branch.  Tracking branches are important for working with remotes.  We'll talk about them more in the next section.
        
###Where Were We?
        
  Getting back to what we were talking about before, the flow of commands for working with a remote repository is:
        
  - `git fetch`
  - `git rebase`
  - Check Your Changes
  - `git push`
        
You need to fetch and rebase before you can push your changes.  Git will not allow you to send your commits to a remote server unless you have all of the commits from that server resolved.  In this way you must resolve any conflicts prior to pushing them back to the server.  This is more than just a minor point of reference with Git.  It is integral to why Git can work in a distributed manner.  Most SCM tools allow the resolution of conflicts to be passed downstream.  With Git each user is required to resolve any conflicts before they can be shared with other users.  This means that though there is no guarantee that the changes will "work" it is guaranteed that no one downstream will need to resolve conflicts.

We are almost done with our introduction to Git.  In the next section we will talk about [branching](/pages/branching.html) and go over the complete workflow again.  In the final section we will look at resources for [additional reading about Git](/pages/next-steps.html).
