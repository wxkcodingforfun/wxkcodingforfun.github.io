---
title: Python学习笔记
date: 2021-02-22 22:51:54
tags:
---

# Python 学习笔记

title() 将字符串里面每个单词的首字母转化为大写

upper() lower() 全部转化为大写和小写

strip() 英文：strip 脱光 -——》去除字符串两端的空白

lstrip() rstrip() 左端和右端的空白



执行import this 会出现python之父写的代码原则 （小彩蛋吧）

-1索引是最后一个元素

pop(n) 删除索引为n的元素

remove(element) 删除值为element的元素



sort() ,sort(reverse = True)

.reverse() 反转列表

len( )求列表 长度





## 遍历

for e in es:

​	print(e)



python里面 只要没有缩进就是在循环里面

[:3] 切片 有点类似matlab里面的数组的表示方式



()可以用来定义元组（有点像C++里面的常量数组）



a = (199,200)





if ************：

​	表达式

else ********：

​	表达式



字典

alien_0 = {'color': 'green', 'points': 5}  



函数参数默认是引用，除非添加[:]表示创建一个切片副本传入函数



明天开始看类