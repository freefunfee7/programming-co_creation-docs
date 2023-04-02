---
title: 14.函数
---

## 1.创建函数

在《5.函数》一篇中，我们学习了最基础的如何自己创建一个函数。

下面是把其中的作业的部分的Python代码转化为了JavaScript代码。

我们的第一个目标，是理解下面的JavaScript代码。

```js
function demo() {
  console.log("Hi");
  return "Hi";
}

demo();

var a = demo();

console.log(a);
```

我把Python代码也粘过来，对照着，相比，猜也猜出来JavaScript的函数该如何写了。

```py
def demo():
    print("Hi")
    return "Hi"

demo()

a = demo()

print(a)
```

## 2.作用域

我们来看一段代码：

```js
> function demo() {
...   var a = 10; // 在函数内部声明变量a
...   console.log(a); // 输出10
... }
undefined
>
> demo();
10
undefined
> console.log(a); // 报错，a未定义
Uncaught ReferenceError: a is not defined
```

这是在nodejs解释器中输入的代码，我们可以看到调用demo()这个函数的时候控制台是输出了10的。

但是直接在控制台执行console.log(a)的时候，却收到了报错，提示：

```sh
Uncaught ReferenceError: a is not defined
```

意思是a未定义。意思就是说找不到a这个变量。

这里有一个概念叫作用域。

JavaScript函数的作用域是指函数可被访问的区域。

函数内部声明的变量和函数参数只能在函数内部访问，而函数外部声明的变量则可以在函数内部和外部访问。

而这个a就是在函数demo()内部声明的变量。

所以，在函数外面是不能使用的。

再对比一下下面的代码：

```js
> var b = 20; // 在函数外部声明变量b
undefined
>
> function demo() {
...   console.log(b); // 可以在函数内部访问变量b并输出20
... }
undefined
>
> demo();
20
undefined
> console.log(b); // 可以在函数外部访问变量b并输出20
20
undefined
```

当我们再demo()函数之外定义了变量b的时候，demo()函数内部和外部，就都可以使用b这个变量了。

这就是函数的作用域。



这一篇就不留什么作业了。
