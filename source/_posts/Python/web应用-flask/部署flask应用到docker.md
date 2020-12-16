---
title: 部署Flask应用到Docker
author: ChangzeYan
date: 2020-12-16 13:04:27
tags: Flask
categories: Python
cover:
---

# 导出与项目有关的依赖
## 安装pipreqs
```bash
pip install pipreqs
```
## 导出项目依赖包
进入到项目根目录，如果用conda装的python，需要切换到项目的python环境：
```bash
activate python36
```
然后执行：
```bash
pipreqs ./
```
成功后在项目根目录下生成requirements.txt文件。

如果报下面错误：

![安装pipreqs](https://github.com/ChangzeYan/ChangzeYan.github.io/raw/hexo/source/pic/flask部署到docker-安装pipreqs.png)
按照路径提示，修改pipreqs.py文件：
将第74行中的
```py
encoding=encoding
改为：
encoding='utf-8'
```
再次执行 pipreqs ./

新环境下安装
```bash
pip install -r requirements.txt
```

# 编写Dockerfile
项目目录结构：
```bash
D:.
│- Dockerfile
│- requirements.txt
│- nltk_data
└─NER
        key_words.txt
        main.py
        __init__.py
```
Dockerfile：
```bash
FROM python:3.6
# 暴露5001端口
EXPOSE 5001
# 将NER目录下（不包括NER目录）所有内容复制到 /usr/src/app目录下
ADD  ./NER /usr/src/app
# 将nltk所需的词典加入到容器的/usr/local/lib/nltk_data目录下
ADD ./nltk_data /usr/local/lib/nltk_data
# WORKDIR设置将要安装应用程序的默认目录,在Dockerfile中的任何剩余命令执行以及运行容器时，其当前目录都会为这个默认目录
WORKDIR /usr/src/app
# COPY将文件从你的机器复制到容器文件系统，后面的点就代表上面设置的目录
COPY requirements.txt .
# 安装依赖
RUN  pip install -r ./requirements.txt -i http://mirrors.aliyun.com/pypi/simple/ --trusted-host mirrors.aliyun.com
# 启动应用
CMD ["python","/usr/src/app/main.py"]
```
## copy和add的区别
COPY指令和ADD指令都可以将主机上的资源复制或加入到容器镜像中，COPY指令和ADD指令的唯一区别在于是否支持从远程URL获取资源。COPY指令只能从执行docker build所在的主机上读取资源并复制到镜像中。而ADD指令还支持通过URL从远程服务器读取资源并复制到镜像中。

# 构建镜像
将项目上传到docker服务器，进入工程目录，执行：
```bash
# 最后还有一个点
docker build -t org_struct_pre:latest .
# 如果远程开启了远程访问，在本地可以部署，不用将项目上传至服务器，执行：
docker -H IP:2375 build -t org_struct_pre:latest .
```

运行容器：
```bash
docker run -d -p 5001:5001 --name org_struct_pre（容器名） org_struct_pre(镜像名)
```
进入容器：
```bash
docker exec -it 7a31796e9cb2 /bin/bash
```
进入到/usr/local/lib目录，可以看到所有词典。可以正常使用nltk：

![容器中使用nltk](https://github.com/ChangzeYan/ChangzeYan.github.io/raw/hexo/source/pic/部署flask-docker-nltk.png)
