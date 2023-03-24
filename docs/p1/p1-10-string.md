---
title: 10.字符串
---

:::info 信息
[视频](https://v.youku.com/v_show/id_XNTk1Mjg2MjExNg==.html)
:::


前面的很多例子中，我们都用到了字符串，比如我们在学习[《逻辑判断与分支》](./p1-7-if.md)的时候，我们判断了用户输入的数据是否等于指定的数据，比如我们在学习[《循环》](./p1-9-loop.md)的时候，我们对hello进行了遍历。

这一篇，我们来深入聊聊字符和字符串。

## 1.如何使用字符串

单引号`'`，双引号`"`。

```js
console.log("hello");
console.log('hello');
```

```js
console.log('"hello"');
console.log("'hello'");
```

反单引号 ` 。

```js
const message = `Hello, Mumu Wu!
How are you doing today?`;
console.log(message);
```

## 2.在字符串中使用变量

```js
input = prompt("请输入你的姓名：");
msg3 = `Hello, ${input}!
How are you doing today?`;
console.log(msg3);
```



## 3.转义符

```
'Hello, Mumu!\nHow are you doing today?'
```

`\n` 换行



## 4.字符串处理

### 4.1.字符串的拼接

使用加号（+）将两个或多个字符串连接起来。

### 4.2.获取字符串的长度

使用 length 属性获取字符串的长度。

### 4.3.字符串查找

使用 indexOf() 或 lastIndexOf() 方法查找指定字符或子字符串在字符串中出现的位置。

indexOf()是第一次出现的位置，lastIndexOf()是最后一次出现的位置

### 4.4.字符串截取

slice()，截取指定范围的内容

slice(1,3)，截取1-3，含左不含右

slice(1)，从1开始截取

substring()和slice()功能类似，但是用法有一些区别，请自行了解。

### 4.5.字符串替换

使用 replace() 方法将字符串中的某些字符或子字符串替换为其他字符或字符串。

### 4.6.字符串转换

使用 String() 函数将其他数据类型转换为字符串；

使用 parseInt() 或 parseFloat() 函数将字符串转换为数字。

### 4.7.字符串的分割

使用 split() 方法将字符串按照指定的分隔符分割成数组。



## 5.作业

今天，让我们的作业更具有挑战性一点，希望您可以通过AI工具，达到对学习内容举一反三，掌握轻松自学的能力。

1. 请借助MixChat，用Python实现今天学习过的字符串的操作，将你的代码和结果作为作业提交；

2. 之前，我们可以用下面的代码实现，无论用户输入的是“Hello”还是“hello”，机器人都可以回复“你好”。

   ```js
   client.loopBlaze({
     onMessage(msg) {
       if (msg.data === "Hello" || msg.data === "hello") {
         client.sendMessageText(msg.user_id, "你好！");
       }
     },
     onAckReceipt() {},
   });
   ```

实际上，更进一步可以让我们代码具有更强的**容错性**，比如用户输入Hello, hello, HELLO 甚至 heLLo，我们的机器人都认得。

也就是，只要用户输入的字符串转换为小写后等于"hello"，就是可以接受的。

试着，通过MixChat找到答案，将你的代码作为作业提交。

传送门：[作业：1-10-字符串](https://github.com/coding-newbies-group/programming-co_creation-docs/issues/145)


## 6.演示代码

```js
console.log("hello");
console.log("hello");

console.log('"hello"');
console.log("'hello'");

msg1 = `Hello, Mumu Wu!
How are you doing today?`;
console.log(msg1);

n = "Mumu Wu";
msg2 = `Hello, ${n}!
How are you doing today?`;
console.log(msg2);

input = prompt("请输入你的姓名：");
msg3 = `Hello, ${input}!
How are you doing today?`;
console.log(msg3);

str1 = "hello";
str2 = "world";

console.log(str1 + str2);

console.log(str1.length);

console.log('"l"第一次出现的位置：' + str1.indexOf("l"));

console.log('"l"最后一次出现的位置：' + str1.lastIndexOf("l"));

console.log(str1.slice(1));
console.log(str1.slice(1, 3));

console.log(str1.replace(/l/g, "L"));

str1.replace();

console.log(1);
console.log("1");
console.log(String(1));
console.log(parseInt("1"));

console.log("123" + "4");

guess = int(input("请才一个数："));

str3 = "apple;banana;kiwi;orange";
console.log(str3);
arr = str3.split(";");
console.log(arr);

msg = new Object();
msg.data = "/fee rmb";

console.log(msg.data.slice(-3));

if (msg.data.slice(0, 4) === "/fee") {
  console.log(`现在开始为您查询${msg.data.slice(-3)}手续费`);
}
```

