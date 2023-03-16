---
title: 4.操作符
---

:::info 信息
[教材](https://coding-newbies-group.github.io/programming-co_creation-docs/docs/pilot/p1-3-structure-2#%E5%B8%B8%E7%94%A8%E6%93%8D%E4%BD%9C%E7%AC%A6)
[视频](https://www.bilibili.com/video/BV1Zg4y177jo/?vd_source=4a888db8814702b2062fcaf2575be745)
:::



这一篇，我们要来学习操作符。

通篇我们要把握2个要点：

1. 什么是操作符？

   比如加减乘除，操作符是连接器

2. 连接的是谁？

   值与变量

回顾一下[1-2.值与变量](./p1-2-values-variables.md)

```python
42  #整数
3.14  #浮点数
'a'  #字符
'abracadabra'  #字符串
True  #布尔值，真
False  #布尔值，假
```

## 算术操作符

```
+  #加
-  #减
*  #乘
/  #除
** #幂
// #商，除后的整数部分
%  #余数
```

## 大小比较操作符

```
>  #大于
<  #小于
>= #大于等于
<= #小于等于
== #等于
!= #不等于
```

得到的结果是布尔值。

## 赋值操作符

```python
=  #赋值操作符
```

请比较[1-2.值与变量](./p1-2-values-variables.md)中赋值用到的=号，和[大小比较操作符](#大小比较操作符)中的==（双等）号作用的不同。

赋值就是把=右边的值，注意是值，也就是说，如果放一个复杂的“式子”，也是把得到的值再赋值给=左边的变量，就这么单纯的简单。

灵活用法： a += 1 等价于 a = a + 1，以此类推， -=，*=，/= 等等。

## 逻辑运算符

and, or, not与其说是与、或、否，不如说是，且、或、不更通俗。

我们将在后面，讲了Mixin机器人，讲了逻辑判断与分支再来讲这部分。

请大家，再对[大小比较操作符](#大小比较操作符)的结果进行熟悉，对True，False这两个值建立充分的印象。



## 作业

```python
a,b = 2,3
```

用大小比较操作符“操作”一遍。

传送门：[作业：1-4-操作符](https://github.com/coding-newbies-group/programming-co_creation-docs/issues/57)

## 补充内容：

>  补充内容属于不强求非要搞懂的内容，大概了解即可，你一时半会用不好，你一时半会不需要理解，完全可以以后再搞懂。

### 代码注释

注释，就是解释器不解释的内容。

对于整行注释，在VSCode中如何快速注释代码；

注释代码还有什么用途？

```python
a = 1
#print(type(a))
print(len(a))
```

```python
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: object of type 'int' has no len()
```



### 浮点误差

```python
a,g = 6,0.7
```

```python
>>> a,g = 6,0.7
>>> a*g
4.199999999999999
>>>
```

计算机里小数是以一种“近似值”的方式表示的。

大部分场景通过设定误差范围来解决。

一段真实代码，虽然不是浮点误差，但是可以用来解释这种解决问题的场景。

```javascript
if (
	msg.data.amount.substring(0, 6) >= "2.8" &&
	msg.data.amount.substring(0, 6) <= "3.1"
)
```
