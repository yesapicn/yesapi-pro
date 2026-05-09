# YesApi Pro Java版安装教程

## 运行环境

YesApi Pro Java版的运行环境要求如下： 

 + 操作系统：Windows/Linux/Mac/Ubuntu/CentOS/docker等；
 + 开发语言：Java JDK 17，后端框架：Spring Boot 3；
 + 数据库：MySQL；配置 Nacos；缓存中间件Redis；消息队列 RabbitMQ；
 + Web服务器：Nginx/Apache/IIS；
 + 正式服务器最低配置：CPU 4核 / 内存 16G / 硬盘空间40G / 带宽10M，推荐使用 CentOS 7；

> 官方推荐使用：CentOS 7 + Java + MySQL + Nginx  

## 安装教程
### 环境安装
  * 服务器安装docker
  ~~~
  yum install -y docker-ce
  ~~~
  * 安装nacos，（推荐使用docker镜像安装）
  ~~~
  # 拉取nacos镜像
  docker pull nacos/nacos-server
  # 运行nacos镜像
  docker run --name nacos -e MODE=standalone -p 8848:8848 -d nacos/nacos-server
  ~~~

### 源码打包docker镜像
  * 确保本地java环境正确安装，JDK17，并配置好环境变量
  * 项目使用maven管理，需要先安装maven，然后按照以下步骤打包：
  #### 源码打包
  * 进入common目录将公共配置模块打包至本地
    ~~~
    mvn clean install
    ~~~
  * 应用模块打包（以admin管理后台为例）
  * 进入admin目录（打管理后台jar包）
    ~~~
    mvn clean package
    ~~~
  #### 制作docker镜像
  * 在模块根目录下编写DockerFile
    ~~~
    # 使用远程jdk
    FROM openjdk:17
    # 维护者信息
    MAINTAINER Mai
    #将jar包添加到容器中
    ADD ./target/admin.jar /app/admin.jar
    #对外暴露端口
    EXPOSE 7113
    # 声明构建参数
    ARG MY_ENV
    # 设置环境变量
    ENV SPRING_PROFILES_ACTIVE ${MY_ENV:-dev}
    # 运行jar包
    ENTRYPOINT ["sh","-c","java -jar /app/admin.jar --spring.profiles.active=${SPRING_PROFILES_ACTIVE}"]
    ~~~
 * 构建镜像
  ~~~
   docker build --build-arg MY_ENV=test -t [镜像名] .
  ~~~
 * 登录远程仓库
  ~~~
  docker login --usernam=[用户名] [远程地址]
  ~~~
 * 镜像打标签
  ~~~
  # 镜像ID即为上面构建的镜像，可通过docker images命令查看镜像ID
  docker tag [镜像ID] [远程地址]/[仓库名]/[镜像名]:[标签]
  ~~~
 * 推送至远程仓库
  ~~~
  docker push [远程地址]/[仓库名]/[镜像名]:[标签]
  ~~~

### 拉取远程镜像
    # 管理后台
    docker pull [远程地址]/[仓库名]/[镜像名]:[标签]

### 运行镜像
    # 端口号需跟nacos配置一致
    docker run -it -d --name [镜像名] -p 7113:7113 [镜像ID]
    docker run -it -d --name [镜像名] -p 7114:7114 [镜像ID]
    docker run -it -d --name [镜像名] -p 7115:7115 [镜像ID]
    docker run -it -d --name [镜像名] -p 7112:7112 [镜像ID]
  
