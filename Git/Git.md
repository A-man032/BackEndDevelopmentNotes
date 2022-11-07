# Git

Taking notes when I read *Pro Git*.

##  About Git

### Version Control

Version control is a system that records changes to a file or set of files over time so that you can **recall specific versions later**. 

### Distributed Version Control Systems (DVCSs)

Local VCSs

![image-20220721114910660](.\png\LocalVCSs)

Centralized VCSs

![image-20220721115016689](.\png\CVCSs)

DVCSs

![image-20220721115156790](.\png\DVCSs)

Both local Version Control Systems and Centralized Version Control Systems face a problem, which you'll lose everything when comes a crash.

In a DVCSs, clients fully mirror the repository, including its full history, rather than just check out latest snapshot of the files.

**Every clone** is a full **backup** of all the data.

If any server dies, any of the client repositories can be copied back up to the server to restore it.

### Git

When you commit or save the state of your project, Git takes a picture of what all your files look like at that moment and stores a reference to that snapshot. **If file have not changed, Git doesn't store the file again, just a link to the previous identical file it has already stored.**

In Git, its data more likes a **stream of snapshots**.

![image-20220721160129122](.\png\GitData)

- Git doesn't need to go out to the server to get the history and display it for you. It reads it directly from your **local database**.
- Everything in Git is checksummed before it is stored and is then referred to by that **checksum**, which means it's impossible to change the contents of any file or directory without Git knowing about it.
- Git uses *SHA-1 hash* for checksumming. *SHA-1hash* is a 40-character string composed of hexadecimal characters and calculated based on the contents of a file or directory structure in Git. 
- **Git generally only adds data**. It is hard to get the system to do anything that is not undoable or to make it erase data in any way.

Git has 3 states:

- **Modified** : you  have changed the file but haven't committed it to your database yet.
- **Staged** : you have marked a modified file in its current version to go into your next commit snapshot.
- **Committed** : the data is stored in your local database.

The basic **Git workflow** goes something like this:

1. You modify files in your working tree.
2. You selectively stage just those changes you want to be part of your next commit, which adds *only* those changes to the staging area.
3. You do a commit, which takes the files as they are in the staging area and stores that snapshot permanently to your Git directory.