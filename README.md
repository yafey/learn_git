# learn_git
git 
git使用杂记 https://idisfkj.github.io/2017/01/26/git%E4%BD%BF%E7%94%A8%E6%9D%82%E8%AE%B0/

github中README.md插入图片  https://idisfkj.github.io/2016/03/13/github-README/



# 如何将 改动 同步 到 master 和 new_master 上
使用 `git checkout -b [branchname] [tagname]` 在特定的标签上创建一个新分支：

```
...current branch...(master) 
$	git checkout -b issue_fix "v2.0_tag"
```

修改完成后， 在 `issue_fix` 的基础上创建 新的 branch (用于提交到 new_master 分支上)：

```
...current branch...(issue_fix) 
$	git checkout -b issue_fix_new_master
```

然后 在两个分支上分别 pull 对应分支最新的源码：

```
...current branch...(issue_fix) 
$	git pull origin master
$	git push origin issue_fix
.... 在 remote git server 上 merge 代码 (issue_fix ---- merge to ----> master) , 同时 删除 issue_fix 分支， 下次用的话 重新再建。

...current branch...(issue_fix_new_master) 
$	git pull origin Apr_PROD_master
$	git push origin issue_fix_new_master
.... 在 remote git server 上 merge 代码 (issue_fix_new_master ---- merge to ----> new_master ) , 同时 删除 issue_fix_new_master 分支， 下次用的话 重新再建。
```
