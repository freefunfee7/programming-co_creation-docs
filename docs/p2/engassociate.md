---
title: 1.英语小助手
---

## 1.作业

#### 1. 实现如下图的字符串拼接效果：[传送门](https://github.com/coding-newbies-group/engassociate/issues/1)

![](https://raw.githubusercontent.com/vwumumu/images/master/20230403211430.png)

#### 2. 自己实现下面的代码，替换你自己的content，输出openai给你返回的结果：[传送门](https://github.com/coding-newbies-group/engassociate/issues/2)

   ```js
   const { Configuration, OpenAIApi } = require("openai");
   
   const configuration = new Configuration({
     apiKey: process.env.OPENAI_API_KEY,
   });
   const openai = new OpenAIApi(configuration);
   
   const completion = await openai.createChatCompletion({
     model: "gpt-3.5-turbo",
     messages: [{role: "user", content: "Hello world"}],
   });
   console.log(completion.data.choices[0].message);
   ```

#### 3. 实现机器人输入部分的功能：[传送门](https://github.com/coding-newbies-group/engassociate/issues/3)

   无论输入中文还是英文，给出中文和英文的结果。参考下图：

   ![](https://raw.githubusercontent.com/vwumumu/images/master/20230403211949.png)

   


## 2.过程记录

1. 创建目录

2. 初始化项目

3. 安装sdk

   [简介 & 安装 & 快速上手 | Mixin JS SDK (liuzemei.github.io)](https://liuzemei.github.io/mixin-js-sdk-docs/docs/server/intro)

4. 添加config.js

5. 运行机器人，拿到消息

   消息的结构是一个对象，有key-value

6. 我们希望的消息的结构

   > 用户   =>   机器人发中文/英文
   >
   > 机器人返回：
   >
   > 用户发的：中文+英文
   >
   > 机器人回的：中文+英文

7. 代码实现消息结构

8. 判断中英文输入

9. 引入ChatGPT

10. 实现通过ChatGPT翻译输入的文本

## 3.视频

[01](https://v.youku.com/v_show/id_XNTk1NTE1MTkyNA==.html) | [02](https://v.youku.com/v_show/id_XNTk1NTE1MzI5Ng==.html) | [03](https://v.youku.com/v_show/id_XNTk1NTE1MzM0NA==.html) | [04](https://youtu.be/23TDNwcPjqE) | [05](https://v.youku.com/v_show/id_XNTk1MzE5MzYwNA==.html) | [06](https://youtu.be/JYrx2oRxiC8)