# python3---1.1.8 Selenium 动态渲染页面爬取.md

## Selenium

### 准备工作
- 安装 Selenium 库
- 安装 ChromDriver 驱动

- 安装参考：[python 网络爬虫开发实战](https://github.com/fairyly/python/blob/master/python3%20%E7%88%AC%E8%99%AB%E5%BC%80%E5%8F%91%E5%AE%9E%E6%88%98%EF%BC%88book%EF%BC%89.md#1%E8%AF%B7%E6%B1%82%E5%BA%93%E5%AE%89%E8%A3%85)


### Selenium 的使用

- 基础使用

```
# selenium 使用
# 
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.support.wait import WebDriverWait

browser = webdriver.Chrome()
try:
  browser.get('https://www.baidu.com')
  input = browser.find_element_by_id('kw')
  input.send_keys('python3')
  input.send_keys(Keys.ENTER)

  wait = WebDriverWait(browser,10)
  wait.until(EC.presence_of_element_located((By.ID,'content_left')))

  print(browser.current_url)
  print(browser.get_cookies())
  print(browser.page_source)
finally:
  browser.close()
```

- 声明浏览器对象

```
from selenium import webdriver

browser = webdriver.Chrome()
browser = webdriver.Firefox()
browser = webdriver.Edge()
browser = webdriver.PhantomJS()
browser = webdriver.Safari()

这就完成浏览器对象的初始化，并将其赋值为 browser 对象
```

- 访问页面: 用 get() 方法请求页面

```
from selenium import webdriver

browser = webdriver.Chrome()

browser.get('https://www.baidu.com')
print(browser.page_source)
browser.close()
```

- 查找节点
  - 通过 id,class,xpath 等获取
  - find_element_by_id
  - find_element_by_name
  - find_element_by_xpath
  - find_element_by_link_text
  - find_element_by_partial_link_text
  - find_element_by_tag_name
  - find_element_by_class_name
  - find_element_by_css_selector

```
input = browser.find_element_by_id('kw')
input_second = browser.find_element_by_css_selector('.q')
input_third = browser.find_element_by_xpath('//*[@id="test"]')
```
>， Selenium 还提供了通用方法 find_element(），它需要传入两个参数 查找方式 By 和值
际上，它就是 find_element_by_id()这种方法的通用函数版本，
```
browser.find_element(By.ID,'content'）
```



- 节点交互
```

from selenium import webdriver
import time


browser = webdriver.Chrome()

input.send_keys('python3')
input.clear()

time.sleep(1)

button = browser.find_element_by_id('kw')
button.click()
```


- 动作链: 如鼠标拖拽，键盘按键等

```
from selenium import webdriver
from selenium.webdriver import ActionChains

browser = webdriver.Chrome()

actions = ActionChains(browser)
actions.drag_and_drop(button,target)
actions.perform()
```

- 执行 js: excute_script()

```
from selenium import webdriver

browser = webdriver.Chrome()

browser.excute_script('alert("test")')
```


- 获取节点信息

  - 获取属性： get_attribute()
  - 获取文本值： input.text 属性
  - 获取 id, 位置，标签名，大小： input.id,input.location,input.tag_name,input.size


- 切换 frame

```
browser.switch_to.frame('iframeResult')

browser.switch_to.parent_frame()
```


- 延时等待
  - 隐式等待： browser.implicitly_wait(10)
  - 显式等待：WebDriverWait(browser,10).until()


- 前进、后退

```
browser.back()

browser.forward()

browser.close()
```


- cookies

```
browser.get_cookies()

browser.add_cookies({})

browser.delete_all_cookies()
```


- 选项卡管理

```
browser.switch_to_window(browser.window_handles[0])
```


- 异常处理

```
try:

except NoSuchElementException:
  print()

finally:
  browser.close()
```



## splash 使用

```
利用 Splash ，我们可以实现如下功能：
1.异步方式处理多个网页渲染过程
2.获取渲染后的页面的源代码或截图；
3.通过关闭图片渲染或者使用 Adblock 规则来加快页面渲染速度；
4.可执行特定的 JavaScript 脚本；
5.可通过 Lua 脚本来控制页面渲染过程；
6.获取渲染的详细过程并通过 HAR ( HTTP Archive ）格式呈现
```

- 第一章中看到 docker 中安装的：支持 win10 专业版、企业版、教育版下才能安装
  - doc: https://docs.docker.com/docker-for-windows/
  - windows/docker-toolbox/： http://mirrors.aliyun.com/docker-toolbox/windows/docker-toolbox/

  - 首先，让我们通过下面的命令来检查Docker的安装是否正确：

  `docker info`

  如果没有找到此命令，则表示Docker没有正确安装。

  所有Docker命令可以用以下命令查看：

  `docker help`

  - [win10家庭版安装Docker](https://blog.csdn.net/hao_kkkkk/article/details/79853752)
