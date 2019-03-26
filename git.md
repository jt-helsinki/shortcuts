# Common Docker Command Cheatsheet

A list of common commands for working with Docker. Ensure you have **_JQ_**
installed on your system (`brew install jq`).

## General

#### Show the current branch

`git status`

#### Show the files which have changed

`git diff`

#### Reset to the last commit

`git reset --hard`

## Branches

#### List all the branches in the repository.

`git branch -r`

#### Checkout an existing branch

`git checkout <branch>`

#### Create a new branch

`git checkout -b <new branch>`

#### Delete a branch

`git branch -D <branch>`

## Rebasing 

#### Rebase and Squash

`git rebase --autosquash -i origin/master`

Fix any issues and add them back in.

`git comit --fixup <branchname>`
   
`git add .`

`git rebase --continue`

Finally push your changes.

`git push --force-with-lease`

#### Bash git-merge-squash command

Add the following to ~/.bash_profile

```
git-current-branch() {
  git branch | grep "*" | cut -d ' ' -f 2
}

git-merge-squash() {
  branch=`git-current-branch`
  git checkout master
  git branch -m $branch $branch-OLD
  git checkout -b $branch
  git merge --squash $branch-OLD
  git branch -D $branch-OLD
}anch -D $branch-OLD
}
```

## Rollbacks

#### Rollback on local to a specific commit

`git reset --hard <commit-hash>`

#### Rollback on local and remote

```
git reset --hard <commit-hash>
git push -f origin <destination branch>
```

## Logs & Tags

#### List tags

`git for-each-ref --format='%(refname) Tag: %(taggername) %(taggeremail) %(taggerdate) Commit: %(authorname) %(authoremail) %(authordate)' refs/tags`

#### Display history in a graphical format

`git log --date-order --graph --tags --simplify-by-decoration --pretty=format:'%ai %h %d'`


`








