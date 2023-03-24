---
title: 7.逻辑判断与分支
---

:::info 信息
[视频](https://v.youku.com/v_show/id_XNTk0Nzc3OTk4OA==.html)
:::



## if, else if, else

当部署好上面的机器人后，用下面的代码：

```js
client.loopBlaze({
  onMessage(msg) {
    if (msg.data === "你好") {
      client.sendMessageText(msg.user_id, "你也好！");
    } else if (msg.data === "你是谁") {
      client.sendMessageText(msg.user_id, "我是编程学习小助手");
    } else {
      client.sendMessageText(msg.user_id, "我不知道你在说什么");
    }
  },
  onAckReceipt() {},
});
```

替换掉：

```js
client.loopBlaze({
  onMessage(msg) {
    client.sendMessageText(msg.user_id,"我的第一个Mixin机器人")
  },
});
```



## 逻辑表达式

值为布尔值的式子



## Python代码

```python
a = 5
if (a > 5):
    print("a大于5")
elif (a < 5):
    print("a小于5")
else:
    print("a等于5")
```



## 课程用到的代码

[coding-newbies-group/learn-code at 1-7](https://github.com/coding-newbies-group/learn-code/blob/main/1-7.js)



## 作业

**录制视频后，作业的问题做了调整，以下面内容为准**

如果给机器人发送你好，机器人会回复什么？请解释一下为什么？

```js
if( msg.data === "你好"){
	client.sendMessageText(msg.user_id, "你也好！")
} else if (msg.data === "你好"){
	client.sendMessageText(msg.user_id, "我是编程学习小助手")
} else {
    client.sendMessageText(msg.user_id, "我不知道你在说什么")
}
```

**下面是视频录制后新加的作业**

请用其他逻辑表达式，替换if后面括号里面的内容，来熟悉逻辑表达式和If逻辑判断。

把你的练习和运行结果，作为作业，提交给技术组。

大家可以自行去搜索一下JavaScript的常见运算符，然后自己修改一下机器人中的代码，来熟悉更多的知识。

传送门：[作业：1-7-逻辑判断与分支](https://github.com/coding-newbies-group/programming-co_creation-docs/issues/142)

