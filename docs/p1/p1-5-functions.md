---
title: 5.函数
---

:::info 信息
[教材](https://coding-newbies-group.github.io/programming-co_creation-docs/docs/pilot/p1-3-structure-2#%E5%87%BD%E6%95%B0)
[视频](https://www.bilibili.com/video/BV1iv4y1b7q8/?vd_source=4a888db8814702b2062fcaf2575be745)
:::



## 函数的基本概念

函数就是一个工具，输入给它一个数据，它可以加工后，输出一个数据。



## 调用现成的函数

提供输入，得到输出。



## 定义函数

```python
def plus(input1,input2):
    return input1 + input2

sum = plus(1,2)

print(sum)
```

def，表示这是个函数；

plus，函数的名字，换成你觉得有意义的名字；

(input1, input2)，需要提供的输入；

input1 + input2，函数内对输入的处理过程；

return，函数返回的结果，即函数的输出。



## 默认输出

回顾一下[赋值](p1-4-operators.md#赋值操作符)，我们可以通过赋值，“保存”函数的输出。

```python
a = type("Hi")
print(a)
```

假如没有不写return？是否有return的值？值是什么？



## 总结



函数就是一段代码，将输入，通过处理，产生输出。输入可以是空，什么都不提供，输出也可以是空，什么都不产出。



## 作业：进一步理解处理过程和输出

```python
def demo():
    print("Hi")
    return "Hi"

demo()

a = demo()

print(a)
```

上面的代码会打印出3个Hi，请解释。

传送门：[作业：1-5-函数](https://github.com/coding-newbies-group/programming-co_creation-docs/issues/59)