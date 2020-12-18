---
title: nlp-nltk使用
author: ChangzeYan
date: 2020-12-15 19:17:58
tags: NLP
categories: Python
cover:
---

# Nltk
## 安装nltk
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
linux中的目录是：
```bash
Searched in:
    - '/home/hadoopcj/nltk_data'
    - '/usr/share/nltk_data'
    - '/usr/local/share/nltk_data'
    - '/usr/lib/nltk_data'
    - '/usr/local/lib/nltk_data'
    - '/home/hadoopcj/nltk_data'
    - ''
```

然后进入到"D:\\nltk_data\\tokenizers"目录，**将punkt.zip解压** 即可。

![packages文件夹内容](https://github.com/ChangzeYan/ChangzeYan.github.io/raw/hexo/source/pic/nlp依赖环境安装-nltk-punkt.png)

## nltk英文分词

```py
from nltk import word_tokenize

paragraph = "When Jobs arrived back at Apple, it had a conventional structure for a company of its size and scope. It was divided into business units, each with its own P&L responsibilities."
words = word_tokenize(paragraph)
print(words)
```

## 词性标注
nltk中的词性：

| tag  | mean    | 释义           |例子     |
|------|------------------------------------------|--------------|-------------------------------------------------------|
| CC   | Coordinating conjunction                 | 连词           | and, or,but, if, while,although                       |
| CD   | Cardinal number                          | 数词           | twenty\-four, fourth, 1991,14:24                      |
| DT   | Determiner                               | 限定词          | the, a, some, most,every, no                          |
| EX   | Existential there                        | 存在量词         | there, there’s                                        |
| FW   | Foreign word                             | 外来词          | dolce, ersatz, esprit, quo,maitre                     |
| IN   | Preposition or subordinating conjunction | 介词连词         | on, of,at, with,by,into, under                        |
| JJ   | Adjective                                | 形容词          | new,good, high, special, big, local                   |
| JJR  | Adjective, comparative                   | 比较级词语        | bleaker braver breezier briefer brighter brisker      |
| JJS  | Adjective, superlative                   | 最高级词语        | calmest cheapest choicest classiest cleanest clearest |
| LS   | List item marker                         | 标记           | A A\. B B\. C C\. D E F First G H I J K               |
| MD   | Modal                                    | 情态动词         | can cannot could couldn’t                             |
| NN   | Noun, singular or mass                   | 名词           | year,home, costs, time, education                     |
| NNS  | Noun, plural                             | 名词复数         | undergraduates scotches                               |
| NNP  | Proper noun, singular                    | 专有名词         | Alison,Africa,April,Washington                        |
| NNPS | Proper noun, plural                      | 专有名词复数       | Americans Americas Amharas Amityvilles                |
| PDT  | Predeterminer                            | 前限定词         | all both half many                                    |
| POS  | Possessive ending                        | 所有格标记        | ’ ‘s                                                  |
| PRP  | Personal pronoun                         | 人称代词         | hers herself him himself hisself                      |
| PRP$ | Possessive pronoun                       | 所有格          | her his mine my our ours                              |
| RB   | Adverb                                   | 副词           | occasionally unabatingly maddeningly                  |
| RBR  | Adverb, comparative                      | 副词比较级        | further gloomier grander                              |
| RBS  | Adverb, superlative                      | 副词最高级        | best biggest bluntest earliest                        |
| RP   | Particle                                 | 虚词           | aboard about across along apart                       |
| SYM  | Symbol                                   | 符号           | % & ’ ” ”\. \) \)                                     |
| TO   | to                                       | 词to          | to                                                    |
| UH   | Interjection                             | 感叹词          | Goodbye Goody Gosh Wow                                |
| VB   | Verb, base form                          | 动词           | ask assemble assess                                   |
| VBD  | Verb, past tense                         | 动词过去式        | dipped pleaded swiped                                 |
| VBG  | Verb, gerund or present participle       | 动词现在分词       | telegraphing stirring focusing                        |
| VBN  | Verb, past participle                    | 动词过去分词       | multihulled dilapidated aerosolized                   |
| VBP  | Verb, non\-3rd person singular present   | 动词现在式非第三人称时态 | predominate wrap resort sue                           |
| VBZ  | Verb, 3rd person singular present        | 动词现在式第三人称时态  | bases reconstructs marks                              |
| WDT  | Wh\-determiner                           | Wh限定词        | who,which,when,what,where,how                         |
| WP   | Wh\-pronoun                              | WH代词         | that what whatever                                    |
| WP$  | Possessive wh\-pronoun                   | WH代词所有格      | whose                                                 |
| WRB  | Wh\-adverb                               | WH副词         |                                                       |


```python
# 分词后的词列表
paragraph='When Jobs arrived back at Apple, it had a conventional structure for a company of its size and scope. It was divided into business units,'
words = word_tokenize(paragraph)
# 词性标注
pos_tag = nltk.pos_tag(words)
print(pos_tag)
```

获取一个词的词性也得用列表：
```py
t=nltk.pos_tag(['news'])
print(t)
```

# Nltk的语料库
语料库在D:\\nltk_data\\corpora下：

参考：[NLTK文本语料库](https://www.cnblogs.com/itdyb/p/5899616.html)
- 古腾堡语料库：gutenberg，包含古腾堡项目电子文本档案的一小部分文本。该项目目前大约有36000本免费的电子图书。
- 网络聊天语料库：webtext、nps_chat；这部分代表的是非正式的语言，包括Firefox交流论坛、在纽约无意听到的对话、《加勒比海盗》电影剧本。个人广告以及葡萄酒的评论。
- 布朗语料库：brown；布朗语意库是第一个百万词集的英语电子语料库，有布朗大学于1961年创建，包含500多个不同来源的文本，按照文本类型，如新闻、社评等分类。布朗语料库是一个研究文体之间系统性差异的资源。
- 路透社语料库：reuters；路透社语料库包括10788个新闻文档，共计130万字。这些文档分成了90个主题，按照‘训练’和‘测试’分为两组。因此，编号为‘test/14826’的文档属于测试组。这样分割是为了方便运用训练和测试算法的自动检验文档的主题。
- 就职演说语料库：inaugural；是55个文本的集合，每个文本都是一个总统的演讲。这个集合的显著特征就是时间维度。
- 标注文本语料库和其他语言语料库
