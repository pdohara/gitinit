---
title: Before the Beginning
template: article.jade
order: 0
----

There are a few things to cover before we start talking about Distributed Version Control with Git.  This tutorial is for programmers that have already decided to use version control.  If have not decided about version control you can take a look at these articles:

https://www.atlassian.com/dvcs/overview/what-is-version-control
http://en.wikipedia.org/wiki/Revision_control

## The Anatomy of Git

  Now that we have decided to use version control we need to make sure we know the vocabulary.

### Repository

  All version control systems store their data somewhere.  For Git this is called the repository.  The repository is in a folder called `.git` in the root folder that holds your files.  This repository is not some sort of client data store like in SVN, but contains all the version data.  All of the commits (changes) of the files are stored here.

### Commit

  A commit is a set of changes to files.  Note that it may be a change to a single file, but more typically a commit will track changes to several files.  This is convenient because changes often involve changes to multiple files.  The Commit is sometimes called a changeset.  Since Git stores these locally it is very quick to commit.  We will talk more about this idea and how you should think about committing changes.

#### SHA1 The name of a Commit

  Commits in Git are "named" by a SHA1 hash.  A hash is a mathematical algorithm that produces a fixed output, generally unique number based on an input.  Essentially, Git reads the changes made in the commit and generates a 20 byte number from it.  Sometimes this is referred to as a digest or message digest.  What is important about the SHA1 hash is that it is essentially always unique no matter what machine it is generated on.  This means that two machines will almost never create the same SHA1 number.  This is important for a distributed system.

### Tags

  Repositories also store Tags.  A tag is a pointer to one of the commits with the repository.  They can be used to remember what commit was used to build a specific release for instance.  There is a special tag called HEAD that points to the current work.  This allows Git to easily find the current commit on the current branch.  Each branch has a special tag that points to the current tip, or Head of the branch.

### Branch

  One reason to use Version Control is to be able to try something new while also being able to get and make changes to the current version of the files.  Branches allow you to work on a new feature while also being able to get the current state of the files without the half finished feature changes.

