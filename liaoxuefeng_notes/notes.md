TODO ： 分支管理 、 标签管理 、自定义Git 


# 0. git 前传



# 0.*. Windows 使用注意事项
**千万不要使用Windows自带的记事本编辑任何文本文件。** 原因是 Microsoft 开发记事本的团队使用了一个非常弱智的行为来保存 UTF-8 编码的文件，他们自作聪明地在每个文件开头添加了 `0xefbbbf`（十六进制） 的字符，你会遇到很多不可思议的问题，比如，网页第一行可能会显示一个“?”，明明正确的程序一编译就报语法错误，等等，都是由记事本的弱智行为带来的。建议你下载 `Notepad++` 代替记事本，不但功能强大，而且免费！**记得把 Notepad++ 的默认编码设置为 `UTF-8 without BOM` 即可**：
![set-utf8-notepad++](http://www.liaoxuefeng.com/files/attachments/001384907170801199e153159cc4a438bed8d255edf157a000/0)

# 1. init 
> {安装Git} + {创建版本库}

## 1.1. Windows 安装 git
Windows 下要使用很多 Linux/Unix 的工具时，__需要 `Cygwin` 这样的模拟环境__，Git也一样。Cygwin的安装和配置都比较复杂，就不建议你折腾了。不过，有高人已经把模拟环境和Git都打包好了，名叫 `msysgit` （https://git-for-windows.github.io/），只需要下载一个单独的exe安装程序，其他什么也不用装，绝对好用。

- (GFW原因)复制到迅雷，满速下载 ： https://github.com/git-for-windows/git/releases/download/v2.12.0.windows.1/Git-2.12.0-64-bit.exe
- __建议 EOL 使用 git 里面是什么，文件就是，注意选择，别选默认。__(后续附图)


在 {开始} 菜单 --> Git --> 'Git Bash' , 类似命令行窗口，进行初始配置：
```
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```

注意 `git config` 命令的 `--global` 参数，用了这个参数，表示你这台机器上 __所有的 Git 仓库都会使用这个配置__，

- 如果 __对单个仓库__， 不要该参数就可单独指定 (如 `git config user.name "Your Name for single project"`)。


## 1.2. 创建版本库
什么是版本库呢？版本库又名仓库，英文名 Repository，你可以简单理解成一个目录。

<span style="color:red;">
如果你使用Windows系统，为了避免遇到各种莫名其妙的问题，请确保目录名（包括父目录）不包含中文。
</span>


### 1.2.1. 简要步骤：

1. 初始化一个 Git 仓库，使用 `git init` 命令。
2. 添加文件到 Git 仓库，分两步：
    - 2.a. 第一步，使用命令 `git add <file>`，注意，可反复多次使用，添加多个文件；
    - 2.b. 第二步，使用命令 `git commit` ，完成。
3. <span style="color:red;">**注意： 如果 在 commit 之后 再 add ， 这次 add 的改动并没有提交，需要再执行 commit 才算提交。**</span>


## 1.3. 远程仓库 （不同目录也算，只是没什么意义）
### 1.3.1. 第1步：创建 `SSH Key` （git 支持 SSH 协议）
在用户主目录(`C:\Users\<user>`)下，看看有没有`.ssh`目录，如果有，再看看这个目录下有没有 `id_rsa`（私钥，决不能泄漏） 和 `id_rsa.pub`（公钥，随便公开） 这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开 Shell（Windows下打开Git Bash），创建 `SSH Key`：
```
# 邮件地址换成你自己的邮件地址
$ ssh-keygen -t rsa -C "youremail@example.com"  
```

#### 1.3.1.1. SSH警告
当你第一次使用 Git 的 clone 或者 push 命令连接 GitHub 时，会得到一个警告：
```
The authenticity of host 'github.com (xx.xx.xx.xx)' can't be established.
RSA key fingerprint is xx.xx.xx.xx.xx.
Are you sure you want to continue connecting (yes/no)?
```
这是因为Git使用SSH连接，而SSH连接在第一次验证GitHub服务器的Key时，需要你确认GitHub的Key的指纹信息是否真的来自GitHub的服务器，输入yes回车即可。

Git会输出一个警告，告诉你已经把GitHub的Key添加到本机的一个信任列表里了：
```
Warning: Permanently added 'github.com' (RSA) to the list of known hosts.
```
这个警告只会出现一次，后面的操作就不会有任何警告了。

如果你实在担心有人冒充GitHub服务器，输入`yes`前可以对照 [GitHub的RSA Key](https://help.github.com/articles/what-are-github-s-ssh-key-fingerprints/ "https://help.github.com/articles/what-are-github-s-ssh-key-fingerprints/") 的指纹信息是否与SSH连接给出的一致。 https://help.github.com/articles/what-are-github-s-ssh-key-fingerprints/


##### 1.3.1.1.1. 每次 clone 都会有警告？
```
Warning: Permanently added 'github.com,192.30.253.112' (RSA) to the list of known hosts.
```
可以看下这里。 https://github.com/blog/1606-ip-address-changes
>If you are accessing your repositories over the SSH protocol, you will receive a warning message each time your client connects to a new IP address for github.com. As long as the IP address from the warning is in the range of IP addresses in the previously mentioned Help page, you shouldn't be concerned. Specifically, the new addresses that are being added this time are in the range from 192.30.252.0 to 192.30.255.255


#### 1.3.1.2. 【推荐】创建 SSH Key 详细步骤
> 参考 ： Github使用（配置SSHkey） ： https://www.yaohepeng.com/2015/05/08/Github%E4%BD%BF%E7%94%A8%EF%BC%88%E9%85%8D%E7%BD%AESSHkey%EF%BC%89/

1. 生成一个新的 SSH key
    ```
    # 邮件地址换成你自己的邮件地址
    $ ssh-keygen -t rsa -C "youremail@example.com" 
    
    ```
2. 添加 key 到 `SSH-agent` 中
    - step 1. 确保 `ssh-agent` 是开启的 , 输入以下命令： `ssh-agent -s` 和 `eval $(ssh-agent -s)`
    ```
    $ ssh-agent -s
    SSH_AUTH_SOCK=/tmp/ssh-BzSOZ9nxOHp3/agent.228; export SSH_AUTH_SOCK;
    SSH_AGENT_PID=4100; export SSH_AGENT_PID;
    echo Agent pid 4100;
    yaohp@Lenovo-PC MINGW64 ~/Desktop
    #如果出现上面提示就输入下面命令
    $ eval $(ssh-agent -s)
    Agent pid 2044
    
    ```
    - step 2. 添加SSH key到 `SSH-AGENT` : `ssh-add ~/.ssh/<id_dsa>`
    ```
    ssh-add ~/.ssh/<id_dsa>
    Enter passphrase for /c/Users/yaohp/.ssh/id_dsa:
    Identity added: /c/Users/yaohp/.ssh/id_dsa (/c/Users/yaohp/.ssh/id_dsa)

    ```

3. 添加你的 SSHkey 到你的 gitgub 帐号中 ： `clip < ~/.ssh/id_rsa.pub`
    ```
    $ clip < ~/.ssh/id_rsa.pub

    ```

4. 测试连接 ： `ssh -T git@github.com`
    ```
    $ ssh -T git@github.com
    The authenticity of host 'github.com (192.30.252.129)' can't be established.
    RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
    Are you sure you want to continue connecting (yes/no)? yes
    Warning: Permanently added 'github.com,192.30.252.129' (RSA) to the list of known hosts.
    Hi yaohp! You've successfully authenticated, but GitHub does not provide shell access.
    
    ```

5. 大功告成。


### 1.3.2. 第2步：将 `id_rsa.pub`（公钥）中的内容 放到 远程仓库的 `SSH Keys` 中。
GitHub 或 内部的 GitLab 中。

### 1.3.3. 第3步：将 本地仓库 关联到 远程库
- 要关联一个远程库（要先创建 或 已存在），使用命令 `git remote add origin git@server-name:path/repo-name.git`；
- 关联后，使用命令 `git push -u origin master` 第一次推送 master 分支的所有内容；
- 此后，每次本地提交后，只要有必要，就可以使用命令 `git push origin master` 推送最新修改；





#### 1.3.3.1. 【推荐】GitHub 提供的 关联 远程库 的 3 种 方式

在 GitHub 上 新建 一个 Repository ， 她 会如下提示： 

1. …or create a new repository on the command line
    ```
    echo "# test123456" >> README.md
    git init
    git add README.md
    git commit -m "first commit"
    git remote add origin git@github.com:yafey/test123456.git
    git push -u origin master
    
    ```

2. …or push an existing repository from the command line
    ```
    git remote add origin git@github.com:yafey/test123456.git
    git push -u origin master
    
    ```

3. …or import code from another repository (在线导入)
You can initialize this repository with code from a Subversion, Mercurial, or TFS project.



#### 1.3.3.2. github上传项目方法：
1. 在github上创建项目
2. 使用 `git clone https://github.com/xxxxxxx/xxxxx.git` 克隆到本地
3. 编辑项目
4. `git add .` （将改动添加到暂存区）
5. `git commit -m "提交说明"`
6. `git push origin master` 将本地更改推送到远程master分支。

这样你就完成了向远程仓库的推送。



#### 1.3.3.3. 【推荐】一个仓库关联多个 远程库 （自己总结的）
1. 假设我们已经在 GitHub 创建了一个 `test123456` 仓库。
    ```
    $ git remote add origin git@github.com:yafey/test123456.git
    
    ```
2. 如果 再次运行上面的语句， 会报错， 解决方式是 先将 origin 别名 删除。
    ```
    YaFey@YaFey-PC MINGW64 /f/____myCode/test (master)
    $ git remote add origin git@github.com:yafey/test123456.git
    fatal: remote origin already exists.
    
    YaFey@YaFey-PC MINGW64 /f/____myCode/test (master)
    $ git remote rm origin      # 删除 origin 别名
    
    YaFey@YaFey-PC MINGW64 /f/____myCode/test (master)
    $ git remote add origin git@github.com:yafey/test123456.git
    
    ```
3. push ，关联 一个 远程库
    ```
    YaFey@YaFey-PC MINGW64 /f/____myCode/test (master)
    $ git push -u origin master
    Counting objects: 3, done.
    Writing objects: 100% (3/3), 227 bytes | 0 bytes/s, done.
    Total 3 (delta 0), reused 0 (delta 0)
    To github.com:yafey/test123456.git
     * [new branch]      master -> master
    Branch master set up to track remote branch master from origin.
    
    ```
4. 我们知道 origin 是一个别名， 此时我们 在 GitHub 再创建一个 仓库 `test222` ， 
然后 我们 将 步骤 1 中的 `origin` 换成 `origin2` ， 没有报错。
    ```
    YaFey@YaFey-PC MINGW64 /f/____myCode/test (master)
    $ git remote add origin git@github.com:yafey/test222.git
    fatal: remote origin already exists.
    
    YaFey@YaFey-PC MINGW64 /f/____myCode/test (master)
    $ git remote add origin2 git@github.com:yafey/test222.git
    
    ```
5. 类似 步骤 3 ， 关联 另外一个 远程库 ， 成功了。
    ```
    $ git push -u origin2 master
    Counting objects: 3, done.
    Writing objects: 100% (3/3), 227 bytes | 0 bytes/s, done.
    Total 3 (delta 0), reused 0 (delta 0)
    To github.com:yafey/test222.git
     * [new branch]      master -> master
    Branch master set up to track remote branch master from origin2.
    
    ```

6. 如果要同步更新到 两个远程库， 需要分别 push：
    ```
    $ git push origin master
    ...
    
    $ git push origin2 master
    ...
    
    ```


##### 1.3.3.3.1. 关联 不存在的 远程库 会报错
需要先在 GitHub 上 创建一个 远程库 （仓库，Repository），才能关联。
>参考： 引起该错误的原因是，目录中没有文件，空目录是不能提交上去的。
http://www.jianshu.com/p/8d26730386f3

```
YaFey@YaFey-PC MINGW64 /f/____myCode/test (master)
$ git remote add origin git@github.com:yafey/test123456.git

YaFey@YaFey-PC MINGW64 /f/____myCode/test (master)
$ git push -u origin master
error: src refspec master does not match any.
error: failed to push some refs to 'git@github.com:yafey/test123456.git'
```



# 2. 时光机穿梭
- 要随时掌握工作区的状态，使用 `git status` 命令。
- 如果 `git status` 告诉你有文件被修改过，用 `git diff` 可以查看修改内容。
- `HEAD` 指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令 `git reset --hard {commit_id}`。
- 穿梭前，用 `git log` 可以查看提交历史，以便确定要回退到哪个版本。
- 要重返未来，用 `git reflog` 查看命令历史，以便确定要回到未来的哪个版本。
- 每次修改，如果不 `add` 到暂存区，那就不会加入到 `commit` 中。
- 撤销修改：[^1] 
    - 场景1：当你改乱了 **工作区** 某个文件的内容，想直接丢弃工作区的修改时，用命令 `git checkout -- <file>`。
    - 场景2：当你不但改乱了工作区某个文件的内容，还添加到了 **暂存区** 时，想丢弃修改，分两步，第一步用命令 `git reset HEAD <file>`，就回到了场景1，第二步按场景1操作。
    - 场景3：**已经提交** 了不合适的修改到版本库时，想要撤销本次提交，~~参考版本回退一节~~(结合使用`reset`及`reflog`命令)，**不过前提是没有推送到远程库**。

- 删除文件：
命令 `git rm` 用于删除一个文件。如果一个文件已经被提交到版本库，那么你永远不用担心误删，但是要小心，你只能恢复文件到最新版本，**你会丢失 最近一次提交后 你 修改的内容**。

[^1]: Git同样告诉我们，用命令 `git reset HEAD file` 可以把暂存区的修改撤销掉（unstage），重新放回工作区 。

## `log` 简介
- `git log` 命令显示从 **最近 到 最远** 的提交日志。
- 如果嫌输出信息太多，看得眼花缭乱的，可以试试加上 `--pretty=oneline` 参数：
    ```
    $ git log --pretty=oneline
    3628164fb26d48395383f8f31179f24e0882e1e0 append GPL
    ea34578d5496d7dd233c827ed32a8cd576c5ee85 add distributed
    cb926e7ea50ad11b8f9e909c05226233bf755030 wrote a readme file
    
    ```
- `commit id`（版本号）是一个 `SHA1` 计算出来的一个非常大的数字，用十六进制表示。




##  `reset`及 `reflog` 简介
Git 必须知道当前版本是哪个版本，在Git中，
```
HEAD ： 当前版本。
HEAD^ ： 上一个版本。
HEAD^^ ： 上上一个版本。
...
HEAD~100：往上100个版本。
```

0. 用 `HEAD` 表示当前版本，也就是最新的提交 `3628164...882e1e0`（注意我的提交ID和你的肯定不一样），
1. 上一个版本就是 `HEAD^`，
2. 上上一个版本就是 `HEAD^^`，
3. `HEAD~100`：往上100个版本  ，写 100 个 `^` 比较容易数不过来。



回退版本与恢复

- 使用 `git reset` 命令 退到 上一个版本： （`--hard`参数有啥意义？这个后面再讲）
    ```
    $ git reset --hard HEAD^
    HEAD is now at ea34578 add distributed
    
    ```
改为指向 add distributed：
![git-head-move](F:\____myCode\learn_git\liaoxuefeng_notes\img\git-head-move.jpg)

- 恢复到新版本: `git reflog` 用来记录你的每一次命令（查找 版本号）。
    ```
    $ git reflog
    ea34578 HEAD@{0}: reset: moving to HEAD^
    3628164 HEAD@{1}: commit: append GPL
    ea34578 HEAD@{2}: commit: add distributed
    cb926e7 HEAD@{3}: commit (initial): wrote a readme file
    
    ```
    然后 用 `reset` 命令 恢复到最新的版本。
    ```
    $ git reset --hard 3628164
    HEAD is now at 3628164 append GPL
    
    ```

## `diff`命令
我认为 `git diff <file>` 等价于 `git diff HEAD -- <file>`, 都是将 **工作区内的内容** 与 **版本库里面最新版本** 进行比较。

- `git diff HEAD -- <file>` 的意义在于, 第一个参数 `HEAD` 是可变的, 也就是可以将 工作区内的内容 与 **版本库里指定的 任意版本** 进行比较. 如 `git diff 3628 -- <file>`。

- add 之后, 再使用 git diff 将看不到变化, 原因是当前工作区的修改已被添加到暂存区

要查看 **暂存区内的内容** 与 **版本库里面最新版本** 的差别需要加 `--cached` 参数：`git diff --cached`


## `rm` 命令
在 文件管理器 中把文件删了，或者用 `rm` 命令删了后， 同时也要将操作反应在 git 版本库中。

现在你有两个选择，

1. 一是 **确实要从版本库中删除该文件**，那就用命令 `git rm` 删掉，并且 `git commit`：这样，文件就从版本库中被删除了。
    ```
    $ git rm test.txt
    rm 'test.txt'
    $ git commit -m "remove test.txt"
    [master d17efd8] remove test.txt
     1 file changed, 1 deletion(-)
     delete mode 100644 test.txt
    
    ```

2. 另一种情况是 **删错了**，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本：
    ```
    $ git checkout -- test.txt
    
    ```
    `git checkout` 其实是用 **版本库里的版本** 替换 **工作区的版本**，无论工作区是修改还是删除，都可以“一键还原”。






# 3. 分支管理

Git鼓励大量使用分支：

- 查看分支：`git branch`
- 创建分支：`git branch <name>`
- 切换分支：`git checkout <name>`
- 创建+切换分支：`git checkout -b <name>`
- 合并某分支到当前分支：`git merge <name>` 
    - *比如在 `master` 上， `git merge dev` 是 将 dev 分支 merge 到 master 分支。*
- 删除分支：`git branch -d <name>`
- **解决冲突**  ： 打开需要合并的文件，找到冲突内容，选择一个版本保留。
    - 当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。
    - 用 `git log --graph` 命令可以看到分支合并图。
    ```
    $ git log --graph --pretty=oneline --abbrev-commit
    *   59bc1cb conflict fixed
    |\
    | * 75a857c AND simple
    * | 400b400 & simple
    |/
    * fec145a branch test
    ...

    ```
- 分支管理策略
    - Git分支十分强大，在团队开发中应该充分应用。
    - 合并分支时，加上 `--no-ff` 参数就可以用普通模式合并，合并后的历史有分支(Yafey注： **有点类似 解决冲突的感觉**)，能看出来曾经做过合并，而 `fast forward` 合并就看不出来曾经做过合并。
    ```
    # 准备合并 dev 分支，请注意 --no-ff 参数，表示禁用 Fast forward ：
    ## 为什么采用 -no-ff 模式要加入参数 -m 的原因 ： 因为本次合并要创建一个新的 commit，所以加上 -m 参数，把 commit 描述写进去。
    $ git merge --no-ff -m "merge with no-ff" dev
    Merge made by the 'recursive' strategy.
     readme.txt |    1 +
     1 file changed, 1 insertion(+)
    
    
    #合并后，我们用git log看看分支历史：
    $ git log --graph --pretty=oneline --abbrev-commit
    *   7825a50 merge with no-ff
    |\
    | * 6224937 add merge
    |/
    *   59bc1cb conflict fixed
    ...

    ```
    - 分支策略
    在实际开发中，我们应该按照几个基本原则进行分支管理：
        - 首先，**master分支应该是非常稳定的**，也就是仅用来发布新版本，平时不能在上面干活；
        - **那在哪干活呢？干活都在 dev 分支上，也就是说，dev 分支是不稳定的**，到某个时候，比如 1.0 版本发布时，再把 dev 分支合并到 master 上，在 master 分支发布 1.0 版本；
        - 你和你的小伙伴们每个人都在 dev 分支上干活，**每个人都有自己的分支，时不时地往 dev 分支上合并就可以了**。
        - 所以，团队合作的分支看起来就像这样：
        ![git-br-policy](http://www.liaoxuefeng.com/files/attachments/001384909239390d355eb07d9d64305b6322aaf4edac1e3000/0)
    
- Bug 分支 （`stash`命令）
    - 修复 bug 时，我们会通过创建新的 bug 分支进行修复，然后合并，最后删除；
    - 当手头工作没有完成时，先把工作现场 `git stash` 一下，然后去修复 bug，修复后，再 `git stash pop`，回到工作现场。
        - （Yafey 注： 有可能多次 stash 之后，分不出 stash list 中的东西， 我觉得还有一种做法是 可以创建一个新的分支，然后提交，再切换到当前分支，创建一个 bug 分支。）

- Feature 分支 （`branch -D`强行删除分支）
    - 开发一个新 feature，最好新建一个分支；
    - 如果要丢弃一个没有被合并过的分支，可以通过 `git branch -D <name>` 强行删除。
        - 删除没有合并的分支， 会报错： （Yafey 注： 实际开发的角度来说，不用删除 Feature 分支，万一头脑发热又需要了……）
        ```
        $ git branch -d feature-vulcan
        error: The branch 'feature-vulcan' is not fully merged.
        If you are sure you want to delete it, run 'git branch -D feature-vulcan'.
        
        ```
- 多人协作
    - 查看远程库信息，使用 `git remote -v`；（**如果没有推送权限，就看不到 push 的地址**）
    ```
    # 用 git remote -v 显示更详细的信息：
    $ git remote -v
    origin  git@github.com:michaelliao/learngit.git (fetch)
    origin  git@github.com:michaelliao/learngit.git (push)

    # 上面显示了可以抓取和推送的origin的地址。       如果没有推送权限，就看不到 push 的地址。
    
    ```
    - 本地新建的分支如果不推送到远程，对其他人就是不可见的；
    - 从本地推送分支，使用 `git push origin branch-name`，如果推送失败，先用 `git pull` 抓取远程的新提交；
    - 在本地创建和远程分支对应的分支，使用 `git checkout -b branch-name origin/branch-name`，本地和远程分支的名称最好一致；
    ```
    # 如 ： 需要 pull 远程的 dev 分支，并在本地创建 分支名 为 dev 的分支。
    $ git checkout -b dev origin/dev
    
    ```
    - 建立本地分支和远程分支的关联，使 用 `git branch --set-upstream branch-name origin/branch-name`；
    - 从远程抓取分支，使用 `git pull`，如果有冲突，要先处理冲突。
    - **多人协作的工作模式** 通常是这样：
        - 首先，可以试图用 `git push origin branch-name` 推送自己的修改；
        - 如果推送失败，则因为远程分支比你的本地更新，需要先用 `git pull` 试图合并；
        - 如果合并有冲突，则解决冲突，并在本地提交；
        - 没有冲突或者解决掉冲突后，再用 `git push origin branch-name` 推送就能成功！
        - 如果 `git pull` 提示 `“no tracking information”`，则说明本地分支和远程分支的链接关系没有创建，用命令 `git branch --set-upstream branch-name origin/branch-name`。


## 3.1. 分支管理的一些概念
- 一开始的时候，`master` 分支是一条线，Git 用 `master` 指向最新的提交，再用 `HEAD` 指向 `master` ，就能确定 `当前分支`，以及 `当前分支的提交点`。
    - **`HEAD` 严格来说不是指向提交，而是指向 `master`，`master` 才是指向提交的**，所以，`HEAD` 指向的就是当前分支。
- 当我们创建新的分支，例如 `dev` 时，Git新建了一个指针叫 `dev`，指向 `master` 相同的提交，再把 `HEAD` 指向 `dev` ，就表示当前分支在 `dev` 上。
    - **Git 创建一个分支很快，因为除了增加一个 `dev` 指针，改改 `HEAD` 的指向**，工作区的文件都没有任何变化！

- 从现在开始，对工作区的修改和提交就是针对 `dev` 分支了，比如新提交一次后，dev 指针往前移动一步，而 `master` 指针不变：
    ![git-br-dev-fd](http://www.liaoxuefeng.com/files/attachments/0013849088235627813efe7649b4f008900e5365bb72323000/0)

- 假如我们在dev上的工作完成了，就可以把 `dev` 合并到 `master` 上。Git怎么合并呢？最简单的方法，**就是直接把 `master` 指向 `dev` 的当前提交，就完成了合并**：
    - **Git合并分支也很快！就改改指针，工作区内容也不变！**

![git-br-ff-merge](http://www.liaoxuefeng.com/files/attachments/00138490883510324231a837e5d4aee844d3e4692ba50f5000/0)

```
# 1. 首先 创建并切换到 dev 分支
$ git checkout -b dev
Switched to a new branch 'dev'

# 2. 查看当前分支：当前分支前面会标一个*号。
$ git branch
* dev
  master

# 3. 改动文件（略） ， 并提交：
$ git add readme.txt 
$ git commit -m "branch test"
[dev fec145a] branch test
 1 file changed, 1 insertion(+)

# 4. 现在，dev 分支的工作完成，我们就可以 切换回 master 分支：
$ git checkout master
Switched to branch 'master'

# 5. 现在，我们把 dev 分支的工作成果合并到 master 分支上：
(Fast-forward信息，Git告诉我们，这次合并是“快进模式”，也就是直接把master指向dev的当前提交)
$ git merge dev
Updating d17efd8..fec145a
Fast-forward    
 readme.txt |    1 +
 1 file changed, 1 insertion(+)

# 6. 合并完成后，就可以放心地删除dev分支了：
$ git branch -d dev
Deleted branch dev (was fec145a).

# 7. 删除后，查看branch，就只剩下master分支了：
$ git branch
* master
```


## 3.2. `stash` 命令
Git提供了一个 stash 功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作：
之后，用 `git status` 查看工作区，就是干净的（除非有没有被Git管理的文件）。
```
$ git stash
Saved working directory and index state WIP on dev: 6224937 add merge
HEAD is now at 6224937 add merge
```

工作区是干净的，刚才的工作现场存到哪去了？用 `git stash list` 命令看看：
```
$ git stash list
stash@{0}: WIP on dev: 6224937 add merge
```


工作现场还在，Git 把 stash 内容存在某个地方了，但是需要恢复一下，有两个办法：

1. 一是用 `git stash apply` 恢复，**但是恢复后，stash 内容并不删**除，你需要用 `git stash drop` 来删除；
    ```
    # 可以多次 stash，恢复的时候，先用 git stash list 查看，然后恢复指定的 stash，用命令：
    $ git stash list
    $ git stash apply stash@{0}
    $ git stash drop  stash@{0}

    ```
2. 【推荐】另一种方式是用 `git stash pop`，**恢复的同时把 stash 内容也删了**。
    ```
    $ git stash pop
    # On branch dev
    # Changes to be committed:
    #   (use "git reset HEAD <file>..." to unstage)
    #
    #       new file:   hello.py
    #
    # Changes not staged for commit:
    #   (use "git add <file>..." to update what will be committed)
    #   (use "git checkout -- <file>..." to discard changes in working directory)
    #
    #       modified:   readme.txt
    #
    Dropped refs/stash@{0} (f624f8e5f082f2df2bed8a4e09c12fd2943bdd40)
    
    # 再用 git stash list 查看，就看不到任何stash内容了：
    $ git stash list
    
    $
    ```



## 3.3. 删除分支 （本地 、 远程）
1. 删除 合并过 的分支
    ```
    $ git branch -d [合并过 的分支名]
    
    ```
2. 删除 未合并的 分支
    ```
    $ git branch -D [合并过 的分支名]
    
    ```
3. 删除远程分支
    > https://git-scm.com/book/zh/v1/Git-%E5%88%86%E6%94%AF-%E8%BF%9C%E7%A8%8B%E5%88%86%E6%94%AF

    ```
    $ git push origin :[要删除的远程分支名字]
    
    例如要删除 远程的github上的dev分支：
    $ git push origin :dev 

    ```




# 4. 标签管理
- 发布一个版本时，我们通常先在版本库中打一个标签（tag），这样，就唯一确定了打标签时刻的版本。将来无论什么时候，取某个标签的版本，就是把那个打标签的时刻的历史版本取出来。所以，标签也是版本库的一个快照。
Git的标签虽然是版本库的快照，但其实它就是指向某个commit的指针（跟分支很像对不对？但是**分支可以移动，标签不能移动**），所以，创建和删除标签都是瞬间完成的。

- Git有commit，为什么还要引入tag？
**tag 就是一个让人容易记住的有意义的名字，它跟某个 commit id (SHA1) 绑在一起。**


小结 ： 

- 命令 `git tag <name>` 用于新建一个标签，**默认为 HEAD**，也可以指定一个 commit id；
- `git tag -a <tagname> -m "blablabla..."` 可以指定标签信息；
- `git tag -s <tagname> -m "blablabla..."` 可以用 PGP 签名标签；
（用PGP签名的标签是不可伪造的，因为可以验证PGP签名。验证签名的方法比较复杂，这里就不介绍了。）
- 命令 `git tag` 可以查看所有标签 （**注意，标签不是按时间顺序列出，而是按字母排序的。**）。
- 操作标签
    - 命令 `git push origin <tagname>` 可以推送一个本地标签；
    - 命令 `git push origin --tags` 可以推送全部未推送过的本地标签；
    - 命令 `git tag -d <tagname>` 可以删除一个本地标签；
    - 命令 `git push origin :refs/tags/<tagname>` 可以删除一个远程标签。


## 4.1. `tag` 命令简介
敲命令 `git tag <name>` 就可以打一个新标签：
```
$ git tag v1.0

```

可以用命令 `git tag` 查看所有标签：
```
$ git tag
v1.0
```

给以前的提交打上 tag ： 找到历史提交的commit id，然后打上就可以了。
```
$ git log --pretty=oneline --abbrev-commit
7825a50 merge with no-ff
6224937 add merge     <--- 这个地方忘记打 tag 了。
59bc1cb conflict fixed
400b400 & simple

它对应的commit id是6224937，敲入命令：

$ git tag v0.9 6224937


再用命令 git tag 查看标签：注意，标签不是按时间顺序列出，而是按字母排序的。

$ git tag
v0.9
v1.0
```

创建带有说明的标签，用-a指定标签名，-m指定说明文字：
```
$ git tag -a v0.1 -m "version 0.1 released" 3628164
```

用命令 `git show <tagname>` 可以看到说明文字：
```
$ git show v0.1
tag v0.1
Tagger: Michael Liao <askxuefeng@gmail.com>
Date:   Mon Aug 26 07:28:11 2013 +0800

version 0.1 released
```
还可以通过 `-s` 用私钥签名一个标签：
```
$ git tag -s v0.2 -m "signed version 0.2 released" fec145a


签名采用 PGP 签名，因此，必须首先安装gpg（GnuPG），如果没有找到gpg，或者没有gpg密钥对，就会报错：
如果报错，请参考 GnuPG 帮助文档配置 Key。

gpg: signing failed: secret key not available
error: gpg failed to sign the data
error: unable to sign the tag
```


---
删除本地标签
```
$ git tag -d v0.1
Deleted tag 'v0.1' (was e078af9)
```

如果要推送某个标签到远程，使用命令 `git push origin <tagname>`：
```
$ git push origin v1.0
Total 0 (delta 0), reused 0 (delta 0)
To git@github.com:michaelliao/learngit.git
 * [new tag]         v1.0 -> v1.0
```

或者，**一次性 推送 全部 尚未推送到 远程的 本地标签**：`git push origin --tags`
```
$ git push origin --tags
Counting objects: 1, done.
Writing objects: 100% (1/1), 554 bytes, done.
Total 1 (delta 0), reused 0 (delta 0)
To git@github.com:michaelliao/learngit.git
 * [new tag]         v0.2 -> v0.2
 * [new tag]         v0.9 -> v0.9
```

删除已经推送到远程的 标签 （远程标签）：

```
1. 先从本地删除：

$ git tag -d v0.9
Deleted tag 'v0.9' (was 6224937)


2. 然后，从远程删除。删除命令也是push，但是格式如下：

$ git push origin :refs/tags/v0.9
To git@github.com:michaelliao/learngit.git
 - [deleted]         v0.9

3. 要看看是否真的从远程库删除了标签，可以登陆 GitHub 查看。
```




# 5. 自定义 Git
已经配置了 `user.name` 和 `user.email`，实际上，Git还有很多可配置项。

小结

- 忽略某些文件时，需要编写`.gitignore`；
   - 不需要从头写 `.gitignore` 文件，GitHub 已经为我们准备了各种配置文件， 可以直接在线浏览：https://github.com/github/gitignore
   - 忽略文件的原则是：
       1. 忽略操作系统自动生成的文件，比如缩略图等；
       2. 忽略编译生成的中间文件、可执行文件等，也就是如果一个文件是通过另一个文件自动生成的，那自动生成的文件就没必要放进版本库，比如Java编译产生的.class文件；
       3. 忽略你自己的带有敏感信息的配置文件，比如存放口令的配置文件。
- `.gitignore`文件本身要放到版本库里，并且可以对`.gitignore`做版本管理！
- 配置别名
    - 配置文件 ： 
        - 加上 `--global` 是针对 当前用户 起作用的。对应配置文件 `~/.gitconfig`
        ```
        $ cat .git/config 
        [core]
            repositoryformatversion = 0
            filemode = true
            bare = false
            logallrefupdates = true
            ignorecase = true
            precomposeunicode = true
        [remote "origin"]
            url = git@github.com:michaelliao/learngit.git
            fetch = +refs/heads/*:refs/remotes/origin/*
        [branch "master"]
            remote = origin
            merge = refs/heads/master
        [alias]
            last = log -1
        
        ```                                                                                 
        - 如果不加，那只针对当前的仓库起作用。每个仓库的Git配置文件都放在 `.git/config` 文件中。
        ```
        $ cat .gitconfig
        [alias]
            co = checkout
            ci = commit
            br = branch
            st = status
        [user]
            name = Your Name
            email = your@email.com
        
        ```



比如，让 Git 显示颜色，会让命令输出看起来更醒目：这样，Git 会适当地显示不同的颜色。
```
$ git config --global color.ui true
```


## 5.1. 常见别名
>`--global` 参数是全局参数，也就是这些命令在 **当前用户** 的所有Git仓库下都有用。 （一般你的工作电脑就一个用户，你自己独占整个硬盘，此时global=system）
` --system` system是**整台电脑**

`st` 就表示 `status` ， 以后敲 `git st` 就表示 `git status`
```
$ git config --global alias.st status
```

很多人都用`co`表示 `checkout`，`ci`表示`commit`，`br`表示`branch`：以后提交就可以简写成：`$ git ci -m "bala bala bala..."`

```
$ git config --global alias.co checkout
$ git config --global alias.ci commit
$ git config --global alias.br branch
```

命令`git reset HEAD file`可以把暂存区的修改撤销掉（unstage），重新放回工作区。既然是一个unstage操作，就可以配置一个unstage别名：
```
$ git config --global alias.unstage 'reset HEAD'

```



**直接替换用户配置文件 `~/.gitconfig` 中的别名 `[alias]`  ** 
```
[alias]
    last = log -1
    co = checkout
    ci = commit
    br = branch
    st = status
    unstage = reset HEAD
    lg = "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```
![git-lg](http://www.liaoxuefeng.com/files/attachments/00138492662982594cbd1a942114472aeeb5f0a502faed1000/0 "`git lg` 的效果")


---
# TODO
## `git add` 各种用法
git add 的各种区别:
```
git add -A   // 添加所有改动

git add *     // 添加新建文件和修改，但是不包括删除

git add .    // 添加新建文件和修改，但是不包括删除

git add -u   // 添加修改和删除，但是不包括新建文件
在 commit 前撤销 add:

git reset <file> // 撤销提交单独文件

git reset        // unstage all due changes
add/commit 前撤销对文件的修改:

git checkout -- README.md  // 注意, add添加后(同commit提交后)就无法通过这种方式撤销修改
```


## `git commit -a`
git提交过程是先 git add将其添加到暂存区，git commit将其提交到当前分支，git commit -m的作用是添加本次提交的备注信息，一般写改动了什么。
git commit -a 就是将git add和git commit合并，用了git commit -a 后不用再git add




