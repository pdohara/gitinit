---
title: Fixing Mistakes
template: article.jade
order: 2
----

  Part of the reason for tracking versions is because things do not always go as planned.  Let's assume that are looking over your work (not making any changes yet) and then you notice that Git says there are changes to the file.  So you look at the diff and discover your editor has changed all the tabs to spaces.  This wasn ot a change you intended to make.  You can change the setting in the editor, but how do we get the file back the way it was?



  This command commits what every you have staged as part of the last commit to the repository.  In this way you can add that missed file, or forgotten copyright update, or what ever occurs to you just after you commit your changes.

  Sometimes we want our SCM tool to get us out of trouble.  It can be wonderful to just get back to a know good point.  Let's assume that are looking over your work (not making any changes yet) and then you notice that Git says there are changes to the files.  So you look at the diff and discover your editor has changed all the tabs to spaces.  This wasn't a change you intended to make.  You can change the setting in the editor, but how do we get the file back the way it was?  Let's start by making sure we know what has changed.

    git status

  If you remember the output will look something like this:

    # On branch master
    # Changes to be committed:
    #   (use "git reset HEAD <file>..." to unstage)
    #
    #       new file:   GettingStarted.html
    #       modified:   index.html
    #
    # Untracked files:
    #   (use "git add <file> to include in what will be committed)
    #
    #       FixingMistakes.html
    #       SharingWithOthers.html

  To go back to a clean slate (no changes) we can use the checkout command:

    git checkout [filename]

  This will replace the file with the latest version of the file from the repository.  This works fine for one or two files, but if you have many changes that need to be undone, then the reset command is more appropriate.

    git reset --hard

  This command will reset the working files and the index (stage area).  Using `--mixed` will reset the index but not change the working files.  This is the default behavior.

  Sometimes we make changes and commit them and then decide that this was a bad idea and we wish to go back to a previous state.  You can checkout a previous commit.  First you need to identify what state you wish to get back to.  The log command will show you a list of your previous commits.

    git log

    commit 390b0692d6587b03f83d0355ec4eff0529d31bc7
    Author: Patrick O'Hara <patrick.ohara@cognex.com>
    Date:   Tue Dec 24 11:59:36 2013 -0600

        Starting to work on FixingMistakes.html

    commit 4952548d775dc7c8fd066764465abaec0da70394
    Author: Patrick O'Hara <patrick.ohara@cognex.com>
    Date:   Tue Dec 24 11:25:23 2013 -0600

        Completed GettingStarted.html

    commit ca93a4cecd5bd185d12ea96856d3bb60b3a28104
    Author: Patrick O'Hara <patrick.ohara@cognex.com>
    Date:   Tue Dec 24 11:00:12 2013 -0600

        Started working on Getting Started page

    commit ca82493642326df3f7461dfa87a34959f2327810
    Author: Patrick O'Hara <patrick.ohara@cognex.com>
    Date:   Tue Dec 24 10:24:53 2013 -0600

        Wrote overview.

    commit 9227e1863089295a81c87ed3e631b1efe2a4c93a
    Author: Patrick O'Hara <patrick.ohara@cognex.com>
    Date:   Tue Dec 24 09:20:46 2013 -0600

        Template commit


  The commit identifiers are a little scary at first.  The good news is that you do not have to type the whole SHA1 ID, just enough to make it unique.  So if we wanted to get back to the Completed GettingStarted.html commit we can run the command:

    git checkout 495254

  Although this changes the state of the working files to the commit specified, it also results in a warning:

    Note: checking out '4952'.

    You are in 'detached HEAD' state. You can look around, make experimental
    changes and commit them, and you can discard any commits you make in this
    state without impacting any branches by performing another checkout.

    If you want to create a new branch to retain commits you create, you may
    do so (now or later) by using -b with the checkout command again. Example:

      git checkout -b new_branch_name

    HEAD is now at 4952548... Completed GettingStarted.html

  Git is telling you that you are looking at a historical commit.  You cannot work here, unless you create a branch.  This is fine if we just wanted to look at the state of the files for this commit, but since we want to move back to this commit we need to try a different approach.

  Consider the following history:

    commit c0d84f689b751f7d9bec63d0fb86711936966419
    Author: Patrick O'Hara <patrick.ohara@cognex.com>
    Date:   Thu Dec 26 10:59:30 2013 -0600

        Spurious 3

    commit f832c8235ca08696f937ef0f094c00b5e05b84aa
    Author: Patrick O'Hara <patrick.ohara@cognex.com>
    Date:   Thu Dec 26 10:59:06 2013 -0600

        Spurious 2

    commit faa8c71ed0fac5f9fc97a03e68c506195eab03e6
    Author: Patrick O'Hara <patrick.ohara@cognex.com>
    Date:   Thu Dec 26 10:58:44 2013 -0600

        Spurious 1

    commit 181df70bf628b80716d8950101a434e1ebce2b41
    Author: Patrick O'Hara <patrick.ohara@cognex.com>
    Date:   Thu Dec 26 10:48:23 2013 -0600

        Working on fixing page.

    commit 390b0692d6587b03f83d0355ec4eff0529d31bc7
    Author: Patrick O'Hara <patrick.ohara@cognex.com>
    Date:   Tue Dec 24 11:59:36 2013 -0600

        Starting to work on FixingMistakes.html

    commit 4952548d775dc7c8fd066764465abaec0da70394
    Author: Patrick O'Hara <patrick.ohara@cognex.com>
    Date:   Tue Dec 24 11:25:23 2013 -0600

        Completed GettingStarted.html

    commit ca93a4cecd5bd185d12ea96856d3bb60b3a28104
    Author: Patrick O'Hara <patrick.ohara@cognex.com>
    Date:   Tue Dec 24 11:00:12 2013 -0600

        Started working on Getting Started page

    commit ca82493642326df3f7461dfa87a34959f2327810
    Author: Patrick O'Hara <patrick.ohara@cognex.com>
    Date:   Tue Dec 24 10:24:53 2013 -0600

        Wrote overview.

    commit 9227e1863089295a81c87ed3e631b1efe2a4c93a
    Author: Patrick O'Hara <patrick.ohara@cognex.com>
    Date:   Tue Dec 24 09:20:46 2013 -0600

        Template commit

  Let us assume that those last three commits where a mistake.  So I would like to get back to the state for commit 181df70bf628b80716d8950101a434e1ebce2b41.  The command to get back to that state is:

    git reset 181df70bf628b80716d8950101a434e1ebce2b41

  Or

    git reset 181df7

  In this case there is another way to specify a commit that may be more useful.  We want to backup 3 commits, so we can specify the commit relative to our current (HEAD) position.

    git reset HEAD~3

  After running this command the history will look like this:

    commit 80be37ff9f4edc2c699ec05d7b3c6d4f1cc60bb1
    Author: Patrick O'Hara <patrick.ohara@cognex.com>
    Date:   Thu Dec 26 11:04:38 2013 -0600

        Revert "Working on fixing page."

        This reverts commit 181df70bf628b80716d8950101a434e1ebce2b41.

        Conflicts:
            FixingMistakes.html

    commit c0d84f689b751f7d9bec63d0fb86711936966419
    Author: Patrick O'Hara <patrick.ohara@cognex.com>
    Date:   Thu Dec 26 10:59:30 2013 -0600

        Spurious 3

    commit f832c8235ca08696f937ef0f094c00b5e05b84aa
    Author: Patrick O'Hara <patrick.ohara@cognex.com>
    Date:   Thu Dec 26 10:59:06 2013 -0600

        Spurious 2

    commit faa8c71ed0fac5f9fc97a03e68c506195eab03e6
    Author: Patrick O'Hara <patrick.ohara@cognex.com>
    Date:   Thu Dec 26 10:58:44 2013 -0600

        Spurious 1

    commit 181df70bf628b80716d8950101a434e1ebce2b41
    Author: Patrick O'Hara <patrick.ohara@cognex.com>
    Date:   Thu Dec 26 10:48:23 2013 -0600

        Working on fixing page.

    commit 390b0692d6587b03f83d0355ec4eff0529d31bc7
    Author: Patrick O'Hara <patrick.ohara@cognex.com>
    Date:   Tue Dec 24 11:59:36 2013 -0600

        Starting to work on FixingMistakes.html

    ...

  So you can see that there is a new commit that contains the changes necessary to get us back to that previous commit.  This is good if the history of this attempt is worth keeping, but what if this was just a false path?  You might think its just embaressment to want to remove these commits from history, but there is more to it than that.

###The Purpose of History

  So this leads to a philosophy question.  Why are we tracking all this history?  What is the purpose of having these different commits recorded?  As with many things there are different purposes depending on what you are doing.

  - **Developer** Keep track of where I am and be able to fix mistakes.
  - **Dev Lead** Track the state of different features.
  - **Release Manager** Track features that make up a release and releases.

  In addition to ones responsibilities there is also a question of time.  We are much more likely to care about the individual commits made last week then we are the ones made last year (or six years ago).

  Git allows a great deal of flexibility with history.  Both in deciding what eventually gets shared with others (See Working with Others later in this tutorial), as well as in changing history.  The idea of changing history may seem odd or even dangerous.  There is a certain amount of risk is deciding to change the history in a local repository.  One should consider if this history has value to keep.  To do this you must think more like a Dev Lead or Release Manager than a Developer.  If you are considering removing a set of commits because you are embarrass by them you should seek a second opinion.  If you have tried something and it just did not work, then you may consider removing that history.  In the end Git is a tool and the decision is up to you.

    git reset --hard HEAD~3

  Looking at the log now will show that rather than have a new commit that "undoes" the changes in those three commits, the three commits are no longer part of the history.  They are gone, but not forgotten.  Those commits are still in the repository for now.  Though `git log` shows that the _Working on fixing page._ is the latest commit, the command `git reflog` will show those spurious commits:

    181df70 HEAD@{0}: checkout: moving from master to 181d
    4952548 HEAD@{1}: reset: moving to HEAD~3
    faa8c71 HEAD@{2}: reset: moving to HEAD~3
    80be37f HEAD@{3}: commit: Revert "Working on fixing page."
    c0d84f6 HEAD@{4}: commit: Spurious 3
    f832c82 HEAD@{5}: commit: Spurious 2
    faa8c71 HEAD@{6}: commit: Spurious 1
    181df70 HEAD@{7}: checkout: moving from 4952548d775dc7c8fd066764465abaec0da70394
    4952548 HEAD@{8}: checkout: moving from master to 4952
    181df70 HEAD@{9}: checkout: moving from master to master
    181df70 HEAD@{10}: checkout: moving from 4952548d775dc7c8fd066764465abaec0da7039
    4952548 HEAD@{11}: checkout: moving from master to 4952548d
    181df70 HEAD@{12}: commit: Working on fixing page.
    390b069 HEAD@{13}: commit: Starting to work on FixingMistakes.html
    4952548 HEAD@{14}: commit (amend): Completed GettingStarted.html
    82a8b57 HEAD@{15}: commit: Completed GettingStarted.html
    ca93a4c HEAD@{16}: commit: Started working on Getting Started page
    ca82493 HEAD@{17}: commit (amend): Wrote overview.
    4665e1a HEAD@{18}: commit: Wrote overview.
    9227e18 HEAD@{19}: commit (initial): Template commit

  If you had reset 4 commits instead of three (`git reset HEAD~4`), you could get back to the "right" commit by reseting to it:

    git reset --hard 181df7

  So long as the detached commits have not been "cleaned up".  These detached commits remain in the repository until the `git gc` command is run.

  Git offers many options for correcting mistakes and oversights as we work.  These will become more clear as you use them.  The important take away is that in most cases it is possible to undo essentially anything in Git if you do so early.  As with any other system, the longer you wait to try and correct something more more involved it is to correct.

  Now that there is something worth sharing, we will look at how we go about [sharing our changes with others](/pages/sharing-with-others.html).

