Commands
--------
* ```git checkout -b __newbranch__``` : get a new branch and switch to it
* ```git checkout -b __branchname__ / __branchname__``` : get a local copy of a remote existing branch
* ```git checkout __branchname__  ``` : switch branch
* ```git checkout --orphan __branchname__``` : create branch without full history
* ```git log -n X -p``` : show X last commits with diffs
* ```git log __branchA__ ^__branchB__``` : commits in branch A that aren't in branch B
* ```git log --pretty=oneline --stat --all __foldername__``` : modified files under a given folder
* ```git fetch```
* ```git pull origin __branchname__```
* ```git pull --rebase```: Avoid having a merge commit if you had changes and origin had commits too (rewrites your local history)
* ```git merge --no-ff __sourcebranch__``` : merge into current branch source one without rebase/FF
* ```git push origin __destinationbranch__``` : push merges/changesets to a branch
* ```git remote show origin``` : display the path of the repository
* ```git remote set-url origin xxx``` : Change remote URI to xxx
* ```git remote rm origin``` : Remove remote URI
* ```git remote add origin git@github.com:CartoDB/cartodb.js.git``` : Add remote URI (for example after adding SSH key with ```ssh-add ~/.ssh/id_rsa```). Must first `git remote rm origin` to remove previous one.
* ```git add xxx``` : add files (use . for everything, __folder__/.. for folder recursive children)
* ```git commit``` : commit changes
* ```git status``` : show status of uncommited files
* ```git checkout __file__``` : revert a file
* ```git checkout __branchname__ __file__``` : Checkout all changes to __file__ from branch __branchname__ into current
* ```git checkout __revision__ .``` : revert a full branch to specified revision if not commited
* ```git revert __commit1__ __commit2__ ...```: Reverts certain commits if commited
* ```git reset __revision__ .```: revert a full branch to specified revision if commited
* ```git clean -f``` : remove all local uncommited modifications
* ```git branch``` : display local branches, active one is with a *
* ```git diff``` : Show changes in files
* ```git diff __branch__ origin/__remotebranch__``` : show a diff between a local branch and a remote one
* ```git rebase __branchname__``` : rebases current branch with specified branch (fetches remote branch changes and then adds yours at the tip)
* ```git rm __filename__``` : delete a file from branch and filesystem
* ```git branch -d __branchname__``` : delete a local branch
* ```git push origin --delete __branchname__``` : delete a remote brach
* ```gitk __filename__``` : show visual git log
* ```git reset --soft HEAD~1``` : reset to last commit (even if pushed). Can re-commit stuff but if already pushed will need to push with `--force`.
* ```git reset --soft <new-root-sha1> && git commit --amend -m "<new message>" && git push --force```: squash all branch pushed commits previous to the one specified into a single commit with the desired new message.
* ```git log origin/__branchname__..__branchname__``` : Show diff between local commits and remote commits
* ```git config --list``` : List currently setup config values
* ```git config --global user.name "Kartones"``` : Setup global user name
* ```git config --global user.email "d...@....net"``` : Setup global user email
* ```git config --global credential.helper 'cache --timeout=28800'``` : Make git cache credentials for 8 hours
* ```git config --global color.ui true``` : Activate colors in diffs, etc.
* ```git config --global core.autocrlf true``` : Fix Convert newlines to Unix-style ones (**Windows**)
* ```git config --global core.autocrlf input``` : Fix Convert newlines to Unix-style ones (**Unix**)* 
* ```git config --global pager.log 'diff-highlight | less'```: Better diff highlighting (same for 3 following options)
* ```git config --global pager.show 'diff-highlight | less'```
* ```git config --global pager.diff 'diff-highlight | less'```
* ```git config --global interactive.diffFilter diff-highlight```
* ```git submodule update --init --recursive``` : Init and update all submodules
* ```git submodule init && git submodule update``` : Retrieve and update all submodules (alt)
* Switch from current branch having a submodule to a branch without it: 
```bash 
rm -Rf __submoduledir__
git reset && git checkout .
git checkout __branchname__
``` 
* ```git pull https://github.com/__username__/__reponame__.git __branchname__```: [Merge a pull request to local branch](https://help.github.com/articles/merging-a-pull-request)
* ```git stash```: [Stash](http://git-scm.com/book/en/Git-Tools-Stashing) current changes
* ```git stash apply```: Unstash and merge stored changes
* ```git checkout --theirs xxxx``` ```git checkout --ours xxxx```: Keep changes from incoming branch or local one, respectively.
* ```git blame -M```: Blames original commit, not the move commit
* ```git blame -CCC```: Looks at all commits in history
* ```git cherry-pick __commit__```: merges and commits a specific commit to current branch
* ```git reflog``` + ```git reset HEAD@__commit__```: show all changes on all branches and revert to a specific one
* ```git commit --amend```: Squash a change on previous commit and change the commit message
* ```git diff --staged```: Show both staged and unstaged changes that you will commit
* Undo a commit removing it from history:
```
git reset --hard HEAD~1
# your new commit here
git push origin <branch> --force
```
* Tag any commit of a repo (e.g. with a certain version):
```
git tag <label> <commit-id>
git push origin <label>
```
* [Syncing a fork](https://help.github.com/articles/syncing-a-fork/) (first [configure remote upstream](https://help.github.com/articles/configuring-a-remote-for-a-fork/))
* Apply a `.diff` file: `git apply <filename>.diff`
* Push to a different remote branch: `git push origin <localb-ranch>:<remote-branch>`
* Rename a branch: `git branch -m <old-name> <new-name>`

Searches
--------
* [Issues in which you're mentioned](https://github.com/issues/mentioned)
* [Pull requests in which you're mentioned](https://github.com/pulls/mentioned)
* [All open issues and pull requests of organization `TEST`](https://github.com/issues?utf8=%E2%9C%93&q=is%3Aopen+org%3ATEST+sort%3Aupdated-desc+): Change to proper organization name, and for example can filter to all assigned to you


Third-party Tools
-----------------
* [tig](http://blogs.atlassian.com/2013/05/git-tig/): to navigate commits & branches
* Github:
  * [hub](https://github.com/github/hub): CLI tool to better interact with Github from the command-line
  * [Commands for automatically closing tickets when merged to default branch](https://help.github.com/articles/closing-issues-via-commit-messages/)
  * Search to display all issues/PRs of an organization:
  ```
  https://github.com/issues?utf8=%E2%9C%93&q=is%3Aopen+org%3Athemotion+sort%3Aupdated-desc
  ```
  * Additionally, filter to things assigned to me or involving me:
  ```
  https://github.com/issues?utf8=%E2%9C%93&q=is%3Aopen+org%3Athemotion+sort%3Aupdated-desc+involves%3Akartones
  ```


Tutorials
---------
* [Atlassian Git tutorial](http://www.atlassian.com/git/tutorial/)
* [9 useful tricks of git branch](https://gitbetter.substack.com/p/9-useful-tricks-of-git-branch-you)
* [Git Cookbook](http://git-scm.com/book)
* [Most Common Git tips & tricks](https://github.com/git-tips/tips) (awesome list)
