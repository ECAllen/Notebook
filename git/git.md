# Git

## Workflow

git status
git fetch origin
resolve any conflicts git merge
edit
git status
verify all changes staged
git commit 
git push

## New Repo
git init
git add <....stuff>
git commit

## Add existing repo to github
create repo on github GUI 
Then create ssh origin
git remote add origin git@github.com:username/new_repo 
git push -u origin master

## Switch branch
git checkout <branch name>

## Create branch
git branch <new branch name> 

## Push new branch
git push -u origin <new branch>

## Reset a remote git repo
Delete the .git directory locally.

Recreate the git repostory:
$ cd (project-directory) 
$ git init 
$ (add some files) 
$ git add . 
$ git commit -m 'Initial commit' 

Push to remote server, overwriting. Remember you're going to mess everyone else up doing this … you better be the only client.
$ git remote add origin <url> 
$ git push --force --set-upstream origin master 

## Remove a file from commit without removing it from the drive
git rm --cached <filename>

## Hard reset remove staged 
git reset --hard

## List only untracked files
git ls-files --others --exclude-standard
## diff
git diff
## unstage a file
git reset <filename>

## reset a file to the lst commit
git checkout -- <filename>

## Remove a branch
git branch -d <branch name>
## Show git tags
git tag
## Add a tag
got tag -a <version> -m "some comment"
## Push a tag
git push origin <version>
## Show merged/un-merged branches
git branch --merged|--no-merged
## Checkout tracking branch on remote
git checkout -b [branch] [remotename]/[branch]



