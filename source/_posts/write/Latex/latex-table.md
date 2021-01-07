---
title: latex-table
author: ChangzeYan
date: 2021-01-06 11:37:01
tags: Latex-table
categories: Latex
cover:
---

## 三线表

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