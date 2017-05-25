
# git 操作集锦
## Git fetch和git pull的区别
http://blog.csdn.net/jeffasd/article/details/53583267

## Git中tag的用法
http://blog.csdn.net/jeffasd/article/details/53583237


GIT pull/fetch from specific tag
https://stackoverflow.com/questions/3964368/git-pull-fetch-from-specific-tag


## git: 行尾crlf换行符\n与\r\n处理，文件属性
- git: 行尾crlf换行符\n与\r\n处理，文件属性  http://www.wy182000.com/2014/01/06/git-行尾crlf换行符n与rn处理/
-  [转]多操作系统平台协同开发时 GIT 的注意事项: 不同操作系统中的换行符 http://blog.csdn.net/igorzhang/article/details/17420949


## git merge是怎样判定冲突的？
https://segmentfault.com/a/1190000003966242
>这篇文章的结论有3：
>- 3路合并
>- revert是重新生成一个新的commit
>- diff是和公共祖先diff
>
>除了第三个之外，其他两个都是看文档即可知道的。
>不知道你翻什么源码。
>最后，其实也并没有解决你自己的问题。在这里我给出一条解决方案：
>- 尽量不要修改同一行上的代码。
>- 如果要加薪的函数，最好每次在不同的位置加。

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


# clone 仓库的时候 自动初始化并更新仓库中的每一个子模块。   （`--recursive` 参数）
```
$ git clone --recursive https://github.com/chaconinc/MainProject
```





--- 
# TODO


## git subtree
> Git管理多个远程仓库 http://www.alivepea.me/linux/git-subtree/

```
git subtree
```


原文
```
Git管理多个远程仓库

“640k ought to be enough for anybody.” –Bill Gates

TinyAlsa有Github和Android两个Git不同的仓库。我希望本地的仓库可以同时 追踪它的更新。并且将自己的更改分别提交到这两个仓库本地分支上。Git subtree可 以很好的胜任这个任务。

实现方法
为远程仓库添加别名。

$ git remote add -f github https://github.com/tinyalsa/tinyalsa.git
$ git remote add -f android https://android.googlesource.com/platform/external/tinyalsa
添加到本地仓库。

# 主分支不显示远程分支的日志
$ git subtree add -P github --squash github/master
# 为远程分支创建本地分支，可基于本地分支开发
$ git subtree split -P github -b github --annotate="(github)"
$ git subtree add -P android --squash android/master
$ git subtree split -P android -b android --annotate="(android)"
更新远程仓库。

$ git subtree pull --squash -P github
$ git subtree pull --squash -P android
对比两个远程仓库的差异

$ git diff android github --
合并两个仓库

# 将Android分支的一个 commit
$ git cherr-pick 73b9c679a656c7b0f5e265dae5a76664c7d03031
好处
这至少带来以下几个好处：

一个仓库就可以很好的管理多个远程仓库。也更方便将本地的更改推送到本地中 心仓库。
与多个仓库比起来，更方便远程仓库间的对比合并等操作。在得到最新的github代码同时，添 加了Android的最新补丁。
在master分支，不用切换分支就可以在两个路径上为不同的仓库编译不同的镜像。
git subtree看起来git submodule要灵活些，还能替代Repo的某些功能。Git的 魔法真多啊，大大扩展了版本控制的应用场景。”永远不要觉得640K就足够了”

~EOF~
```