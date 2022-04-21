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