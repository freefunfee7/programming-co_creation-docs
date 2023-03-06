# 程序中的文档

你在调用函数的时候，你像是函数这个产品的用户。

而你写一个函数，像是做一个产品，这个产品将来可能会被很多用户使用——包括你自己。

产品，就应该有产品说明书，别人用得着，你自己也用得着——很久之后的你，很可能把当初的各种来龙去脉忘得一干二净，所以也同样需要产品说明书，别看那产品曾经是你自己设计的。

产品说明书最好能说明这个函数的输入是什么，输出是什么，以及需要外部了解的实现细节（如果有的话，比如基于什么论文的什么算法，可能有什么局限等等）。

但是程序员往往都很懒，写程序已经很烧脑和费时间了，还要写说明书，麻烦；而且还有个最大的问题是程序和说明书的同步，程序和家电不一样，经常会改，一旦改了就要同步说明书，这真是有点强人所难了。

Python 在这方面很用功，把函数的“产品说明书”当作语言内部的功能，也就是你在写代码的时候给每个函数写一点注释，只要这些注释按照某种格式去写，调用函数的人就可以方便的看到这些注释，还能用专门的工具自动生成说明书文档，甚至发布到网上去给你的用户（其他程序员）看。这并不是 Python 发明的方法，但 Python 绝对是主流语言里做的最好的之一，Python 社区提供了不是一个，而是好几个这类文档工具，比较流行的有 [Sphinx](http://www.sphinx-doc.org/en/master/index.html)、[pdoc](https://github.com/BurntSushi/pdoc)、[pydoctor](https://github.com/twisted/pydoctor) 和 [Doxygen](http://www.doxygen.nl/) 几个。

这些工具各有优劣，一般来说对小型项目 [pdoc](https://github.com/BurntSushi/pdoc) 是最好最快的解决方案，而对比较复杂的项目 [Sphinx](http://www.sphinx-doc.org/en/master/index.html) 是公认的强者。

## Docstring 简介

*Docstrings* 就是前面我们提到的“有一定格式要求的注释”，我们可以在我们书写的函数体最开始书写这些文档注释，然后就可以使用内置的 `help()` 函数，或者 *function* 类对象的 `__doc__` 这个属性去查到这段注释。

我们看看下面这个例子，这是判断给定参数是否素数的一个函数：

> 你会发现我们的例子经常换，似乎没啥必要的情况下也会用一些没写过的东西做例子，其实这都是小练习，我们强烈的鼓励你对每一个这样的例子，除了理解我们正在讲的东西（比如这一章讲函数文档），也要顺便搞清楚这个例子里其他东西，比如怎么判断一个数是不是素数之类的。编程是一门手艺，越练越精。


```python
from math import sqrt

def is_prime(n):
    """Return a boolean value based upon whether the argument n is a prime number."""
    if n < 2:
        return False
    
    if n in (2, 3):
        return True

    for i in range(2, int(sqrt(n)) + 1):
        if n % i == 0:
            return False
        
    return True
```


```python
is_prime(23)
```




    True




```python
help(is_prime)
```

    Help on function is_prime in module __main__:
    
    is_prime(n)
        Return a boolean value based upon whether the argument n is a prime number.


​    

如上所示，*docstring* 是紧接着 `def` 语句，写在函数体第一行的一个字符串（写在函数体其他地方无效），用三个单引号或者双引号括起来，和函数体其他部分一样缩进，可以是一行也可以写成多行。只要存在这样的字符串，用函数 `help()` 就可以提取其内容显示出来，这样调用函数的人就可以不用查看你的源代码就读到你写的“产品手册”了。


```python
is_prime.__doc__
```




    'Return a boolean value based upon whether the argument n is a prime number.'



后面我们会学到，函数也是一种对象（其实 Python 中几乎所有东西都是对象），它们也有一些属性，比如 `__doc__` 这个属性就会输出函数的 *docstring*。

## 书写 Docstring 的基本原则

规范，虽然是人们最好遵守的，但其实通常是很多人并不遵守的东西。

既然学，就要**像样**——这真的很重要。所以，非常有必要认真阅读 Python [PEP 257](https://www.python.org/dev/peps/pep-0257/)，也就是 Python 社区关于 *docstring* 的规范。

简要总结一下 PEP 257 中所强调的部分要点：

* 无论是单行还是多行的 *docstring*，一概使用三个双引号括起来；
* 在 *docstring* 前后都不要有空行；
* 多行 *docstring*，第一行是概要，随后空一行，再写其它部分；
* 书写良好的 *docstring* 应概括描述以下内容：参数、返回值、可能触发的错误类型、可能的副作用，以及函数的使用限制等。

类定义也有相应的文档规范建议，你可以阅读 [PEP 257](https://www.python.org/dev/peps/pep-0257/) 文档作为起步，并参考 [Sphinx](https://sphinx-rtd-tutorial.readthedocs.io/en/latest/docstrings.html)、[numpy/scipy](https://numpydoc.readthedocs.io/en/latest/format.html)（这是非常著名的 Python 第三方库，我们以后会讲）和 [Google](https://sphinxcontrib-napoleon.readthedocs.io/en/latest/example_google.html) 的文档来学习大而规范的项目文档通常是怎么要求的。

而关于 *docstring* 的内容，需要**格外注意**的是：

> *Docstring* 是**写给人看的**，所以，在复杂代码的 *docstring* 中，写清楚 **why** 要远比写 *what* 更重要，因为 *what* 往往可以通过阅读代码来了解，而 *why* 就要难很多。你先记住这点，以后的体会自然会不断加深。

## 文档生成工具简介

前面提到过，除了 `help` 和 `__doc__` 这样的内置函数和属性可以方便我们使用 *docstring*，还有一系列文档工具可以从源代码中的注释自动生成在线文档。

这些工具通常需要在一个项目中做好约定才能顺利使用起来，而且有自己一套对 *docstring* 的内容与格式要求，比如 [Sphinx](http://www.sphinx-doc.org/en/master/index.html) 标准的 *docstring* 写出来大致是这个样子的：

```python
class Vehicle(object):
    '''
    The Vehicle object contains lots of vehicles
    :param arg: The arg is used for ...
    :type arg: str
    :param `*args`: The variable arguments are used for ...
    :param `**kwargs`: The keyword arguments are used for ...
    :ivar arg: This is where we store arg
    :vartype arg: str
    '''


    def __init__(self, arg, *args, **kwargs):
        self.arg = arg

    def cars(self, distance, destination):
        '''We can't travel a certain distance in vehicles without fuels, so here's the fuels

        :param distance: The amount of distance traveled
        :type amount: int
        :param bool destinationReached: Should the fuels be refilled to cover required distance?
        :raises: :class:`RuntimeError`: Out of fuel

        :returns: A Car mileage
        :rtype: Cars
        '''  
        pass
```

这种格式叫做 *reStructureText*，通过 Sphinx 的工具处理之后可以生成在线文档，差不多是[这个样子](https://simpleble.readthedocs.io/en/latest/simpleble.html#the-simplebledevice-class)。

通过插件 Sphinx 也能处理前面提过的 Numpy 和 Google 的格式文档。

## 小结

* Python 提供内置的文档工具来书写和阅读程序文档；
* 对自己写的每个函数和类写一段简明扼要的 *docstring* 是培养好习惯的开始；
* 通过扩展阅读初步了解 Python 社区对文档格式的要求。
