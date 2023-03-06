# 元组，集合，字典

在详细的学习过 *list* 之后，剩下的三种就简单多了，因为很多东西都是类似甚至一样的。这一章的重点是掌握**元组**（*tuple*）、**集合**（*set*）和**字典**（*dictionary*）的独特性以及常见的应用场景。

## Tuples

*Tuple* 是一个造出来的数学名词，中文可以翻译为”元组“，几乎没有意义。

*Tuple* 和 *list* 非常类似，实际上 *tuple* 和 *list* 都属于序列（*sequence*）类型的数据结构，即包含的元素是有序的。

在 Python 中 *tuple* 用圆括号括起来表示（区别于 *list* 的方括号）。


```python
t1 = (1, 24, 'hello')
print(t1)
```

    (1, 24, 'hello')



```python
type(t1)
```




    tuple



其实圆括号可以省略，下面这句和上面的完全等价：


```python
t2 = 1, 24, 'hello'
print(t1, t2)
```

    (1, 24, 'hello') (1, 24, 'hello')


注意，*tuple* 只有一个元素时，最后必须跟一个逗号，这和 *list* 是不同的：


```python
t3 = (1,)
print(type(t3))
```

    <class 'tuple'>


与 *list* 类似，*tuple* 也用下标来访问其中元素：


```python
t1[0]
```




    1



*Tuple* 里的元素本身也可以是 *tuple*，也就是嵌套的 *tuple*：


```python
t = t1, (1, 2, 3)
print(t)
```

    ((1, 24, 'hello'), (1, 2, 3))


### 不可更改（*immutable*）

*Tuple* 和 *list* 的主要区别在于，*tuple* 一经创建不可更改，这种特性叫 *immutable*，这是一种非常苛刻的要求，但是也有很多好处，最主要一点是降低了程序的复杂度，让程序运行更加可控，测试和验证更容易。

相反的，*list* 是 *mutable*。

如果我们尝试修改 *tuple* 的元素，会出现 `TypeError` 运行时异常（表示访问了特定类型不支持的属性或者方法），例如下面这句：


```python
# 如果我们尝试更改 *tuple* 中的元素，会抛出异常
# t[0] = 888
```

需要注意，*tuple* 包含哪些元素及其顺序不可以改变，但 *tuple* 包含的某个元素可能是 *mutable*，其内容是可以改变的，请看下面的例子：


```python
a = [1, 2]
b = [4, 5]
t = a, b
print(t)
a.append(3)
print(t)
```

    ([1, 2], [4, 5])
    ([1, 2, 3], [4, 5])


上面这一点有点绕，但也很重要，请务必理解清晰。

### Pack 和 Unpack

*Tuple* 和 *list* 很像，而且也是 *iterable*，所以我们在上一章章节讲的很多内容也适用于 *tuple*，当然，元素的增删改和排序等操作除外。

虽然很像，但通常 *tuple* 会用在和 *list* 不一样的场合，两个主要的差别是：
* 一般 *list* 包含的元素都是相同类型的（虽然允许是不同类型），而 *tuple* 没有这个约束，经常包含不同类型的元素；
* 一般 *list* 用循环和迭代方式来处理，而 *tuple* 被广泛地用在数据“打包（*pack*）”和“解包（*unpack*）”操作中。

这一节我们重点看看 *pack* 和 *unpack*。

把一组数据（不管什么类型）打包进一个 *tuple* 的操作叫 *pack*：


```python
t = 1, 24, 'hello', True
print(t)
```

    (1, 24, 'hello', True)


前面我们讲过，初始化一个 *tuple* 时可以省略括号，其实也是 *pack*。

反过来的操作就是把 *tuple* 中的元素分别赋值给几个变量，就叫 *unpack*。

注意左边的变量个数和 *tuple* 的元素个数必须严格相等，多了少了都会抛出 TypeError 异常：


```python
start, end, s, f = t
print(start, end, s, f)
```

    1 24 hello True


*Pack* 和 *unpack* 在 Python 中应用广泛，随处可见，下面是几个例子：

* 我们在[介绍函数定义的一章](p2-1-function-def.ipynb)里介绍的 *arbitrary argument*  
调用时传入的不定个数的值被打包进一个 *tuple*，在函数体中通常用 `for...in` 循环来遍历它。

* 函数的多返回值  
`return` 语句会把后面的多个返回值打包成一个 *tuple*，调用端用 `a, b = f(x)` 的语法其实就是 *unpack*。

* [上一章](p2-8-list.ipynb)中介绍的 `enumerate(iter)` 函数返回的迭代器  
里面每个元素都是一个 *tuple*，是迭代序号和 `iter` 里的元素配对组成的 *tuple*。

另外一个 *tuple* 和 *list* 的差异是：*tuple* 可用作 *dictionary* 中的 *key*，而 *list* 不可以。我们下面讲 *dictionary* 的时候会举例说明。

## Sets 集合

集合（*set*）基本就是数学里集合的概念：
* 集合里的元素没有顺序；
* 集合里没有重复元素。

Python 支持数学上集合的基本操作：判断一个集合是否包含某个元素（*belongs to*），两个集合之间的并集（*union*）、交集（*intersection*）、差集（*difference*）和对称差集（*symmetric difference*）。

### 创建集合和基本操作

Python 中的集合是用花括号括起来的一组值，一般情况下是相同类型的值。如果我们创建集合时指定了重复的值，Python 解释器会自动去重：


```python
fruits = {'apple', 'orange', 'apple', 'pear', 'orange', 'banana'}
print(fruits)
```

    {'banana', 'pear', 'orange', 'apple'}



```python
type(fruits)
```




    set



操作符 `in` 用于判断某个元素是否属于某个集合：


```python
'orange' in fruits
```




    True




```python
'coconut' in fruits
```




    False



集合创建后可以用 `add` `remove` 方法增加和删除元素：


```python
print('coconut' in fruits)
fruits.add('coconut')
print('coconut' in fruits)
```

    False
    True



```python
print('apple' in fruits)
fruits.remove('apple')
print('apple' in fruits)
```

    True
    False


集合的元素不可重复的特点很适合用来表示分类、关键字之类的概念。集合的实现保证了 `in` 判断效率很高，对很大的集合都能快速给出结果。

和 *list* 类似，Python 也提供了一个 `set()` 函数来把字符串、其他数据容器和任何 *iterable* 转换为集合，比如：


```python
a = set('abracadabra') # => {'a', 'r', 'b', 'c', 'd'}
b = set('alacazam') # => {'a', 'z', 'l', 'm', 'c'}
```

集合之间可以进行并集（*union*）、交集（*intersection*）、差集（*difference*）和对称差集（*symmetric difference*）四种运算。


```python
# 并集（union），集合 a 和 b 的元素合并起来的集合，只要在 a 或者 b 中的元素都存在于并集中
a | b
```




    {'a', 'b', 'c', 'd', 'l', 'm', 'r', 'z'}




```python
# 交集（intersection），集合 a 和 b 的公有元素的集合，只有既在 a 中也 b 中的元素才存在于交集中
a & b
```




    {'a', 'c'}




```python
# 差集（difference），在 a 中但不在 b 中的元素的集合
a - b
```




    {'b', 'd', 'r'}




```python
# 对称差集（symmetric difference），在 a 或者 b 中但不同时在 a 和 b 中的元素的集合
a ^ b
```




    {'b', 'd', 'l', 'm', 'r', 'z'}




```python
# 数学公式之一：a - b = a - (a & b)
a - (a & b)
```




    {'b', 'd', 'r'}




```python
# 数学公式之二：a ^ b = (a | b) - (a & b)
(a | b) - (a & b)
```




    {'b', 'd', 'l', 'm', 'r', 'z'}



### 集合的遍历

集合也是 *iterable*，所以和 *list* 类似，集合的遍历也有 for-in 循环和 *set comprehension* 两种方式。


```python
for index, fruit in enumerate(fruits):
    print(f'#{index}: {fruit}')
```

    #0: coconut
    #1: orange
    #2: banana
    #3: pear



```python
a = {x for x in 'abracadabra' if x not in 'abc'}
print(a)
```

    {'r', 'd'}


## Dictionaries 字典

Python 里的字典（*dictionary*），也叫“哈希表（*hashmap*）”，是另一种常见数据容器，其特征为：
* 其中每个元素都是一个 key-value 对，就是一个值（*value*）和它的名字（*key*）；可以通过 key 来快速定位一个元素；
* 元素排列没有顺序，甚至没有“排列”的概念，可以看做是“散落”的，我们做的操作都是通过 *key* 来进行的。

> *Dictionary* 的实际类型叫 `dict`，所以下面我们也会直接用 *dict*。

### 创建字典和读写元素

Python 中的字典是花括号括起来的 key-value 对，每个 key-value 对写作 `key: value`：


```python
pets = {'cat': 'cute', 'dog': 'furry'}
print(pets)
```

    {'cat': 'cute', 'dog': 'furry'}



```python
type(pets)
```




    dict



某种意义上我们可以把 key 看做是 *dict* 的“下标”，我们可以通过 key 来访问对应的值，也可以用 `in` 关键字来判断某个 key 是不是存在于 *dict* 中。


```python
def find_pet(name):
    if name in pets:
        print(pets[name])
    else:
        print(f'没找到宠物 {name} 的信息')

find_pet('cat')
find_pet('turtle')
```

    cute
    没找到宠物 turtle 的信息


我们可以用赋值语句直接给某个 *key* 指定 *value*：
* 如果这个 *key* 原来不存在，这个 *key-value* 对会被添加进 *dict*；
* 如果这个 *key* 原来就在 *dict* 中，则会修改对应的 *value*。

如下例：


```python
find_pet('fish')
pets['fish'] = 'wet'
find_pet('fish')

find_pet('dog')
pets['dog'] = 'brave'
find_pet('dog')
```

    没找到宠物 fish 的信息
    wet
    furry
    brave


如果要读取一个一个不存在的 *key* 对应的值，会遇到 *KeyError* 运行时异常。所以为了安全的获取 *dict* 中的元素，Python 提供了 `get()` 方法，这个方法需要两个参数，第一个是 `key`，第二个是“缺省值”，也就是如果 `key` 不存在时就会返回缺省值，而不会抛出异常：


```python
# 直接访问一个不存在的 key 会抛出异常
# print(pets['monkey'])  # KeyError: 'monkey' not a key of pets

# 所以安全的标准方法是用 get() 方法
print(pets.get('monkey', 'N/A'))
print(pets.get('fish', 'N/A'))
```

    N/A
    wet


可以用 `del` 语句类删除 `dict` 中的元素：


```python
if 'fish' in pets:
    del pets['fish']

find_pet('fish')
```

    没找到宠物 fish 的信息


### 字典的遍历

正如我们前面说过的 *dict* 也是 *iterable*，所以也可以用 `for...in` 和 *dict comprehension* 来遍历。

由于 *dict* 的元素是 key-value 对，所以 `for...in` 也提供了两种形式：只遍历 *key* 和遍历 *key-value* 对：


```python
d = {'person': 2, 'cat': 4, 'spider': 8}
```


```python
for animal in d:
    legs = d[animal]
    print(f'A {animal} has {legs} legs')
```

    A person has 2 legs
    A cat has 4 legs
    A spider has 8 legs



```python
for animal, legs in d.items():
    print(f'A {animal} has {legs} legs')
```

    A person has 2 legs
    A cat has 4 legs
    A spider has 8 legs


`dict` 类的 `items()` 方法返回一个列表，里面每个元素是一个 *tuple*，*tuple* 里面是两个元素，对应 *dict* 中的 *key-value* 对。所以上面第二个 `for...in` 语句可以针对 `animal`（*key*）和 `legs`（*value*）两个循环变量进行遍历。

*Dict comprehension* 的形式和其他容器也差不多，我们甚至也可以从一个 *list* 出发构造出一个 *dict*：


```python
nums = [0, 1, 2, 3, 4]
even_num_to_square = {x: x ** 2 for x in nums if x % 2 == 0}
print(even_num_to_square)
```

    {0: 0, 2: 4, 4: 16}


### Dict 和 Tuple

*Dict* 和 *tuple* 是一对好搭档，经常配合使用。

有一个很常见的设计是这样的：从某些数据源（比如调用某个函数）得到两个有关联的返回值，比如某个人的 id 及其电子邮件地址：

```python
def get_next_id_and_email():
    # 访问在线服务接口或者数据库
    # 然后返回获得的用户 id 及 email，注意下面的写法，逗号隔开的两个值，这其实是一个 tuple
    return id, email

# 可以多次调用上面的函数，得到很多包含 id 和 email 的 tuple，我们将这些 tuple 都加入一个 list
while has_more_ids:
    t = get_next_id_and_email()
    all_ids_and_emails.append(t)
    
# 最终 all_ids_and_emails 会是这样的内容：[('neo', 'neo@zion.org.net'), ('trinity', 'trinity@zion.org'), ...]
# 然后我们就可以用 dict() 函数将这个 list 转换成 dict
dict(all_ids_and_emails) # => {'neo': 'neo@zion.org', 'trinity': 'trinity@zion.org'), ...}
```

上面的伪代码是一种“设计模式（*design pattern*）”，可以应用在很多场景下，其基本逻辑是：
1. 把 key 和 value 这一对变量 `pack` 成 `tuple`；
2. 把同类的 `tuple` 放进一个 `list`；
3. 用 `dict()` 函数将 `list` 转换为 `dict`；
4. 通过 key 可以很方便高效地在 `dict` 中查出对应的 value。

另外，因为 ***dict* 的 *key* 必须是 *immutable***，所以 *tuple* 可用作 *dict* 中的 *key*，而 *list* 不可以。

举个例子。假定我们想统计学生们参加体育活动的次数，我们可以用 `(学生名，体育项目)` 作 key，而对应的参加次数作为 value，那么这个 *dict* 大致就是这样的：

```python
{('张三', '足球'): 3, ('张三', '羽毛球'): 1, ('李四', '足球'): 1, ('王五', '羽毛球'): 2, ('王五', '游泳'): 4, ...}
```

这里把 `('张三', '足球')` 换成列表 `['张三', '足球']` 是不可以的。

### Dict 与排序

*Dict* 和 *list* *tuple* 不一样，其元素没有“顺序”的概念，但是对其元素按照某种规则排序这个需求还是经常有的，这时候我们的做法是：按照规则排序，但结果输出为一个 *list*，这样才能有顺序的概念嘛。

假定我们有一个 *dict* 里面装着几种水果的价钱：


```python
prices = {'apple': 1.99, 'banana': 0.99, 'orange': 1.49, 'cantaloupe': 3.99, 'grapes': 0.39}
```

如果我们要按照 *key* 来排序：


```python
print(sorted(prices.items()))
```

    [('apple', 1.99), ('banana', 0.99), ('cantaloupe', 3.99), ('grapes', 0.39), ('orange', 1.49)]


因为 `dict` 类本身没有顺序概念，也不提供 `sort()` 方法，所以这里我们用了内置函数 `sorted()`，这个函数和上一章我们介绍的 `list` 类的 `sort()` 方法几乎一样。

另外我们这里用 *dict* 的 `items()` 方法来把 *dict* 转换成了一个 *list*（在前面 **字典的遍历** 一节介绍过），然后直接排序就可以。

> 为什么这里直接排序就可以呢？因为：
> * *dict* 的 `items()` 输出的 *list* 里每个元素都是 *tuple*；
> * *tuple* 里面两个值，第一个对应 *dict* 中的 *key*，第二个对应 *value*；
> * 在排序时比较两个 *tuple* 的大小，就是比较其第一个值的大小，即比较 *key* 的大小。
> 
> 你可以试试写几行代码来验证上面这几条事实。

如果我们要按照 *value* 来排序，那么就需要在比较上述 *tuple* 时不要比较 *tuple* 的第一个值（*key*），而是比较第二个值（*value*）：


```python
print(sorted(prices.items(), key=lambda x:x[1]))
```

    [('grapes', 0.39), ('banana', 0.99), ('orange', 1.49), ('apple', 1.99), ('cantaloupe', 3.99)]


这次我们指定的 `key` 函数是一个匿名函数，用于在比较两个 *tuple*（`lambda` 表达式中的 `x`）时以其第二个值（`lambda` 表达式中的 `x[1]`）为准。

当然我们也可以指定逆序排列，只要在 `sorted()` 函数调用时指定 `reverse=True` 即可：


```python
print(sorted(prices.items(), key=lambda x:x[1], reverse=True))
```

    [('cantaloupe', 3.99), ('apple', 1.99), ('orange', 1.49), ('banana', 0.99), ('grapes', 0.39)]


这样我们就可以对 *dict* 进行排序并把结果输出为一个列表，里面用一个个 *tuple* 来表示 *dict* 中的 *key-value* 对。

那么作为一个复习题，请你思考：假定有一个 *list*，里面每个元素都是一个 *dict*，可以怎么对其进行排序呢？

比如说，我们的系统里每个用户都有姓名、年龄、身高等信息，每个人的信息存放在类似这样的一个 *dict* 中：


```python
person1 = {'name': 'Neo', 'age': 30, 'height': 175}
person2 = {'name': 'Trinity', 'age': 29, 'height': 169}
person3 = {'name': 'Morpheus', 'age': 43, 'height': 180}
```

然后我们把所有用户的信息放在一个列表中：


```python
data = [person1, person2, person3]
```

我们可以按照姓名来排序：


```python
data.sort(key=lambda x:x['name'])
print(data)
```

    [{'name': 'Morpheus', 'age': 43, 'height': 180}, {'name': 'Neo', 'age': 30, 'height': 175}, {'name': 'Trinity', 'age': 29, 'height': 169}]


也可以按照年龄排序：


```python
data.sort(key=lambda x:x['age'])
print(data)
```

    [{'name': 'Trinity', 'age': 29, 'height': 169}, {'name': 'Neo', 'age': 30, 'height': 175}, {'name': 'Morpheus', 'age': 43, 'height': 180}]


还可以按照身高，逆序排序（从高到低）：


```python
data.sort(key=lambda x:x['height'], reverse=True)
print(data)
```

    [{'name': 'Morpheus', 'age': 43, 'height': 180}, {'name': 'Neo', 'age': 30, 'height': 175}, {'name': 'Trinity', 'age': 29, 'height': 169}]


这种情况颇为常见，叫做“按照对象的某个属性排序”，有时候我们用 *dict* 来表示一个对象，有时候用自定义类的实例来表示，都可以用上面这样的方法来排序。

## 小结

在系统学习了 *iterable* 和 *list* 的基础上，这一章介绍了其余三个数据容器：**元组**（*tuple*）、**集合**（*set*）和**字典**（*dict*）的独特性及各自的应用场景。
* *Tuple* 是元素不可更改（*immutable*）的有序容器，广泛用于打包和解包（比如调用函数时的参数和返回值）；
* *Set* 是元素具有不重复性的无序容器，可以进行各种集合的操作；
* *Dict* 是带有名字（*key*）的值（*value*）的无序容器，有一些比较独特的用法。

上一章和这一章介绍的四种数据容器都是非常常用的，所以请你务必要把这两章的内容扎实的读懂、搞清，对所有的例子极其背后的原理充分理解和熟悉。
