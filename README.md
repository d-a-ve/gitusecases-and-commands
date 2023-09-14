# Git usecases and commands needed to solve them

## Display the git log in simple readable one line

Use `git log --oneline`

## Undo changes using `revert`
1. Use `git revert <commit-id>`  to undo a commit and return the files to the state of that commit. This creates a new commit to the for the reverted changes.

## Undo changes using `reset`
1. Use `git reset 89f6c3d` to undo a commit while retaining all the changes made after that commit.
1. Use `git reset <commit-id> --hard` to undo a commit and all the changes made after that commit. You can also use `git reset --hard HEAD` if you want to undo to the most recent commit made.
1. Use `git reset <commit-id> --soft` to undo a commit but still want to retain all the unstaged changes.

**Note:** The difference between `revert` and `reset` is that `revert` will undo changes up to the state before the specified commit and `reset` will undo changes up to the state of the specified commit. Also, use `revert` when the changes have been pushed to the remote and `reset` when the changes are still local.

## How to recover files after committing them
Let's say you committed a change but did a hard reset (`git reset --hard HEAD`) to a different commit which removed the latest commit from your current branch. You can restore the file(s) using either `git reflog` or `git checkout`.
1. Use `git checkout <commit-id>` to revert back.
1. In a case where you cannot find or get the `commit-id`, use `git reflog`. `reflog` is a logging mechanism and keeps a track of all the changes against their unique `hash-id or commit-id`. Once you get the hash-id, run `git reflod <hash-id>`.

## How to Recover Files When Changes Are Staged but Not Committed
Suppose you staged a file with `git add <file-name>` and then did a hard reset with `git reset --hard HEAD` before committing. Afterward, you found out that the staged file is missing. In this case, you can recover the file(s).

You can use the command `git fsck` to recover the files after a hard reset. This stands for *file system check*. It checks for all the "dangling blobs" in the .git directory that are not part of any changes. For example, there could be some changes that were staged but not added anywhere.

Once found, you can use `git show <blob-id>` to view the details of the blob. You find out it is the one you are looking for and you want to get the contents back, you can redirect the output to a file using the `>` operator. like such `git show <blob-id> > restored_file.txt`.

Therefore `restored_txt` will contain the contents of that blob.

## Stash unfinished work
let's say you want to start a new work on a local repo and you have unfinished work that you do not want to commit, use `git stash`. 
To view all you stash, use `git stash log`. 
> To delete all stash, use `git stash clear`. 
Note: This action is irreversible. To delete just a single stash, use `git stash drop stash@{index}`

## Recover stashed work
Get the index number for the stash from `git stash list`. Then use one of the two ways below.
1. Using `git stash pop stash@{index}`. 
1. Using `git stash apply stash@{index}`.

> The difference between the two methods is that `git stash pop` schedules the stash for deletion after recovering it, and `git stash apply` keeps the reference even after recovering it.


## Revert to a previous commit

Use `git checkout <commit-id> .`  
**N.B** Don’t forget the final `"."` — You aren’t required to add this, and it may look like it has worked but if you leave this off it will take you to a new “detached head state” where you can make changes and it will allow you to make commits, but nothing will be saved and any commits you make will be lost.

## Check if pull needed in Git

First use `git remote update`, to bring your remote refs up to date. Then you can do one of several things, such as:

1. `git status -uno` will tell you whether the branch you are tracking is ahead, behind or has diverged. If it says nothing, the local and remote are the same.

2. `git show-branch *master` will show you the commits in all of the branches whose names end in 'master' (eg master and origin/master).

3. If you Use -v with git remote update (`git remote -v update`) you can see which branches got updated, so you don't really need any further commands.
