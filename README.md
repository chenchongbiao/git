# Git本地仓库

## 创建仓库

Git使用 `git init`命令来初始化一个 Git 仓库，Git 的很多命令都需要在 Git 的仓库中运行，所以 git init 是使用 Git 的第一个命令。

在执行完成 `git init`命令后，Git 仓库会生成一个.git目录，该目录包含了资源的所有元数据，其他的项目目录保持不变

```bash
cd ~
git init #使用当前目录作为Git仓库

# 或者可以使用指定已存在的目录作为Git仓库
mkdir repotest
git init repotest 
```

现在你可以看到在你的项目中生成了 .git 这个子目录。 这就是你的 Git 仓库了，所有有关你的此项目的快照数据都存放在这里。

## 加入跟踪

初始化后，会在 repotest 目录下会出现一个名为 `.git`的目录，所有 Git 需要的数据和资源都存放在这个目录中。

如果当前目录下有几个文件想要纳入版本控制，需要先用 git add 命令告诉Git开始对哪些文件进行跟踪。

```bash
touch aaa 
git add aaa
```

## 查看当前仓库状态

`git status`命令用于查看仓库已被追踪的文件和未被追踪的文件

```bash
echo '# README' >> README
git add README 
git status 
```

## 查看跟踪文件的不同版本

`git diff`命令来查看执行 `git status`的结果的详细信息。在这里可以看到对README文件的变化的记录。

```bash
#修改README后使用git diff查看README这个文件日哟微信
echo '## Title 1' >> README
git diff
```

## 提交

使用 `git add` `命令将想要快照的内容写入缓存区， 而执行 git commit`将缓存区内容添加到仓库中。

```bash
git add README #将已经发生更新的README加入追踪
git commit -m 'add file: aaa and README'
```

现在我们已经将快照写入仓库。如果我们再执行 `git status`会显示清空的状态。

```bash
git status
```

## 提交历史

`git log`显示所有的提交历史记录

```bash
git log
git log --oneline #简要版
```

## 复制一个现在有仓库

使用 `git clone`拷贝一个Git仓库到本地，让自己能够查看该项目，或者进行修改。

克隆仓库的命令格式为：`git clone <reponame>`

```bash
git clone https://github.com/tensorflow/tensorflow.git
#将Github上tensorflow这个Repo克隆至当前目录下的tensorflow目录
ll tensorflow #查看clone下来的东西
```

# Git分支管理

## 在gitbranch目录下创建仓库

```bash
mkdir ./gitbranch && cd ./gitbranch
```

## 对master分支进行操作

```bash
touch master-test
git add master-test
git commit -m 'add file master-test'
```

## 列出gitbranch仓库下所有分支,并创建一个新的分支dev

```bash
git branch #查看当前所处分支
git branch dev #创建新分支dev
git checkout dev #切换分支至dev
git branch# #查看当前所处分支
```

## 在该分支下创建文件并加入仓库

```bash
touch dev-test
git add dev-test
git commit -m 'add file dev-test'
ll
```

## 切换分支至master

```bash
git checkout master
ls
```

## 合并

当你切换分支的时候，Git 会用该分支的最后提交的快照替换你的工作目录的内容， 所以多个分支不需要多个目录。

一旦某分支有了独立内容，你终究会希望将它合并回到你的主分支。 你可以使用以下命令将任何分支合并到当前分支中去：
 `git merge`命令可以多次合并到统一分支，也可以选择在合并之后直接删除被并入的分支。

```bash
git checkout master #切回主分支master
git merge dev #将dev分支合并至master
```

## 合并冲突

合并并不仅仅是简单的文件添加、移除的操作，Git 也会合并修改。

例如，我们可以将在master上的一个文件，在另一个分支dev下进行修改并提交。切换回master分支也进行修改后。对两个分支进行合并，这时git就会提醒会有合并冲突，需要我们手工来解决

```bash
git checkout master
touch merge
git add merge
git commit -m 'add merge empty file'
 
git checkout dev
echo merge in dev >> merge
git add merge
git commit -m 'modify merge in dev'
 
git checkout master
echo merge in master >> merge
git add merge
git commit -m 'modify merge in master'
 
git merge dev
```

## 删除分支

```bash
git branch -d dev #删除分支
ll #查看分支合并后的文件
```

# Git 远程仓库(remote)

## 克隆远程仓库

`git clone 远程git仓库地址`从远程仓库完全克隆下来

## 提取远程仓库

### `git fetch`

`git fetch`从远程仓库下载新分支与数据

```bash
git fetch origin
```

### `git merge`

`git merge`从远端仓库提取数据并尝试合并到当前分支,该命令就是在执行 git fetch 之后紧接着执行 `git merge`远程分支到你所在的任意分支。

### `git pull`

可以这样简单理解 `git pull = git fetch + git merge`

每次工作之前，先把远程库的最新的内容pull下来，可以立刻看到别人的改动，然后在前人的工作基础上开始一天的工作，这是一个好习惯！！！

另外所有的push操作，只有在pull之后才能进行,换句话来说就是你本地的分支必须先拥有远程分支所有的东西之后，才允许你提交新的修改内容。

#### `git pull`命令格式

`git pull <远程主机> <远程分支>:<本地分支>`  //将远程分支拉取到本地，并与本地分支合并

`git pull <远程主机> <远程分支>`  //将远程分支拉取到本地，并与当前分支合并,可省略本地分支

`git pull <远程主机>` //本地的当前分支自动与对应的origin主机追踪分支(remote-tracking branch)进行合并。

Git也允许手动建立追踪关系。

`git branch --set-upstream master origin/next`将本地master分支与远程的next分支建立tracking关系

## 设置姓名和邮箱地址

接下来我们要做的就是把本地仓库传到github上去，在此之前还需要设置username和email，因为github每次commit都会记录他们。

```bash
git config --global user.name "bluesky"
git config --global user.email "379865549@qq.com"
```

## 配置远程仓库

远程(remote)服务器端包含多个repository，每个repository可以理解为一个仓库或项目。而每个repository下有多个branch。

### 远程branch指针:master

服务器端的"master"（强调服务器端是因为本地端也有master）就是指向某个repository的一个branch的指针。

远程repo指针

"origin"是指向远程某个repository的指针。

### branch指针:head，`*`号表示

```bash
git remote #查看当前已经配置了哪些远程仓库
 
git remote add origin https://github.com/giter/test #添加一个远程仓库
git remote show origin #查看origin指向远程仓库的信息
 
git remote -v #查看远程仓库的版本变动记录
 
git ls-remote #查看远程仓库的分支
```

## 推送到远程仓库

命令格式：

`git push [alias] [branch]`

将本地仓库 [branch] 分支推送成为 [alias] 远程仓库上的 [branch] 分支

`git push <远程git库完整库名> <本地分支>:<远程分支>`

指定本地分支推送至远程分支

```bash
echo local >> local.txt
git add local.txt
git commit -m 'add a local file'
#此时改动已经提交到了 HEAD，但是还没到远端仓库。
 
git push -u origin master
#origin是指向某个远程仓库，master指向某个仓库下某个branch
#-u 第一次提交时将本地的master与远程的master关联起来
```

返回GitHub上查看，可以在master分支上看到刚刚从本地push上来的local.txt这个文件

## 删除远程仓库

我们需要将远程仓库与本地解除绑定。

```bash
git remote rm origin #查看origin指标指向远程仓库的信息
git remote -v
```

# Git 标签(tag)

如果你达到一个重要的阶段，并希望永远记住那个特别的提交快照，你可以使用 git tag 给它打上标签。

比如说，我们想为我们的 test项目发布一个"1.0"版本。 我们可以用 `git tag -a v1.0` 命令给最新一次提交打上（HEAD）"v1.0"的标签。

-a 选项意为"创建一个带注解的标签"。 不用 -a 选项也可以执行的，但它不会记录这标签是啥时候打的，谁打的，也不会让你添加个标签的注解。 我推荐一直创建带注解的标签。

```bash
git tag -a v1.0
```

当你执行 `git tag -a`命令时，Git 会打开你的编辑器，让你写一句标签注解，就像你给提交写注解一样。

现在，注意当我们执行 `git log --decorate`时，我们可以看到我们的标签了：

```bash
git log --oneline --decorate --graph
```

如果我们忘了给某个提交打标签，又将它发布了，我们可以给它追加标签。

例如，假设我们发布了提交 85fc7e7(上面实例最后一行)，但是那时候忘了给它打标签。 我们现在也可以：

```bash
git tag -a v0.9 85fc7e7
git log --oneline --decorate --graph
```

如果我们要查看所有标签可以使用以下命令：

```bash
git tag
```

指定标签信息命令：

```bash
git tag -a <tagname> -m "test标签"
```

PGP签名标签命令：

```bash
git tag -s <tagname> -m "test标签"
```
