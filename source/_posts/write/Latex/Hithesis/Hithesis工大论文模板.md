---
title: Hithesis 工大论文模板
author: ChangzeYan
date: 2020-12-14 19:58:50
tags:
categories: Latex
cover:
---

# 中期报告

## hithesis
模板下载地址：[hithesis](https://github.com/dustincys/hithesis)

将zip包下载解压，根目录为：

![解压后的目录](https://github.com/ChangzeYan/ChangzeYan.github.io/raw/hexo/source/pic/hithesis-zip.png)

### 生成样式文件
然后在根目录下执行（在github项目介绍中）：
```bash
# windows:
lualatex hithesis.ins

# mac/linux
latex hithesis.ins

# make
make cls
```


![生成的样式文件](https://github.com/ChangzeYan/ChangzeYan.github.io/raw/hexo/source/pic/hithesis-生成样式文件.png)

### 编译
生成好格式后，进入到示例文件夹中
```bash
examples
├── hitart
│   ├── reportplus  %深圳校区博士中期报告
│   └── reports     %除去深圳校区博士中期报告的一校三区本硕博开题、中期报告
└── hitbook
    ├── chinese     %一校三区本硕博毕业论文以及博后出站报告
    └── english     %一校三区本硕博英文版毕业论文
```

在hitart/reports目录下执行：
```bash
xelatex -shell-escape report.tex
bibtex report
xelatex -shell-escape report.tex
xelatex -shell-escape report.tex
```
或者用vscode打开该目录，直接执行。

最后生成的开题中期报告格式不太正确：

![hithesis生成的样式-本部硕士中期](https://github.com/ChangzeYan/ChangzeYan.github.io/raw/hexo/source/pic/hithesis-中期样式.png)
因此换用hithesis-alpha。

## hithesis-alpha
下载地址：[hithesis-alpha](https://github.com/Regulust/hithesis-alpha#%E4%BD%BF%E7%94%A8%E8%AF%B4%E6%98%8E)

### 生成样式文件
同hithesis一样，在根目录下运行：
```bash
# windows:
lualatex hithesis.ins

# mac/linux
latex hithesis.ins
```

### 编译
用vscode直接打开根目录，编辑main.tex，修改documentclass的参数：
```bash
\documentclass[newtxmath=true,newgeometry=two,capcenterlast=true,
subcapcenterlast=true,openright=false,absupper=true,
type=master,stage=zhongqi,campus=harbin]{hithesis}
```
生成的样式如下：

![hithesis-alpha生成的样式-本部硕士中期](https://github.com/ChangzeYan/ChangzeYan.github.io/raw/hexo/source/pic/hithesis-alpha-样式.png)

# 学位论文
使用 hithesis的example/hitbook即可。
在thesis.tex中修改：
```bash
\documentclass[fontset=fandol,type=master,campus=harbin]{hithesisbook}
```
