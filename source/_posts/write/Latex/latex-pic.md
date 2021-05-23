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
    % 调整图片与标题之间的距离
    \setlength{\abovecaptionskip}{0.5em}
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

# 图标题
```
\vspace{-0.8cm}  %调整图片与上文的垂直距离
\setlength{\abovecaptionskip}{-0.2cm}   %调整图片标题与图距离
\setlength{\belowcaptionskip}{-1cm}   %调整图片标题与下文距离
```

# 子标题与子图的距离
参考：[subfigure命令插入多行多列图片修改子图与子图、子标题的距离](https://www.pianshen.com/article/8282354102/)
\subfigcapskip=-5pt %设置子图与子标题之间的距离

```
\usepackage{graphicx}  %插入图片的宏包
\usepackage{float}  %设置图片浮动位置的宏包
\usepackage{subfigure}  %插入多图时用子图显示的宏包
 
\begin{figure}[H] %这里使用的是强制位置，除非真的放不下，不然就是写在哪里图就放在哪里，不会乱动
	\centering  %图片全局居中
	\vspace{-0.35cm} %设置与上面正文的距离
	\subfigtopskip=2pt %设置子图与上面正文或别的内容的距离
	\subfigbottomskip=2pt %设置第二行子图与第一行子图的距离，即下面的头与上面的脚的距离
	\subfigcapskip=-5pt %设置子图与子标题之间的距离
	\subfigure[original]{
		\label{level.sub.1}
		\includegraphics[width=0.32\linewidth]{./figure/original.png}}
	\quad %默认情况下两个子图之间空的较少，使用这个命令加大宽度
	\subfigure[level=9]{
		\label{level.sub.2}
		\includegraphics[width=0.32\linewidth]{./figure/level9.png}}
	  %这里是空了一行，能够实现强制将四张图分成两行两列显示，而不是放不下图了再换行，使用\\也行。
	\subfigure[level=8]{
		\label{level.sub.3}
		\includegraphics[width=0.32\linewidth]{./figure/level8.png}}
	\quad
	\subfigure[level=7]{
		\label{level.sub.4}
		\includegraphics[width=0.32\linewidth]{./figure/level7.png}}
	\caption{不同level的渲染效果}
	\label{level}
\end{figure}
```