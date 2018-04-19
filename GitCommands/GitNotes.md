---
## Initialize a Repo
Navigate to your directory that you wish to initialize a new repository. Execute the following command:

    git init

Afterwards, you will notice a new hidden folder, `.git` in the directory.

----
## Clone a Repo
Navigate to the directory that you wish to clone a repository. Execute the following command:

    git clone REPOSITORYlocation

Where `REPOSITORYlocation` is the directory, link, or location of the repo. For example, I have a dummy repo at `http://github.com/beecha/DummyRepo` then if I want to clone the repo, I will run the following command:

    git clone http://github.com/beecha/DummyRepo

----
## Status Change in Repo
When making changes inside a repo, the following command will show what files are affected:

    git status

You may get a message that will say:

    Untracked files:
        (use "git add <file>..." to include in what will be committed)

        filename

    nothing added to commit but untracked files present (use "git add" to track)

`filename` is where git will list affected filenames.

----
## Adding Affected Files
To add the affected files, run the following command:

    git add filename

After doing so, run the `git status` command. You will get this message:

    git status
    On branch master

    No commits yet

    Changes to be committed:
        (use "git rm --cached <file>..." to unstage)
            new file:   filename
    
----
## Committing Files
This is a way of telling Git that you're committed to adding the "new" changes to your repo files. The following command will commit the files:

    git commit -m "Added add/delete functionality"

Where `Added add/delete functionality` is the message or note option you may include to your commit.

----
## Pushing a Repo
The following command will push the changes to the repo:

    git push origin master

This will push the changes to the `master`

To push changes to a branch:

    git push origin BranchName

----
## Branching
Do you need to make changes to your code but you're afraid of adding more bugs? Usually, you'll want to branch off from the main code when you're adding features. You can think of it as parallel processing!

### Branch Commands
The following command will list all branches in the repo:

    git branch

To create a branch with a `BranchName`:

    git branch BranchName

To delete a branch:

    git branch -d BranchName

To force delete a branch:

    git branch -D BranchName

To rename the current branch to `NewName`:

    git branch -m NewName

To list all your remote branches:

    git branch -a

