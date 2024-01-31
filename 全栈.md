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



# 3、JAVA

## JAVA HOME

可以看作是Java的一个安装目录，告诉系统和应用程序Java安装的位置。

## 链表

> java中，没有对于指针的定义，只需要使用正常定义

### 结构

``` java
public class LinkNode
{
  int val;
  LinkNode next;
}
```

> 在其中存在两个变量，存储值val和指向下一个节点的指针next

### 读和写

对于链表的操作需要使用两个指针，head和current，head指头节点，current指当前正在操作的节点

在输入时，首先考虑头节点是否为空，若为空则首先为头节点赋值，否则操作current节点进行赋值

两个操作都要在结束后更新current节点的位置。

**head：**

```java
head = node;
current = node;
```

**current**

```java
current.next = node;
current = node;
```



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

# 6、数据库SQL

## 1、数据库的创建和显示

`create database [name]`

`show databases`

## 2、切换数据库

`use name`



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

# 8、Django框架

## 1、Django框架的组成

在Django框架中

- `README.md`：项目的说明文档，描述项目的目的、使用方法等。
- `manage.py`：Django项目的管理脚本，用于运行开发服务器、执行数据库迁移等操作。
- `apps`：包含Django应用程序的目录，每个应用程序通常代表项目中的一个模块或功能。
- `static`：用于存储静态文件（如CSS、JavaScript和图像文件）的目录。
- `templates`：用于存储HTML模板文件的目录。
- `media`：用于存储用户上传的媒体文件（如用户头像、图书封面等）的目录。
- `db_book.sql`：可能是一个数据库备份文件，用于还原数据库或导入初始数据。

**conda的问题 **

在mac中，似乎需要使用`source activate xx`才能进入环境

## 2、Django的运行

### 1、链接数据库

*使用mysql与Django链接*

#### 1、创建数据库

见6-1

#### 2、修改设置

修改Django的settings.py文件

```sql
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql'，  # 默认
        'NAME': 'test'，  # 连接的数据库
        'HOST': '127.0.0.1'，  # mysql的ip地址
        'PORT': 3306，  # mysql的端口
        'USER': 'root'，  # mysql的用户名
        'PASSWORD': 'rootroot'  # mysql的密码
    }
}
```

修改_init.py文件

添加语句：

```sql 
import pymysql

pymysql.install_as_MySQLdb()//更换了数据库引擎
```

#### 3、迁移数据库

- 创建APP

`python manage.py startapp web`

- 在setings中添加APP

``` sql
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'book',
    'web.apps.WebConfig',
]
```

- 迁移数据库

``` sql
python manage.py makemigrations
python manage.py migrate//同步了数据库
```

### 2、编写Django

#### **1.编写url**

```sql
urlpatterns = [
    path('admin/'， admin.site.urls)，
    path('student_list'， views.student_list)，
]
```

#### **2.编写视图(views)**

```sql
def student_list(request):
    student_queryset = models.Student.objects.all()
    return render(request，"student.html"，{"student_queryset":student_queryset})
```

#### **3.编写html(templates)**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<table border="1">
    <thead>
    <tr>
        <td>id</td>
        <td>姓名</td>
        <td>年龄</td>
        <td>性别</td>
        <td>年纪</td>
    </tr>
    </thead>
    <tbody>
    {% for student in student_queryset %}
        <tr>
            <td>{{ student.id }}</td>
            <td>{{ student.name }}</td>
            <td>{{ student.age }}</td>
            <td>{{ student.gender }}</td>
            <td>{{ student.grade }}</td>
        </tr>
    {% endfor %}

    </tbody>
</table>
</body>
</html>
```

### 3、运行Django

`python manage.py runserver 127.0.0.1:8000`

## 2、Django环境的配置

Django的运行建议使用venv环境而不是conda环境

### 1、创建虚拟环境

`python -m venv name`

### 2、进入虚拟环境

`source env/bin/activate`

**在虚拟环境中进行相关的配置下载，使用pip等包管理软件**

### 3、环境要求

以一个链接mysql的图书馆管理系统为例：

```txt
asgiref==3.2.10
crontab==0.22.9
Django==2.1.15
django-crontab==0.7.1
django-mssql==1.8
django-pyodbc==1.1.3
django-pyodbc-azure==2.1.0.0
django-pytds==1.5
django-sqlserver==1.11
mysqlclient==2.0.1
PyMySQL==0.10.0
pyodbc==4.0.30
python-tds==1.10.0
pytz==2020.1
six==1.15.0
sqlparse==0.3.1
```

# 9 、Makefile的使用

Makefile是用于在unix/Linux编译下的一种脚本式的方法

## 1、具体语法

想生成的文件：源文件

​	生成方法

## 2、实际应用

``` makefile
CC = g++
CFLAGS = -O2 -D_REENTRANT -w
out: main.o loadDatafromFile.o sumData.o displayData.o calculateMean.o calculateVariance.o leastSquareFit.o loginUser.o registerUser.o isUsernameExists.o
	$(CC) $(CFLAGS) -o out main.o loadDatafromFile.o sumData.o displayData.o calculateMean.o calculateVariance.o leastSquareFit.o loginUser.o registerUser.o isUsernameExists.o

main.o: main.cpp
	$(CC) $(CFLAGS) -c main.cpp

loadDatafromFile.o: loadDatafromFile.cpp
	$(CC) $(CFLAGS) -c loadDatafromFile.cpp

sumData.o: sumData.cpp
	$(CC) $(CFLAGS) -c sumData.cpp

displayData.o: displayData.cpp
	$(CC) $(CFLAGS) -c displayData.cpp

calculateMean.o: calculateMean.cpp
	$(CC) $(CFLAGS) -c calculateMean.cpp

calculateVariance.o: calculateVariance.cpp
	$(CC) $(CFLAGS) -c calculateVariance.cpp

leastSquareFit.o: leastSquareFit.cpp
	$(CC) $(CFLAGS) -c leastSquareFit.cpp

loginUser.o: loginUser.cpp
	$(CC) $(CFLAGS) -c loginUser.cpp

registerUser.o: registerUser.cpp
	$(CC) $(CFLAGS) -c registerUser.cpp

isUsernameExists.o: isUsernameExists.cpp
	$(CC) $(CFLAGS) -c isUsernameExists.cpp

clean:
	rm -f *.o out
```

 生成out可执行文件，由生成的.o文件链接而成

生成的.o文件，是由cpp文件生成的

Makefile的核心原理，是自顶向下的，也就是说，第一个想生成的文件就是最终文件了

