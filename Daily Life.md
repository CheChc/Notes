# 1、ctags

## 1、制作tags

`ctags *.c`

## 2、查看tags

### 1、使用vim命令模式

`:ptags + func ` 

### 2、使用快捷键模式

`control+]`

## 3、退出ctags

### 1、vim命令行模式

`pclose`

### 2、快捷键模式

`control + T`

# 2、Vim

## 1、copy&paste

普通模式 `v`后使用光标键选取，使用`y`copy

copy后使用`p`|`P`粘贴

## 2、复制与撤销

`yy`复制当前行

`u`撤销上一次操作

## 3、跳转

`gg`跳转到首行

`G`跳转到尾行

`gng`跳转到n行

`$`跳转到行尾

`0`跳转到行首

# 3、JAVA

## JAVA HOME

可以看作是Java的一个安装目录，告诉系统和应用程序Java安装的位置。

# 4、Linux

## 1、zsh的使用

一个shell的运行窗口

### 修改环境变量

在～目录下打开 .zshrc，使用echo修改环境变量

结束后，在zsh下使用`source ~/.zshrc`使命令生效。

### 显示文件内容

`cat`

## 2、Linux的文件管理

### opt目录

可以视为optional，也就是可选的软件安装，一般是放置第三方软件的位置。

### bin目录

是binary的缩写，一般是用来存储可执行文件的文件夹

在Linux下的bin目录，一般会存储系统命令，脚本命令等。

在其他应用程序下，可能也会有bin目录，都是用来存储可执行文件的。

### etc目录

et cetera，是用来存放系统配置的文件夹

etc文件夹包含了各种应用程序、服务和用户配置信息等等。

### lib目录

用来存放一些预编译的二进制文件，比如存放系统共享库，应用程序共享库等等

也可以使用第三方的lib库进行链接

*在Makefile中，使用链接第三方库的方法调用其他库，Macos系统下，usr文件暂时不能被修改*

### boot目录

在开机时，系统调用的内核文件，以及链接文件和镜像文件。

# 5、git 的使用

## 1、git仓库的创建

`git init`

## 2、git的提交

### 1、将改动上传到版本库

`git add`

### 2、将改动版本提交

`git commit -m"some word to complain"`

## 3、版本控制

### 1、查看git状态

`git status`

### 2、查看代码修改之处

`git diff`

### 3、查看版本历史记录

`git log`

只显示一行：`git log --pretty=oneline`

*git log语句只能显示最近三次的提交记录*

### 4、回退版本

`git reset --hard HEAD^`

回退到某一个版本：`git reset --hard *****`

**在git中，HEAD表示当前版本，HEAD^表示上一个版本，以此类推**

### 5、查找之前的命令

`git reflog`

### 6、撤销修改

#### 1、撤销工作区的修改

`git checkout -- <file>`

可以撤销未add的修改

#### 2、撤销暂存区的修改

如果只是add，并未commit，请先使用

`git reset HEAD <file>`

再使用

`git checkout -- file`

*git checkout实际上就是用版本库的版本代替自己的版本*

### 7、删除文件

`git rm +<file>`

`git commit`

## 4、远程仓库

**使用SSH登录github**

### 1、远程库的配置

1、先输入`git remote rm origin `删除关联的origin的远程库
2、关联自己的仓库 `git remote add origin http…`
3、最后`git push origin main`，这样就推送到自己的仓库了

### 2、查看相关远程库

`git remote -v`

### 3、删除远程库

`git remotrm origin`

## 1、数据库的创建和显示

`create database [name]`

`show databases`

# 7、一个全栈项目

一个全栈项目通常由以下组成部分构成：

## 1、前端（Front-end）

前端是用户与应用程序交互的界面，负责展示数据、处理用户输入和与后端进行通信。前端通常使用HTML、CSS和JavaScript等技术来构建用户界面。常见的前端框架和库包括React、Angular、Vue.js等。

## 2、后端（Back-end

后端是处理应用程序的逻辑、数据存储和与前端进行通信的部分。后端通常使用服务器端编程语言（如Java、Python、Ruby等）和框架（如Spring、Django、Ruby on Rails等）来开发。后端负责处理业务逻辑、数据库访问、用户认证和授权等任务。

## 3、数据库（Database）

数据库用于存储应用程序的数据。常见的数据库类型包括关系型数据库（如MySQL、PostgreSQL）和非关系型数据库（如MongoDB、Redis）。后端应用程序通过数据库与数据进行交互，进行数据的读取、写入和查询等操作。

## 4、API（Application Programming Interface）

API是前端和后端之间进行通信的接口。后端通过API暴露出一组规定的接口，前端通过调用这些接口来获取数据和执行操作。API可以使用RESTful风格、GraphQL等方式进行设计和实现。

## 5、部署和运维（Deployment and DevOps）

部署和运维是将应用程序部署到服务器上并进行运行和维护的过程。这包括服务器配置、应用程序的打包和部署、监控和日志管理等任务。DevOps工具和实践可以帮助自动化这些过程，如Docker、Kubernetes、CI/CD（持续集成/持续交付）等。

## 6、其他

除了上述部分，全栈项目还可能涉及其他组件和技术，如身份验证和授权、缓存、消息队列、搜索引擎、日志记录等，具体取决于应用程序的需求和功能。

总而言之，一个全栈项目由前端、后端、数据库、API和部署运维等组件构成，它们协同工作以实现一个完整的应用程序。每个组件都有自己的任务和技术栈，通过合理地组织和协调，实现前后端的无缝集成和良好的用户体验。
