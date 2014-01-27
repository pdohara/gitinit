---
title: Before the Beginning
template: article.jade
order: 0
----

There are a few things to cover before we start talking about Distributed Version Control with Git.  Before we start talking about Distributed Version Control you need to understand what Version Control is.  If you are already familiar with Version Control you can skip to Rethinking Version Control.

## What is Version Control?

 A Version control system tracks versions of files over time.  This allows you to go back to a file version and to understand and merge changes that happen on different machines.

### Version Control the Time Machine

 Imagine that you are working on a project.  It is in good enough shape on Monday that you share it with some people so they can test it for you.  Meanwhile you continue working on it.  Now it is Wednesday.  They send you a bug report.  You could try and recreate the bug in the current version of the software, but the changes of the last two days may hide the bug even though it is not fixed.  With version control you can get the version from Monday.  Then recreate the bug and figure out what to change. Then you can bring that change into the current version as of Wednesday.

### Merging changes

 Suppose a friend has offered to help you with a project.  You try and organize the work so you will not both be working on the same parts of the project but from time to time there is still overlap.  Version Control will help you to know when you have both changed the same thing and help you bring both sets of changes together.  Allowing you to continue to work together.

## The Anatomy of Git

  Before we can talk about Git we need to know what the peices are called.  

### Repository

  All version control sysetms store their data somewhere.  For Git this is called the repository.  The repository is in a folder called `.git` in the folder that holds the working set of files.  This repository is not some sort of client data store like in SVN, but is a full repository.  All of the commits (changes) of the files are stored here.

### Commit

  A commit is a set of changes to files.  Note that it may be a change to a single file, but more typically a commit will track changes to several files.  This is because it is typically necessary to change multiple files to get some new peice of functionality working.  The Commit represets this set of changes or a changeset.  We will talk more about this idea and how you should think about commiting changes to take advantage of it.

#### SHA1 The name of a Commit

  Commits in Git are "named" by a SHA1 hash.  A hash is a mathmatical algorythm that produces a fixed output, generally unique number based on an input.  Essentially, Git reads the changes made in the commit and generates a 20 byte number from it.  Sometimes this is refered to as a digest or message digest.  What is important about the SHA1 hash is that it is almost gaurunteed to be unique no matter what machine it is generated on.  This means that two machines will almost never create the same SHA1 number assuming that the input (the changes made) are not the same.  This is important for a distributed system.

#### Distributed Storage

 Git (and other Distributed Version Control Systems DVCS's) allow for more than a single store of versions.  Typically there is a repository on your machine and there is a repository on another machine.  There can be a central machine that everyone shares their changes with.  This is the most like a traditional version control system.  If your project is setup in functional teams there could be a repository for each team.  The developers on that team share their changes to the teams shared repository, then the team lead decides what changes should go to the project repository and be built with all the other teams work.  In this case a team can choose to pull changes from another team (directly, not through the project server), to ensure that a difficult merge is given the time that it should be.  There are many workflows that are possible with a DVCS that would be difficult to do with a traditional version control system.  This also means that there are new commands used by a DVCS and that it takes some time to become accustome to how this new tool works.

### Tags

  Repositories also store Tags.  A tag is a pointer to one of the commits with the repository.  They can be used to remeber what commit was used to build a specific release for instance.  Each branch has a special tag that points to the current tip, or Head of the branch.  There is a particular Head tag with all capitols (HEAD) that points to the head of the current branch.  This allows Git to easily find the current commit on the current branch.

