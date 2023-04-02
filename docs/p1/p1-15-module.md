---
title: 15.模块
---

通过函数的学习，我们知道函数的作用就是接收一个输入，经过函数的处理，然后输出一个结果。

比如，我们的一个需求是让函数完成简单的加法运算。

我们可以这样写：

```js
function plus(a, b) {
  sum = a + b;
  return sum;
}
```

然后，我们计算任何两个数的和时，就只需要“喂给”plus()这个函数就可以了。

比如这样：

```js
a = plus(1,2)
console.log(a)
```

屏幕上会输出3。

大家可能觉得，1+2挺简单，为什么需要做成一个函数呢？

这是因为，就这个需求而言，一个加法运算可能很简单，`a = plus(a,b)` 可能比`a = 1 + 2`敲的内容还多。

但是，假如函数中间的过程是复杂的呢？

比如：

```js
async function toMultiSignAddress({ data, created_at }, user) {
  const rec = await client.transaction({
    asset_id: data.asset_id,
    amount: data.amount,
    trace_id: client.newUUID(),
    memo: "小白慢爬营编程共创",
    opponent_multisig: {
      receivers,
      threshold,
    },
  });
  const symbol = (await client.readAsset(rec.asset_id)).symbol;
  const amount = (parseFloat(rec.amount) * -1).toString();
  const msg_recive_crypto = `多签钱包 ${new Date(
    created_at
  ).toLocaleString()} 收到来自 "${user}" 的转账 ${symbol} ${amount};\n点击查看交易快照：https://mixin.one/snapshots/${
    rec.snapshot_id
  }`;
  await toGroup(msg_recive_crypto, group);
}
```

这是我之前做的小白慢爬营收款机器人其中的一个函数。

使用的时候，只是如下调用该函数，并提供msg和user作为输入就可以了。

```js
await toMultiSignAddress(msg, user);
```

显然，使用的时候更简洁，否则，每次就得输入一大堆函数里面的代码。

更关键的是，函数让使用者在通过函数实现某个功能的时候，只需要考虑输入，输出，不需要关心中间的过程是什么。

函数也更可以很方便的提供给别人来使用，别人也不用关心函数的过程，只需要关心输入和输出就可以了。



如何能使用别人写好的模块呢？就是通过引入模块。

可以理解为模块就是功能扩展包，通过引入这个扩展包，我们就可以直接使用别人写好的一些函数了。



## 1.安装模块

在[《6.做个Mixin机器人》](https://coding-newbies-group.github.io/programming-co_creation-docs/docs/p1/p1-6-1-mixinbot-windows#32安装-mixin-node-sdk)一篇中，这就是安装模块的方式：

![](https://raw.githubusercontent.com/vwumumu/images/master/20230402221330.png)

通常来讲，我们找到需要的模块的名字，然后可以通过nodejs的包管理器npm来进行安装，命令是`npm install <模块的名字>`。



## 2.引入模块

在[《6.做个Mixin机器人》](https://coding-newbies-group.github.io/programming-co_creation-docs/docs/p1/p1-6-1-mixinbot-windows#33%E5%88%9D%E5%A7%8B%E5%8C%96mixin%E6%9C%BA%E5%99%A8%E4%BA%BA)一篇中，这就是引入模块的方式，实际上，模块的官方网站，通常都会告诉如何开始。后面就是熟悉模块中的函数，然后用起来就可以了。

![](https://raw.githubusercontent.com/vwumumu/images/master/20230402221700.png)

关于，模块，我们就先涉及这些，大概有个了解，知道引入模块后，就可以使用别人写好的函数就可以了，具体项目中，我们有了需求，需要通过引入模块去解决时，就会看到引入模块并开始使用的模块中函数的代码。


这一篇就不留什么作业了。