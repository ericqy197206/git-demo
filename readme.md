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

## 4 通过git log查看版本演变历史

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
