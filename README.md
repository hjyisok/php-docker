# php-docker

<a name="Intro"></a>
## 介绍

php-docker 创建开发环境过程。

**使用概览：**

1. 克隆仓库：
    ```bash
    git https://github.com/hjyisok/php-docker.git
    ```

2. 进入目录
    ```bash
    cp env-example .env
    ```

3. 运行这些容器。
    ```bash
    docker-compose up -d nginx memcached redis
    ```

4. 打开浏览器，访问 localhost

**使用dock.sh简化操作：**
```bash
./dock

Available options:
   web   Starts docker-sync and runs docker compose for web services.
   up [services]         Starts docker-sync and runs docker compose.
   stop                  Stops containers and docker-sync.
   build [services]      Build docker compose.
   down                  Stop and remove containers, networks, images, and volumes and docker-sync.
   cli                   Opens bash on the php-cli with user.
   cli72                 Opens bash on the php-cli72 with user.
   install               Installs docker-sync and docker-compose gem on the host machine.
   sync                  Manually triggers the synchronization of files.
   clean                 Removes all files from docker-sync.

```

<a name="features"></a>
### 特点

- 在 PHP 版本：5.6, 7.2, 7.4 之中可以简单切换。
- 可运行自己的软件组合，比如：Memcached, Redis...
- 所有软件运行在不同的容器之中，比如：php-fpm, nginx, php-cli...
- 通过简单的编写 `Dockerfile` 容易定制任何容器。
- 所有镜像继承自一个官方基础镜像（Trusted base Images）
- 容易应用容器中的配置 配置文件（`Dockerfile`）
- 最新的 Docker Compose 版本（`docker-compose`）
- 所有的都是可视化和可编辑的
- 快速的镜像构建

<a name="Supported-Containers"></a>
### 支持的软件 (容器)

- nginx
- php-fpm
- php-cli
- redis
- memcached
- mysql
- mongo
- elasticsearch

<a name="what-is-docker"></a>
### Docker 是什么?

[Docker](https://www.docker.com) 是一个开源项目,自动化部署应用程序软件的容器,在 Linux, Mac OS and Windows 提供一个额外的抽象层和自动化的[操作系统级的虚拟化](https://en.wikipedia.org/wiki/Operating-system-level_virtualization)

<a name="Requirements"></a>
## 依赖

- [Git](https://git-scm.com/downloads)       
- [Docker](https://www.docker.com/products/docker/)

<a name="Documentation"></a>
## 文档

<a name="Docker"></a>
### [Docker]

<a name="List-current-running-Containers"></a>
### 列出正在运行的容器
```bash
docker ps
```

你也可以使用以下命令查看某项目的容器
```bash
docker-compose ps
```

<a name="Close-all-running-Containers"></a>
### 关闭所有容器
```bash
docker-compose stop
```

停止某个容器:

```bash
docker-compose stop {容器名称}
```

<a name="Delete-all-existing-Containers"></a>
### 删除所有容器
```bash
docker-compose down
```

小心这个命令,因为它也会删除你的数据容器。(如果你想保留你的数据你应该在上述命令后列出容器名称删除每个容器本身):*

<a name="Enter-Container"></a>
### 进入容器 (通过 SSH 进入一个运行中的容器)

1 - 首先使用 `docker ps` 命令查看正在运行的容器

2 - 进入某个容器使用:

```bash
docker-compose exec {container-name} bash
```

*例如: 进入 MySQL 容器*

```bash
docker-compose exec mysql bash
```

3 - 退出容器, 键入 `exit`.


<a name="Edit-a-Docker-Image"></a>
### 编辑 Docker 镜像

1 - 找到你想修改的镜像的 `Dockerfile` ,
<br>
例如： `mysql` 在 `mysql/Dockerfile`.

2 - 按你所要的编辑文件.

3 - 重新构建容器:

```bash
docker-compose build mysql
```

更多信息在容器重建中[点击这里](#Build-Re-build-Containers).

<a name="Build-Re-build-Containers"></a>
### 建立/重建容器

如果你做任何改变 `Dockerfile` 确保你运行这个命令,可以让所有修改更改生效:

```bash
docker-compose build
```

选择你可以指定哪个容器重建(而不是重建所有的容器):

```bash
docker-compose build {container-name}
```

如果你想重建整个容器，你可能需要使用 `--no-cache` 选项  (`docker-compose build --no-cache {container-name}`).

<a name="Add-Docker-Images"></a>
### 增加更多软件 (Docker 镜像)

为了增加镜像（软件）, 编辑 `docker-compose.yml` 添加容器细节， 你需要熟悉 [docker compose 文件语法](https://docs.docker.com/compose/compose-file/).

<a name="View-the-Log-files"></a>
### 查看日志文件
Nginx的日志在 `logs/nginx` 目录

然后查看其它容器日志(MySQL, PHP-FPM,...) 你可以运行:

```bash
docker logs {container-name}
```

<a name="PHP"></a>
### [PHP]

<a name="Install-PHP-Extensions"></a>
### 安装 PHP 拓展

安装 PHP 扩展之前,你必须决定你是否需要 `FPM` 或 `CLI`,因为他们安装在不同的容器上,如果你需要两者,则必须编辑两个容器。

PHP-FPM 拓展务必安装在 `php-fpm/Dockerfile`.

PHP-CLI 拓展应该安装到 `workspace/Dockerfile`.

<a name="Change-the-PHP-FPM-Version"></a>
### 修改 php-fpm 版本
默认运行 **php-fpm 7.2** 版本.

#### A) 切换版本 PHP `7.0` 到 PHP `7.4`

1 - 打开 `.env`。

2 - 修改版本号, 用 `PHP_VERSION=7.4` 替换 `PHP_VERSION=7.2`

3 - 最后重建php容器

```bash
docker-compose build php-fpm
```

> 更多关于PHP基础镜像, 请访问 [PHP Docker官方镜像](https://hub.docker.com/_/php/).


<a name="Change-the-PHP-CLI-Version"></a>
### 修改 php-cli 版本
默认运行 **PHP-CLI 7.2** 版本

<a name="Use-custom-Domain"></a>
### 使用自定义域名

假定你的自定义域名是 `xxx.test`

`nginx/vhost` 目录中增加对应域名的conf配置文件

<a name="Thanks"></a>
### [感谢]

@Laradock
