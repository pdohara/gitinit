---
title: Branching
template: article.jade
order: 4
----

##Branching

So far all of the work we have been doing has been on the `master` branch.  As we discussed earlier this is the default or trunk branch in a Git repository.  If you are working alone or with a very small team this may be enough.  However parallel development can be desirable for a number of different reasons.  Perhaps you wish to try something out.  Perhaps you are working on refactoring part of the system while others on your team continue to work on a new feature.  Whatever the reason it is not uncommon to wish to work in parallel (or isolation) from the master branch.

###Creating a Branch

    git branch [branch name]

  This command will create a branch with the name specified.  In order to work on this branch you will need to change the repository to be focused on it using the checkout command.

    git checkout [branch name]
 
Since this is a common workflow there is a shortcut version of this using only the checkout command: 

    git checkout -b [branch name]

So let's assume you are refactoring while others continue to develop on master.  To create a branch called refactor you issue the command `git checkout -b refactor`.  At this point your repository could look like this: 
        
    4132fa --- b83f42 --- e51a09 --- 284617 (master)(origin\master)
                                         \ (refactor)
        
The `origin` server will look like this:

    4132fa --- b83f42 --- e51a09 --- 284617 (master)
      
Note that the branch will not show up on the `origin` server until we push it there.  So now you make some change and your repository looks like this: 

    e51a09 --- 284617 --- 3c07fd --- 4f7d6a (refactor)
                     \ (master)

In the mean time your teammates have been working on the master branch so origin now looks like this:
        
    e51a09 --- 284617 --- d17089 --- 32860a (master)
           
You want to stay current with what they are doing so you fetch the changes using `git fetch`, but you are not ready to combine the two lines of development so instead of using rebase, we are going to merge the changes into our refactor branch.

    git merge master 

Now your local repository will look something like this:

    e51a09 --- 284617 --- d17089 --- 32860a (master)
                 \                          \
                  \ --- 3c07fd --- 4f7d6a --- 375a4d (refactor)

You can (and probably should) push these changes to the remote.  You already know the command to push changes is `git push`, but in this case we also want Git to remember that our local refactor branch should push to the new refactor branch on origin.  So the command will look like this:

    git push -u origin refactor

This will push the new refactor branch to origin and setup the local refactor branch to track it.  This makes the local refactor branch a tracking branch of the origin refactor branch:

    e51a09 --- 284617 --- d17089 --- 32860a (master)(origin/master)
                 \                          \
                  \ --- 3c07fd --- 4f7d6a --- 375a4d (refactor)(origin/refactor)

Now as you continue to work on the refactor branch you can use fetch and push without having to specify the remote branch you wish to fetch from or push to.  At some point you will be done with the work on the refactor branch.  Then you will want to merge the changes back to master.  Let's say that your repository looks like this:

    e51a09 --- 284617 --- d17089 --- 32860a -- 3f450a (master)(origin/master)
                 \                          \
                  \ --- 3c07fd --- 4f7d6a --- 375a4d --- 3f670a --- 4d2a3c (refactor)(origin/refactor)

To merge the changes to the master branch you will checkout the master branch and then merge:

    git checkout master
    git merge refactor

After resolving and conflicts your repository will look like this:

                                                   /(origin/master)
    e51a09 --- 284617 --- d17089 --- 32860a -- 3f450a ----------------- 276a54 (master)
                 \                          \                           /
                  \ --- 3c07fd --- 4f7d6a --- 375a4d --- 3f670a --- 4d2a3c (refactor)(origin/refactor)

  Now you are ready to puch the changes to share with the rest of your team.

### Merging vs. Rebasing

In the previous section we used the `git rebase` command to combine the changes from the remote master branch with the changes on our master branch.  This resulted in all the changes having a straight line history.  In this section we have been using merge to combine changes from different branches.  This creates a new commit on the current branch with all the changes from the other branch.  This is done, so that the history in the repository will tell the story we want it to tell.  When you are working a branch and someone else is also working on that same branch we want the history in the repository to show that as a single branch.  So in this case we use rebase and this:

    9227e1 --- ca8249 --- ca93a4 --- 495254 (master)
                    \ --- 390b06 --- 181df7 (origin/master)

becomes this:

    9227e1 --- ca8249 --- ca93a4 --- 495254 --- f62e30 --- a993fe (master)(origin/master)

On the other hand When we where working on on the refactor branch we want that to show as a branch of development se we used the merge command:

    e51a09 --- 284617 --- d17089 --- 32860a -- 3f450a ----------------- 276a54 (master)(origin/master)
                 \                          \                           /
                  \ --- 3c07fd --- 4f7d6a --- 375a4d --- 3f670a --- 4d2a3c (refactor)(origin/refactor)

The choice of merge vs. rebase is a choice of how the repository will be structured when we are done.  So you will typically use rebase to work on a branch and merge to bring branches back together.

####Git Pull

`git pull` is a shorthand version of `get fetch` and `git merge`.  People new to Git will often make the mistake of assuming `git pull` and `git push`.  This will work fine but it will create a repository that looks like you create branches all the time for no real reason.  Most Git users use fetch and rebase rather than pull.

##Review

So let's look at the commands we've learned about for interacting with Git:

- `git clone [URL]`
- `git add`
- `git commit`
- `git fetch`
- `git rebase`
- `git push`
- `git branch`
- `git checkout`

After going through the set of Git command a couple of times you will start to become comfortable with them.  Obviously this is not all that there is to know about Git.  Please consider what you [next steps](/pages/next-steps.html) with Git may be.

