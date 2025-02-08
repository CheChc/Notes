## 在Mac上部署DeepSeek大模型

### 一、下载Ollma（大模型框架）

> https://ollama.com/download

### 二、ollma链接下载

16G内存的mac，可以运行7b的大模型

### 三、运行（命令行）

`ollama run deepseek-r1:7b`

#### 四、使用代理服务器//to do list

#### 1、下载nginx

先下载brew，然后使用brew下载nginx

brew下载链接（命令行运行）

`/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`

brew下载后，使用命令

`brew install nginx`

`brew info nginx`

修改conf文件

```
server {
    listen 80;
    server_name deepseek.com;  # Replace with your domain or IP
    location / {
        proxy_pass http://localhost:11434;
        proxy_set_header Host localhost:11434;
    }
}
```



nginx启动方法：nginx

nginx关闭方法：nginx -s stop

### 四、tips

自定义zsh命令，便于启动

编辑~/.zshrc文件，增加函数功能：

```txt
function deepseek()
{
		ollama run deepseek-r1:7b
}
```

之后执行命令`source ~/.zshrc`，即可快速打开deepseek～。