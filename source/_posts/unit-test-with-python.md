---
title: unit test with python
date: 2020-04-21 08:10:07
tags:
	- python
	- unit test
---

```python
//test_clac.py
import unittest
import calc
class TestCals(unittest.TestCase):
  def test_add(self):
    result = cals.add(10, 5)
    self.assertEqual(result, 15)
    
if __name__ == "__main__":
  unittest.main()
```



```bash
//命令行
python3 test_clac.py
```

<!-- more -->

单元测试属于白盒测试，可以看到测试的所有内容；

功能性测试属于黑盒测试，只能看到提供的接口，但是不能看到其他部分；