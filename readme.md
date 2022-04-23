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
C:\Workspace\Development\git\git-demo>git log
commit 21430cbf157e509ca49d1d649958f93326e7f47d (HEAD -> master)
Author: eric <6350938@qq.com>
Date:   Sat Apr 23 09:51:02 2022 +0800

    commit、tree和blob三个对象之间的关系

commit 20ded561f4e4ff34591254e1c6ce89cde747cc9f
Author: eric <6350938@qq.com>
Date:   Thu Apr 21 19:33:12 2022 +0800

    探密.git目录

commit 8ac3bb78a4bd4ba0715121dd1af6d027e9216b8a
Author: eric <6350938@qq.com>
Date:   Thu Apr 21 18:35:02 2022 +0800
```

- 切换到其中某一个commit（20ded561f）

```bash
git checkout 20ded561f
```

- 修改文件并提交

```bash
git commit -am"演示分离头指针"
```
