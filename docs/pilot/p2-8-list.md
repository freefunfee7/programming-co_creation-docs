# 列表

有了前一章关于 *iterable* 和 *iterator* 的知识基础，这一章开始正式学习**数据容器**（*data container*）。

顾名思义，数据容器就是可以装其他数据的数据，Python 有四种基本的数据容器：**列表**（*list*）、**元组**（*tuple*）、**字典**（*dictionary*）、**集合**（*set*），这些容器有共性也有差异，它们都是 *iterable*，所以对这些容器要迭代遍历里面的元素是非常简单的，方法也几乎一样。我们会首先重点介绍**列表**（*list*），然后介绍另外三个，共性的部分基本都会在这一章介绍，后面就重点讲另外三个各自的特色了。

## 列表简介

Python 的列表（*list*），很多编程语言里称为数组（*array*），是最常用的的数据结构之一。有序性是列表的最核心特征，也就是说：
1. 列表里的元素排列是有序的，每个元素有唯一的序号；
2. 第一个元素的序号为 0，第二个序号是 1，依次递增；
3. 可以使用序号来读写指定位置的元素。

Python 的列表中可以是任何类型的数据，一个列表里甚至可以有不同类型的数据，不过一般情况下同一个列表里的数据类型应该是相同的，所以我们可以有整数列表、浮点数列表、字符串列表等等，也可以有列表的列表（列表里的每个元素是一个列表），还可以有我们自定义对象组成的列表。

## 创建列表

Python 提供了若干种方法来创建列表，最基本的就是创建一个空的列表：


```python
lst = []
```

要给列表一些初始值的话，就用方括号括起来就行了：


```python
lst = [3, 1, 2]
```

列表拥有类型 `list`，这是一个内置的类，有很多定义好的方法给我们使用：


```python
type(lst)
```




    list



Python 还提供了一个函数 `list()` 来将其他数据类型转换为列表，可以被转换的数据类型包括：
* **string**：之前我们就说过字符串其实就是字符列表，`list()` 会把字符串转换为其字符组成的数组；
* **tuple**：后面我们会介绍的一种数据容器，和列表其实很像，区别在于 *tuple* 一旦建立其值不可以再更改（*list* 则可以）；
* **set**：后面我们会介绍的一种数据容器，里面没有重复元素，`list()` 会把其中所有元素取出来组成一个列表，顺序是不可预期的（因为 *set* 中的元素是没有顺序概念的）；
* **dict**：后面我们会介绍的一种数据容器，里面每个元素都是一个 *key-value* 对，就是一个值（value）和它的名字（key），`list()` 会把其中所有的 keys 取出来组成一个列表，顺序是不可预期的（因为 *dictionary* 中的元素是没有顺序概念的）；

事实上任何“**可迭代**（*iterable*）”的对象都可以被转换为 *list*，这也是为啥我们要先学 [iterable](p2-7-iterable-iterator.ipynb) 的原因之一。

下面是 `list()` 的例子。


```python
# 不带参数调用 list() 返回空列表
print(list())
# 输入字符串，list() 返回字符组成的列表
print(list("aeiou"))
# 输入 tuple（和列表的区别在于不是用方括号而是圆括号），list() 返回元素及排序严格一样的列表
print(list(('a', 'e', 'i', 'o', 'u')))
# 输入 set（用花括号括起来的一组元素），返回其元素组成的列表，顺序不可预测
print(list({'a', 'e', 'i', 'o', 'u'}))
# 输入 dictionary（用花括号括起来的一组 key-value 对，每个元素是 “key: value” 的格式）
# list() 会返回其所有 key 组成的列表，顺序不可预测
print(list({'a': 1, 'e': 2, 'i': 3, 'o':4, 'u': 5}))
```

    []
    ['a', 'e', 'i', 'o', 'u']
    ['a', 'e', 'i', 'o', 'u']
    ['a', 'u', 'o', 'e', 'i']
    ['a', 'e', 'i', 'o', 'u']


`list()` 可以把任何 *iterable object*——包括我们要讲的四种数据容器，还有任何迭代器和生成器——转换为列表，比如下面这个来自[上一章](p2-7-iterable-iterator.ipynb)中的例子：


```python
from itertools import islice

def fib():
    prev, curr = 0, 1
    while True:
        yield curr
        prev, curr = curr, prev + curr

f = fib()
list(islice(f, 0, 10))
```




    [1, 1, 2, 3, 5, 8, 13, 21, 34, 55]



Python 本身也内置了不少生成器帮我们快速生成列表，比如我们已经熟悉的[等差数列](https://zh.wikipedia.org/wiki/%E7%AD%89%E5%B7%AE%E6%95%B0%E5%88%97)生成器 `range()`，输出的是 *iterable*，所以也可以很方便的变成列表：


```python
# range(n) 生成 0 开始但小于 6（不包含）的整数列表：
print(list(range(6)))
# range(n) 生成 3 开始但小于 10（不包含）的整数列表：
print(list(range(3, 10)))
# range(n) 生成 3 开始但小于 10（不包含）、相邻两数相差 2 的整数列表：
print(list(range(3, 10, 2)))
# range(n) 生成 3 开始但小于 20（不包含）、相邻两数相差 4 的整数列表：
print(list(range(3, 20, 4)))
```

    [0, 1, 2, 3, 4, 5]
    [3, 4, 5, 6, 7, 8, 9]
    [3, 5, 7, 9]
    [3, 7, 11, 15, 19]


## 访问列表元素和列表长度

创建列表之后就可以用列表中元素的序号来访问元素，这个序号一般称为“下标（*index*）”，语法如下：


```python
lst = [3, 1, 2]
```


```python
print(lst[0], lst[1], lst[2])
```

    3 1 2



```python
# Python 允许使用负的序号，-1 表示从最后开始倒数第一个元素，-2 则是倒数第二个，依此类推
print(lst[-1], lst[-2])
```

    2 1



```python
# 使用列表序号不仅可以读取指定数据也可以写入，而且可以写入不同类型的数据（虽然一般不推荐这么做）
lst[2] = 'foo'
print(lst)
```

    [3, 1, 'foo']



```python
# len() 函数返回列表的长度，也就是列表中元素的个数
print(len(lst))
```

    3


列表的下标从 0 开始，最大的下标应该是 `len(lst)-1`，如果访问超出列表下标的元素（无论读写）都会产生 `IndexError` 运行时异常，这叫“下标越界（*out of range*）”，这也是最容易出现的程序错误之一。

## 列表切片

我们可以非常简单地从已有列表中切出一段子列表出来，在列表下标中使用 `[m:n]`（m 和 n 都是列表合法整数下标）即返回列表从 `m` 到 `n-1` 的这一段组成的子列表，这个机制一般称为“切片（*slicing*）”。

注意这个语法和上面用下标 `[n]` 访问列表元素很接近，但是含义完全不一样，`[n]` 返回的是列表里的一个元素，类型是该元素的类型；而 `[m:n]` 返回的是一个列表，类型是列表，哪怕里面只有一个元素也如此。


```python
nums = list(range(7)) # => [0, 1, 2, 3, 4, 5, 6]
```


```python
# 切出下标为 2 到 4 的子列表，由于列表的下标是从 0 开始的，所以这个相当于取出了列表第 3 到第 5 的元素
print(nums[2:5])
```

    [2, 3, 4]



```python
# 切出下标为 3 及以后的子列表
print(nums[3:])
```

    [3, 4, 5, 6]



```python
# 切出下标为 3 以前的子列表
print(nums[:3])
```

    [0, 1, 2]



```python
# 从头到尾，其实就是整个列表了，不过切片返回的总是一个新的列表而不是原先的列表，所以这会建立一个原列表的副本
print(nums[:])
```

    [0, 1, 2, 3, 4, 5, 6]



```python
# 同样的，负数下标也可以，:-1 就是倒数第一个之前（不包含倒数第一个）的所有元素
print(nums[:-1])
```

    [0, 1, 2, 3, 4, 5]


上面使用切片的方式会生成原来列表一个片段组成的新列表对象，原列表不会变化。

我们也可以把列表切片放在赋值语句的左边，给列表的某一个片段赋值，这样会直接替换掉原列表中指定的那个片段，比如下面的例子，就是用右边的 `[9, 12]` 替换掉原列表中的第 3 和第 4 个元素：


```python
# 如果切片出现在赋值语句左边，
nums[2:4] = [9, 12]
print(nums)
```

    [0, 1, 9, 12, 4, 5, 6]


上面这个代码其实有不少坑，我们稍微解释下。首先，左边片段的元素个数和右边列表的元素个数不必一致，比如原列表片段有两个元素，右边可以是 0 个 1 个 2 个 3 个以至任何多个元素，这个赋值是吧原片段整体替换为右边的列表，所以可能会改变原列表的元素个数，看下面两个例子：


```python
# 右边的列表元素更多，那么都会塞进去，原列表总元素数会变多
nums[2:4] = [10, 11, 12, 13]
print(nums)
```

    [0, 1, 10, 11, 12, 13, 4, 5, 6]



```python
# 右边的列表元素较少，原列表总元素数会变少
nums[2:4] = [2]
print(nums)
```

    [0, 1, 2, 12, 13, 4, 5, 6]


另一个坑是，上面这样的赋值，和单个列表元素的赋值不一样。单个元素赋值时指定的下标是不能越界的，比如上面这个列表目前是 8 个元素，最大下标为 7，`nums[8] = 20` 这样的赋值会抛出 `IndexError: list assignment index out of range` 的下标越界异常；

但切片的赋值是可以的，如果切片范围超过了最大下标，就会把右边的列表扩充到原列表的最后，比如下面的例子，`[8:]` 似乎已越界，但此语句成立，会把 `[7, 8]` 扩充到原列表最后：


```python
nums[8:] = [7, 8]
print(nums)
```

    [0, 1, 2, 12, 13, 4, 5, 6, 7, 8]


这段代码里，不仅 `[8:]` 可以，写 `[100:]` 效果也一样，都是紧跟原列表最后添加上去，不过这种做法并不易于理解，我们要在列表最后增加元素，最好用下面一节介绍的方法。

## 增加、删除元素和其他列表操作

Python 的 `list` 类里面有很多方法可以让我们对已建立的列表进行增删元素等操作。


```python
fruits = ['orange', 'apple', 'pear', 'banana']
```

`append(x)` 方法把 x 加入到列表的最后；如上节所述，下面的代码等价于 `fruits[4:] = ['coconut']`，但是要清晰很多：


```python
fruits.append('coconut')
print(fruits)
```

    ['orange', 'apple', 'pear', 'banana', 'coconut']


`append()` 有个批量版本叫 `extend(x)`，也是把 x 扩展到原列表最后，但这里 x 不是一个元素，而是一组元素，任何 *iterable* 都可以：


```python
fruits.extend(['apple', 'banana'])
print(fruits)
```

    ['orange', 'apple', 'pear', 'banana', 'coconut', 'apple', 'banana']


`insert(i, x)` 把 x 插入到列表下标 i 的元素之前（成为新的下标 i 的元素），比如 `i = 0` 就会插入到原列表最前面：


```python
fruits.insert(0, 'kiwi')
print(fruits)
```

    ['kiwi', 'orange', 'apple', 'pear', 'banana', 'coconut', 'apple', 'banana']


`pop()` 删除原列表最后一个元素并返回该元素，原列表长度减一：


```python
fruit = fruits.pop()
print(fruit, 'popped')
print(fruits)
```

    banana popped
    ['kiwi', 'orange', 'apple', 'pear', 'banana', 'coconut', 'apple']


`remove(x)` 删除列表中第一个出现的值等于 x 的元素，如果整个列表里都没有等于 x 的元素，会扔出一个 `ValueError` 运行时异常


```python
fruits.remove('apple')
print(fruits)
```

    ['kiwi', 'orange', 'pear', 'banana', 'coconut', 'apple']


方法 `reverse()` 将整个列表中的元素排列顺序倒转，注意这个方法是直接修改原列表的：


```python
fruits.reverse()
print(fruits)
```

    ['apple', 'coconut', 'banana', 'pear', 'orange', 'kiwi']



```python
# clear() 删除列表中的所有元素，还原为一个空列表 []
fruits.clear()
print(fruits)
```

    []


列表里每个元素都有下标，我们可以把上面的操作分成两类：
* 一类是在列表尾部进行的操作，不影响绝大部分元素的下标，包括 `append`、`extend`、`pop` 这几个；
* 另一类是其他的，它们会在列表的中间增加或者删除元素，那么在操作点之后的所有元素下标都要改变。

一般来说前一类的性能会很好，后面一类要做的处理更多，性能会差一些，尤其是在很大很大的列表中会更明显。

## 查找和排序


```python
fruits = ['kiwi', 'orange', 'apple', 'pear', 'banana', 'coconut', 'apple', 'banana']
```

`index(x)` 查找列表中值为 x 的第一个元素并返回其下标，如果没找到会扔出一个 `ValueError` 运行时异常：


```python
fruits.index('apple')
```




    2



`index(x)` 还可以指定查找范围，用起始和结束下标表示，包括起始下标但不包括结束下标；如果不指定结束下标，则查找到列表尾：


```python
fruits.index('apple', 4)
```




    6



`count(x)` 返回列表中出现值为 x 的元素个数，如果没有找到则返回 0：


```python
fruits.count('apple')
```




    2




```python
fruits.count('grape')
```




    0



列表还有个重要的操作，就是根据列表里元素的值来排序，`sort()` 方法就是用来做排序的，最简单的例子什么参数也不用，`sort()` 会自动按照缺省逻辑从小到大排序，所谓缺省逻辑就是用 Python 内置的 `<` `>` 操作符的逻辑，对字符串来说是字母顺序，对数字来说是大小顺序：


```python
# sort() 将整个列表排序
fruits.sort()
print(fruits)
```

    ['apple', 'apple', 'banana', 'banana', 'coconut', 'kiwi', 'orange', 'pear']


`sort()` 允许定制这个大小判断的逻辑，输入一些参数来定制排序的规则，实现非常个性化的效果。在介绍这些定制参数之前我们先来看看排序这个事情的本质。

排序，最终的结果是列表里的元素按照**一定的顺序**排列，那么怎么定义**一定的顺序**呢？要么从小到大排（正序，下面的解说如果没有特别说明，都以正序为例），要么从大到小排（逆序），所以我们做排序之前必须先定义元素间“大小规则”，即随便拿两个元素出来，能分出大小，只有这样才能进行有意义的排序。

我们以后会看到越来越多的例子，要排序的东西是各种各样的，其大小关系并不都是那么直观和显而易见的。比如要对一组人排序，怎么定义两个人之间谁大谁小呢？年龄？身高？体重？工作年限？职位？这就需要我们根据需要去选择和定义。

定义好大小比较的规则之后，计算机可以按照一定的**算法**来进行排序的操作。排序算法是非常好的编程入门练习，有很多经典的排序算法，这些算法本质都一样：**比较和移动**。

以正序排序为例：选取两个元素 `a` 和 `b`，按照我们实现约定好的大小规则作比较，如果 `a <= b` 那么 `a` 应该在前面，`b` 在后面（有少数场景下 `a == b` 的情况需要特别处理，我们暂时不考虑这种情况）；反之如果 `a > b`，`a` 应该在后面，`b` 应该在前面；然后对元素做相应的移动。

各种算法的差别，在于选取元素和移动元素的策略，随着策略的不同，进行比较和移动的次数会有巨大差异，那么运行的性能（占用的内存和消耗的时间）都会有明显的差别。有兴趣的话可以看看各种排序算法可视化的演示，比如 [这个](http://sorting.at/)、[这个](https://www.cs.usfca.edu/~galles/visualization/ComparisonSort.html)、或者 [这个](https://www.toptal.com/developers/sorting-algorithms)，进而可以找本讲算法的入门教材来看看这些算法的实现，以及怎么评估它们在不同情况下的“**时间复杂度**”。

上面说的可以有空时候去研究，回到我们目前的问题，关键是理解一点：排序的结果由我们定义的“大小规则”决定。比如我们不想按照字母顺序，而想按照每个词包含字母的多少来排序，可以这么写：


```python
fruits.sort(key=len)
print(fruits)
```

    ['kiwi', 'pear', 'apple', 'apple', 'banana', 'banana', 'orange', 'coconut']


我们给 `sort()` 方法传入了一个参数名为 `key` 的参数，这个参数是一个**函数**，我们在前面提到过，**函数也是数据**，这里是一个例子：函数也可以作为参数传递。传进来的函数是内置的 `len()` 函数，这么做的效果是：排序的大小规则由这个 `len()` 函数的返回值大小来决定：任取列表中两个元素 `a` `b`，如果 `len(a) < len(b)` 那么 `a` 就比 `b` “小”（这个小是我们特别定义的，不是常规概念的“小”）；反之如果 `len(a) > len(b)` 那么 `a` 就比 `b` 大。

`sort()` 方法还可以传一个布尔型参数 `reverse` 进去，告诉它要不要逆序排列（大的在前小的在后），`reverse` 缺省值是 `False`，即不用逆序，就正序排列，如果传入 `reverse=True`，`sort()` 方法会按逆序排列：


```python
fruits.sort(key=len, reverse=True)
print(fruits)
```

    ['coconut', 'banana', 'banana', 'orange', 'apple', 'apple', 'kiwi', 'pear']


继续说 `sort()` 的这个 `key` 参数，我们不仅可以传给它一个内置函数（如 `len()`），还可以传自定义的函数，比如我们想按照词的最后一个字母的字母表顺序排序，我们可以这么写：


```python
def last_letter(s):
    return s[-1]

fruits.sort(key=last_letter)
print(fruits)
```

    ['banana', 'banana', 'orange', 'apple', 'apple', 'kiwi', 'pear', 'coconut']


这个 `last_letter` 函数太简单了，而且就用于排序，我们可以简单地将其用 `lambda` 写成一个**匿名函数**：


```python
fruits.sort(key=lambda s:s[-1])
print(fruits)
```

    ['banana', 'banana', 'orange', 'apple', 'apple', 'kiwi', 'pear', 'coconut']


效果完全一样，如果你不记得 `lambda` 可以回去重新看看前面的[相关章节](p2-5-functional-1.ipynb)。

## 列表的遍历

遍历（*traverse*）是指不重不漏地访问数据容器中的每个元素各一次，由于列表是一种有序容器，遍历通常暗含着要按照列表顺序访问的要求。而“访问”的含义是取得元素并做相应的处理。

Python 提供两种对列表进行遍历的方式，一种是我们前面已经熟悉的 `for...in` 循环，实际上对任何 *iterable* 都适用；另外一种称为 *list comprehension*，在某些情况下更加简洁和清晰。先来看看 `for...in` 循环。


```python
animals = ['cat', 'dog', 'monkey']
```


```python
for animal in animals:
    print(animal)
```

    cat
    dog
    monkey


这个 `for...in` 还有另一种写法，可以带着列表的下标做操作，需要用到 Python 内置函数 `enumerate()`：


```python
enumerate(animals).__next__()
```




    (0, 'cat')




```python
# 另一种写法允许我们连带取得每个元素的下标
for index, animal in enumerate(animals):
    print(f'#{index}: {animal}')
```

    #0: cat
    #1: dog
    #2: monkey


`enumerate()` 函数接受一个 *iterable* 对象，然后会调用其迭代器的 `__next__()` 方法，每次调用会把一个序号和 `__next__()` 返回结果绑在一起，形成一个元素，然后用这些绑着序号的元素生成一个新的迭代器返回，比如 `enumerate(animals)` 会生成下面这个序列的迭代器：

`(0, 'cat'), (1, 'dog'), (2, 'monkey')`

于是上面的 `for...in` 循环每次取出一个 `(0, 'cat')` 这样的元素，然后把里面两个值分别赋给循环变量 `index` 和 `animal`。

小括号括起来的 `(0, 'cat')` 这种数据叫**元组**（*tuple*），我们会在下一章介绍。

### List Comprehension

下面我们来看看 *list comprehension*，这实际上是一种列表变换，就是把一个列表变成另外一个列表，变换后列表的元素是原列表元素按照我们指定算法操作得来的。比如我们有一个人名的列表，现在希望把里面所有的人名都变成全小写，如果采用循环的方式我们会这么写：


```python
names = ['Neo', 'Trinity', 'Morpheus', 'Smith']
```


```python
lowercased = []
for name in names:
    lowercased.append(name.lower())
print(lowercased)
```

    ['neo', 'trinity', 'morpheus', 'smith']


运用 *list comprehension* 的写法可以更加简洁：


```python
lowercased = [name.lower() for name in names]
print(lowercased)
```

    ['neo', 'trinity', 'morpheus', 'smith']


上面这个简单的例子可以看出，*list comprehension* 是从一个列表的元素出发构造另一个列表的方法，其格式为：

`lst2 = [expr_of_x for x in lst1]`

含义是“对列表 `lst1` 里的每个元素 `x` 计算表达式 `expr_of_x` 的值并将结果依序组成列表 `lst2`”。

有的时候我们并不需要对所有原来列表里的元素都进行变换，所以上面这个格式还可以带个条件尾巴，变成这样：

`lst2 = [expr_of_x for x in lst1 if cond]` 

含义和上面一样但多了个条件：只对 `cond` 表达式值为 `True` 的那些元素进行处理，其他那些就直接忽略了。或者更加通用的：

`lst2 = [expr_of_x1 if cond else expr_of_x2 for x in lst1 ]`

含义是：对 `cond` 表达式值为 `True` 的那些元素 `x` 计算表达式 `expr_of_x1` 的值，否则计算表达式 `expr_of_x2` 的值并放入新列表。下面有个例子：


```python
lowercased_good = [name.lower() if name != 'Smith' else name.upper() for name in names]
print(lowercased_good)
```

    ['neo', 'trinity', 'morpheus', 'SMITH']


*List comprehension* 是源自函数式抽象的一种强大而优雅的工具，以后我们会看到更多的应用实例。

## 容器与函数参数

前面我们[提到过](p2-1-function-def.ipynb)，全局变量和局部变量是彼此隔离的，即使名字一样也是完全不同的东西，就像下面的代码，函数体内的 n 和外面的 n 其实完全是两个变量，函数体内对 n 执行加一，但外面的全局变量 n 不会改变：


```python
def inc(n):
  n += 1

n = 0
inc(n)
print(n)
```

    0


当我们用一个数据容器做函数的输入参数时，情况会变得复杂起来。我们来看下面的例子：


```python
def reassign(l):
  l = [0, 1, 2, 3]

def append(l):
  l.append(3)

def modify(l):
  l[1] = 100

l = [0, 1, 2]
reassign(l)
print(l)
append(l)
print(l)
modify(l)
print(l)
```

    [0, 1, 2]
    [0, 1, 2, 3]
    [0, 100, 2, 3]


`reassign` `append` `modify` 三个函数都接受一个列表输入，我们发现 `reassign` 没有改变全局变量 `l`——这很好理解，因为函数体内给局部变量 `l` 重新赋值，影响的是局部变量 `l`，全局变量 `l` 是另一个东西，自然不受影响，和前面我们学过的知识点完全吻合。

但是 `append` `modify` 确实改变了全局变量 `l` 的内容，分别给它增加了一个元素，以及修改了一个元素的值，这又是怎么回事呢？

原来，当数据容器（其实对任何对象都如此，数据容器也是对象）作为函数输入参数时，传递的是一种“**引用**（*reference*）”，你可以理解为是对同一个对象的不同名字。按照这个思路我们来详细分析下上面的三个函数。

**reassign(l)**

`l = [0, 1, 2]` 创建了一个全局变量 `l`，并且赋值，这时候内存里有了 `[0, 1, 2]` 这样的一个列表对象，全局变量 `l` 是指向它的一个名字。

当我们调用 `reassign(l)` 时首先吧小括号里的参数值求出来，就是内存里的那个  `[0, 1, 2]` 对象，匹配到函数 `reassign()` 的唯一参数 `l`，这个 `l` 是函数 `reassign` 内的局部变量（所有函数参数都是局部变量），于是局部变量 `l` 和全局变量 `l` 目前都指向内存里同一个对象。

然后函数内执行 `l = [0, 1, 2, 3]`，这一赋值语句实际上在内存中创建了另外一个列表对象 `[0, 1, 2, 3]`，并让局部变量 `l` 指向它，注意这时候全局变量没有动，还是指向原先的那个 `[0, 1, 2]` 对象，所以第一个 `print(l)` 语句输出的还是 `[0, 1, 2]`（全局变量 `l` 指向的对象）。

这个过程和前面输入参数为整数的情形是完全一样的，只要理解了全局变量 `l` 和局部变量 `l` 是两个变量就可以理解。

**append(l)**

前面都一样，函数调用发生时，局部变量 `l` 和全局变量 `l` 都指向内存里同一个 `[0, 1, 2]` 对象。

然后函数内执行 `l.append(3)`，并没有新的对象创建，这个 `append()` 就是针对上述 `[0, 1, 2]` 对象操作的，于是内存中的这个对象在尾部增加了一个元素，变成了 `[0, 1, 2, 3]`。注意，全局变量 `l` 也是指向这个对象的。

所以第二个 `print(l)` 语句输出了变化之后的对象，变成了 `[0, 1, 2, 3]`。

**modify()**

同 `append(l)` 理，不再赘述。

所以，在[前面关于函数和变量作用域的规则](p2-1-function-def.ipynb)基础之上，我们要再补充下面的规则，才算完整：
* 当数据容器等对象作为函数变量是，传递的是指向内存中对象的 *reference*，对这个对象直接进行的操作会改变对象的内容。这样的操作包括修改其元素或属性值，调用其（会改变自身内容）的方法等。

注意有些对象方法本来就不会改变自身内容，而是生成一个新对象返回的，不在此列，比如列表的切片、*comprehension* 这些。

## 小结

*List* 是我们学习的第一个数据容器（可能也会是最多用到的），我们花了不少的时间来介绍了列表的特性：有序、*iterable*，进而学习了列表的各种操作：
* 创建列表的方法，尤其是用 `list()` 可以把任何 *iterable* 转换为列表；
* 用 `len()` 可以获得列表的长度，用 `lst[n]` 可以访问下标 `n` 的元素，注意下标是从 `0` 开始的；
* 用 `lst[n1:n2]` 获得列表下标 `n1` 到 `n2-1` 的**切片**（*slice*）；需要格外留意给切片赋值的语法和效果；
* 了解列表增加和删除元素等操作，注意在列表尾部增加删除元素开销较小；
* 了解在列表中查找元素和对列表做排序的方法，注意函数对象和匿名函数在 `sort()` 方法中的应用；
* 熟悉列表遍历的两种方法，尤其是新的 *list comprehensions* 方法；注意匿名函数在 *list comprehensions* 的应用；
* 当容器或者其他对象做函数参数时，传递的是对象引用，在函数内对这个对象的操作可能影响其他指向该对象的变量。
