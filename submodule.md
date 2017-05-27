

.gitconfig
```
[user]
 name = Huang, Yuefei
 email =
[alias]
 co = checkout
 ci = commit
 br = branch
 plo = pull origin
 plom = pull origin master ; git pull
 m = master
 sbforeach = submodule foreach 'git pull origin $(git config --file $toplevel/.gitmodules submodule.$name.branch)'
 unstage = reset HEAD
 last = log -1
 lg = log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit
 lgbr = log --oneline --decorate
 st = status
 pso = push origin
 sbi = submodule init
 sbu = submodule update
 visual = !gitk
#[diff]
# tool = bc3
[merge]
 tool = bc3
#[difftool "bc3"]
# path = C:\\dev4yafey\\_GreenSoft\\BCompare4\\BCompare.exe
[mergetool "bc3"]
 path = C:\\dev4yafey\\_GreenSoft\\BCompare3\\BCompare.exe

[core]
 autocrlf = false
```



---
# Google key : `git config --file $toplevel`
- git-submodule foreach: Add $toplevel variable
https://git.kaarsemaker.net/git/commit/f030c96d8643fa0a1a9b2bd9c2f36a77721fb61f/
```
    git submodule foreach 'git pull origin $(git config --file $toplevel/.gitmodules submodule.$name.branch)'
```
- remove the [gitlink](https://stackoverflow.com/a/16581096/6309): `git rm --cached mysubmodule`
 - How to make top-level git to track all the files under another sub-directory git  https://stackoverflow.com/questions/3115249/how-to-make-top-level-git-to-track-all-the-files-under-another-sub-directory-git

# Git submodules: Specify a branch/tag
https://stackoverflow.com/questions/1777854/git-submodules-specify-a-branch-tag
umläute refines dtmland's command with a simplified version [in the comments](https://stackoverflow.com/questions/1777854/git-submodules-specify-a-branch-tag/18799234#comment54105039_18799234):
```
git submodule foreach -q --recursive 'git checkout $(git config -f $toplevel/.gitmodules submodule.$name.branch || echo master)'
```
multiple lines:
```
git submodule foreach -q --recursive \
  'git checkout \
  $(git config -f $toplevel/.gitmodules submodule.$name.branch || echo master)'
```


## GETTING GIT SUBMODULE TO TRACK A BRANCH (add submodule 的步骤)
https://www.activestate.com/blog/2014/05/getting-git-submodule-track-branch
  - 删除 submodule : `git rm --cached <mysubmodule>`
  - add submodule : `git submodule add --force -b BUAT <git url> <submodule path>`
  - Add a submodule which can't be removed from the index
  https://stackoverflow.com/questions/12218420/add-a-submodule-which-cant-be-removed-from-the-index
>  If the output adding a new submodule is:
```
  'FolderName' already exists in the index
```
>  Tip the next commands
```
  git ls-files --stage
>```
>  The output will be something similar to:
```
  160000 d023657a21c1bf05d0eeaac6218eb5cca8520d16  0  FolderName
```
>Then, to remove the folder index tip:
```
  git rm -r --cached FolderName
```
>  Try again add the submodule




# push tag to remote repository
Git: Is there a way to tag remote branch HEAD directly by commit id?
https://stackoverflow.com/questions/41129817/git-is-there-a-way-to-tag-remote-branch-head-directly-by-commit-id
like this:
```
git push origin refs/remotes/origin/<branch1>:refs/tags/<tagged-branch1>
```

# How do I check out a remote Git branch?
https://stackoverflow.com/questions/1783405/how-do-i-check-out-a-remote-git-branch
```
git checkout -b test origin/test
```
