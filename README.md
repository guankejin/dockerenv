## 配置目录

*  目录
*  安装Docker Engine
*  安装Docker Compose
*  编译并运行


## 目录说明

- **`data`** 默认用于存储容器内数据

- **`mysql`** MySQL配置文件

- **`redis`** Redis配置文件

- **`mongo`** Mongo配置文件

- **`docker-compose.yml`** 容器编排配置文件

- **`.env.example`** 配置文件模版

## 安装Docker Engine（以Ubuntu为例）

> 官方文档请点击以下链接：<br>
> [Mac](https://docs.docker.com/docker-for-mac/install/)<br>
> [Windows](https://docs.docker.com/docker-for-windows/install/)<br>
> [CentOS](https://docs.docker.com/engine/install/centos/)<br>
> [Debian](https://docs.docker.com/engine/install/debian/)<br>
> [Fedora](https://docs.docker.com/engine/install/fedora/)<br>
> [Ubuntu](https://docs.docker.com/engine/install/ubuntu/)<br>
> [Binaries](https://docs.docker.com/engine/install/binaries/)<br>

更新`apt`软件包索引
```
sudo apt-get update
```

安装软件包以允许`apt`通过HTTPS使用repository源
```
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
```

添加Docker的官方GPG密钥
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

使用以下命令来设置稳定的repository源
```
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```

更新`apt`程序包索引，并安装最新稳定版本的`Docker Engine`和`Container`
```
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

测试查看当前`docker`版本
```
sudo docker -v

// Docker version 20.10.0, build 7287ab3
```

## 安装Docker Compose（[官方文档](https://docs.docker.com/compose/install/)）

下载并安装最新稳定版`Docker Compose`(目前为1.27.4)
```
sudo curl -L "https://github.com/docker/compose/releases/download/1.27.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

给予`docker-compose`可执行权限
```
sudo chmod +x /usr/local/bin/docker-compose
```

测试查看当前`docker-compose`版本
```
sudo docker-compose -v

// docker-compose version 1.27.4, build 40524192
```

## .env配置

进入到本项目的根目录, 拷贝一份`.env.example`并重命名为`.env`

```
cp .env.example .env
```

## 编译并运行

编译

```
sudo docker-compose build
```

> 修改`Dockerfile`或`.env`文件后需要重新编译

启动(后台运行)
```
sudo docker-compose up -d
```

## PHP配置

在`php/ini`目录下选择需要的开发或生产环境`php.ini`配置，拷贝并重命名为`php.ini`，以生产为例：

```
cp php/ini/php.ini-production php/ini/php.ini
```

## MySQL配置

授权root用户允许远程访问

查询所有容器
```
GRANT ALL PRIVILEGES ON *.* TO root@"%" IDENTIFIED BY "此处填写远程访问密码";
flush privileges;
```