---
title: 8.逻辑运算符
---

:::info 信息
[视频](https://v.youku.com/v_show/id_XNTk1MDE2OTE0MA==.html)
:::


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

## 获得user_id

第一步，要获取到我们的user_id。

我们在`client.loopBlaze({...})`的后面添加如下的代码：

```js
//把37351541替换成你的Mixin ID
client.readUser("37351541").then(console.log);
```

在命令行中，我们会得到如下的结果：

```js
{
  type: 'user',
  user_id: 'cbb20923-9020-490a-b8f6-e816883c9c99',
  identity_number: '37351541',
  phone: '',
  full_name: 'Mumu Wu',
  biography: '日日是好日，时时是花时',
  avatar_url: 'https://mixin-images.zeromesh.net/ypAHTYUtttQtLLe3ehay-yKKi5Q4kIBwWPFGVElu5zTLSgMs6wb9GwpefS0z_gwjtIJoLPddXQUmTdfZhiHe2AJYyjWTgNHFejj_gw=s256',
  relationship: 'STRANGER',
  mute_until: '2022-01-15T20:05:18.108345252Z',
  created_at: '2019-07-29T06:57:52.513754183Z',
  is_verified: false,
  is_scam: false,
  code_id: 'ec8bd72e-dde8-4321-8d56-2a8ea624e859',
  code_url: 'https://mixin.one/codes/ec8bd72e-dde8-4321-8d56-2a8ea624e859'
}
```

其中：

```js
user_id: 'cbb20923-9020-490a-b8f6-e816883c9c99',
```



## &&, 且

```js
if (msg.data === "你好" && msg.user_id === "cbb20923-9020-490a-b8f6-e816883c9c99") {
```



## !, 非

```js
if (msg.data === "你好" && msg.user_id !== "cbb20923-9020-490a-b8f6-e816883c9c99") {
```



## ||，或

```js
if (msg.data === "你好！" || msg.data === "你好!") {
```



| And   | True  | False |
| ----- | ----- | ----- |
| True  | True  | False |
| False | False | False |

| Or    | True | False |
| ----- | ---- | ----- |
| True  | True | True  |
| False | True | False |

|      | Ture  | False |
| ---- | ----- | ----- |
| Not  | False | True  |

## 整体取反

```js
client.loopBlaze({
  onMessage(msg) {
    if (!(msg.data === "你好")) {
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

## Python代码

and, or, not

```python
a = 10
b = 20
c = 30

# And
if (c > b and b > a):  # and两边都是真
    print(f'"c > b and b > a"的结果是: {c > b and b > a}')
    print("and两边都为真时整个式子为真")
if (c > b and b < a):  # and两边有一边是真
    print(f'"c > b and b < a"的结果是: {c > b and b < a}')
    print("and两边有一边为真时整个式子为真")

# Or
if (a > b or b > c):  # or的两边都是假
    print(f'"a > b or b > c"的结果是: {a > b or b > c}')
    print("or两边都为假时才是假")
if (a < b or b > c):  # or的两边有一边是真
    print(f'"a < b or b > c"的结果是: {a < b or b > c}')
    print("or两边有一边是真的时候就为真")

# Not
if (not (a > b)):  # a>b是假
    print(f'"not (a > b)"的结果是: {not (a > b)}')
    print("not可以把假变成真")
if (not (a < b)):  # a<b是真
    print(f'"not (a < b)"的结果是: {not (a < b)}')
    print("not可以把假变成真")
```

## 课程用到的代码

[coding-newbies-group/learn-code at 1-8](https://github.com/coding-newbies-group/learn-code/blob/main/1-8.js)


## 作业：

1.用代码把表格中的所有情况实现：

| And   | True  | False |
| ----- | ----- | ----- |
| True  | True  | False |
| False | False | False |

| Or    | True | False |
| ----- | ---- | ----- |
| True  | True | True  |
| False | True | False |

|      | Ture  | False |
| ---- | ----- | ----- |
| Not  | False | True  |

2.将逻辑运算符应用在你的机器人代码上，并作为作业提交。
