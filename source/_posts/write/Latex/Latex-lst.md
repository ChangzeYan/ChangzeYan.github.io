---
title: 代码和算法
author: ChangzeYan
date: 2021-03-11 21:21:26
tags: Lst
categories: Latex
cover:
---

# 定制lstlisting 的标题

参考：[按照算法标题样式定制listings的标题](https://www.latexstudio.net/archives/625.html)

```
\renewcommand*\thelstnumber{\arabic{lstnumber}:}

\DeclareCaptionFormat{mylst}{\hrule#1#2#3}
\captionsetup[lstlisting]{format=mylst,labelfont=bf,singlelinecheck=off,labelsep=space}


\documentclass{article}
\usepackage{listings}
\usepackage{caption}

\lstset{
language=C++,
basicstyle=\small\ttfamily,
numbers=left,
numbersep=5pt,
xleftmargin=20pt,
showstringspaces=false, %去掉空格时产生的下划的空格标志, 设置为true则出现
frame=tb,
framexleftmargin=20pt
}

\renewcommand*\thelstnumber{\arabic{lstnumber}:}

\DeclareCaptionFormat{mylst}{\hrule#1#2#3}
\captionsetup[lstlisting]{format=mylst,labelfont=bf,singlelinecheck=off,labelsep=space}

\begin{document}

\begin{lstlisting}[caption={test algorithm},label={lst1}]
#include
using namespace std;

int main()
{
cout << "Welcome to the wonderful world of C++!!!\n";
return 0;
}
\end{lstlisting}

\end{document}
```

![hithesis生成的样式-本部硕士中期](https://github.com/ChangzeYan/ChangzeYan.github.io/raw/hexo/source/pic/latex-lst.png)