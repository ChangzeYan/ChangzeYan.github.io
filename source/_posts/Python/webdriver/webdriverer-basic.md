---
title: webdriverer命令
author: ChangzeYan
date: 2021-09-03 11:43:31
tags:
categories: Webdriver
cover:
---

# Webdriver使用
安装
```
pip install selenium
```

下载与浏览器版本对应的驱动：
chrome：
http://npm.taobao.org/mirrors/chromedriver/
http://chromedriver.storage.googleapis.com/index.html
下载后放到以下目录：
C:\Program Files (x86)\Google\Chrome\Application
然后修改环境变量，在path后面加：
;C:\Program Files (x86)\Google\Chrome\Application
导包
```
from selenium import webdriver
from selenium.webdriver import ChromeOptions
```

启动与打开网页：
```
本地:
option = ChromeOptions()
# 启动chrome时，去掉window.navigator.webdriver属性，使浏览器检测不到webdriver
option.add_experimental_option('excludeSwitches', ['enable-automation'])
browser = webdriver.Chrome(options=option)
browser.get(r"C:\Users\Changze\Desktop\gongzhong.html")
```

拖动滚动条
```
# 拖动十次
for i in range(10):
    browser.execute_script("window.scrollTo(0, document.body.scrollHeight);")
    # 两次拖动之间间隔随机时间
    time.sleep(random.randint(2, 5))
```

获取标签的属性值：
```
<time datetime="2021-07-01T09:17:23.000Z">7月1日</time>

ele_time = twitter.find_element_by_tag_name('time')
datetime = ele_time.get_attribute('datetime')
```