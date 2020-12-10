---
title: idea部署springboot项目到docker
author: ChangzeYan
date: 2020-12-10 18:22:44
tags: 部署
categories: Docker
cover:
---

# idea中安装docker插件
在setting-plugin中搜索并安装docker：
![安装docker插件](https://github.com/ChangzeYan/ChangzeYan.github.io/raw/hexo/source/pic/idea-docker插件.png)


# 连接远程docker服务器
在build，docker中添加一个连接，并填写好服务器ip和端口：
![连接远程docker](https://github.com/ChangzeYan/ChangzeYan.github.io/raw/hexo/source/pic/idea-连接远程docker.png)

# 将springboot项目打包
点击上面的skip test按钮，可以跳过测试：
![将项目打成jar包](https://github.com/ChangzeYan/ChangzeYan.github.io/raw/hexo/source/pic/idea-打包.png)

# 编写DockerFile
DockerFile位置与target文件夹同级目录，DockerFile中的路径是相对路径：

 指定了临时文件目录为/tmp，其效果是在主机 /var/lib/docker 目录下创建了一个临时文件，并链接到容器的/tmp，ENTRYPOINT 执行项目 cloud-activiti.jar。为了缩短 Tomcat 启动时间，添加一个系统属性指向 “/dev/./urandom” 作为 Entropy Source
```bash
FROM java:8
#暴露容器的9023端口
EXPOSE 9023
#将复制指定的cloud-activiti-0.0.1-SNAPSHOT.jar为容器中的cloud-activiti.jar，相当于拷贝到容器中取了个别名
ADD target/cloud-activiti-0.0.1-SNAPSHOT.jar /cloud-activiti.jar

VOLUME /tmp
RUN bash -c 'touch /cloud-activiti.jar'
ENTRYPOINT ["java","-Djava.security.egd=file:/dev/./urandom","-jar","/cloud-activiti.jar"]

```
# 部署和运行
点击Edit Configuration：
![编辑配置](https://github.com/ChangzeYan/ChangzeYan.github.io/raw/hexo/source/pic/部署和执行docker.png)
点击左上角加号，“+”：

选择DockerFile：
![配置configuration](https://github.com/ChangzeYan/ChangzeYan.github.io/raw/hexo/source/pic/docker-files-configuration.png)
选择要部署的目标docker服务器，dockerfile文件，端口号，tag等：
![部署和运行](https://github.com/ChangzeYan/ChangzeYan.github.io/raw/hexo/source/pic/run-configuration.png)
然后点击run，即可部署到docker服务器中。
