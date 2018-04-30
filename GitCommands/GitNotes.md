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

To switch to another branch:

    git checkout BranchName

## Branch And Merge Example
Usually, working code is left alone. Create a branch to add new features without affecting the main working code.

    git branch NewFeature

You can switch to that branch and start doing work on it.

    git checkout NewFeature

Suppose there's a major bug in the main code that needs an immediate fix. You must switch branch back to the master to do the fix. But do NOTE that if there are uncommitted changes in the current branch that CONFLICTS with the master (or another branch), then GIT will not let you switch branch. So make a commit before switching branch. Now we switch to the master branch.

    git checkout master

And then from the master branch, we will create another branch to do the hotfix.

    git branch -b hotfix

The `-b` flag will automatically switch to that newly created branch. We make changes and fix the bug. Then we commit those changes.

    git commit -a -m "Fixed the duplication bug"

Notice the `-a` flag. This tells git to add all files that were changed/editted. When you're satisfied with the hotfix, you can now merge it back to the master branch.

    git checkout master
    git merge hotfix

We can now delete the hotfix branch because it's no longer needed.

    git branch -d hotfix

Great. Now we can switch to the NewFeature branch and continue our work. Once you're finished with the features, run a commit, and then merge with the master.

    git commit -a -m "New Feauture Added: Navigation Tab"
    git checkout master
    git merge NewFeature

Now we will delete the `NewFeature` branch because it's no longer needed as well.

    git branch -d NewFeature

