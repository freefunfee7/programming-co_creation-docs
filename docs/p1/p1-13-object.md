---
title: 13.对象
---

有了[《12.数组》](./p1-12-array.md)一篇打底，后面的就好办了。

今天我们学习一下JavaScript中的对象，和Python中的字典比较像。

首先，我们来看一段示例代码。

```js
var person = {
  name: "Tom",
  age: 18,
};

console.log(person.name); // 输出 "Tom"
person.sayHello(); // 输出 "Hello, my name is Tom"
```

变量`person`后面的值，用大括号`{}`包裹起来的部分就是对象。

对象里面的内容的结构是一个值和这个值的名字。

比如，`Tom`，是一个值，这个值是谁的值呢？是`name`的值，"name"是名字的意思。

"person"是人的意思，也就是人有一个属性叫`name`名字，`name`的值是`Tom`，也就是这个人叫"Tom"。

这种成对儿出现的属性名和属性值，称为键值对。

键值对，不光可以是字符串，也可以是整数，甚至是另一个对象。

## 1.创建对象

类似数组。直接将一个空对象或者有内容的对象赋值给一个变量或者使用对象的构造函数创建一个对象。

```js
obj = {name:"Mumu", age:30}
obj2 = new Object
```

`new Object`就是对象的构造函数。可以再回忆一下数组的构造函数`new Array`。

## 2.读取对象中的元素

还记得怎么读取数组中的元素吗？

我们使用了数组元素的序号来读取数组中的元素，比如`arr[0]`，代表的数组arr的第1个元素。

对于对象，同样可以使用这个方法，比如，我们上面的例子，我们可以使用：

```js
obj["name"]
```

来读取到`obj`这个对象中的`key`为`name`的值。

出资之外，以为对象中的键值对的这种成对的关系，我们可以用键（key）读取到值（value）。

比如：

```js
obj.name
```

这个点，叫点运算符，`name`是`obj`这个对象的一个属性，通过点运算符，就可以读取到obj这个对象的name这个属性的值。

仿佛就在说，点运算符像是“的”的意思，`obj.name`就像是在说，`obj`的"name"是什么？

## 3.对象的常用操作方法

> 问：js对象的常见操作方法
>
> 答：常见的JavaScript对象操作包括：
>
> 1. 创建新对象：使用对象字面量、构造函数、Object.create()等方式创建新对象。
> 2. 添加属性和方法：使用“.”或“[]”运算符和赋值操作符添加新属性和方法。
> 3. 删除属性和方法：使用delete运算符删除对象中的属性和方法。
> 4. 访问对象属性和方法：使用“.”或“[]”运算符访问对象中的属性和方法。
> 5. 遍历对象属性和方法：使用for-in循环遍历对象中的所有属性和方法。
> 6. 比较对象：使用==或===运算符比较对象是否相等。
> 7. 共享对象：对象是引用类型，可以通过引用在代码中共享和传递。
> 8. 继承对象：使用原型链实现对象间的继承关系。

## 4.作业

传送门：[作业：1-13-对象](https://github.com/coding-newbies-group/programming-co_creation-docs/issues/156)


完成以下操纵对象的操作，将代码和代码执行结果作为作业提交。

创建一个空对象，即不包含任何内容的对象。

### 4.1.添加属性

### 4.2.删除属性

### 4.3.遍历对象

思考题：

为什么遍历获取对象的`key`的`value`是通过`obj[key]`，而不是通过`obj.key`?



## 5.参考代码

```js
obj = {};
console.log("空数组：", obj);

//添加元素
obj.name = "Mumu";
console.log("添加元素后：", obj);

//删除元素
delete obj.name;
console.log("删除元素后：", obj);

//遍历对象
obj2 = { name: "Mumu", age: 30 };
console.log("遍历对象Obj2：", obj2);
for (const key in obj2) {
  console.log("key:", key); //通过for in遍历key
  const value = obj2[key]; //通过key获取value
  console.log("value:", value);
}
```
