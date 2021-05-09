---
title: Latex-参考文献
author: ChangzeYan
date: 2021-05-09 19:22:34
tags: 
categories: Latex
cover:
---

# vscode 编译bib文件的配置

参考：[vscode latex 踩坑记 ： 文献索引 bib 文件和setting.json的那点事](https://blog.csdn.net/lishu14/article/details/102774145)

在vscode中 file->preferences->settings：

点击右上角的（open settings(Json)）:

![vscode中配置文件位置](https://github.com/ChangzeYan/ChangzeYan.github.io/raw/hexo/source/pic/vscode-latex-setting.png)
复制下面代码：
```
{
    "latex-workshop.latex.recipes": [
        {
            "name": "xelatex -> bibtex -> xelatex*2",
            "tools": [
                "xelatex",
                "bibtex",
                "xelatex",
                "xelatex"
            ]
        },
        {
            "name": "XeLaTeX",
            "tools": [
                "xelatex"
            ]
        },
        {
            "name": "LaTeXmk",
            "tools": [
                "latexmk"
            ]
        },
        // {
        //     "name": "PDFLaTeX",
        //     "tools": [
        //         "pdflatex"
        //     ]
        // },
        {
            "name": "BibTeX",
            "tools": [
                "bibtex"
            ]
        },
        
        // {
        //     "name": "pdflatex -> bibtex -> pdflatex*2",
        //     "tools": [
        //         "pdflatex",
        //         "bibtex",
        //         "pdflatex",
        //         "pdflatex"
        //     ]
        // }
    ],
  "latex-workshop.latex.tools": [
    {
        "name": "xelatex",
        "command": "xelatex",
        "args": [
            "-synctex=1",
            "-interaction=nonstopmode",
            "-file-line-error",
            "%DOCFILE%"
        ]
    },
    {
        "name": "pdflatex",
        "command": "pdflatex",
        "args": [
            "-synctex=1",
            "-interaction=nonstopmode",
            "-file-line-error",
            "%DOCFILE%"
        ]
    },
    {
        "name": "latexmk",
        "command": "latexmk",
        "args": [
            "-synctex=1",
            "-interaction=nonstopmode",
            "-file-line-error",
            "-pdf",
            "-outdir=%OUTDIR%",
            "%DOCFILE%"
        ]
    },
    {
        "name": "bibtex",
        "command": "bibtex",
        "args": [
            "%DOCFILE%"
        ]
    }
  ],
  "latex-workshop.latex.clean.fileTypes": [
      "*.aux",
      "*.bbl",
      "*.blg",
      "*.idx",
      "*.ind",
      "*.lof",
      "*.lot",
      "*.out",
      "*.toc",
      "*.acn",
      "*.acr",
      "*.alg",
      "*.glg",
      "*.glo",
      "*.gls",
      "*.ist",
      "*.fls",
      "*.log",
      "*.fdb_latexmk"
  ],
  "files.autoSave": "afterDelay",
  "editor.wordWrap": "on",
  "explorer.confirmDelete": false,
  "git.autofetch": true,
  "window.zoomLevel": 0,
  "explorer.confirmDragAndDrop": false,
  }
```

## 配置文件注意点
原来我的配置是下面这样（也是很多博客中的做法）：
```
 "latex-workshop.latex.recipes": [
  {
    "name": "xelatex",
    "tools": [
        "xelatex"
    ]
  },
 
  {
    "name": "latexmk",
    "tools": [
        "latexmk"
    ]
  },
  
  {
    "name": "pdflatex -> bibtex -> pdflatex*2",
    "tools": [
        "pdflatex",
        "bibtex",
        "pdflatex",
        "pdflatex"
    ]
  }
  ],

```

**这个配置的问题在于，只会使用第一个recipe**

```
{
    "name": "xelatex",
    "tools": [
        "xelatex"
    ]
  },
```

会导致只编译一次，而bib文件不会被编译，导致参考文献出不来

而用命令行编译是四次，结果正常：
```bash
xelatex -shell-escape thesis.tex
bibtex thesis
xelatex -shell-escape thesis.tex
xelatex -shell-escape thesis.tex
```


假设我们的核心文件是hello.tex，我们的参考文件是ref.bib，在hello.tex文末通过  \bibliography{ref}  来指明引用的文件叫做ref.bib。然后编译hello.tex，发现文章除了参考文献之外的其他内容都是正常的，唯独参考文献。这是因为只用了xelatex来编译hello.tex。而ref.bib也需要编译，通过bibtex。而直接在命令行里输入bibtex ref.bib会有问题报错说找不到ref.aux。本文件夹里只有hello.aux，因此只需要将ref.bib改名为hello.bib即可（在hello.tex中也需要将对应的 \bibliography{ref} 改为 \bibliography{hello}），然后先编译hello.tex（这个能在vscode里通过ctrl+s自动保存hello.tex进而自动编译）生成hello.aux，再通过命令行编译hello.bib生成hello.bbl，然后再编译hello.tex更新hello.aux，然后再编译hello.tex将其与参考文献(hello.bbl)真正结合起来。所以一共需要编译4次！

很麻烦！那么我们只需要对上述的三个recipe调换顺序即可，把第三个放到第一位置：
```
"latex-workshop.latex.recipes": [
  {
    "name": "pdflatex -> bibtex -> pdflatex*2",
    "tools": [
        "pdflatex",
        "bibtex",
        "pdflatex",
        "pdflatex"
    ]
  }
 
  {
    "name": "xelatex",
    "tools": [
        "xelatex"
    ]
  },
 
  {
    "name": "latexmk",
    "tools": [
        "latexmk"
    ]
  },
  ],
  ```