---
title: Test driven development with python
date: 2020-04-21 08:16:54
tags:
	- python
	- test
---

# 准备工作

准备工作

<!-- more -->



# 第一章

在拥有测试之前什么都不要做，在TDD中，第一步始终都是一样的：写一个测试。首先我们编写测试，然后我们运行测试，检查他是否按照预先的一样失败。只有这样，我们才回去构建一些属于我们的APP。



## 初步尝试

```python
//functional_tests.py
from selenium import webdriver 
browser = webdriver.Firefox()
browser.get('http://localhost:8000') 

assert 'Django' in browser.title
```



# 第二章

功能测试 == 验收测试 == 端到端测试 == 黑盒测试

功能测试中应该有一个可以让人看懂的故事，我们使用测试代码所附带的注释来明确它。

```python
from selenium import webdriver browser = webdriver.Firefox()
# Edith has heard about a cool new online to-do app. She goes # to check out its homepage browser.get('http://localhost:8000')
# She notices the page title and header mention to-do lists
assert 'To-Do' in browser.title
# She is invited to enter a to-do item straight away
# She types "Buy peacock feathers" into a text box (Edith's hobby # is tying fly-fishing lures)
# When she hits enter, the page updates, and now the page lists # "1: Buy peacock feathers" as an item in a to-do list
# There is still a text box inviting her to add another item. She
# enters "Use peacock feathers to make a fly" (Edith is very methodical)
# The page updates again, and now shows both items on her list
# Edith wonders whether the site will remember her list. Then she sees # that the site has generated a unique URL for her -- there is some
# explanatory text to that effect.
# She visits that URL - her to-do list is still there. # Satisfied, she goes back to sleep
browser.quit()
```



结果：

```python
$ python manage.py runserver
And then, in another shell, run the tests:
$ python functional_tests.py Traceback (most recent call last):
File "functional_tests.py", line 10, in <module> assert 'To-Do' in browser.title
    AssertionError
```




> 小贴士：
>
> * 不要写一些无意义的注释
> * 用好变量名和函数名
> * 建立一个用户故事来描述功能测试
> * 可以使用预期失败来进行铺垫，虽然我们的测试没有成功，但是我们知道失败的原因



## 改进1

* 浏览器窗口没有自动关闭
* 错误的提示信息不够明显

```python
from selenium import webdriver import unittest
class NewVisitorTest(unittest.TestCase): 
  def setUp(self):
			self.browser = webdriver.Firefox() 
      
  def tearDown(self):
			self.browser.quit()
def test_can_start_a_list_and_retrieve_it_later(self):
# Edith has heard about a cool new online to-do app. She goes # to check out its homepage self.browser.get('http://localhost:8000')
# She notices the page title and header mention to-do lists
self.assertIn('To-Do', self.browser.title) self.fail('Finish the test!')
# She is invited to enter a to-do item straight away
[...rest of comments as before] if __name__ == '__main__':
unittest.main(warnings='ignore')
```

理解：使用测试类





TDD

用户故事

期待的失败



# 第三章

### 单元测试与功能测试：

* 单元测试从程序员角度出发测试
* 功能测试从用户角度出发



### 工作流程

* 写一个功能测试描述新的功能特性
* 一旦我们的功能测试失败了，我开始思考怎样最小的通过测试。使用单元测试区描述我们想要代码做的事情。
* 单元测试失败了，进行修改能够通过单元测试
* 然后重新运行功能测试