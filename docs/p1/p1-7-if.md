---
title: 7.逻辑判断与分支
---

:::info 信息
[教材](https://coding-newbies-group.github.io/programming-co_creation-docs/docs/pilot/p1-3-structure-2#%E5%87%BD%E6%95%B0)
[视频](https://www.bilibili.com/video/BV1Hx4y1F7pH/?vd_source=4a888db8814702b2062fcaf2575be745)
:::

## if...else语句

if true:
    执行后面的语句



当部署好上面的机器人后，用下面的代码：

```js
client.loopBlaze({
  onMessage(msg) {
    console.log(msg);
    if (msg.data === "?" || msg.data === "？") {
      client.sendMessageText(msg.user_id, "帮助信息");
    } else if (msg.data === "/") {
      client.sendMessageText(msg.user_id, "命令信息");
    } else {
      client.sendMessageText(
        msg.user_id,
        "请发送“?”获得帮助，或者“/”获得命令列表"
      );
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
