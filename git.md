git

1. git add \<file1>: add file to your local repository (index)

   git add . : add all files to your local repository

   git add \<directory>: add directory 

2. git commit: 

   commit and save your files to current branch in your repository

3. git push: 

   upload local repository to remote repository

4. git pull: 

   download files from remote repository (GitHub)

5. git clone:

   clone remote repository

   Build new local repository: git clone \<url> (use https to transfer files), git init 

6. git reset --hard commit_id:

   Change to another version

7. git log: check previous commit id. displays committed snapshots.

   git reflog: check commond history to find commit id

8. git status: displays the state of the working directory and the staging area

9. git diff: check the difference between workspace and stage

   git diff HEAD -- \<file name>: check the difference between workspace and the latest version in your repository

10. git checkout -- file : abandon current changes in workspace and retrieve to the latest version in your repository or index

11. git reset HEAD \<file>: unstage changes after reset and retrieve to the latest version in your repository



分支：

1. git branch dev: create a new branch

2. git checkout dev / git switch dev

   switch to the branch dev

3. git branch: check the state of all branches

4. git merge dev: merge dev branch to the current branch

5. git branch -d \<branch name>

储藏当前变更：

git stash：if you want to switch to another branch and don't want to submit current workspace. You can use git stash to save the current workspace.