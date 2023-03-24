---
title: 9.循环
---

:::info 信息
[视频](https://v.youku.com/v_show/id_XNTk1MjUxODI0MA==.html)
:::


生活中，我们总是会遇到一些重复的事情，枯燥而乏味。比如，把你好抄写一百遍。

这个动作，也可以描述为，抄写一遍你好，然后重复一百遍。这个重复，就是循环。



## for循环

```js
str = "hello";

console.log(str[0]);

for (i = 0; i < str.length; i++) {
  console.log(i);
  console.log(str[i]);
}

for (i of str){
  console.log(i);
}
```



### 遍历

#### Python代码

```py
str = "hello"
for i in str:
    print(i)
```

#### JavaScript代码

```js
for (i of str){
  console.log(i);
}
```



## while循环

下面的代码有一处**错误**，会在视频中演示正确的代码。

```python
number = 3
guess = 0

print("我想了一个1到100之间的数，你来猜吧！")

while guess != number:
    guess = input("请猜一个数：")
    if guess != number:
        print("猜错了，请在试试。")
    else:
        print("恭喜你猜对了。")
```



## break和continue

```python
for i in "hello":
    if i == "l":
        break
    print(i)
```

```python
for i in "hello":
    if i == "l":
        continue
    print(i)
```



## 作业

这里，我就举贤不避亲了。我们可以开始借助MixChat帮助我们学习了。

用MixChat帮你进行代码转换，然后分别解释这个代码工作的过程，作为作业提交。

![image-20230319124858342](https://raw.githubusercontent.com/vwumumu/images/master/image-20230319124858342.png)

传送门：[作业：1-9-循环](https://github.com/coding-newbies-group/programming-co_creation-docs/issues/144)