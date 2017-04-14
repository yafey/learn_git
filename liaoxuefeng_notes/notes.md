TODO ： 分支管理 、 标签管理 、自定义Git 


# 0. git 前传



# 0.*. Windows 使用注意事项
**千万不要使用Windows自带的记事本编辑任何文本文件。** 原因是 Microsoft 开发记事本的团队使用了一个非常弱智的行为来保存 UTF-8 编码的文件，他们自作聪明地在每个文件开头添加了 `0xefbbbf`（十六进制） 的字符，你会遇到很多不可思议的问题，比如，网页第一行可能会显示一个“?”，明明正确的程序一编译就报语法错误，等等，都是由记事本的弱智行为带来的。建议你下载 `Notepad++` 代替记事本，不但功能强大，而且免费！**记得把 Notepad++ 的默认编码设置为 `UTF-8 without BOM` 即可**：
![set-utf8-notepad++](F:\____myCode\learn_git\liaoxuefeng_notes\img\set-utf8-notepad++.jpg)

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




