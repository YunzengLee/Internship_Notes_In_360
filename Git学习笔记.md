本文参考[廖雪峰的官方网站](https://www.liaoxuefeng.com/wiki/896043488029600)整理。

[常用命令及方法大全](https://www.cnblogs.com/miracle77hp/articles/11163532.html)（很有用的参考资料博客）

## 版本控制

- 跨域协同开发
- 追踪记载文件的历史记录
- 并行开发

主流的版本控制器：

- Git
- SVN

### 版本控制的分类

1. 本地版本控制

   记录每次更新，可以对每个版本做一个快照，或者记录补丁文件。

2. 集中版本控制（SVN）

   所有代码放在服务器上，协同开发者从服务器上同步更新或上传修改。

3. 分布式版本控制（Git）

   所有版本信息都同步到每个用户，每个人都有全部代码，可以再本地看到历史记录，可以进行本地提交，联网时再上传服务器。

   不会因网络差或服务器损坏导致无法工作。

### Git与SVN的区别

- SVN：集中式版本控制系统，版本库集中在中央服务器，工作时先从服务器获取最新版本，完成后提交到服务器，必须联网工作。
- Git：分布式版本控制系统，每个用户都是一个完整的版本库，不需联网。每个人在自己电脑上修改，然后将修改部分推送给对方。是目前最先进的版本控制系统。

## 环境配置

[官网](https://git-scm.com/download/)下载exe，安装即可。

安装完成后，开始菜单会有 Git Bash，Git GUI，Git CMD。任意文件夹下右键也可以看到Git Bash，Git GUI.

Git Bash：Linux风格的命令行，使用最多，推荐最多

Git CMD：Win风格的命令行

Git GUI：图形界面的Git

### 修改配置

```bash
git config -l  #查看Git配置
git config --system --list  #查看Git系统配置
```

**相关配置文件：**

所有配置文件其实都保存在本地。

1. Git\etc\gitconfig： Git安装目录下的gitconfig   --system 系统级
2. C:\user\Administrator\ .gitconfig : 只适用于当前用户的配置  --global  全局

> 设置用户名与邮箱 （用户标识，必要）

安装后第一件事，非常重要，因为每次提交都会使用该信息。它被永远地嵌入到了你的提交中：

```bash
git config --global user.name "liyunzeng"
git config --global user.email liyunzeng@360.cn
# 引号加不加都可以
```



如果某个项目中需要使用特殊的用户信息，就去掉 --global



## 基本理论（核心）

> 工作区域

git本地有三个工作区域：工作目录，暂存区，本地资源库，如果再加上远程仓库就是四个工作区域。

Workspace：存放项目代码的地方

Index/Stage：暂存区，临时存放改动。事实上他只是一个文件。

Repository：仓库区，本地仓库。

Remote：远程仓库。托管代码的服务器。

> 工作流程

Git的工作流程一般是这样：

1. 在工作目录中添加、修改文件；
2. 将需要进行版本管理的文件放入暂存区域
3. 将暂存区域的文件提交到Git仓库

因此git管理的文件有三种状态：已修改，已暂存，已提交

## 项目搭建

> 创建工作目录与常用命令

工作目录一般是你希望Git帮你管理的文件夹，可以是项目目录或空目录。

日常使用只需要记住以下几个命令：

![image-20200715101047565](https://github.com/YunzengLee/my_pictures/blob/master/md_pic/image-20200715101047565.png?raw=true)



> 本地仓库搭建

1. 创建全新的

   在Git管理的项目的根目录打开git bash，执行

   ```bash
   git init
   ```

   根目录出现一个  .git文件。说明这个项目开始被git管理。

2. 克隆远程的

   ```bash
   git clone [远程仓库url]
   ```

无论是在本地创建新的，还是克隆了远程的到本地，项目的根目录下都有一个 .git文件。使用git bash进入这个项目后，就会出现一个括号，括号内是当前使用的分支名称（默认是master分支），这就说明进入了一个git管理的项目。

![image-20200715162550181](https://github.com/YunzengLee/my_pictures/blob/master/md_pic/image-20200715162550181.png?raw=true)

> 文件的简单操作

1. 在git管理的项目目录下，新增，删除或修改文件之后，通过 git add . 命令将改动存入暂存区。（git add  <file>）是将指定文件的改动存入暂存区。
2. 然后通过git commit -m “注释信息” 将暂存区的改动存入本地仓库。（commit会将暂存区的多次add一次性存入本地仓库，没有通过add指令放入暂存区的改动不会被commit到本地仓库）
3. 如何将本地仓库推送到远程仓库在下文

## 文件说明

> 文件四种状态

- Untracked：未跟踪，此文件在文件夹中未被加入到git库，不参与版本控制，通过git add状态变为staged
- Unmodify：文件已经入库，未修改，即版本库中文件快照与文件夹中的完全一致。如果被修改就变为Modified状态，也可以通过git rm移出版本库，成为Untracked文件
- Modified：已修改。仅仅是已经修改，没有其他操作。可以通过git add进入暂存staged状态；或者通过git checkout丢弃修改，返回unmodify状态（实际上是从库中读取文件并覆盖已修改文件）
- staged：暂存状态。执行git commit则将修改同步到库中，这时库中的文件和本地文件又变为一致。文件为Unmodify状态，执行git reset HEAD filename 取消暂存，文件状态为Modified。

> 查看文件状态

```bash
git status [filename]# 查看指定文件
git status # 查看所有文件状态
# git add .  添加所有文件到暂存区
# git commit -m "注释消息内容" 提交暂存区中的内容到本地仓库 -m代表提交信息
# 使用方式： git commit -m “first.txt is a new file”

```

> 忽略文件

一个项目中不是所有文件都需要提交（比如临时文件，设计文件），如何忽略一些文件？

在主目录下建立.gitignore 文件，此文件有以下规则：

```bash
#为注释
*.txt  # 忽略所有 .txt结尾的文件
!lib.txt # 但lib.txt除外
/TODO  # 忽略根目录下的TODO文件，不包括其他目录
build/ # 忽略build下所有文件
doc/*.txt # 会忽略doc/notes.txt 但不会忽略doc/server/arch.txt
```

.gitignore只能忽略那些原来没有被track的文件，如果某些文件已经被纳入了版本管理中，则修改.gitignore是无效的。那么解决方法就是先把本地缓存删除（改变成未track状态），然后再提交：

git rm -r --cached .
git add .
git commit -m 'update .gitignore'

## 版本控制

### 版本回退

git log 查看历史commit记录。

HEAD表示当前版本，也就是最近一次提交的版本，上一个版本为HEAD^,  上上个版本是HEAD^^,  往前100个版本就是 HEAD~100.

读取上一个版本：git reset --hard HEAD^

读取某一个版本（以前的版本，或者是站在以前的版本读取未来的版本）：git reset --hard \<commit id> 。 commit id 可以通过git log查看，命令中只输入commit id的前几位是其能够区分即可（不必输入完整commit id）

git reflog可以查看命令的历史记录。





### 撤销修改

#### 工作区的修改

git checkout -- file  ：撤销工作区的修改，让指定文件回到最近一次git commit或git add 时的状态。（如果修改后还没有放入暂存区，就回到和版本库一样的状态，如果放入暂存区后进行了修改，就回到和暂存区一致的状态）

#### 暂存区的修改

```bash
git reset HEAD <filename>
```

可以撤销add到暂存区 但还没有commit的修改。也就是把加入暂存区的文件移出到工作区。

#### 本地仓库的修改

参照版本回退。

#### 撤销远程仓库的最近一次提交

假设远程某分支最新的版本为a，需要撤销，就需要：

1. `git reset --hard [commit id]` 回退到以前的某个版本b，在版本b上修改得到版本c，修改完成后push版本c时（`git push origin 分支名`）会报错，因为当前不是最新版本，在该版本的基础上做的改动无法推送到远程，
2. 这时就要使用命令`git push origin 分支名 -f` , -f 代表强制提交。此时远程仓库的分支上版本a就没有了，版本b之后就是版本c。也就是用版本c替代了版本a。

更多撤销远程提交的方式参考[这里](https://zhuanlan.zhihu.com/p/39645306?utm_source=wechat_session)。

### 删除文件

用git rm \<filename> 删除文件，并commit。

如果误删了某文件，而本地仓库中还有该文件，那么可以恢复误删文件

git checkout -- \<filename>



## 代码提交步骤

- git clone 获取远程仓库的代码
- 修改代码
- git status 查看所有文件的当前状态（可以看到哪些被修改）
- git add . 或 git add xxx 将修改过的文件提交到暂存区
- git commit -m “注释信息” 将暂存区的修改提交到本地仓库
- git pull <远程主机名> <远程分支名> 取回远程仓库某个分支的更新，再与本地的某个分支合并
- git push <远程主机名> <远程分支名>   把当前提交到本地仓库的代码推送到远程主机的某个分支上

## 使用码云

1. 注册登录，完善信息

2. 设置本机绑定ssh公钥，实现免密登录

   ```bash
   # 使用git bash进入C:\users\Administrator\.ssh\ 目录
   # 生成公钥
   ssh-keygen -t rsa ["邮箱地址"]
   # 生成两个文件，一个私钥id_rsa 一个公钥id_rsa.pub，复制公钥文件内容粘贴到码云【ssh公钥】设置里面
   ```

3. 将公钥信息添加到码云账户中即可（也就是在【用户中心】的SSHKey里添加公钥信息）

4. 使用码云创建自己的仓库

   - 点击【新建仓库】，设置名称，介绍，是否开源。（选择的许可证就是对其他人对自己开源代码的使用限制，比如不准商业使用，收费等）

5. 克隆到本地

   git clone [url]

### Linux CentOS7上设置公钥

https://zhidao.baidu.com/question/1639494387685857420.html

### 关联远程仓库

在本地仓库（也就是一个git init新建的项目）的根目录下运行

```bash
git remote add origin git@github.com:michaelliao/learngit.git
```

可以将该新建仓库与远程仓库相关联，origin为该远程仓库的别名，后面是远程仓库的url。

### 克隆远程仓库到本地

在一个存放远程仓库的文件夹下打开Git bash运行：

```bash
git clone 远程仓库url
```

注意：这里是默认克隆所有分支，可以使用`git checkout branchname`进行切换。如果clone指定分支，使用命令：

```bash
git clone -b branchname 远程仓库url
```



将当前本地仓库上当前所在分支 推送到相关联的远程仓库的指定分支上：

```bash
git push 远程仓库url 分支名（比如 master）
```

## 分支管理

### 创建 合并 删除

Git默认有一个名为master的分支。

创建并切换分支：git checkout -b dev

相当于： git branch dev（创建名为dev的分支） + git checkout dev（切换到dev）

（注意：需要在分支a上创建新分支b，就要先切换到分支a。）

git branch 查看当前所有分支及所在分支。

把某个分支的工作**合并**到当前分支： git merge  <分支名>

删除分支： git branch -d dev



最新版本的Git提供了新的`git switch`命令来切换分支：

创建并切换到新的`dev`分支，可以使用：

```
$ git switch -c dev
```

直接切换到已有的`master`分支，可以使用：

```
$ git switch master
```

### 分支冲突

当两个分支对同一个文件的修改有不同时，合并这两个分支就会冲突。需要修改并commit一条分支的内容再合并。

用`git log --graph`命令可以看到分支合并图。

### 分支管理策略

合并分支时，如果可能，Git会用`Fast forward`模式，但这种模式下，删除分支后，会丢掉分支信息。（这种模式本质上是直接把分支master的指针指向当前分支，称为Fast forward模式。加上--no-ff参数就可以禁用这种模式，这样合并分支才是将真正两条分支汇合到一起，汇合点就是一个新的commit）



如果要强制禁用`Fast forward`模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。

合并分支时，加上`--no-ff`参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而`fast forward`合并就看不出来曾经做过合并。命令如下：

```bash
git merge --no-ff -m "merge with no-ff" 分支名
```

合并后，使用`git log` 查看合并历史。

### Bug分支

当前位于分支a下，可以通过`git stash`命令保存当前工作状态（包括目前分支的工作区、暂存区、本地仓库的状态）

然后就可以切换到其他分支进行工作。

切换回分支a后，可以用`git stash list`命令查看保存的工作现场。

恢复到之前保存的工作现场，有两个办法：

一是用`git stash apply`恢复，但是恢复后，stash内容并不删除，你需要用`git stash drop`来删除；

另一种方式是用`git stash pop`，恢复的同时把stash内容也删了

> 问题：如果有master和dev两个分支，在master上修复了一个bug并commit，commit id为4c805e2，那么如何在dev分支上修复同样的bug？

答：切换到dev分支，执行`git cherry-pick 4c805e2`即可，相当于在dev分支上重放这个commit。

### Feature分支

添加新功能时，最好新建一个分支，最后合并到主分支上，如果要丢弃一个未被合并的分支，使用命令`git branch -D <name>`来说删除。

### 多人协作

查看远程仓库信息：

```bash
git remote
git remote -v # 详细信息
```

推送分支：

```bash
git push 远程仓库url或代号origin 远程分支名
```

但是，并不是一定要把本地分支往远程推送，那么，哪些分支需要推送，哪些不需要呢？

- `master`分支是主分支，因此要时刻与远程同步；
- `dev`分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；
- bug分支只用于在本地修复bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个bug；
- feature分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发。

抓取分支：

使用clone命令`git clone git@github.com:michaelliao/learngit.git`克隆远程仓库到本地，只能在本地看到远程仓库的master分支：

```bash
$ git branch
* master
```

要在远程仓库的dev分支上继续开发就需要克隆远程仓库的dev分支到本地：

```bash
$ git checkout -b dev origin/dev
```

然后就可以在dev上继续修改，并add  commit  push。



多人协作的工作模式通常是这样：

1. 首先，可以试图用`git push origin <branch-name>`推送自己的修改；
2. 如果推送失败，则因为远程分支在你读取到本地之后被更新了，需要先用`git pull`命令将远程的更新拉取到本地并试图合并；
3. 如果合并有冲突，`git pull`命令后就会出现报错信息。则需要解决冲突，解决的方法和【分支管理】中的【分支冲突】完全一样。并在本地commit；
4. 没有冲突或者解决掉冲突后，再用`git push origin <branch-name>`推送就能成功！

5. 如果`git pull`提示`no tracking information`，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream-to <本地branch-name> origin/<远程branch-name>`来建立本地某分支与远程某分支的链接。

## 标签管理

### 创建

切换到需要打标签的分支上，

```bash
$ git branch
* dev
  master
$ git checkout master
Switched to branch 'master'
```

然后，敲命令`git tag <name>`就可以打一个新标签,默认打在最新的一次commit上。

也可以找到历史提交的commit id，

```bash
$ git log --pretty=oneline --abbrev-commit
12a631b (HEAD -> master, tag: v1.0, origin/master) merged bug fix 101
4c805e2 fix bug 101
e1e9c68 merge with no-ff
f52c633 add merge
cf810e4 conflict fixed
5dc6824 & simple
14096d0 AND simple
b17d20e branch test
d46f35e remove test.txt
b84166e add test.txt
519219b git tracks changes
e43a48b understand how stage works
1094adb append GPL
e475afc add distributed
eaadf4e wrote a readme file
```

然后对某一个commit打上标签 (v0.9是标签名， 后面跟的是commit id)：

```bash
$ git tag v0.9 f52c633
```

还可以创建带有说明的标签，用`-a`指定标签名，`-m`指定说明文字：

```
$ git tag -a v0.1 -m "version 0.1 released" 1094adb
```

可以用命令`git tag`查看所有标签：

```bash
$ git tag
v0.9
v1.0
```

可以用`git show <tagname>`查看标签信息。

### 其他操作

- 命令`git push origin <tagname>`可以推送一个本地标签到远程仓库；
- 命令`git push origin --tags`可以推送全部未推送过的本地标签；
- 命令`git tag -d <tagname>`可以删除一个未被推送到远程的本地标签；
- 命令`git push origin :refs/tags/<tagname>`可以删除一个远程标签。



