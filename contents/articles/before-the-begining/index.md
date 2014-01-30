Before The Beginning
====================

  There are a few things to cover before we start talking about Distributed Version Control with Git.  Before we start talking about Distributed Version Control you need to understand what Version Control is.  If you are already familiar with Version Control you can skip to Rethinking Version Control.

## What is Version Control?

  A Version control system tracks versions of files over time.  This allows you to go back to a file version and to understand and merge changes that happen on different machines.

### Version Control the Time Machine

  Imagine that you are working on a project.  It is in good enough shape on Monday that you share it with some people so they can test it for you.  Meanwhile you continue working on it.  Now it is Wednesday.  They send you a bug report.  You could try and recreate the bug in the current version of the software, but the changes of the last two days may hide the bug even though it is not fixed.  With version control you can get the version from Monday.  Then recreate the bug and figure out what to change.  Then you can bring that change into the current version as of Wednesday.

### Merging changes

  Suppose a friend has offered to help you with a project.  You try and organize the work so you will not both be working on the same parts of the project but from time to time there is still overlap.  Version Control will help you to know when you have both changed the same thing and help you bring both sets of changes together.  Allowing you to continue to work together.

## Rethinking Version Control

  Over the years there have been many Version Control systems.  The feature lists have grown over time but they have all worked on the same general concepts.  There is a central store that holds the different versions.  This central store is then access from software on a client machine.  The client software can add new versions, list the versions that exist, update the local files with new changes from the server, etc.  

### Changesets rather than Changes

  These versions may be tracked on a file by file basis, where each revision to a file is stored.  Alternately, these versions may be stored as sets of changes.  It is common for a change to involve more than one file.  With revision based systems something outside the system must track that revision 4 or the first file and revision 3 of the second file are a single change.  Systems that track version as changesets track all the changes (files added, files removed and files changed) in a version.  This allows the system to identify file renames for instance.  Since the version shows that file a was deleted and file b was created with the same content that file a had, the system can infer that the file was renamed from a to b.  It also makes it easy to identify all the files that need to be changed to accomplish a version.  Git uses changesets.

### Distributed Storage

  Git (and other Distributed Version Control Systems DVCS's) allow for more than a single store of versions.  So a repository (what Git calls the place it stores versions) is on your machine and there is likely a repository on another machine.  There can be a central machine that everyone shares their changes with.  This is the most like a traditional version control system.  If your project is setup in functional teams there could be a repository for each team.  The developers on that team share their changes to the teams shared repository, then the team lead decides what changes should go to the project repository and be built with all the other teams work.  In this case a team can choose to pull changes from another team (directly, not through the project server), to ensure that a difficult merge is given the time that it should be.  There are many workflows that are possible with a DVCS that would be difficult to do with a traditional version control system.  This also means that there are new commands used by a DVCS and that it takes some time to become accustome to how this new tool works.

