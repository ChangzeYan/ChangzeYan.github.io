---
title: Hithesis 工大论文模板
author: ChangzeYan
date: 2020-12-14 19:58:50
tags: Hithesis
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

## windows下使用docker
```
第一步，下载tinytex-hithesis镜像，

  docker pull dustincys/tinytex-hithesis:latest
第二步，在hithesis根目录下执行抽取格式

  docker run --rm -i  -v $(pwd):/home/runner dustincys/tinytex-hithesis:latest latex hithesis.ins

  Windows下改为：
  docker run --rm -i  -v D:\Document\Hit\HitThesis\Thesis\hithesis-newVersion-master\hithesis:/home/runner dustincys/tinytex-hithesis:latest latex hithesis.ins

  -v 冒号前面是本地项目路径，后面是容器内路径

第三步，在hithesis毕业论文文件夹hitbook或报告文件夹report下执行以下命令进行编译

  docker run --rm -i  -v $(pwd):/home/runner dustincys/tinytex-hithesis:latest make thesis

    Windows下改为：
  docker run --rm -i  -v D:\Document\Hit\HitThesis\Thesis\hithesis-newVersion-master\hithesis\examples\hitbook\chinese:/home/runner dustincys/tinytex-hithesis:latest make thesis



  docker run --rm -i  -v $(pwd):/home/runner dustincys/tinytex-hithesis:latest make report
或者编译文档

  docker run --rm -i  -v $(pwd):/home/runner dustincys/tinytex-hithesis:latest make doc
```

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



# 样式
以下命令打开官方文档：
```bash
texdoc hithesis
```
## 参考文献
将“文献[x]”中的\[x\]表示为正常的文本，而不是引用格式上标：
```bash
\inlinecite{key}
```
直接用vscode 编译时有时不会产生参考文献，用下面命令编译一遍即可：
```bash
xelatex -shell-escape thesis.tex
bibtex thesis
xelatex -shell-escape thesis.tex
xelatex -shell-escape thesis.tex
```


## 列表
### 更改标号样式
```bash
# 使用方括号阿拉伯数字标号
\begin{enumerate}[label={[\arabic*]}] 
    \item xxxxx   %[1] xxxxx
    \item yyyyyy  %[2] yyyyyy
    \item zzzzz   %[3] zzzzz
\end{enumerate}

# 使用圆括号阿拉伯数字标号
\begin{enumerate}[label={(\arabic*)}] 
    \item xxxxx   %(1) xxxxx
    \item yyyyyy  %(2) yyyyyy
    \item zzzzz   %(3) zzzzz
\end{enumerate}
```

在\begin{enumerate}[label={[\arabic*]}]后面跟\setlength{\itemsep}{0pt}可以设置当前列表环境里item条目之间的间距。

\arabic可以替换为\roman、\Roman、\Alph 或 \alph来表示小写罗马数字、大写罗马数字、大写字母编号 或 小写字母编号。


## 目录
### 设置三级目录
默认的目录只显示两级，在main.tex中设置显示三级目录，在\document{}后添加\setcounter{tocdepth}{3}：
```
\documentclass[newtxmath=true,newgeometry=two,capcenterlast=true,subcapcenterlast=true,openright=false,absupper=true,type=master,stage=zhongqi,campus=harbin]{hithesis}

% 设置三级目录
\setcounter{tocdepth}{3}
```

### 去掉目录中摘要和第一章之间的空行
参考：[issues-目录格式问题](https://github.com/dustincys/hithesis/issues/65)

在主文件中中设置：
```
\documentclass[fontset=fandol,type=master,campus=harbin,tocblank=false]{hithesisbook}

% tocblank=true|false
%   含义：目录中第一章之前，是否加一行空白。缺省值为true。
```

## 去掉图表标题中的冒号
当增加了一些自定义设置后，原来的图表标题可能会发生变化，可以按照下面修改：
```
\usepackage{caption}
% 设置表格
\captionsetup[table]{labelsep=space}
% 设置图片，skip表示图片和标题之间距离
\captionsetup[figure]{font=small,skip=0pt}
% 全部设置
\captionsetup{font={small},labelsep=space}
```

## 算法

### 修改输入输出样式
中文latex模式下，更改Input为输入，更改Output为输出的方法：**在算法内部插入**：
```
\SetKwInOut{KIN}{输入}
\SetKwInOut{KOUT}{输出}
```
例如：
```
\begin{algorithm}[!ht]
    \SetKwInOut{KIN}{输入}
    \SetKwInOut{KOUT}{输出}
    \caption{xxxx方法}
    \label{department_fun}
    
    \KIN{data,s,dic}
    \KOUT{vec}
    \lIf{s 为空串}{直接返回}
    len$\gets$ s.length()\;
    ss$\gets$s.toCharArray() \;
    dp$\gets$ new boolean[len][len] \;
    ans$\gets$ ""+ss[0] \;
    maxLen$\gets$1 \;
    \For{i from 0 To len}{
        dp[i][i]$\gets$true \;
    }
\end{algorithm}
```
### 基本语法
| 代码       | 含义           |
|-------------|-----------------------|
| \\;        | 在行末添加分号，并自动换行   |
| \\caption{}     | 插入标题                |
| \\KwData{输入信息}  | 效果：“Data:输入信息” |
| \\KwIn{输入信息}       | 效果：“Input:输入信息” |
| \\KwOut{输出信息}       | 效果：“Output:输出信息” |
| \\KwResult{输出信息}       | 效果：“Result:输出信息” |
| \\tcc{注释}  | 效果：/* 注释*/ |
| \\tcp{注释}  | 效果：// 注释 |
