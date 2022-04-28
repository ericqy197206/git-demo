# GIT 笔记

## 1.git配置

### 配置用户信息

```bash
git config --global user.name 'eric'
git config --global user.email '6350938@qq.com'
```

### config的三个作用域

```bash
git config --system     # 对系统所有的登录用户有效
git config --global     # 对当前用户的所有项目有效
git config --local      # 只对摸一个项目有效（缺省值）
```

___local只有在git仓库工作区才有效，即存在.git目录.___
___优先级: local>global>system___

### 显示配置 --list

例如

```bash
git config --global --list
```

## 2.创建git仓库

- 项目已目录存在

```bash
cd git-demo
git init
```

- 新建项目，包括项目目录

```bash
git init git-demo
cd git-demo
```

- 添加并提交文件

```bash
git add readme.md
git commit -m"add readme.md"
```

_注：在windows下，如果commit注释中有空格，必须要用双引号._

## 3 工作区和缓存区

### 添加图片

- 添加到暂存区

```bash
git add index.html images
```

- 提交到工作区

```bash
git commit -m"Add index + logo"
```

- 查看Git 日志

```bash
git log
```

### 添加CSS

- 添加到暂存区

```bash
git add index.html styles
```

- 提交到工作区

```bash
git commit -m"Add title line in html & styles"
```

### 再次修改，填写html内容，并添加对应CSS样式

- 添加到暂存区

```bash
git add -u
```

_注：如果只是修改了文件，没有新增删除，直接使用-u参数，git自动识别修改的文件

- 提交到工作区

```bash
git commit -m"Add content in html & add styles"
```

## 4 文件重命名

### 方法1

```cmd
move index.html git.html
git add git.html
git rm index.html
```

git status 可见

```console
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        renamed:    index.html -> git.html
```

### 方法2

- 先还原

```bash
git reset --hard    # 清除当前工作区及暂存区，从当前最新版本中恢复工作区
```

- 使用git命令重命名文件

```bash
git mv index.html git.html
```

git status 可见

```console
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        renamed:    index.html -> git.html
```

### 最后提交

```bash
git commit -m"完成文件名变更"
```

## 5 通过git log查看版本演变历史

git log命令

### 查看当前分支的历史log

```bash
git log
```

输出

```console
commit 1d6aef8ede19a16ae3f7806b937feadce2ee55df (HEAD -> master)
Author: eric <6350938@qq.com>
Date:   Thu Apr 21 14:57:33 2022 +0800

    文件命名

commit 907e4ba5a39015213f800a31dc9427f74166df8b
Author: eric <6350938@qq.com>
Date:   Thu Apr 21 13:46:29 2022 +0800

    完成文件名变更
... ...
```

### --oneline 以简洁模式查看历史log（一个commit一行）

```bash
git log --oneline
```

输出

```console
1d6aef8 (HEAD -> master) 文件命名
907e4ba 完成文件名变更
4feeba9 Add content in html & add styles & update readme.md
605d5f8 Add title line in html & styles
d6f57d2 Add index + logo
4c90c64 add readme.md
```

### -n? 展示最近?次log

```bash
git log -n3 --oneline
```

```console
1d6aef8 (HEAD -> master) 文件命名
907e4ba 完成文件名变更
4feeba9 Add content in html & add styles & update readme.md
```

### -all 展示所有分支log

```bash
git log --all
```

### 展示指定分支log（例如下面例子中的master分支）

```bash
git log master
```

### --graph 以图像方式展示log

```bash
git log -all -graph
```

1. 以上参数可按需要组合
2. 可在浏览器中查看帮助文件git help --web log

## 6 gitk 通过图形界面工具来查看版本历史

在工程目录下运行gitk

```bash
gitk
```

- 在windows平台下gitk中文乱码的解决方案:

```bash
git config --global gui.encoding utf-8
```

- git提交记录中author与committer不是同一个人的情况  
committer从其他分支中选择一个版本进行更新提交，author是当时提交那个版本的作者，committer是当前执行commit操作的人

- parent  
每一次commit都有parent，为上次的基准版本; 除非是第一次提交才会没有parent

- branch  
哪些分支包含了这次commit

- 定制
在view菜单里选择edit view, 可定制界面，例如显示所有分支等等

- 右键菜单
右键菜单提供了很多功能，例如创建tag等

## 7 探索.git目录

- CONFIG文件

git仓库配置，即 git config --local 的配置项

- HEAD文件
内容为指向当前工作分支

- refs目录
heads目录中为分支信息
tags目录中tag标签信息

- objects目录
所有的对象信息都存储这个目录下，包括commit、blob、tree  
例如

```bash
$ find .git/objects/
.git/objects/
.git/objects//6a
.git/objects//6a/85891810bcd4d5afa7e364d13d704dc603a73b    //这个是head指向的第一个对象
.git/objects//pack
.git/objects//7d
.git/objects//7d/4e0fa616551318405e8309817bcfecb7224cff    // 这个是blob对象
.git/objects//9a
.git/objects//9a/327d5e3aa818b98ddaa7b5b369f5deb47dc9f6    //这个是tree对象,记录的是该层目录的信息，子目录依然是tree类型，文本类型是blob
.git/objects//info
```

可以用git cat-file命令查看对象类型和内容

```bash
$git cat-file -t 6a85     // 查看对象类型
commit
$git cat-file -p 6a85     // 查看对象内容
tree 9a327d5e3aa818b98ddaa7b5b369f5deb47dc9f6
author xxx <> 1590659575 +0800
committer xxx <> 1590659575 +0800

add emphasis
```

## 8 commit、tree和blob

### commit

某个时间点的仓库快照，commit指向了一个tree，用于存放这个时间点的仓库文件系统的目录及文件快照

### tree

一个树形文件结构，tree中可以包含tree（子目录）和blob（文件）

### blob

git使用blob存放仓库中的文件，只要文件内容相同，他就是同一个blob，这样可以大大节省存储空间

### 示例

- 查看commit

```bash
C:\Workspace\Development\git\git-demo>git log
commit 20ded561f4e4ff34591254e1c6ce89cde747cc9f (HEAD -> master)
Author: eric <6350938@qq.com>
Date:   Thu Apr 21 19:33:12 2022 +0800

    探密.git目录

commit 8ac3bb78a4bd4ba0715121dd1af6d027e9216b8a
Author: eric <6350938@qq.com>
Date:   Thu Apr 21 18:35:02 2022 +0800

    gitk：通过图形界面工具来查看版本历史
```

- 查看第一个commit内容

```bash
C:\Workspace\Development\git\git-demo>git cat-file -p 20ded561f4e4ff34591254e1c6ce89cde747cc9f
tree f442bdf5415a2f3c897e3caffe74e719494ed679
parent 8ac3bb78a4bd4ba0715121dd1af6d027e9216b8a
author eric <6350938@qq.com> 1650540792 +0800
committer eric <6350938@qq.com> 1650540792 +0800

探密.git目录
```

- 查看commit指向的tree

```bash
C:\Workspace\Development\git\git-demo>git cat-file -p f442bdf5415a2f3c897e3caffe74e719494ed679
100644 blob f5619ea24d29fef82082e8fdd6e5193fbd122c80    git.html
040000 tree d9d84d307e4930baab08c3810adc56f233fc8461    images
100644 blob 1a41c6b0e1b9e575c514fed54241f32447c6c39e    readme.md
040000 tree c3f3edead0f67a10cad0e90241ef5f3df3d8aeed    styles
```

- 查看git.html文件内容

```bash
C:\Workspace\Development\git\git-demo>git cat-file -p f5619ea24d29fef82082e8fdd6e5193fbd122c80
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GIT Demo</title>
    <link rel="stylesheet" href="styles/style.css" type="text/css">
</head>

<body>
    <p class="title-line">This is a GIT demo app.</p>
    <img alt="logo" src="images/logo.png" />
    <p class="content-line">This is content</p>
</body>

</html>
```

- 查看images目录

```bash
C:\Workspace\Development\git\git-demo>git cat-file -p d9d84d307e4930baab08c3810adc56f233fc8461
100644 blob 57a5bdd36689171ba47540af83e88ade21a74e48    logo.png
```

## 9 分离头指针

分离头指针是的是commit从仓库的某一个commit进行分离开发，并且这个来源的commit并不属于任何一个分支的头部

示例

- 查看所有commit

```bash
C:\Workspace\Development\git\git-demo>git log --oneline
4952472 (HEAD -> master) 临时提交
21430cb commit、tree和blob三个对象之间的关系
20ded56 探密.git目录
8ac3bb7 gitk：通过图形界面工具来查看版本历史
27549bb 通过gitlog查看版本演变历史
1d6aef8 文件命名
907e4ba 完成文件名变更
4feeba9 Add content in html & add styles & update readme.md
605d5f8 Add title line in html & styles
d6f57d2 Add index + logo
4c90c64 add readme.md
```

- 切换到其中某一个commit（20ded561f）

```bash
C:\Workspace\Development\git\git-demo>git checkout 907e4ba
Note: switching to '907e4ba'.

You are in 'detached HEAD' state. You can look around, make experimental    
changes and commit them, and you can discard any commits you make in this   
state without impacting any branches by switching back to a branch.

If you want to create a new branch to retain commits you create, you may    
do so (now or later) by using -c with the switch command. Example:

  git switch -c <new-branch-name>

Or undo this operation with:

  git switch -

Turn off this advice by setting config variable advice.detachedHead to false

HEAD is now at 907e4ba 完成文件名变更
```

- 修改文件并提交

例如在git.html中增加一行，然后提交

```bash
git commit -am"演示分离头指针"
```

- 查看当前状态

git log可见当前当前HEAD没有指向任何一个分支

```bash
C:\Workspace\Development\git\git-demo>git log
commit 1a04d7a6a90e325cdf9f14f42a0d9e004e8f17c0 (HEAD)
Author: eric <6350938@qq.com>
Date:   Sat Apr 23 11:42:22 2022 +0800

    演示分离头指针
... ...
```

- 切换到master主分支

```bash
C:\Workspace\Development\git\git-demo>git checkout master
Warning: you are leaving 1 commit behind, not connected to
any of your branches:

  1a04d7a 演示分离头指针

If you want to keep it by creating a new branch, this may be a good time
to do so with:

 git branch <new-branch-name> 1a04d7a

Switched to branch 'master'
```

其中可见git警告  
Warning: you are leaving 1 commit behind, not connected to
any of your branches:

- 使用gitk命令查看

gitk界面中看不到1a04d7a这个commit，这个commit因为与分支分离，后续可能会被git清理掉

- 基于1a04d7a这个commit创建分支

```bash
git branch fix-html 1a04d7a
```

然后在gitk中，可见fix-html分支，并且这个分支指向1a04d7a这个commit

## 10 进一步理解HEAD和branch

### 创建并切换到新分支

```bash
git checkout -b another-fix fix-html
```

1. 基于fix-html，创建一个名为another-fix的分支(也可以基于某一个commit创建分支)
2. -b参数表示创建并直接切换到此分支

### git log 可以看到现在HEAD指向another-fix

```bash
$ git log -n1
commit 1a04d7a6a90e325cdf9f14f42a0d9e004e8f17c0 (HEAD -> another-fix, fix-html)
Author: eric <6350938@qq.com>
Date:   Sat Apr 23 11:42:22 2022 +0800

    演示分离头指针
```

### 查看HEAD文件

```bash
$ cat .git/HEAD
ref: refs/heads/another-fix

$ cat .git/refs/heads/another-fix
1a04d7a6a90e325cdf9f14f42a0d9e004e8f17c0

$ git cat-file -t 1a04d7a6a90e325cdf9f14f42a0d9e004e8f17c0
commit
```

可见HEAD指向heads中的another-fix, another-fix的内容指向一个commit

### 版本比较

- 先查看git log

```bash
$ git log --all
commit f7e5b67819d0da7e1120d4a75e0f07eb9ee161d1 (master)
Author: eric <6350938@qq.com>
Date:   Sat Apr 23 16:02:05 2022 +0800

    临时提交

commit 3c2e792ab19910ccc3f8da89b2118db267621e0f
Author: eric <6350938@qq.com>
Date:   Sat Apr 23 14:49:40 2022 +0800

    临时提交

commit 81344e9fda2e412b7bbcab40889abc3dba6ea7e0
Author: eric <6350938@qq.com>
Date:   Sat Apr 23 12:00:48 2022 +0800

    调整了readme.md格式

commit f394a98a0928968d43b411e14fed776cc61d4b5c
Author: eric <6350938@qq.com>
Date:   Sat Apr 23 11:58:40 2022 +0800

    分离头指针情况下的注意事项

commit 1a04d7a6a90e325cdf9f14f42a0d9e004e8f17c0 (HEAD -> another-fix, fix-html)
Author: eric <6350938@qq.com>
Date:   Sat Apr 23 11:42:22 2022 +0800

    演示分离头指针
```

- 比较两个commit之间的差异

```bash
git diff 3c2e792ab19910ccc3f8da89b2118db267621e0f f7e5b67819d0da7e1120d4a75e0f07eb9ee161d1
```

- 当前版本与commit 3c2e792ab19910ccc3f8da89b2118db267621e0f 之间的差异

```bash
git diff HEAD 3c2e792ab19910ccc3f8da89b2118db267621e0f
```

- 比较当前版本与上一次版本

```bash
git diff HEAD HEAD^
```

- 比较当前版本与之前第2个版本

```bash
git diff HEAD HEAD^^
```

或者

```bash
git diff HEAD HEAD~2
```

## 11 删除不需要的分支

### 先查看分支

```bash
$ git branch -av
  another-fix 1a04d7a 演示分离头指针
  fix-html    1a04d7a 演示分离头指针
  help        0ab2430 进一步理解HEAD和branch
* master      0ab2430 进一步理解HEAD和branch
```

### 分支删除命令

以下命令删除help分支

```bash
git branch -d help
```

_如果分支存在异常无法删除（例如可能报错：The branch 'XXXXXXXX' is not fully merged）, 如果确认要删除, 可以用-D参数强制删除. -D: -d + -f._

## 12 修改最新commit的message

```bash
git commit --amend
```

## 13 修改历史commit的message

### 13.1 先查看历史提交

```bash
$ git log -3
commit 04297535a59d6671be70c0e17e5410f2e9a76e1b (HEAD -> master)
Author: eric <6350938@qq.com>
Date:   Sun Apr 24 09:04:16 2022 +0800

    怎么修改最新commit的message

commit f79593015cde6bf61609dc548e96fc54be8e78ea
Author: eric <6350938@qq.com>
Date:   Sun Apr 24 08:59:49 2022 +0800

    怎么删除不需要的分支（经过amend修改）

commit 0ab2430e6f009ab14bf44ebd92f2c6247dde974e
Author: eric <6350938@qq.com>
Date:   Sat Apr 23 20:40:20 2022 +0800

    进一步理解HEAD和branch
```

### 13.2 修改提交

计划修改commit f7959301的提交信息，因此需要针对commit f7959301的父commit 0ab2430e进行rebase

```bash
git rebase -i 0ab2430e
```

1. git 弹出的编辑器中列出commit 0ab2430e之后的各个子commit, 用于编辑对各个commit的操作命令. 在编辑器中将commit f7959301 的命令由pick(直接采用原来的commit, 不做改动)改为reword(修改commit message), 修改完成后关闭.
2. git 再次弹出的编辑器, 用于修改commit f7959301的message, 修改完成后关闭

_注意:应该只在本地仓库的commit中使用rebase, 如果在远程公用仓库中使用rebase, 容易造成团队开发人员之间工作版本的混乱._

## 14 怎样把连续的多个commit整理成一个

### 14.1 先查看历史提交

```bash
$ git log -3
commit 8928e1c240dc0811c36632fb3f7b4ddb4a4465e9 (HEAD -> master)
Author: eric <6350938@qq.com>
Date:   Sun Apr 24 09:33:35 2022 +0800

    补充了备注信息

commit db72f76a90eec5cf2924330465eb5a7e62d1581b
Author: eric <6350938@qq.com>
Date:   Sun Apr 24 09:28:13 2022 +0800

    怎么修改老旧commit的message

commit 5f4c49e4f12578824a2c51543186e8530db06fae
Author: eric <6350938@qq.com>
Date:   Sun Apr 24 09:04:16 2022 +0800

    怎么修改最新commit的message
```

### 14.2 合并 commit db72f76a 和 commit 8928e1c2

```bash
git rebase -i 5f4c49e4
```

1. git 弹出的编辑器中列出commit 5f4c49e4之后的各个子commit. 保留commit db72f76a不做变化(pick),将commit 8928e1c2合并到上一个commit db72f76a中(squash), 修改完成后关闭.
2. git 再次弹出的编辑器, 用于修改合并后commit的message, 修改完成后关闭

## 15 怎样把不连续的多个commit整理成1个

### 15.1 场景

仓库中存在以下三个commit, 按时间顺序从前往后如下:

1. commit XXXXXXX1 修改了文件A
2. commit XXXXXXX2 修改了文件B
3. commit XXXXXXX3 修改了文件A

现希望合并commit XXXXXXX1和commit XXXXXXX3

### 15.2 步骤

1. 如上例执行 git rebase -i XXXXXXX1之前的那个commit
2. 调整commit行顺序, 将commit XXXXXXX1和commit XXXXXXX3调整为连续的两行
3. 在弹出的编辑器中, 将commit XXXXXXX3的command由skip改为squash, 保存并退出
4. 然后在再次弹出的编辑器里修改合并后commit的message，并保存关系

注意:

1. 如果 XXXXXXX1是仓库的第一个commit，没有之前的commit，则在第1步执行git rebase -i XXXXXXX1，并在第2步把XXXXXXX1手工添加到第一行
2. 如果第3步完成后弹出告警提示, 终止了第4步执行。 如果确认要继续, 可以执行 git rebase --continue

## 16 比较暂存区与HEAD的差异

```bash
git diff --cached
```

## 17 比较工作区与暂存区的差异

```bash
git diff
```

只比对一个文件(readme.md)

```bash
git diff -- readme.md
```

## 18 将暂存区恢复成和HEAD一样

- 恢复整个暂存区

```bash
git reset HEAD
```

或者

```bash
git restore --staged
```

- 只恢复其中一个文件

```bash
git reset HEAD -- readme.md
```

或者

```bash
git restore --staged readme.md
```

## 19 将工作区恢复成和暂存一样

- 恢复当前目录及子目录的变更

```bash
git restore .
```

- 只恢复其中一个文件

```bash
git restore readme.md
```

## 20 消除最近的几次提交

### 20.1 先查看提交历史记录

```bash
$ git log -3
commit 25e5604eb36a46f13a28a3d02b80164dd3f7b945 (HEAD -> master)
Author: eric <6350938@qq.com>
Date:   Tue Apr 26 09:15:15 2022 +0800

    又加上了新的演示内容

commit f517262e4466bc163e33c30cf8af1129e60334ad
Author: eric <6350938@qq.com>
Date:   Tue Apr 26 09:14:41 2022 +0800

    演示内容

commit 6ba59ba64681c4df9ae461ef22cf09d8265baa74
Author: eric <6350938@qq.com>
Date:   Tue Apr 26 09:11:15 2022 +0800

    对课程的内容进行了部分修正,用最新的git restore命令取代了之前的git reset和git checkout
```

### 20.2 删除commit 25e5604e和f517262e, 使HEAD指向6ba59ba6

```bash
git reset --hard 6ba59ba6
```

___注意：此操作不可逆，慎用___

## 21 查看不同commit之间，指定文件的差别

```bash
git diff f54ed705 d1e49316 -- readme.md
```

_也可以直接用分支名称进行比较，例如master, dev..._

## 22 删除文件

在工作区删除文件后，可以通过git add或git rm完成向暂存区的提交

```bash
rm -f test-delete.txt
git add .
```

或者直接用git rm直接完成以上两个步骤

```bash
git rm test-delete.txt
```

## 23 开发中临时加塞了紧急任务怎么办

### 23.1 将当前工作区和缓存区的变更放入临时存储区域

```bash
git stash
```

### 23.2 查看临时存储区域

```bash
git stash list
```

### 23.3 历史任务完成提交后，从临时存储区域将之前的变更还原到工作区

- 还原变更后不清空临时存储区域，可调用git RESET HEAD后反复调用

```bash
git stash apply
```

- 还原变更同时清空临时存储区

```bash
git stash pop
```

## 24 指定不需要git管理的文件

__使用.gitignore指定不需要git管理的文件.__

常见项目类型的.gitignore文件参见github: [A collection of .gitignore templates](https://github.com/github/gitignore)

## 25 git的备份

### 25.1 git常用的传输协议

|常用协议|语法格式|说明|
|:--|:--|:--|
|本地协议(1)|/path/to/repo.git|哑协议|
|本地协议(2)|file:///path/to/repo.git|智能协议|
|http/https协议|<http://git-server.com:port/path/to/repo.git> <br> <https://git-server.com:port/path/to/repo.git>|平时接触到的都是智能协议|
|ssh协议|user@git-server.com:path/to/repo.git|工作中最常用的智能协议|

_智能协议与哑协议的区别：_

- 直观区别: 哑协议传输进度不可见，智能协议传输进度可见
- 传输速度: 智能协议比哑协议传输速度快 ___(按git官方文档的说法，未必是这样)___

### 25.2 git仓库支持多点备份, 以下是git仓库备份示例

- 用哑协议克隆一个纯仓库

```bash
$ git clone --bare ./.git /home/eric/Workspace/Development/git/git-demo-repo
克隆到纯仓库 '/home/eric/Workspace/Development/git/git-demo-repo'...
完成。
```

- 用智能协议克隆一个纯仓库

```bash
$ git clone --bare file:///home/eric/Workspace/Development/git/git-demo/.git /home/eric/Workspace/Development/git/git-demo-repo
克隆到纯仓库 '/home/eric/Workspace/Development/git/git-demo-repo'...
remote: 枚举对象中: 109, 完成.
remote: 对象计数中: 100% (109/109), 完成.
remote: 压缩对象中: 100% (103/103), 完成.
接收对象中: 100% (109/109), 35.79 KiB | 2.56 MiB/s, 完成.
remote: 总共 109 （差异 57），复用 0 （差异 0）
处理 delta 中: 100% (57/57), 完成.
```

__关于使用file://前缀，git官网有如下解释:__

1. 指定file://，Git会启动它通常用于通过网络传输数据的进程，这通常效率要低得多。

2. 指定file://前缀的主要原因是，如果您想要存储库的一个干净副本，而不包含无关的引用或对象 — 通常是在从另一个VCS或类似的东西导入之后（有关维护任务，请参阅[Git Internal](https://git-scm.com/book/en/v2/ch00/ch10-git-internals)）。

### 25.3 向远程仓库推送

- 查看当前的远程仓库

```bash
git remote -v
```

- 添加远程仓路

```bash
git remote add git-demo-repo file:///home/eric/Workspace/Development/git/git-demo-repo
```

- 当前工作区分支是master, 与远端git-demo-repo仓库的master分支建立跟踪，并推送

```bash
$ git push --set-upstream git-demo-repo master
分支 'master' 设置为跟踪来自 'git-demo-repo' 的远程分支 'master'。
Everything up-to-date
```

## 26 注册一个github帐号

内容略

### 26.1 github访问优化

- 确定github的ip

进入网址[IPAddress](https://github.com.ipaddress.com), 查看GitHub的ip地址。

```text
140.82.113.3 github.com
```

- 确定域名ip

进入网址[IPAddress](https://fastly.net.ipaddress.com/github.global.ssl.fastly.net)

```text
199.232.69.194 github.global.ssl.fastly.net
```

- 确定静态资源ip

进入网址[IPAddress](https://github.com.ipaddress.com/assets-cdn.github.com)

```text
185.199.108.153 assets-cdn.github.com
185.199.109.153 assets-cdn.github.com
185.199.110.153 assets-cdn.github.com
185.199.111.153 assets-cdn.github.com
2606:50c0:8000::153 assets-cdn.github.com
2606:50c0:8001::153 assets-cdn.github.com
2606:50c0:8002::153 assets-cdn.github.com
2606:50c0:8003::153 assets-cdn.github.com
```

- 修改hosts

```text
140.82.114.3    github.com
199.232.69.194  github.global.ssl.fastly.net
185.199.108.153 assets-cdn.github.com
185.199.109.153 assets-cdn.github.com
185.199.110.153 assets-cdn.github.com
185.199.111.153 assets-cdn.github.com
```

## 27 配置公私钥

### 27.1 检查是否已存在公私钥

```bash
ls -al ~/.ssh
# Lists the files in your .ssh directory, if they exist
```

github默认的公钥文件有以下几种：

1. id_rsa.pub
2. id_ecdsa.pub
3. id_ed25519.pub

### 27.2 生成公私钥对

```bash
ssh-keygen -t ed25519 -C "6350938@qq.com"
```

如果系统不支持Ed25519签名算法, 可采用RSA

```bash
ssh-keygen -t rsa -b 4096 -C "6350938@qq.com"
```

然后一路回车，生成的公私钥文件如下:

```bash
$ ls -al ~/.ssh
总用量 20
drwx------  2 eric eric 4096 4月  27 11:29 .
drwxr-xr-x 61 eric eric 4096 4月  27 10:23 ..
-rw-------  1 eric eric  411 4月  27 11:29 id_ed25519       # 私钥
-rw-r--r--  1 eric eric   96 4月  27 11:29 id_ed25519.pub   # 公钥
-rw-r--r--  1 eric eric  888 4月  26 15:33 known_hosts
```

### 27.3 将SSH公钥添加到github的profile中

1. 登录github，点击右上角settings
2. SSH and GPG keys -> New SSH key
3. 把id_ed25519.pub文件的内容粘贴到key输入框, title可填可不填, 然后点击Add SSH key

## 28 在github上创建个人仓库(由于github太慢, 后面所有的示例均使用gitee)

过程略，在gitee上创建[git-demo](git@gitee.com:ericqy/git-demo.git), 选择MIT授权协议，不生成.gitignore和readme

## 29 把本地仓库同步到Gitee

### 29.1 添加gitee到remote

```bash
git remote add gitee git@gitee.com:ericqy/git-demo.git
```

### 29.2 向远端推送所有分支

```bash
$ git push gitee --all
The authenticity of host 'gitee.com (180.97.125.228)' can't be established.
ECDSA key fingerprint is SHA256:FQGC9Kn/eye1W8icdBgrQp+KkGYoFgbVr17bmjey0Wc.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'gitee.com,180.97.125.228' (ECDSA) to the list of known hosts.
枚举对象中: 121, 完成.
对象计数中: 100% (121/121), 完成.
使用 4 个线程进行压缩
压缩对象中: 100% (115/115), 完成.
写入对象中: 100% (121/121), 46.90 KiB | 1.56 MiB/s, 完成.
总共 121 （差异 64），复用 0 （差异 0）
remote: Resolving deltas: 100% (64/64), done.
remote: Powered by GITEE.COM [GNK-6.3]
To gitee.com:ericqy/git-demo.git
 * [new branch]      another-fix -> another-fix
 * [new branch]      fix-html -> fix-html
 ! [rejected]        master -> master (fetch first)
error: 推送一些引用到 'git@gitee.com:ericqy/git-demo.git' 失败
提示：更新被拒绝，因为远程仓库包含您本地尚不存在的提交。这通常是因为另外
提示：一个仓库已向该引用进行了推送。再次推送前，您可能需要先整合远程变更
提示：（如 'git pull ...'）。
提示：详见 'git push --help' 中的 'Note about fast-forwards' 小节。
```

__注意: 远端master分支与本地master分支尚未建立关联关系, 因此master分支push报错.__

### 29.3 拉取github上的master分支

```bash
git fetch gitee master
```

_注意:_

1. fetch仅从远程仓库拉取
2. pull在从远程仓库拉取后，与本地仓库做merge(即: git pull直接合并了27.3和27.4两步)

### 29.4 将远端master分支与本地master分支进行merge

```bash
git checkout master                                 # 本地切换到master分支
git merge gitee/master --allow-unrelated-histories # 与远端master分支进行merge
```

### 29.5 向远端推送master分支

```bash
git push gitee master
```

## 30 不同人修改了不同的文件

### 30.1 在gitee上创建一个新分支feature/add_git_commands, 用于演示不同人修改了不同的文件

内容略

### 30.2 将项目克隆到一个新目录

```bash
cd ..
git clone git@gitee.com:ericqy/git-demo.git git-demo-2
cd git-demo-2
```

### 30.3 配置新的工作区的帐号和邮箱

```bash
git config --add --local user.name 'qianyong'
git config --add --local user.email 'qianyong@dscomm.com.cn'
```

### 30.4 创建feature/add_git_commands对应的本地分支, 并切换到此分支

```bash
git checkout -b feature/add_git_commands origin/feature/add_git_commands
```

### 30.5 新添加一个文件 & add & commmit & push remote

```bash
echo 'hello another user.' > new-user.txt
git add .
git commit -m'用另一个用户身份添加文件并提交'
git push
```

### 30.6 切换回原来的目录, 并重新获取远端的更新

```bash
cd ../git-demo
git fetch gitee
```

### 30.7 切换到新增的feature/add_git_commands分支

```bash
git checkout -b feature/add_git_commands gitee/feature/add_git_commands
```

可见新增加的new-user.txt文件

```bash
$ cat new-user.txt
hello another user.
```

### 30.8 用原来的用户添加并提交文件

```bash
$ echo 'hello old user'> old-user.txt
$ git add .
$ git commit -m'用原用户添加文件并提交'
[feature/add_git_commands 08b1694] 用原用户添加文件并提交
 1 file changed, 1 insertion(+)
 create mode 100644 old-user.txt
$ git push gitee feature/add_git_commands
枚举对象中: 4, 完成.
对象计数中: 100% (4/4), 完成.
使用 4 个线程进行压缩
压缩对象中: 100% (2/2), 完成.
写入对象中: 100% (3/3), 312 字节 | 156.00 KiB/s, 完成.
总共 3 （差异 1），复用 0 （差异 0）
remote: Powered by GITEE.COM [GNK-6.3]
To gitee.com:ericqy/git-demo.git
   abcc0c6..08b1694  feature/add_git_commands -> feature/add_git_commands
```

### 30.9 再次切换到新建目录那个工作区

```bash
$ cd ../git-demo-2/
$ git branch -av
* feature/add_git_commands                abcc0c6 用另一个用户身份添加文件并提交
  master                                  50893a9 Merge branch 'master' of gitee.com:ericqy/git-demo
  remotes/origin/HEAD                     -> origin/master
  remotes/origin/another-fix              1a04d7a 演示分离头指针
  remotes/origin/feature/add_git_commands abcc0c6 用另一个用户身份添加文件并提交
  remotes/origin/fix-html                 1a04d7a 演示分离头指针
  remotes/origin/master                   50893a9 Merge branch 'master' of gitee.com:ericqy/git-demo
```

_在gitee上可以看到用原用户添加并提交old-user.txt产生的commit 08b1694与本地remotes/origin/feature/add_git_commands分支的 abcc0c6不一致，原因是本地尚未从服务器上拉取最新的提交._

### 30.10 尝试直接修改new-user.txt，提交并尝试推送远端

```bash
$ echo 'something append to text.' >> new-user.txt
$ git commit -am'再次修改new-user.txt'
[feature/add_git_commands c229cf9] 再次修改new-user.txt
 1 file changed, 1 insertion(+)
$ git push
To gitee.com:ericqy/git-demo.git
 ! [rejected]        feature/add_git_commands -> feature/add_git_commands (fetch first)
error: 推送一些引用到 'git@gitee.com:ericqy/git-demo.git' 失败
提示：更新被拒绝，因为远程仓库包含您本地尚不存在的提交。这通常是因为另外
提示：一个仓库已向该引用进行了推送。再次推送前，您可能需要先整合远程变更
提示：（如 'git pull ...'）。
提示：详见 'git push --help' 中的 'Note about fast-forwards' 小节。
```

_由于远端已经有新的提交，本地分支最新commit与远端不一致，因此push出错

### 30.11 fetch后merge, 然后再提交

```bash
git fetch
git merge origin/feature/add_git_commands
git push
```

## 31 不同人修改了同文件的不同区域

演示在git-demo和git-demo-2这两个工作区, 修改了同文件git.html的不同区域，并提交

### 31.1 修改git.html，增加header和foot两个段落, 并将两个工作区的feature/add_git_commands分支同步到最新版本

```bash
cd ../git-demo
git checkout feature/add_git_commands
git pull
vi git.html
```

git.html内容如下

```html
<body>
    <p>This is header</p>
    <p class="title-line">This is a GIT demo app.</p>
    <img alt="logo" src="images/logo.png" />
    <p class="content-line">This is content</p>
    <p>This is foot</p>
</body>
```

```bash
git commit -am'修改了git.html, 准备演示多人同时修改一个文件'
git push
cd ../git-demo-2/
git pull
```

### 31.2 git-demo-2工作区修改header, 并提交推送gitee

```bash
cd ../git-demo-2
vi git.html
```

git.html的header段落改为如下

```html
    <p>This is header, modified</p>
```

```bash
git commit -am'modify header'
git push
```

### 31.3 git-demo工作区修改foot, 并提交推送gitee

```bash
cd ../git-demo
vi git.html
```

git.html的foot段落改为如下

```html
    <p>This is foot, modified</p>
```

```bash
$ git commit -am'modify foot'
$ git push
To gitee.com:ericqy/git-demo.git
 ! [rejected]        feature/add_git_commands -> feature/add_git_commands (non-fast-forward)
error: 推送一些引用到 'git@gitee.com:ericqy/git-demo.git' 失败
提示：更新被拒绝，因为您当前分支的最新提交落后于其对应的远程分支。
提示：再次推送前，先与远程变更合并（如 'git pull ...'）。详见
提示：'git push --help' 中的 'Note about fast-forwards' 小节。
```

_因为git.html在另外一个工作区已修改并提交, 远程仓库上对应分支的当前commit与本地不一致, 因此无法push._

### 31.4 merge后重新push, 成功完成合并

```bash
$ git fetch --all
正在获取 gitee
$ git branch -av
  another-fix                            1a04d7a 演示分离头指针
* feature/add_git_commands               5c0622c [领先 1，落后 1] modify foot
  fix-html                               1a04d7a 演示分离头指针
  master                                 2a00827 不同人修改了不同文件如何处理
  remotes/gitee/another-fix              1a04d7a 演示分离头指针
  remotes/gitee/feature/add_git_commands 48d0926 modify header
  remotes/gitee/fix-html                 1a04d7a 演示分离头指针
  remotes/gitee/master                   2a00827 不同人修改了不同文件如何处理
$ git merge gitee/feature/add_git_commands
自动合并 git.html
Merge made by the 'recursive' strategy.
 git.html | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)
$ git push
枚举对象中: 10, 完成.
对象计数中: 100% (10/10), 完成.
使用 4 个线程进行压缩
压缩对象中: 100% (6/6), 完成.
写入对象中: 100% (6/6), 609 字节 | 304.00 KiB/s, 完成.
总共 6 （差异 4），复用 0 （差异 0）
remote: Powered by GITEE.COM [GNK-6.3]
To gitee.com:ericqy/git-demo.git
   48d0926..204d415  feature/add_git_commands -> feature/add_git_commands
```

## 32 不同人修改了同文件的同一区域

过程类似前一章(这里修改了new-user.txt这个文件)，但这种情况下git merge会产生一个报错, 详细过程略

```bash
$ git merge gitee/feature/add_git_commands
自动合并 new-user.txt
冲突（内容）：合并冲突于 new-user.txt
自动合并失败，修正冲突然后提交修正的结果。
$ cat new-user.txt 
<<<<<<< HEAD
hello another user.
2 something append to text.
=======
2 hello another user.
something append to text.
>>>>>>> gitee/feature/add_git_commands
```

_需要merge后手工修改new-user.txt的内容，确保没有错误后再提交仓库._
