# Git
使用这个插件实现数据定时同步到`github`仓库，但是中途 遇到太多报错了，想着记录一下吧
下面是一些我依次遇到的问题以及相关的操作

[主要的路线参考的是 这里](https://blog.csdn.net/qq_41653564/article/details/156830834)


## 关联Ob和Github
### 如果ob是从0开始的
- `git clone`对应的仓库
	- 不要管理员权限执行Git，否则报错：安全权限的问题
	需要执行：`git config --global --add safe.directory "D:/Paper_tool/Obsidian/Vault/Cognition`
	或者：属性 → 安全 → 高级 → 所有者 → 更改 → 选择你的用户 → 勾选“替换子容器和对象的所有者”。
- `cd`进去
- 把之前的 `.obsidian`的文件夹 复制过去（便于迁移配置）
- 然后obsidian中“打开文件夹”就可以啦！！！
	- ![image](https://img2024.cnblogs.com/blog/3579252/202606/3579252-20260603201739584-14459856.png)
- 搜索栏输入 Git: Set upstream branch，选择：origin/master，完成主分支设置
- 在obsidian输入快捷键"Cmd + P"，然后搜索栏输入 Git: Commit all changes
- "Cmd + P"，然后搜索栏输入 Git: Push
- 好嘞，去看看Github 仓库里面有没有东西把！！！


### 如果ob里面有一些东西了
- 最佳流程是：==GitHub 新建仓库时不要勾选 README/LICENSE/.gitignore，创建一个真正空仓库==
	- 要不然 后续会报错的！！！
- 输入快捷键"Cmd + P"，搜索栏输入 Git: Initialize a new repo，进行初始化。
- 输入快捷键"Cmd + P"，搜索栏输入 Git: Edit remotes，Remote name选origin，url格式：https://github.com/你的用户名/你的仓库名.git
- 搜索栏输入 Git: Set upstream branch，选择：origin/master，完成主分支设置
- 在obsidian输入快捷键"Cmd + P"，然后搜索栏输入 Git: Commit all changes
- "Cmd + P"，然后搜索栏输入 Git: Push
- 好嘞，去看看Github 仓库里面有没有东西把！！！




## Git: Set upstream branch 时候fetch 不了
![屏幕截图 2026-02-15 183932](https://img2024.cnblogs.com/blog/3579252/202602/3579252-20260216132342396-1763917344.png)
报错：
> fatal: unable to access https://github.com/Chasing-Aurora/obsidian-app-data.git/': Failed to connect to github.com port 443 after 2056ms: Couldn't connect to server

- 问题原因：==ti 导致的本机系统端口号和git的端口号不一致导致的== 	，所以需要修改本地访问GitHub的代理设置
- [如果没有挂着 但是还是遇到了以上报错，则直接需要 **取消代理**](https://blog.csdn.net/qq_40296909/article/details/134285451)
- 取消代理是因为，==访问 Gitee 或其它是不需要的，记得取消==，所以要取消代理；或者后悔设置代理了，也可以利用此取消
- 解决办法汇总：
	- 你在git clone吗？可以用镜像源，[就是在http前加一个​ghfast.top/就好了](https://ghfast.top/)
	- [直接上GitHub Desktop，搞定99%的问题](https://zhuanlan.zhihu.com/p/1918967866381808530)
	- [使用ssH 而不是 通过http](https://blog.csdn.net/ljk126wy/article/details/87881923)，ssH需要key 去验证的 
		- [如果最后通过ssh 拉取的时候报错ssh: connect to host github.com port 22: Connection refused.](https://blog.csdn.net/hjy_mysql/article/details/131596257)	
	- 对于在`Obsidian` 中而言，建议使用 修改代理的地址，方法如下哦：

```powershell
# 设置代理——改为ti 的服务地址
git config --global http.proxy http://127.0.0.1:7897   
git config --global https.proxy http://127.0.0.1:7897  
// 或者以下的方式
git config --global http.proxy 127.0.0.1:7897
git config --global https.proxy 127.0.0.1:7897


# 第一次修改完成之后可能 需要重启电脑
# 或者取消代理之后，再次设置一遍
# 这种方法大概率可以成功的，所以不要着急的。可能就是端口不对或者缓存没更新还
# 保险起见可以先在cmd窗口中使用 ipconfig/flushdns 刷新dns缓存
# ---

# 取消代理
git config --global --unset http.proxy
git config --global --unset https.proxy

# 查看代理
git config --global --get http.proxy
git config --global --get https.proxy

# 列表显示
git config --global -l  // 等价于 git config --global --list
git config --global -e

# 修改之后ti速度变慢，访问不了github？
我是重修关闭 然后开启 clas 的 系统代理就可以的 
```

### 测试Github

```powershell
git clone https://github.com/Chasing-Aurora/obsidian-app-data.git 

git clone git@github.com:Chasing-Aurora/obsidian-app-data.git
```

- 从2021年8月13日起，GitHub不再支持使用账户密码进行Git操作，需要使用个人访问令牌（Personal Access Token）或SSH密钥来替代
- 个人访问令牌（经典）的功能类似于普通的 OAuth 访问令牌。它们可以用来代替 HTTPS 上 Git 的密码，或者可以用来 通过 ‘基本身份验证’ 对 API 进行身份验证。
- [使用Token进行拉取](https://zhuanlan.zhihu.com/p/1958283157481722150) 

### 测试Gitee

```powershell
git clone https://gitee.com/coding_chasing/springboot2202.git // Gitee
```


## 选择：origin/main  的时候报错
![屏幕截图 2026-02-16 122955](https://img2024.cnblogs.com/blog/3579252/202602/3579252-20260216132348437-2031500080.png)
> Pushing to https://github.com/Chasing-Aurora/obsidian-app-data.git error: src refspec master does not match any 
error: failed to push some refs to https://github.com/Chasing-Aurora/obsidian-app-data.git'

如果这个时候点击手动去push 的话，会报错如下:
![image](https://img2024.cnblogs.com/blog/3579252/202602/3579252-20260216184252126-1167999662.png)
> fatal: The upstream branch of your current branch does not match the name of your current branch. To push to the upstream branch on the remote, use {{date}} git push origin HEAD:main To push to the branch of the same name on the remote,use git push origin HEAD
To choose either option permanently, see push.default in 'git help config'.
To avoid automatically configuring an upstream branch when its name won't match the local branch, see option 'simple' of branch.autoSetupMerge in 'git help config'.

原因：Obsidian git 插件默认的就是master分支，也就是说提交的代码就是等于
```
$ git pull origin master master
fatal: couldn't find remote ref master
```
意思就是说，插件会把默认的远程的分支的名字叫做master，但是远程刚新建立的一个仓库之后只有默认的分支main ！！！

![image](https://img2024.cnblogs.com/blog/3579252/202602/3579252-20260216190808558-242535643.png)
为什么插件默认了是master，但是Github默认的是main了呢？
[这个是因为自从2023年爆发的美国大规模种族冲突问题之后，技术圈也受到了影响，其中就牵连到了GitHub中用于管理默认分支master，以避免联想奴隶制。在持续的外界影响之下，默认分支由master改为main这一举措被确定在2023年10月1日开始执行！！！](https://github.com/github/renaming "但是因为2023年爆发的美国大规模种族冲突问题之后，技术圈也受到了影响，其中就牵连到了GitHub中用于管理默认分支master，以避免联想奴隶制。在持续的外界影响之下，默认分支由master改为main这一举措被确定在2023年10月1日开始执行！！！")

至于如何将Github修改默认的分支名为之前的master：[方法看这里](https://juejin.cn/post/7326268998491176969)
[关于 报错提示的push.default=simple 的讲解，可以看这里](https://geek-docs.com/git/git-questions/832_git_git_what_is_the_difference_between_pushdefault_matching_and_simple.html)

==所以最后正确的操作是 默认的分支就是master，然后点击Git: Set upstream branch，选择的是origin/master==

### 手动去Git操作 
==保底操作，可以先建立仓库连接，然后成功在git bash里面pull+push 一次，后续的自动化pull+push就可以的自然通过 Obsidian git 操作啦==
```powershell
// 初始化，建立本地文件夹
cd ~/Obsidian-Notes
git init


// 建立关联
# 添加远程仓库
git remote add origin 地址
# 检验添加成功没有
git remote -v
# 具体查看当前关联的origin仓库
git remote show origin

// 拉取
git pull origin 远程分支:本地分支
# 如果远程分支是与当前分支合并，则冒号后面的部分可以省略
git pull origin <远程分支名>

// 检查
# 查看远端分支
git branch -r
# 查看本地分支
git branch 
# 查看所有本地分支及其最后一次提交的信息
git branch -v
# 查看本地分支与远程分支的关系
git branch -vv      // 这将显示本地分支是否跟踪了远程分支


// 推送
git push origin 本地分支:远程分支
# 如果本地分支名与远程分支名相同
git push  origin <本地分支名>
# 首次推送并建立上游关联， Upstream
git push -u origin 本地分支   // 后续可直接 git push 推送，无需每次指定分支


# 补充查看git位置
$ where git
E:\DevOps\Git\Git\mingw64\bin\git.exe
```


[Git部分操作可以参考](https://blog.csdn.net/2301_79518550/article/details/147248298 "可以参考")

### 插件常用的命令

```powershell
Git: Initialize a new repo
Git: Open source control view
Git: Edit remotes
Git: Set upstream branch
Git:Raw command   // 不明白为什么我的会报错呢？请求大神解答
```




# 2026 马年大吉、快快乐乐、家人健健康康！ ！ ！ 
