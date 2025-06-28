# Git从入门到精通


<!--more-->

>Git在工作中经常用到，然后我相信包括我在内大部分人应该都停留在简单的clone,push,pull等等简单的操作上（虽然大部分时候也确实够用了:joy:）。不过在遇到冲突的时候合并冲突的时候脑子就会一团浆糊，因此我最近仔细学习了一遍git~从命令到原理，希望这篇文章可以帮助到阅读这篇文章的人:wink:

## 版本控制
版本控制是一个项目开发中非常重要的一环，举一个例子，如果你是位图形或网页设计师，可能会需要保存某一幅图片或页面布局文件的所有修订版本（这或许是你非常渴望拥有的功能），采用版本控制系统（VCS）是个明智的选择。 有了它你就可以将选定的文件回溯到之前的状态，甚至将整个项目都回退到过去某个时间点的状态，你可以比较文件的变化细节，查出最后是谁修改了哪个地方，从而找出导致怪异问题出现的原因，又是谁在何时报告了某个功能缺陷等等。 使用版本控制系统通常还意味着，就算你乱来一气把整个项目中的文件改的改删的删，你也照样可以轻松恢复到原先的样子。 但额外增加的工作量却微乎其微。


为了解决这个问题，人们开发了许多中本地版本控制系统，大多都是采用简单的数据库来记录。但是还有一个非常重要的问题必须解决，那就是如果有多个开发者该怎么办？本地记录固然方便，但是不能解决多个系统的多开发者问题。

### 集中化的版本控制系统
集中化的版本控制系统都有一个单一的集中管理的服务器，保存所有文件的修订版本，而协同工作的人们都通过客户端连到这台服务器，取出最新的文件或者提交更新。每个人都可以在一定程度上看到项目中的其他人正在做些什么。 而管理员也可以轻松掌控每个开发者的权限，并且管理一个 CVCS 要远比在各个客户端上维护本地数据库来得轻松容易。事分两面，有好有坏。 这么做最显而易见的缺点是中央服务器的单点故障。 如果宕机一小时，那么在这一小时内，谁都无法提交更新，也就无法协同工作。

### 分布式版本控制系统
于是分布式版本控制系统（Distributed Version Control System，简称 DVCS）面世了。 在这类系统中，像 Git、Mercurial 以及 Darcs 等，客户端并不只提取最新版本的文件快照， 而是把代码仓库完整地镜像下来，包括完整的历史记录。 这么一来，任何一处协同工作用的服务器发生故障，事后都可以用任何一个镜像出来的本地仓库恢复。 因为每一次的克隆操作，实际上都是一次对代码仓库的完整备份。

## Git下文件的三种状态
Git有三种状态，分别是`已提交(committed)`，`已修改(modified)`和`已暂存(staged)`.对应的，Git项目有三个区域，分别是工作区（工作目录），暂存区（索引）、Git目录（git仓库）。

1. 工作区： os上的文件，所有的代码开发编辑在这上面完成，其实这里的工作区就是实际上你操作的文件夹。
2. 暂存区： 可以理解为一个暂存区域，这里面的代码会在下一次commit被提交到Git仓库。不过其实暂存区是一个文件，它保存了下次将要提交的文件列表信息，一般在Git的仓库目录中，按照git的术语叫做“索引”。
3. Git仓库： 由Git object记录着每一次提交的快照，以及链式结构记录的提交变更历史。这是Git中最重要的部分，从其他计算机克隆仓库时，复制的就是这里的数据。

基本的工作流程就是：
1. 在工作区修改文件
2. 将你想要下次提交的更改选择性暂存
3. 提交更新，找到暂存区的文件，将快照永久性存储到 Git 目录。

![git的工作流](/git/workflow.png)

## Git下的一些基本指令

### 获取git仓库
有两种方式可以获得Git项目仓库：
1. 将一个不是Git仓库的本地目录转换为Git仓库
2. 从其他服务器克隆一个已经存在的Git仓库

#### 方法一（初始化仓库）
```shell
git init
```
该命令将创建一个名为 .git 的子目录，这个子目录含有你初始化的 Git 仓库中所有的必须文件，这些文件是 Git 仓库的骨干。 但是，在这个时候，我们仅仅是做了一个初始化的操作，你的项目里的文件还没有被跟踪。 

如果你想追踪某些文件并进行初始提交，可以按如下流程：
```shell
git add xxx
git commit -m 'xxxx'
```

#### 方法二（克隆一个现有的仓库）
```shell
git clone https://github.com/libgit2/libgit2
```
你也可以自定义本地仓库的名字：
```shell
git clone https://github.com/libgit2/libgit2 xxx
```
Git 支持多种数据传输协议。 上面的例子使用的是 https:// 协议，不过你也可以使用 git:// 协议或者使用 SSH 传输协议，比如 user@server:path/to/repo.git 。

### 一个简单的提交更新的流程
要记住，你工作目录下的每一个文件都不外乎这两种状态：`已跟踪` 或 `未跟踪`。在这里你可以认为“已跟踪就是你git add 的文件，而未跟踪就是没有被add过的文件（本博客作者拙见）”。

![文件的生命周期](/git/文件生命周期.png)

上面这四个状态分别是未追踪，未修改，已修改和已暂存。

我相信你应该可以根据这四个词的意思大致推断出每个状态的含义。
1. 未被add的文件（从未被add）是未追踪状态
2. add一个文件后该文件变为暂存状态

#### git status查看文件状态
git status是一个非常有用的指令，它可以帮助你明白你接下来要干什么，限于篇幅（一个人可以专注阅读的最大文字）这里仅仅做一个简单展示（强烈建议你自己在新建一个git仓库后使用add，commit等指令后尝试输入git status试一下）。

如果在克隆仓库后立即使用此命令，会看到类似这样的输出：

```shell
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
nothing to commit, working directory clean
```

这个命令会告诉你当前的分支名称，工作目录中文件的状态等等。

#### 忽略文件
一般我们总会有些文件无需纳入 Git 的管理，也不希望它们总出现在未跟踪文件列表。 通常都是些自动生成的文件，比如日志文件，或者编译过程中创建的临时文件等。 在这种情况下，我们可以创建一个名为 `.gitignore` 的文件，列出要忽略的文件的模式。 来看一个实际的 .gitignore 例子：
```shell
$ cat .gitignore
*.[oa]
*~
```
第一行告诉 Git 忽略所有以 .o 或 .a 结尾的文件。一般这类对象文件和存档文件都是编译过程中出现的。 第二行告诉 Git 忽略所有名字以波浪符（~）结尾的文件.

#### 查看已暂存和未暂存的修改
`git diff` 可以具体的显示哪些行发生了改变

#### 提交更新
git commit -m 'xxx'

如果你不想使用暂存区域，你可以使用`git commit -a -m 'xxx'`来直接进行提交，使用这个命令Git 就会自动把所有已经跟踪过的文件暂存起来一并提交，从而跳过 git add 步骤。

#### 查看历史提交
git log

### 撤销操作
有时候我们提交完了才发现漏掉了几个文件没有添加，或者提交信息写错了。 此时，可以运行带有 --amend 选项的提交命令来重新提交：
```shell
git commit --amend
```
这个命令作者的理解是可以覆盖上一次的-m中的内容。（他是用一个新的提交替换旧的提交，因此旧提交不会出现在历史中）

#### 取消暂存的文件
假设我们现在修改两个文件：
```shell
echo 111 >> a.txt
echo 111 >> b.txt
```
我们本来想`git add a.txt`但是一不小心使用了'git add *'，那么该如何将b.txt取消暂存呢？

我们输入git status试一下：
```shell
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   1.txt
        modified:   a.txt
        modified:   b.txt
```
git status 告诉我们“use "git restore --staged <file>..." to unstage”，请注意这里与官方的教程中“use "git reset HEAD <file>..." to unstage”并不一样，这是因为古早的git reset承载了太多的功能，而`git restore`是在Git 2.23版本（2019年）引入的新命令，作为更清晰、更专门化的命令之一，与`git switch`一起被引入，旨在分解`git checkout`的功能，使得各个命令的职责更明确。

因此我们输入：
```shell
$ git restore --staged b.txt

$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   1.txt
        modified:   a.txt

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   b.txt
```

#### 撤销对文件的修改
如果我不想保留对某一个文件的修改怎么办呢？（如何将它还原成之前某一个时刻的状态？）
注意看上一个条目的第二段：
```shell
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   b.txt
```
也就是说我们只要执行：
```shell
git restore b.txt
```
这样它就会被撤销修改。

> 请务必记得 git restore <file> 是一个危险的命令。 你对那个文件在本地的任何修改都会消失——Git 会用最近提交的版本覆盖掉它。 除非你确实清楚不想要对那个文件的本地修改了，否则请不要使用这个命令。

### 远程仓库
通过运行`git remote`来查看已经配置的远程仓库服务器。如果你是git clone的仓库，那么至少可以看到`origin`，这是默认的名字。
也可以通过运行`git remote -v`查看详细的url。

#### 添加远程仓库
`git remote add <shortname> <url>`添加一个新的远程git仓库，shortname表示一个简写。

#### 从远程仓库抓取与拉取
```shell
$ git fetch <remote>
```
这个命令会访问远程仓库，从中拉取所有你还没有的数据。 执行完成后，你将会拥有那个远程仓库中所有分支的引用，可以随时合并或查看。

如果你使用 clone 命令克隆了一个仓库，命令会自动将其添加为远程仓库并默认以 “origin” 为简写。 所以，git fetch origin 会抓取克隆（或上一次抓取）后新推送的所有工作。 必须注意 git fetch 命令只会将数据下载到你的本地仓库——它并不会自动合并或修改你当前的工作。 当准备好时你必须手动将其合并入你的工作。

当然`git pull`也可以拉取，但是并不一样，这将在后面提及。

#### 推送到远程仓库
`git push <remote> <branch>`,比如当你想将`main`推送到`origin`时，你需要执行：
```shell
$ git push origin main
```
这里需要注意几点：

1. 首先，你推送的那个服务器要写在第一个位置
2. 你可以直接使用`git push`，他会帮你自动填充后面
3. 你要有目标服务器的写入权限，而且之前没有人推送过（否则可能会冲突）。

#### 查看远程仓库
`git remote show <remote>`

#### 远程仓库的重命名与移除
`git remote rename <prev> <next>`

`git remote remove xxx`或者`git remote rm xxx`


## Git分支
分支的好处在于你可以把你的工作从开发主线上分离开，以免影响开发主线。Git的分支是它区别于其他版本管理系统的重要原因，因为Git的分支十分轻量。

**Git保存的不是文件的变化和差异，而是一系列不同时刻的快照**
> 快照与备份不同，快照是数据存储的某一时刻的状态记录；备份则是数据存储的某一个时刻的副本。想象你有一个装满玩具的箱子，快照就像给你的玩具箱拍一张全景照片， 备份就像把整个玩具箱复制一份存到阁楼。

### Git存储信息的本质（Git对象）


#### blob对象
首先我们创建两个文件:
```shell
$ git init
$ echo '111' > a.txt
$ echo '222' > b.txt
$ git add *.txt
```
Git会将整个数据库存储在`.git/`目录下，此时我们查看`.git/objects`目录，会发现里面多了两个文件：
```shell
$ ll
total 0
drwxr-xr-x 1 July 197609 0 Mar 14 16:01 58/
drwxr-xr-x 1 July 197609 0 Mar 14 16:01 c2/
drwxr-xr-x 1 July 197609 0 Mar 14 16:01 info/
drwxr-xr-x 1 July 197609 0 Mar 14 16:01 pack/
```
58和c2就是多出来的文件，里面放了什么东西？
打开58，可以发现里面有一个名为“c9bdf9d017fcd178dc8c073cbfcbb7ff240d6c”的文件（c2里面也有一个）。
里面写了什么？使用cat会发现是一串乱码，不过git为我们提供了api工具来查看文件内容。git cat-file [-t] [-p]， -t可以查看object的类型，-p可以查看object储存的具体内容。
```shell
git cat-file -t 58c9
blob
git cat-file -p 58c9
111
```

这里要查看内容的话要输入“58c9”，也可以输入“58c9bdf9d017fcd178dc8c073cbfcbb7ff240d6c”，实际上这一串就是当此操作的hash值。这就是开始时 Git 存储内容的方式——一个文件对应一条内容， 以该内容加上特定头部信息一起的 SHA-1 校验和为文件命名。 校验和的前两个字符用于命名子目录，余下的 38 个字符则用作文件名。
可以发现这个object是一个`blob`类型的节点，他的内容是111.也就是说这个object存储了a.txt的内容。它只储存的是一个文件的内容，不包括文件名等其他信息。然后将这些信息经过SHA1哈希算法得到对应的哈希值，作为这个object的唯一凭证。

#### 树对象
现在我们创建一个commit
```shell
git commit -am 'init'
```
现在再次查看object下的文件：
```shell
$ ll
total 0
drwxr-xr-x 1 July 197609 0 Mar 14 16:54 4c/
drwxr-xr-x 1 July 197609 0 Mar 14 16:54 51/
drwxr-xr-x 1 July 197609 0 Mar 14 16:01 58/
drwxr-xr-x 1 July 197609 0 Mar 14 16:01 c2/
drwxr-xr-x 1 July 197609 0 Mar 14 16:01 info/
drwxr-xr-x 1 July 197609 0 Mar 14 16:01 pack/

```
你会发现又多了两个文件4c,51。使用cat-file查看其内容和类型：
```shell
$ git cat-file -t 4caa
tree

$ git cat-file -p 4caa
100644 blob 58c9bdf9d017fcd178dc8c073cbfcbb7ff240d6c    a.txt
100644 blob c200906efd24ec5e783bee7f23b5d7c941b0c12c    b.txt

$ git cat-file -p 5104
\tree 4caaa1a9ae0b274fba9e3675f9ef071616e5b209
author July <zhzihao2023@lzu.edu.cn> 1741942475 +0800
committer July <zhzihao2023@lzu.edu.cn> 1741942475 +0800

init


$ git cat-file -t 5104
commit

```
第二种Git object类型——tree，它将当前的目录结构打了一个快照。从它储存的内容来看可以发现它储存了一个目录结构（类似于文件夹），以及每一个文件（或者子文件夹）的权限、类型、对应的身份证（SHA1值）、以及文件名。

第三种Git object类型——commit，它储存的是一个提交的信息，包括对应目录结构的快照tree的哈希值，上一个提交的哈希值（这里由于是第一个提交，所以没有父节点。在一个merge提交中还会出现多个父节点），提交的作者以及提交的具体时间，最后是该提交的信息。

分支信息存储在哪里呢？
```shell
$ cat .git/refs/heads/master
5104a36b91f41192bff5d90c7cced8afbcf24693

```
在Git仓库里面，HEAD、分支、普通的Tag可以简单的理解成是一个指针，指向对应commit的SHA1值。(注意这里指向的就是5104那个commit)
当前时刻Git就像下图一样：
![Git某一时刻的状态](/git/对象树.png)

至此我们知道了Git是什么储存一个文件的内容、目录结构、commit信息和分支的。其本质上是`一个key-value的数据库加上默克尔树形成的有向无环图（DAG）`。


### 分支简介
Git 保存的不是文件的变化或者差异，而是一系列不同时刻的`快照`。在进行提交操作时，Git 会保存一个提交对象（commit object）。 知道了 Git 保存数据的方式，我们可以很自然的想到——该提交对象会包含一个指向暂存内容快照的指针。 但不仅仅是这样，该提交对象还包含了作者的姓名和邮箱、提交时输入的信息以及指向它的父对象的指针。 首次提交产生的提交对象没有父对象，普通提交操作产生的提交对象有一个父对象， 而由多个分支合并产生的提交对象有多个父对象。

让我们新建一个git仓库:
```shell
$ git init
Initialized empty Git repository in C:/study/试验记录/test/.git/
```
新建三个文件如下：
```shell
touch README;touch test.rb;touch LICENSE
```
此时观察.git目录下的objects：
```shell
$ ll ./.git/objects/
total 0
drwxr-xr-x 1 July 197609 0 Mar 18 15:25 info/
drwxr-xr-x 1 July 197609 0 Mar 18 15:25 pack/

```

执行暂存命令：
```shell
$ git add README test.rb LICENSE
$ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   LICENSE
        new file:   README
        new file:   test.rb

```

再次查看.git目录下的objects：
```shell
$ ll ./.git/objects/
total 0
drwxr-xr-x 1 July 197609 0 Mar 18 15:27 e6/
drwxr-xr-x 1 July 197609 0 Mar 18 15:25 info/
drwxr-xr-x 1 July 197609 0 Mar 18 15:25 pack/

```
> :wink:这里会有小伙伴问了，明明创建了三个文件，为什么文件夹里只多了一个e6文件夹呢？事实上，当文件内容为空时，Git 会为它生成一个特定的 SHA-1 哈希值：e69de29bb2d1d6434b8b29ae775ad8c2e48c5391。无论你暂存多少个空文件，只要内容为空，它们都会共享同一个 Blob 对象，因此 `.git/objects/`中只会新增一个目录（e6/），其中包含这个唯一的 Blob 对象。

让我们修改README和test.rb再次add观察：
```shell
$ echo readme >> README ;echo 'test' >> test.rb
git add *
$ ll ./.git/objects/
total 0
drwxr-xr-x 1 July 197609 0 Mar 18 15:43 81/
drwxr-xr-x 1 July 197609 0 Mar 18 15:43 9d/
drwxr-xr-x 1 July 197609 0 Mar 18 15:27 e6/
drwxr-xr-x 1 July 197609 0 Mar 18 15:25 info/
drwxr-xr-x 1 July 197609 0 Mar 18 15:25 pack/
```

此时我们进行commit操作：
```shell
$ git commit -m 'The initial commit of my project'
[master (root-commit) 5cd2079] The initial commit of my project
 3 files changed, 2 insertions(+)
 create mode 100644 LICENSE
 create mode 100644 README
 create mode 100644 test.rb

```
再次查看目录：
```shell
$ ll ./.git/objects/
total 0
drwxr-xr-x 1 July 197609 0 Mar 18 15:43 32/
drwxr-xr-x 1 July 197609 0 Mar 18 15:43 5c/
drwxr-xr-x 1 July 197609 0 Mar 18 15:43 81/
drwxr-xr-x 1 July 197609 0 Mar 18 15:43 9d/
drwxr-xr-x 1 July 197609 0 Mar 18 15:27 e6/
drwxr-xr-x 1 July 197609 0 Mar 18 15:25 info/
drwxr-xr-x 1 July 197609 0 Mar 18 15:25 pack/
```

当使用 `git commit` 进行提交操作时，Git 会先计算每一个子目录（本例中只有项目根目录）的校验和， 然后在 Git 仓库中这些校验和保存为树对象。随后，Git 便会创建一个提交对象， 它除了包含上面提到的那些信息外，还包含指向这个树对象（项目根目录）的指针。 如此一来，Git 就可以在需要的时候重现此次保存的快照。

现在，Git 仓库中有五个对象：三个 blob 对象（保存着文件快照）、一个 树 对象 （记录着目录结构和 blob 对象索引）以及一个 提交 对象（包含着指向前述树对象的指针和所有提交信息）。

让我们查看是否真的如此：
```shell
$ git cat-file -p 32c5
100644 blob e69de29bb2d1d6434b8b29ae775ad8c2e48c5391    LICENSE
100644 blob 8178c76d627cade75005b40711b92f4177bc6cfc    README
100644 blob 9daeafb9864cf43055ae93beb0afd6c7d144bfa4    test.rb

$ git cat-file -t 32c5
tree

$ git cat-file -p 5cd2
tree 32c52e68ba2d6782ea0c127570df0ea2b537f706
author July <zhzihao2023@lzu.edu.cn> 1742283836 +0800
committer July <zhzihao2023@lzu.edu.cn> 1742283836 +0800

The initial commit of my project

$ git cat-file -t 5cd2
commit

```

当前的树结构如下（偷懒用的官方文档中的图片，有时间再画吧。。。）
![首次提交对象及其树结构](/git/首次提交对象及其树结构.png)

我们修改任意一个文件后再次提交。

```shell
$ echo 123123 >> test.rb
$ git add test.rb
$ git commit -m "change"
[master 54e1ea3] change
 1 file changed, 1 insertion(+)

$ ll ./.git/objects/
total 0
drwxr-xr-x 1 July 197609 0 Mar 18 15:43 32/
drwxr-xr-x 1 July 197609 0 Mar 18 15:51 54/
drwxr-xr-x 1 July 197609 0 Mar 18 15:43 5c/
drwxr-xr-x 1 July 197609 0 Mar 18 15:43 81/
drwxr-xr-x 1 July 197609 0 Mar 18 15:43 9d/
drwxr-xr-x 1 July 197609 0 Mar 18 15:51 c7/
drwxr-xr-x 1 July 197609 0 Mar 18 15:27 e6/
drwxr-xr-x 1 July 197609 0 Mar 18 15:51 fd/
drwxr-xr-x 1 July 197609 0 Mar 18 15:25 info/
drwxr-xr-x 1 July 197609 0 Mar 18 15:25 pack/

```

会发现多了三个文件54，c7，fd。
查看其中内容：
```shell
$ git cat-file -p 54e1
tree fd11b6cf48cd9dbb9c6c047d63f1370697bb2407
parent 5cd2079b476e981c9f3f86b6b2c7187f66452fd0
author July <zhzihao2023@lzu.edu.cn> 1742284299 +0800
committer July <zhzihao2023@lzu.edu.cn> 1742284299 +0800

change

$ git cat-file -p c7bc
test
123123

$ git cat-file -p fd11
100644 blob e69de29bb2d1d6434b8b29ae775ad8c2e48c5391    LICENSE
100644 blob 8178c76d627cade75005b40711b92f4177bc6cfc    README
100644 blob c7bc15bebf7caa69e8e6f473c5da515dd0b86e0d    test.rb

```

请观察54e1，你会发现它比之前的commit多了一个`parent 5cd2079b476e981c9f3f86b6b2c7187f66452fd0`。如下图（同样未作更改，使用官方原图）
![提交对象与父对象](/git/提交对象与其父对象.png)

>Git 的分支，其实`本质上仅仅是指向提交对象的可变指针`。 Git 的默认分支名字是 master。 在多次提交操作之后，你其实已经有一个指向最后那个提交对象的 master 分支。 master 分支会在每次提交时自动向前移动。Git 的 master 分支并不是一个特殊分支。 它就跟其它分支完全没有区别。 之所以几乎每一个仓库都有 master 分支，是因为 git init 命令默认创建它，并且大多数人都懒得去改动它（当然在最新版本的git中已经变成了main）。

### 分支创建
使用`git branch <name>`创建新分支，git创建分支十分简单，他只是为你`创建一个可以移动的新的指针`。

```shell
$ git branch
* master


$ git branch testing


$ git branch
* master
  testing


$ git switch testing
Switched to branch 'testing'


$ git branch
  master
* testing

```
这个命令会在当前所在的提交对象上创建一个新的指针。
![两个指向相同提交历史的分支](/git/两个指向相同提交历史的分支.png)

那么，Git 又是怎么知道当前在哪一个分支上呢？ 也很简单，它有一个名为`HEAD`的特殊指针。在 Git 中，它是一个指针，指向当前所在的本地分支（将HEAD想象为当前分支的别名）。

让我们查看一下：
```shell
$ cat .git/logs/HEAD
0000000000000000000000000000000000000000 5cd2079b476e981c9f3f86b6b2c7187f66452fd0 July <zhzihao2023@lzu.edu.cn> 1742283836 +0800        commit (initial): The initial commit of my project
5cd2079b476e981c9f3f86b6b2c7187f66452fd0 54e1ea31276270458ec461f140864b3a37fccac7 July <zhzihao2023@lzu.edu.cn> 1742284299 +0800        commit: change
54e1ea31276270458ec461f140864b3a37fccac7 54e1ea31276270458ec461f140864b3a37fccac7 July <zhzihao2023@lzu.edu.cn> 1742285117 +0800        checkout: moving from master to testing

```
或者使用`git log --oneline --decorate`来查看。
```shell
$ git log --oneline --decorate
54e1ea3 (HEAD -> testing, master) change
5cd2079 The initial commit of my project

```

>你也可以使用git checkout xxx来进行分支切换，但是并不建议，前文中已经提到过git checkout能做的事情太多，容易混淆，建议使用功能更明确的switch。

现在的分支图如下：
![初始分支图](/git/初始分支图.png)

那么这样的好处是什么呢？我们现在修改test.rb并再次提交：
```shell
$ echo new >> test.rb

$ git add test.rb

$ git commit -m "new"
[testing b02a51a] new
 1 file changed, 1 insertion(+)

```
此时我们再次查看head：
```shell
$ git log --oneline --decorate
b02a51a (HEAD -> testing) new
54e1ea3 (master) change
5cd2079 The initial commit of my project
```
也就是说当前的分支图类似这样：
![分支图2](/git/分支图2.png)
如图，`testing`分支向前移动了，但是`master`分支却没有，它仍然指向运行`git witch`时所指的对象。 这就有意思了，现在我们切换回`master`分支看看：
```shell
git switch master
```

那么现在的分支图应该是这样的：
![分支图3](/git/分支图3.png)

>切换分支一是使 HEAD 指回 master 分支，二是将工作目录恢复成 master 分支所指向的快照内容。 也就是说，你现在做修改的话，项目将始于一个较旧的版本。 本质上来讲，这就是忽略 testing 分支所做的修改，以便于向另一个方向进行开发。在切换分支时，一定要注意你工作目录里的文件会被改变。 如果是切换到一个较旧的分支，你的工作目录会恢复到该分支最后一次提交时的样子。 如果 Git 不能干净利落地完成这个任务，它将禁止切换分支。

现在做一些修改再次提交：
```shell
$ echo newnew >> test.rb
$ git add test.rb
$ git commit -m "change test.rb twice"
[master f60624d] change test.rb twice
 1 file changed, 1 insertion(+)

$ git log --oneline --decorate
f60624d (HEAD -> master) change test.rb twice
54e1ea3 change
5cd2079 The initial commit of my project

```

现在，这个项目的提交历史已经产生了分叉。 因为刚才你创建了一个新分支，并切换过去进行了一些工作，随后又切换回 master 分支进行了另外一些工作。 上述两次改动针对的是不同分支：你可以在不同分支间不断地来回切换和工作，并在时机成熟时将它们合并起来。 而所有这些工作，你需要的命令只有 branch、checkout 和 commit。
也就是说当前的分支图是这样的：
![分支图4](/git/分支图4.png)

使用`git log --oneline --decorate --graph --all`查看所有分支。
```shell
$ git log --oneline --decorate --graph --all
* f60624d (HEAD -> master) change test.rb twice
| * b02a51a (testing) new
|/
* 54e1ea3 change
* 5cd2079 The initial commit of my project

```
>由于 Git 的分支实质上仅是包含所指对象校验和（长度为 40 的 SHA-1 值字符串）的文件，所以它的创建和销毁都异常高效。 创建一个新分支就相当于往一个文件中写入 41 个字节（40 个字符和 1 个换行符），如此的简单能不快吗？这与过去大多数版本控制系统形成了鲜明的对比，它们在创建分支时，将所有的项目文件都复制一遍，并保存到一个特定的目录。 完成这样繁琐的过程通常需要好几秒钟，有时甚至需要好几分钟。所需时间的长短，完全取决于项目的规模。 而在 Git 中，任何规模的项目都能在瞬间创建新分支。 

---

> Author: July  
> URL: http://localhost:1313/posts/9ed4b52/  

