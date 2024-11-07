# 1、ctags

**制作tags**

`ctags *.c`

**查看tags:**

- 使用vim命令模式

`:ptags + func ` 

- 使用快捷键模式

`control+]`

**退出ctags**

- vim命令行模式

`pclose`

- 快捷键模式

`control + T`

# 2、Vim

## 1、文本编辑

普通模式 `v`后使用光标键选取，使用`y`copy

copy后使用`p`|`P`粘贴

`yy`复制当前行

`u`撤销上一次操作

## 2、跳转

`gg`跳转到首行

`G`跳转到尾行

`ngg`跳转到n行

`$`跳转到行尾

`0`跳转到行首

`^`跳转到第一个字符

`dd`删除所在行



# 3、Linux

## 1、修改环境变量

在～目录下打开 .zshrc，使用echo修改环境变量

结束后，在zsh下使用`source ~/.zshrc`使命令生效。

## 2、命令

### 1、进程管理

1. `ps`：显示当前正在运行的进程的快照。常见选项包括：
   - `ps aux`：显示所有用户的所有进程。
   - `ps -ef`：显示系统中所有进程的完整信息。

2. `top`：实时监视系统的进程活动和系统性能。它会动态地显示进程的CPU使用率、内存占用等信息。

3. `kill`：终止一个正在运行的进程。常见选项包括：
   - `kill PID`：通过进程ID（PID）终止一个特定的进程。
   - `killall process_name`：通过进程名称终止所有具有相同名称的进程。

4. `pgrep`：根据进程名称查找进程ID。
   - 例如，`pgrep sshd`会返回所有名为"sshd"的进程的PID。

5. `pkill`：根据进程名称终止进程。
   - 例如，`pkill firefox`会终止所有名为"firefox"的进程。

6. `nice`：设置进程的优先级。
   - 例如，`nice -n 10 command`会以较低的优先级运行"command"命令。

7. `renice`：修改正在运行的进程的优先级。
   - 例如，`renice -n 5 -p PID`会将进程ID为PID的进程优先级改为5。

### 2、权限管理

当涉及到Linux权限管理时，以下是一些常用的命令：

1. `chmod`：修改文件或目录的权限。
   - 例如，`chmod +x script.sh`会将名为"script.sh"的文件设置为可执行权限。

   chomd命令的详解：
   
   三组数字实际上是二进制数字转化为十进制
   
   第一组：文件所有者 User
   
   第二组：用户组 Group
   
   第三组：其他用户 Other
   
   三位数字，分别代表读、写和执行
   
   所以777为允许全部
   
2. `chown`：修改文件或目录的所有者。
   - 例如，`chown user:group file.txt`会将名为"file.txt"的文件的所有者更改为"user"，所属组更改为"group"。

3. `chgrp`：修改文件或目录的所属组。
   - 例如，`chgrp group file.txt`会将名为"file.txt"的文件的所属组更改为"group"。

4. `sudo`：以超级用户权限执行命令。
   - 例如，`sudo apt-get update`会以超级用户权限运行"apt-get update"命令。

5. `su`：切换用户。
   - 例如，`su username`会切换到"username"用户。

6. `passwd`：更改用户密码。
   - 例如，`passwd username`会更改"username"用户的密码。

7. `visudo`：编辑sudoers文件。
   - 例如，`sudo visudo`会打开sudoers文件，允许修改sudo的配置。

8. `adduser`：创建新用户。
   - 例如，`sudo adduser newuser`会创建一个名为"newuser"的新用户。

### 3、文件管理

当涉及到Linux文件管理时，以下是一些常用的命令：

1. `ls`：列出目录中的文件和子目录。
   - 例如，`ls -l`会以长格式显示文件和目录的详细信息。

2. `cd`：改变当前工作目录。
   - 例如，`cd /path/to/directory`会将当前工作目录更改为"/path/to/directory"。

3. `pwd`：显示当前工作目录的路径。

4. `mkdir`：创建新目录。
   - 例如，`mkdir new_directory`会在当前目录下创建一个名为"new_directory"的新目录。

5. `rm`：删除文件或目录。
   - 例如，`rm file.txt`会删除名为"file.txt"的文件。
   - 若要删除目录及其内容，可以使用`rm -r directory`命令。

6. `cp`：复制文件或目录。
   - 例如，`cp file.txt new_file.txt`会将名为"file.txt"的文件复制为"new_file.txt"。

7. `mv`：移动文件或目录，或重命名文件或目录。
   - 例如，`mv file.txt new_directory/`会将名为"file.txt"的文件移动到名为"new_directory"的目录中。
   - 若要重命名文件，可以使用`mv old_name.txt new_name.txt`命令。

8. `touch`：创建新文件或更新现有文件的访问和修改时间。
   - 例如，`touch file.txt`会创建一个名为"file.txt"的新文件。

9. `find`：在文件系统中查找文件和目录。
   - 例如，`find /path/to/search -name *.txt`会在指定路径下查找所有扩展名为".txt"的文件。

10. `grep`：在文件中搜索指定的模式或文本。
    - 例如，`grep "keyword" file.txt`会在名为"file.txt"的文件中搜索包含"keyword"的行。

### 4、**用户相关**

1. su 切换用户

2. sudo 以管理员身份执行

> sudo -i切换到root

3. who 查看当前用户名

4. ssh 远程连接

5. logout 注销

6. useradd 创建用户

7. userdel 删除用户

8. usermod 修改用户

9. groupadd 创建用户组

10. groupdel 删除用户组

11. groupmod 修改用户组

12. passwd 修改密码

13.  last 显示用户或终端的登录情况

## 3、结构

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

*在Makefile中，使用链接第三方库的方法调用其他库，MacOS系统下，usr文件暂时不能被修改*

### boot目录

在开机时，系统调用的内核文件，以及链接文件和镜像文件。

## 4、使用

### 1、基础环境

Mac虚拟机软件：VMware Fusion，使用Ubuntu22.04.3的live server版本

Description:    Ubuntu 22.04.3 LTS

SSH软件：Terminus

> M2芯片会要求使用arm的Linux

### 2、初始化

#### 关闭防火墙

```shell
# 关闭
sudo ufw disable
# 禁用服务
sudo systemctl stop ufw && sudo systemctl disable ufw
```

#### 配置时间同步

```shell
# 配置NTP同步地址为`ntp.aliyun.com`
sudo vim /etc/systemd/timesyncd.conf
# [Time]
# NTP=ntp.aliyun.com
sudo timedatectl set-timezone Asia/Shanghai
sudo timedatectl set-ntp off
sudo timedatectl set-ntp on
sudo systemctl daemon-reload
sudo systemctl restart systemd-timesyncd
```

#### 优化ssh连接，提高连接速度

```shell
sudo sed -i 's/^GSSAPIAuthentication yes$/GSSAPIAuthentication no/' /etc/ssh/sshd_config
sudo sed -i 's/#UseDNS yes/UseDNS no/' /etc/ssh/sshd_config
sudo systemctl  restart sshd.service
```

#### 更换apt软件源为国内源，提高包下载速度

一、更换清华源

` sudo vim /etc/apt/sources.list`

``` she
# 默认注释了源码镜像以提高 apt update 速度
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ focal main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ focal main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ focal-updates main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ focal-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ focal-backports main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ focal-backports main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ focal-security main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ focal-security main restricted universe multiverse
```



二、换源结束,进行更新

`sudo apt-get update `
`sudo apt-get upgrade`

# 4、git 

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

*覆盖提交*

`git push -f origin main`

#### SSH失效的方法

在很多时候，SSH登录也会有失效的时候

解决方法：

1、查看远程仓库

​     ```git remote -v ```

2、出现

``` shell
origin	git@github.com:YANG-DEMIN/learngit.git (fetch)
origin	git@github.com:YANG-DEMIN/learngit.git (push)
```



大概率是因为SSH

3、重新配置HTTP

```shell
$ git remote rm origin
$ git remote add origin add https://github.com/YANG-DEMIN/learngit
```

### 2、查看相关远程库

`git remote -v`

### 3、删除远程库

`git remote rm origin`

## 5、分支控制

### 1、分支的创建

`git branch name`

`git checkout name`

*创建分支并进入*

`git checkout -b name`

### 2、分支的合并

`git merge name`

### 3、分支的删除

`git branch -d name`

### 4、分支的查看

`git branch`

### 5、gitignore

git rm -r --cache .

git add -A

git push ……
