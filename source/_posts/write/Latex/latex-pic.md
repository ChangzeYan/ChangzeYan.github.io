---
title: Latex-pic
author: ChangzeYan
date: 2021-01-06 10:06:37
tags: Latex-pic
categories: Latex
cover:
---
## 设置图片存放路径
设置存放图片的根路径
导包：
```
\usepackage{graphicx}
```
使用：
在根目录下建立figures目录，将图片存放在该目录中，以后直接引用图片名称即可，不用扩展名：
```
\graphicspath{{figures/}} 
```
## 插入图片
isNumberDFA是图片名，不用加扩展名：
```
\begin{figure}[htb]
    \centering
    \includegraphics[width=1\textwidth]{isNumberDFA}
    \caption{DFA状态转移图}
    \label{isNumberDFA}
\end{figure}
```

## 两张图片横着放
```
\begin{figure}[htb]
    \centering
    \subfigure[冠状树]{
        \begin{minipage}[b]{0.4\textwidth}
            \includegraphics[width=1\textwidth]{isNumberDFA}
        \end{minipage}
    }
    \subfigure[龙曲线]{
        \begin{minipage}[b]{0.4\textwidth}
            \includegraphics[width=1\textwidth]{status-table-DFA}
        \end{minipage}
    }
    \caption{DFA状态转移图}
    \label{isNumberDFA}
    
\end{figure}
```

## 四张图片两两并列排放
空行表示换行。
```
\begin{figure}[H]
    \centering
    \begin{minipage}{7cm}
    	\includegraphics[width=1\textwidth]{./pic/info1}
    	\label{1}
    \end{minipage}
    \begin{minipage}{7cm}
    	\includegraphics[width=1\textwidth]{./pic/info2}
    	\label{2}
    \end{minipage}

    % 空一行会分两行排版
    \begin{minipage}{7cm}
    	\includegraphics[width=\textwidth]{./pic/info3}
    	\label{3}
    \end{minipage}
    \begin{minipage}{7cm}
    	\includegraphics[width=\textwidth]{./pic/info4}
    	\label{4}
    \end{minipage}
    \caption{填写信息}
\end{figure}
```

## 九张图片
```
\begin{figure}[htb]
    \centering
    \subfigure[冠状树]{
        \label{xx}
        \begin{minipage}[b]{0.3\textwidth}
            \includegraphics[width=1\textwidth]{runcoronaltree}
        \end{minipage}
    }
    \subfigure[龙曲线]{
        \label{xx}
        \begin{minipage}[b]{0.3\textwidth}
            \includegraphics[width=1\textwidth]{rundragon}
        \end{minipage}
    }
    \subfigure[丢勒五边形]{
        \label{xx}
        \begin{minipage}[b]{0.3\textwidth}
            \includegraphics[width=1\textwidth]{rundurer}
        \end{minipage}
    }
    
    \subfigure[蕨类植物]{
        \label{fern0}
        \begin{minipage}[b]{0.3\textwidth}
            \includegraphics[width=1\textwidth]{runfern0} 
        \end{minipage}
    }
    \subfigure[鱼群]{
        \begin{minipage}[b]{0.3\textwidth}
            \includegraphics[width=1\textwidth]{runfish} 
        \end{minipage}
    }
    \subfigure[树]{
        \begin{minipage}[b]{0.3\textwidth}
            \includegraphics[width=1\textwidth]{runtree}
        \end{minipage}
    }

    \subfigure[c曲线]{
        \begin{minipage}[b]{0.3\textwidth}
            \includegraphics[width=1\textwidth]{runc} 
        \end{minipage}
    }
    \subfigure[枫树]{
        \begin{minipage}[b]{0.3\textwidth}
            \includegraphics[width=1\textwidth]{runmapletree} 
        \end{minipage}
    }
    \subfigure[Sierpinski三角形]{
        \begin{minipage}[b]{0.3\textwidth}
            \includegraphics[width=1\textwidth]{runsierpinski}
        \end{minipage}
    }
    \caption{程序运行结果}
    \label{runresult}
    
\end{figure}
```