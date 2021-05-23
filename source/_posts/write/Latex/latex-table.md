---
title: Latex-table
author: ChangzeYan
date: 2021-01-06 11:37:01
tags: Latex-table
categories: Latex
cover:
---

## 三线表
\vspace{-0.5em}用于设置与表标题之间的距离
```
\begin{table}[!ht]
    \bicaption{八种DNA编码规则}{Eight kinds of DNA coding rules.}
    \label{tab:atcg}
    \vspace{-0.5em}\centering\wuhao
    \begin{tabular}{cccc}
        \toprule
        词 & 经济 & 政治 & 体育 \\
        \midrule
        “冠军” & -6.9077 & -6.9077 & 0.6931\\
        \bottomrule
    \end{tabular}
\end{table}
```

## 设置表格列宽
参考：[Latex设定表格列宽](https://blog.csdn.net/sptoor/article/details/21493777)

固定列宽可以使用 array 宏包的 p{2cm} 系列命令，如果需要指定水平对齐方式，可以使用下面的形式 >{\centering}p{2cm} 实现，但如果使用这种方式，缺省情况下不能使用 \\ 换行，需要使用\tabularnewline 代替。为了仍然使用 \\ 换行，需要在导言区加上下面的代码：
```
\usepackage{array}
\newcommand{\PreserveBackslash}[1]{\let\temp=\\#1\let\\=\temp}
\newcolumntype{C}[1]{>{\PreserveBackslash\centering}p{#1}}
\newcolumntype{R}[1]{>{\PreserveBackslash\raggedleft}p{#1}}
\newcolumntype{L}[1]{>{\PreserveBackslash\raggedright}p{#1}}
```

使用 C{3cm} 命令即可指定该列宽度为 3cm，并且文字居中对齐，左对齐和右对齐命令分别是 L{2cm} 和R{2cm}。
```
\begin{table}[htbp]
  \centering\caption{\label{tab:test}2000 和~2004 年中国制造业产品的出口份额}
  \begin{tabular}{L{2cm}C{2cm}R{2cm}}
    \toprule
    & 2000 & 2004 \\
    \midrule
    钢铁 & 3.1 & 5.2 \\
    化学制品 & 2.1 & 2.7 \\
    办公设备及电信设备 & 4.5 & 15.2 \\
    汽车产品 & 0.3 & 0.7 \\
    纺织品 & 10.4 & 17.2 \\
    服装 & 18.3 & 24\\
    \bottomrule
  \end{tabular}
\end{table}
```