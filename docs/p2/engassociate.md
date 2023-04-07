---
title: 1.英语小助手
---

## 1.视频

[01](https://v.youku.com/v_show/id_XNTk1NTE1MTkyNA==.html) | [02](https://v.youku.com/v_show/id_XNTk1NTE1MzI5Ng==.html) | [03](https://v.youku.com/v_show/id_XNTk1NTE1MzM0NA==.html) | [04](https://youtu.be/23TDNwcPjqE) | [05](https://v.youku.com/v_show/id_XNTk1MzE5MzYwNA==.html) | [06](https://youtu.be/JYrx2oRxiC8) | [07](https://youtu.be/ZUGQ4Z2WeNk) | [08](https://youtu.be/Ju1Rg4x84Zg) | [09](https://youtu.be/as2ZBIDx41E) | [10](https://youtu.be/FqCSgyDulfA) | [11](https://youtu.be/4Vj_pcaKubY) | [12](https://youtu.be/m759dozLp4Q) | [13](https://youtu.be/4T-DxsXC5yk)



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

11. 找一个合适的对ChatGPT的角色的文字描述来定义英文对话，口语提升的功能

12. 写了一个新的函数，叫做conversion()用来处理对话

13. 将chatgpt的请求部分，单独抽离出一个函数叫queryChatGPT()

14. 调整根据单词生成句子并展开对话

15. 随机选择单词

16. 实现根据艾宾浩斯生成下一次学习时间

17. 将单词由数组保存改为由对象保存，添加一些单词的属性，学习次数，最后一次学习时间，下一次学习时间

18. 实现根据下一次学习时间选取单词

19. 实现学习单词后更新单词信息


## 3.作业

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

#### 4. 实现机器人回应的代码部分

#### 5. 为机器人添加一个白名单用户判断
