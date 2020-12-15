---
title: nlp依赖环境安装
author: ChangzeYan
date: 2020-12-15 19:17:58
tags: NLP
categories: Python
cover:
---

# Nltk
安装nltk：
```py
pip install nltk
```

然后使用的时候报错：

Resource punkt not found. Please use the NLTK Downloader to obtain the resource:
\>>> import nltk >>> nltk.download('punkt')
使用提示代码下载词典：
```py
nltk.download('punkt')
```
发现下载不下来，报错：getaddrinfo failed。
## 方法一
参考：[nltk_data LookupError](https://blog.csdn.net/weixin_39712314/article/details/106173356)
到：[nltk_data](http://www.nltk.org/nltk_data/)中下载punkt包，然后解压到D:\\nltk_data\\tokenizers目录下即可。

## 方法二
参考：[离线安装nltk_data](https://www.cnblogs.com/eksnew/p/12909814.html)

打开[Github-nltk_data](https://github.com/nltk/nltk_data)，将第二个文件夹“packages”下载下来，下载Github文件夹可以用chrome插件：GitZip for github. 右键文件夹右边空白处就可以下载了
![packages文件夹内容](https://github.com/ChangzeYan/ChangzeYan.github.io/raw/hexo/source/pic/nlp依赖环境安装-nltk.png)

然后，将packages中的所有内容拷贝到以下目录中任意一个：
```bash
- 'C:\\Users\\cunzhang/nltk_data'
    - 'D:\\Anaconda\\nltk_data'
    - 'D:\\Anaconda\\share\\nltk_data'
    - 'D:\\Anaconda\\lib\\nltk_data'
    - 'C:\\Users\\cunzhang\\AppData\\Roaming\\nltk_data'
    - 'C:\\nltk_data'
    - 'D:\\nltk_data'
    - 'E:\\nltk_data'
```

然后进入到"D:\\nltk_data\\tokenizers"目录，将punkt.zip解压即可。

![packages文件夹内容](https://github.com/ChangzeYan/ChangzeYan.github.io/raw/hexo/source/pic/nlp依赖环境安装-nltk-punkt.png)
