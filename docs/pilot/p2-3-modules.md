# 模块

其实我们前面已经多次引用过 Python 的各种非内置模块，也熟悉了两种引入模块中函数或变量的语法：
* `import xxxx`：引入 `xxxx` 模块，可以用 `xxxx.func()` 的语法调用其中的函数（变量也类似）；
* `from xxxx import func1, func2`：引入 `xxxx` 模块中的若干函数或变量，可以直接用不带模块名的 `func1` `func2` 来使用。

不过到目前为止我们都是使用现成模块（Python 自带或者我们通过 `pip install` 命令安装的第三方模块），我们自己也能创建自己的模块吗？

答案当然是肯定的。**模块**（*modules*），其实就是保存成文件的 Python 源代码。

当我们做点稍微复杂的事情是，就不太能够把所有代码写在一个文件里了，文件会变得巨大，非常难查找难阅读难修改。而 *module* 是 Python 提供给我们的模块化方法，让我们可以把源代码分开不同的文件，并在需要时引用其他文件里的代码。

这一章我们就介绍如何创建自己的模块以及如何使用。

## 创建模块

这几乎没啥可讲的，因为你只要把 Python 源代码存成一个 `.py` 的文件，这就是一个模块（*module*），而这个文件的文件名（不含后缀 `.py`）就是模块的名字。比如我们把我们之前写好的如下函数保存为一个叫做 `myutil.py` 的文件：


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

def say_hi(*names, greeting='Hi', capitalized=False):
    """
    Print a string, with a greeting to everyone.
    :param *names: tuple of names to be greeted.
    :param greeting: 'Hi' as default.
    :param capitalized: Whether name should be converted to capitalized before print. False as default.
    :returns: None
    """
    for name in names:
        if capitalized:
            name = name.capitalize()
        print(f'{greeting}, {name}!')
```

## 模块引入

保存好 `myutil.py` 文件之后，只要告诉解释器在哪里可以找到这个文件，就可以用 `import` 语句来引入和使用了，无论在 Jupyter Notebook 中还是其他 `.py` 文件中都是如此。


```python
import sys
sys.path
```




    ['/Users/neo/Code/Repo/selfteaching/pilot',
     '/Users/neo/Code/Repo/selfteaching/pilot',
     '/Users/neo/.envs/jupyter/lib/python37.zip',
     '/Users/neo/.envs/jupyter/lib/python3.7',
     '/Users/neo/.envs/jupyter/lib/python3.7/lib-dynload',
     '/usr/local/Cellar/python/3.7.4_1/Frameworks/Python.framework/Versions/3.7/lib/python3.7',
     '',
     '/Users/neo/.envs/jupyter/lib/python3.7/site-packages',
     '/Users/neo/.envs/jupyter/lib/python3.7/site-packages/IPython/extensions',
     '/Users/neo/.ipython']



`sys` 是 Python 管理系统环境的模块，而 `sys.path` 保存着一个列表，里面是所有解释器“认识”的目录，这些目录都是解释器会自动去找模块的地方，我们可以看到，运行 Python 解释器的当前目录是自动包含在里面的；然后就是 Python 环境的库所在目录，我们用 `pip` 安装的库都会放在这些目录下。

注意：你的环境里显示出来的这个 `sys.path` 列表内容可能和我不一样，这没关系。

由于我把上面那个 `myutil.py` 放在这些 *notebooks* 所在目录下的 `assets` 子目录下，这个子目录不在上面显示的 `sys.path` 中，但我们可以把它加进去：


```python
sys.path.append('./assets/')
```

然后我们就可以方便的引入这个模块了，和我们使用 Python 自带的模块一样：


```python
import myutil
myutil.say_hi('Neo', 'Trinity')
```

    Hi, Neo!
    Hi, Trinity!



```python
from myutil import is_prime
is_prime(47)
```




    True



在上面的例子中，当使用 `import myutil` 的时候，就向当前环境引入了 `myutil` 文件中定义的所有函数，和下面的语句等价：

```python
from myutil import *
```

当然我们也可以只引入其中需要的函数，比如：

```python
from myutil import is_prime
```

这种情况下，就不需要使用 `myutil.is_prime()`，而是直接写 `is_prime()` 就好。

### 模块的分层

模块的使用可以分层。设想我们在一个目录下有很多 `.py` 源代码文件，我们可以分若干子目录，根据用途把 `.py` 文件分到这些子目录中去，比如我们有这么一个目录：

```
~/Code/some_project:
├── modules/
│   ├── __init__.py
│   ├── auth.py
│   ├── helper.py
│   ├── math/
│   │   ├── calculus.py
│   │   ├── logic.py
│   └── string/
│       ├── codec.py
│       └── l10n.py
├── setup.py
└── main.py
```

我们看到我们在这个目录下有一些主程序的源文件 `setup.py` `main.py`，然后一些工具性的源代码被放到了 `modules` 这个子目录下，并根据其用途分了一些更细的子目录（比如 `math` `string`）。这样的结构很合理和美观，也非常有伸缩性，以后再大的工程、再多源代码，也都能组织和管理好。那么要如何使用这些源代码呢？

第一步，你需要确保 `modules` 这个目录在解释器的搜索路径中，也就是将其加入 `sys.path` 列表（这件事可以在 `setup.py` 或者 `main.py` 里做）：

```python
sys.path.append('./modules/')
```

第二步，要确保 `modules` 目录下有一个叫做 `__init__.py` 的文件，这个文件通常为空文件，用于标识该目录是一个包含多个模块的包（*package*），所有这些模块都处在这个包的独立名字空间（*namespace*）下。

关于 *package* 和 *namespace* 目前还不是我们的重点，有兴趣可以参考[官方文档](https://docs.python.org/3/tutorial/modules.html#packages)。

然后就可以引入 `modules` 目录下的所有这些模块了，比如我们想引入 `auth.py` 里的所有函数，可以这样（假定是要在 `main.py` 里使用，下同）：

```python
import auth
```

如果要使用 `math` 目录下的 `calculus.py` 文件里的所有函数，可以：

```python
import math.calculus
```

也可以写成：

```python
from math import calculus
```

或者:

```python
from math.calculus import *
```

如果我们只要其中某几个函数，可以：

```python
from math.calculus import ode, pde
```

如果我们要一次引入 `string` 目录下的所有模块，可以：

```python
from string import *
```

所以，只要一个目录满足下面两个条件就是一个 Python 的软件包（*package*）：
1. 在 `sys.path` 列表中；
2. 里面有个 `__init__.py` 文件。

以这个目录作为根，下面所有 `.py` 源文件（包括无论多少级子目录下的）都可以作为 *module* 被其他 Python 程序引入和使用。引入的方法就是逐级目录加最后的文件名，中间用 `.` 隔开即可。

### 引入中的别名

引入模块或者模块中的函数时，可以设置模块或者函数的别名，这个能力很重要，因为难免我们引入的模块或者函数可能与我们正在写的东西重名，别名可以避免发生冲突。别名的语法很简单，在引入的时候加上 `as xxx` 就行，可以用于模块也可以用于函数，比如：


```python
from myutil import is_prime as isp
isp(57)
```




    False




```python
import myutil as m
m.is_prime(27)
```




    False



## 查看系统内置模块

前面用到的 `sys` 模块还有别的用途，可以用它提供的 `sys.builtin_module_names` 列表查看所有系统内置模块或者检查某个模块是不是内置模块：


```python
sys.builtin_module_names
```




    ('_abc',
     '_ast',
     '_codecs',
     '_collections',
     '_functools',
     '_imp',
     '_io',
     '_locale',
     '_operator',
     '_signal',
     '_sre',
     '_stat',
     '_string',
     '_symtable',
     '_thread',
     '_tracemalloc',
     '_warnings',
     '_weakref',
     'atexit',
     'builtins',
     'errno',
     'faulthandler',
     'gc',
     'itertools',
     'marshal',
     'posix',
     'pwd',
     'sys',
     'time',
     'xxsubtype',
     'zipimport')



和之前介绍的系统关键字列表一样，我们也最好不要用这里面有的名字，无论是作为我们自己的模块、类、函数还是变量名。

## 模块中不只有函数

一个模块文件中，不一定只包含函数；它也可以包含函数之外的可执行代码。`import` 这个命令不过是让解释器加载指定的模块（源代码），并且从头到尾执行一遍，其中定义函数的部分让这些函数可以被我们使用，其余部分就在 `import` 语句执行时执行一次而已。

有一个 Python 的彩蛋，恰好是可以用在此处的最佳例子——这个模块叫 this：


```python
import this
```

    The Zen of Python, by Tim Peters
    
    Beautiful is better than ugly.
    Explicit is better than implicit.
    Simple is better than complex.
    Complex is better than complicated.
    Flat is better than nested.
    Sparse is better than dense.
    Readability counts.
    Special cases aren't special enough to break the rules.
    Although practicality beats purity.
    Errors should never pass silently.
    Unless explicitly silenced.
    In the face of ambiguity, refuse the temptation to guess.
    There should be one-- and preferably only one --obvious way to do it.
    Although that way may not be obvious at first unless you're Dutch.
    Now is better than never.
    Although never is often better than *right* now.
    If the implementation is hard to explain, it's a bad idea.
    If the implementation is easy to explain, it may be a good idea.
    Namespaces are one honking great idea -- let's do more of those!


这个模块的源代码我们也抄在下面，有兴趣可以试试能否独立读懂这个文件里的代码——对你来说，还挺练脑子的呢！

提示：这段代码先通过一个规则生成了一个密码表，保存在 `d` 这个字典中（也可以在我们学完字典这种数据容器之后再来读这段代码）；而后，将 `s` 这个变量中保存的 “密文” 翻译成了英文。或许，你可以试试，看看怎样能写个函数出来，给你一段英文，你可以把它加密成跟它一样的 “密文”？


```python
s = """Gur Mra bs Clguba, ol Gvz Crgref
Ornhgvshy vf orggre guna htyl.
Rkcyvpvg vf orggre guna vzcyvpvg.
Fvzcyr vf orggre guna pbzcyrk.
Pbzcyrk vf orggre guna pbzcyvpngrq.
Syng vf orggre guna arfgrq.
Fcnefr vf orggre guna qrafr.
Ernqnovyvgl pbhagf.
Fcrpvny pnfrf nera'g fcrpvny rabhtu gb oernx gur ehyrf.
Nygubhtu cenpgvpnyvgl orngf chevgl.
Reebef fubhyq arire cnff fvyragyl.
Hayrff rkcyvpvgyl fvyraprq.
Va gur snpr bs nzovthvgl, ershfr gur grzcgngvba gb thrff.
Gurer fubhyq or bar-- naq cersrenoyl bayl bar --boivbhf jnl gb qb vg.
Nygubhtu gung jnl znl abg or boivbhf ng svefg hayrff lbh'er Qhgpu.
Abj vf orggre guna arire.
Nygubhtu arire vf bsgra orggre guna *evtug* abj.
Vs gur vzcyrzragngvba vf uneq gb rkcynva, vg'f n onq vqrn.
Vs gur vzcyrzragngvba vf rnfl gb rkcynva, vg znl or n tbbq vqrn.
Anzrfcnprf ner bar ubaxvat terng vqrn -- yrg'f qb zber bs gubfr!"""

d = {}
for c in (65, 97):
    for i in range(26):
        d[chr(i+c)] = chr((i+13) % 26 + c)

print("".join([d.get(c, c) for c in s]))
```

    The Zen of Python, by Tim Peters
    Beautiful is better than ugly.
    Explicit is better than implicit.
    Simple is better than complex.
    Complex is better than complicated.
    Flat is better than nested.
    Sparse is better than dense.
    Readability counts.
    Special cases aren't special enough to break the rules.
    Although practicality beats purity.
    Errors should never pass silently.
    Unless explicitly silenced.
    In the face of ambiguity, refuse the temptation to guess.
    There should be one-- and preferably only one --obvious way to do it.
    Although that way may not be obvious at first unless you're Dutch.
    Now is better than never.
    Although never is often better than *right* now.
    If the implementation is hard to explain, it's a bad idea.
    If the implementation is easy to explain, it may be a good idea.
    Namespaces are one honking great idea -- let's do more of those!


注意，虽然这个模块里什么函数都没有，但是 `import` 之后，里面所有全局变量我们都可以使用。

## `dir()` 函数

系统内置函数 `dir()` 可以查看一个已经引入的模块里有哪些可以访问的变量和函数：


```python
dir(myutil)
```




    ['__builtins__',
     '__cached__',
     '__doc__',
     '__file__',
     '__loader__',
     '__name__',
     '__package__',
     '__spec__',
     'is_prime',
     'say_hi',
     'sqrt']



可以看到前面有一大堆都不是我们写的，而是系统自动加进去的，这属于模块的**元数据**（*metadata*），你可以试试一个个看看里面是什么，比如：


```python
myutil.__name__
```




    'myutil'



## `__main__` 模块

关于模块中不在任何函数里的部分，还有一点值得了解。

设想我们编写了一些函数，并在函数以外的部分写了一些代码来测试这些函数，然后将这些代码保存在一个模块（`.py` 文件）中供其他代码使用。如果别的代码 `import` 这个模块，那么那些测试代码就会被运行一遍，怎么看都觉得这不是个好事情，但是我们又不想删掉这些代码，因为我们以后对这个模块进行修改维护，还需要它们。Python 对此情况也提供了一个解决方案，这个方案就是使用特殊的模块名 `__main__`。

前面看到，当我们 `import` 一个模块之后，可以用 `myutil.__name__` 查看它的模块名，通常是它的文件名；Python 做了一个特殊的设定，那就是当我们在命令行界面用 `python myutil.py` 来运行这个文件时，它的模块名 `__name__` 的值为 `'__main__'`，这是个独一无二的值，不会出现在任何引入的模块中，这样我们就可以区分开“运行一个文件”和“引入一个模块”这两种行为，加以区别对待区别，也就是把模块写成这个样子：

```python
def func1();
    ...

def func2();
    ...

if __name__ == '__main__':
    # 把所有测试或者不希望被引入时执行的代码放在这里
```

这么做的结果是：

* 当 Python 文件被当作模块，被 import 语句导入时，if 判断失败，下面的代码不被执行；
* 当 Python 文件被 `python -m` 运行的时候，`if` 判断成功，下面的代码被执行。

## 小结

* 模块是 Python 管理多个源代码文件的基本单元，一个模块对应一个 `.py` 源文件；
* 模块名可以包含若干节，用 `.` 分开，`.` 分开的每一节对应子目录和文件名，比如当前目录下 `foo/goo/bar.py` 这个文件的模块名就是 `foo.goo.bar`；
* 可以用 `import` 语句来引入解释器能找到的任何模块，可以全模块引入也可以只引入指定的函数或者变量，还可以给引入的模块、函数或变量用 `as` 指定别名；
* 解释器会在 `sys.path` 这个变量保存的所有目录下查找我们想引入的模块；
* 了解 `sys.builtin_module_names` 和 `dir()` 的用法，有助于我们探索当前环境的可用模块以及了解模块的信息；
* 可以把不希望被引入时执行的代码放在 `if __name__ == '__main__':` 判断下面，让一个文件既可以被当做模块引入，也可以作为一个程序独立执行。
