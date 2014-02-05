---
title: Next Steps
template: article.jade
order: 5
----

##Review

So let's look at the commands we've learned about for interacting with Git:

To Start:
- `git init`

Creates a new local repostory

- `git clone [URL]`

Creates a local repository that is based on the repostory specified by the URL.

While Working Locally:
- `git add`
- `git commit`

Add the files you are ready to commit and then commit them.  Remember to commit smaller chucks of work, then it will be easier to edit the repository to tell the story that you want it to tell later.

To Share with Other Repositories:
- `git fetch`

Brings the changes from the remote repository to your machine.

- `git rebase`

Works your changes on to the remote machines branch.

- `git merge`

Creates a new commit that has your changes and the changes from the other machine.  Remember to think about the story you want the repository to tell about your changes.  That will help you choose between rebase and merge.

- `git push`

Now send the changes to the other machine.

Working on Branches:
- `git branch branch_name`
- `git checkout -b branch_name`

These create a branch in the local repository.  You may also need to push the branch to the remote repository.

- `git push origin branch_name`

After going through the set of Git command a couple of times you will start to become comfortable with them.  Obviously this is not all that there is to know about Git.  Please consider what you're next steps with Git may be.

## Next Steps

  Now that you have an idea how Git works.  What are the next steps?  First you need to use Git.  I recommend spending a little time using the command line tools.  This will help you to become comfortable with the concepts you have learned.  The danger that the various GUIs for Git have is that they often use different terms.  This is fine until you want to ask a question of someone else.  If you are not speaking the same language (using the same words) then you are going to end up frustrated and not get an answer to your question.
  After you spent some time with the command line go ahead and get a GUI tool is you like.  Also after you have become comfortable with what you've learned and you want to know more about Git, or you need a reminder about how a command works, take a look at these resources.

## Git User Manual

http://schacon.github.io/git/user-manual.html

## Git Book

http://www.git-scm.com/book

## Other Tutorials

http://www.jackmaney.com/post/git_for_ages_4_and_up
http://marklodato.github.io/visual-git-guide/index-en.html
http://gitready.com/

