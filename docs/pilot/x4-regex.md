# 正则表达式入门

## 基本概念

**正则表达式**（*regular expression*）是一个罕见的例子，它既有极其深刻的理论背景，又成为了一种极其常用的工具。

Wikipedia 上对正则表达式的说明如下：

> **正则表达式**（英语：Regular Expression，在代码中常简写为 *regex*、*regexp* 或 *RE*），又称*正规表示式*、*正规表示法*、*正规运算式*、*规则运算式*、*常规表示法*，是计算机科学的一个概念。正则表达式使用单个字符串来描述、匹配一系列符合某个句法规则的字符串。在很多文本编辑器里，正则表达式通常被用来检索、替换那些符合某个模式的文本。许多程序设计语言都支持利用正则表达式进行字符串操作。例如，在 Perl 中就内建了一个功能强大的正则表达式引擎。正则表达式这个概念最初是由 Unix 中的工具软件（例如 sed 和 grep）普及开的。

英文里 *regular* 本是“正规”的意思，在翻译 *regular expression* 时，可能是为了和太过通用的“正规”一词区分开来，使用了“正则”作为技术名词特有的译法。

在正则表达式中，还有两个词儿需要了解：

* 一个是 **pattern**，一个写出来的正则表达式就是一个 *pattern*，也就是前面定义里的“用于匹配的语法规则”；
* 一个是 **match**，可以用作动词和名词，作动词是就是拿上述 *pattern* 去字符串中寻找符合规则的子串，作名词就是找到的那个子串。

所有主流的程序设计语言都内置对 *regex* 的支持，实际上就是内置了一个“正则表达式引擎”，这个引擎能够拿着我们写好的规则（*pattern*）去任何字符串里寻找匹配的子串，还提供找到之后替换等功能。

我们来看看 Python 里的例子：


```python
import re
```


```python
str = 'The quick brown fox jumps over the lazy dog'
pattern = re.compile(r'\wo\w')
re.findall(pattern, str)
```




    ['row', 'fox', 'dog']



首先引入 Python 自带的正则表达式模块 `re`，然后用 `re` 提供的 `compile()` 函数把我们书写的正则规则 `\wo\w` 编译成一个 *pattern*，这个规则里的 `\w` 表示任意字母数字或者下划线，而 `o` 就是字母 ‘o’。

> 这个规则字符串前面的 `r` 表示 *raw string*，Python 不会对其中的 `\` 做转义处理。

最后调用 `re` 提供的 `findall()` 函数用这个 *pattern* 去寻找 `str` 中所有满足这个规则的匹配（*matches*）。

输出结果是所有 *matches* 组成的列表。

正则引擎不仅可以寻找匹配，还可以**捕获**（*capture*）匹配中的一个片段，可以将其**替换**（*replace* 或 *substitute*）成别的字符串。我们来看下面一个例子：


```python
str = 'The white dog wears a black hat.'

pattern = r'The (white|black) dog wears a (white|black) hat.'
repl = r'The \2 dog wears a \1 hat.'
re.sub(pattern, repl, str)
```




    'The black dog wears a white hat.'



和 `re.find()` 或者 `re.findall()` 不一样，`re.sub()` 有三个参数，第一个是用来匹配的规则 `pattern`，第二个是找到 *matches* 之后执行替换的目标模板 `target`，第三个是被操作的字符串。这里需要留意的是：
* 正则规则 `pattern` 中有两个小括号 `()`，这两个小括号的作用就是“捕获”匹配到的内容，规则中第一对小括号捕获的内容会被存在临时变量 `$1` 中，第二对小括号对应的内容则存在 `$2` 中，依此类推；
* 替换目标模板 `target` 中有一个 `\1` 和一个 `\2`，代表这里要换成 `$1` 和 `$2` 的值。

于是上面的代码就把原句中的 *white* 和 *black* 交换了位置。

试试看下面的代码，能预测它的输出吗？


```python
repl = r'The \1 dog wears a \1 hat.'
re.sub(pattern, repl, str)
```




    'The white dog wears a white hat.'



小结一下：
* 我们可以按照正则表达式（*regex*）的语法书写正则规则（*pattern*）；
* 正则引擎可以用这个 *pattern* 去匹配（*match*）指定字符串；
* 输出所有匹配（*matches*）；
* 这个过程中还可以捕获匹配中的特定部分，并进行替换。

使用 *regex* 的最主要场景有二：
* 用规则去匹配字符串，确认字符串是不是包含符合规则定义的子串，通常用来确认字符串是不是符合特定“格式”；
* 用规则去匹配字符串，捕获并替换其中的特定片段。

顺便，在自学的过程中，想尽一切办法把一切术语用简单直白的“人话”重新表述，是特别有效的促进进步的行为模式，也可以检验你是不是真的搞懂学会了。

## 准备工作

虽然基本概念并不复杂，但 *regex* 还是一种比较抽象的语言，其语法远不如 Python 那么直观易懂，所以在继续学习前我们先介绍两个工具，在学习 *regex* 的过程中会很有帮助。

第一个是正则规则可视化工具，可以对输入的规则给出可视化的解析，比如 [Regexper](https://regexper.com)。

第二个是正则规则测试工具，可以对输入的规则给出说明，并对给出的测试字符串运行规则给出结果，比如 [regex101](https://regex101.com)。

你现在就可以试试这两个工具，将前面例子里的规则填进去，看看效果，还可以试着写点别的规则玩玩，反正也不怕出错。

另外，我们还需要个测试文本文件，用来当作下面练习使用正则表达式时的字符串，因为有时长一点才能说明一些问题，所以存在一个文件中比直接写在代码里好。我们已经把这个文件放在 `assets` 自目录下，文件名是 `sample.txt`。你可以打开这个文件看看，里面有很多用来测试 *regex* 的字符串，是个不错的测试基底。

下面的代码就拿这个文件做测试：


```python
with open('assets/sample.txt', 'r') as f:
    str = f.read()
pattern = r'beg[iau]ns?'
re.findall(pattern, str)
```




    ['begin', 'began', 'begun', 'begins', 'begin']



第一行是 Python 操作文件的标准方法，内置函数 `open` 按照指定方式（`'r'` 表示只读模式）打开指定文件（第一个参数指定路径），并用一个文件对象（`f`）的方式给我们使用；下面用 `f.read()` 来读取文件的内容并赋给 `str` 变量。

这里我们使用的 `pattern` 比前面复杂多了，你可以把 `beg[iau]ns?` 分别贴到 [Regexper](https://regexper.com/#beg%5Biau%5Dns%3F) 和 [regex101](https://regex101.com) 看看。你会看到可视化解析能很清晰的告诉我们这个规则是什么意思。所以在下面每个例子中你都可以这么做。

最后 `findall()` 将所有 *matches* 输出成一个列表。 

## 操作符，原子与优先级

学到这里的你已经不是“啥都不懂”的人了，你已经知道一个事实：编程语言无非是用来运算的。

所谓的运算，就有**操作符**（*operators*）和**操作元**（*operands*），而操作符肯定是有优先级的，不然的话，那么多操作元和操作符放在一起，究竟先操作哪个呢？就和四则运算（括号由内向外处理、先乘除后加减、同级别从左到右等）是一个道理。

*Regex* 本身就是个迷你语言（*mini language*），它也有很多操作符，操作符也有优先级；而它的操作元有个专门名称，叫做**原子**（*atom*）。

看看下面列出的 *regex* 操作符优先级，你会对它有相当不错的了解：

| 排列 |         原子与操作符优先级      |（从高到低）|
|---|-----------------------------------|------------------------|
| 1 | 转义符号 (Escaping Symbol)               | `\` |
| 2 | 分组、捕获 (Grouping or Capturing)                          | `(...)` `(?:...)` `(?=...)` `(?!...)` `(?<=...)` `(?<!...)`     |
| 3 | 数量 (Quantifiers)      | `a*` `a+` `a?` `a{n, m}` |
| 4 | 序列与定位（Sequence and Anchor）| `abc` `^` `$` `\b` `\B`               |
| 5 | 或（Alternation）| <code>a&#124;b&#124;c</code>                   |
| 6 | 原子 (Atoms)                 | `a` `[^abc]` `\t` `\r` `\n` `\d` `\D` `\s` `\S` `\w` `\W` `.` |

下面我们来看看所有这些东西到底是什么。

## 原子

*Regex pattern* 中最基本的元素被称为**原子**（*atom*），包括下面这些类型：

### 本义字符

最基本的原子，就是本义字符，它们都是单个字符。

本义字符包括从 `a` 到 `z`，`A` 到 `Z`，`0` 到 `9`，还有下划线 `_`，它们所代表的就是它们的字面值。

相当于 Python 中的 `string.ascii_letters`、`string.digits` 及 `_`。

以下字符在 *regex* 中都有特殊含义：`\` `+` `*` `.` `?` `-` `^` `$` `|` `(` `)` `[` `]` `{` `}` `<` `>`。

所以你在写规则时，如果需要匹配这些字符，建议都在前面加上转义符 `\`，比如，你想匹配 `$`，那你就写 `\$`，或者想匹配 `|` 那就写 `\|`。

跟过往一样，所有的细节都很重要，它们就是需要花时间逐步熟悉到牢记。

### 集合原子

原子的集合还是原子。

*Regex* 使用方括号 `[]` 来标示集合原子，`[abc]` 的意思就是这个位置匹配 `a` 或者 `b` 或者 `c`，即 `abc` 中任一字符。

如下面的例子 [`beg[iau]n`](https://regexper.com#beg[iau]n) 能够匹配 `begin`、`began` 和 `begun`：


```python
str = 'begin began begun bigins begining'
re.findall(r'beg[iau]n', str)
```




    ['begin', 'began', 'begun', 'begin']



在集合原子中，我们可以使用两个操作符：`-`（表示一个区间）和 `^`（表示排除）。

* `[a-z]` 表示从小写字母 `a` 到 `z` 中的任意一个字符。
* `[^abc]` 表示 `abc` 以外的其它任意字符；注意，**一个集合原子中，`^` 符号只能用一次，只能紧跟在 `[` 之后**，否则不起作用。

### 类别原子

类别原子，是指能够代表“一类字符”的原子，类别有特殊转义符定义，包括下面这些：
* `\d` 任意数字；等价于 `[0-9]`
* `\D` 任意非数字；等价于 `[^0-9]`
* `\w` 任意本义字符；等价于 `[a-zA-Z0-9_]`
* `\W` 任意非本义字符；等价于 `[^a-zA-Z0-9_]`
* `\s` 任意空白；相当于 `[ \f\n\r\t\v]`（注意，方括号内第一个字符是空格符号）
* `\S` 任意非空白；相当于 `[^ \f\n\r\t\v]`（注意，紧随 `^` 之后的是一个空格符号）
* `.` 除 `\r` `\n` 之外的任意字符；相当于 `[^\r\n]`

类别原子挺好记的，如果你知道各个字母是哪个词的首字母的话：
* `d` 是 *digits*；
* `w` 是 *word characters*；
* `s` 是 *spaces*。

另外，在空白的集合 `[ \f\n\r\t\v]` 中：`\f` 是分页符；`\n` `\r` 是换行符；`\t` 是制表符；`\v` 是纵向制表符（很少用到）。各种关于空白的转义符也同样挺好记忆的，如果你知道各个字母是那个词的首字母的话：
* `f` 是 *flip*；
* `n` 是 *new line*；
* `r` 是 *return*；
* `t` 是 *tab*；
* `v` 是 *vertical tab*。


```python
str = '<dl>(843) 542-4256</dl> <dl>(431) 270-9664</dl>'
re.findall(r'\d\d\d\-', str)
```




    ['542-', '270-']



### 边界原子

边界原子用来指定“边界”，有时也叫“定位操作符”：
* `^` 匹配被搜索字符串的开始位置；
* `$` 匹配被搜索字符串的结束位置；
* `\b` 匹配单词的边界；[`er\b`](https://regexper.com#er%5Cb)，能匹配 `coder` 中的 `er`，却不能匹配 `error` 中的 `er`；
* `\B` 匹配非单词边界；[`er\B`](https://regexper.com#er%5CB)，能匹配 `error` 中的 `er`，却不能匹配 `coder` 中的 `er`。


```python
str = 'never ever verb never however everest'
re.findall(r'er\b', str)
```




    ['er', 'er', 'er', 'er']




```python
re.findall(r'er\B', str)
```




    ['er', 'er']




```python
re.findall(r'nev', str)
```




    ['nev', 'nev']




```python
re.findall(r'^nev', str)
```




    ['nev']



`^` 和 `$` 在 Python 语言中也可以用 `\A` `\Z`。事实上，每种语言或多或少都对 *regex* 有自己的定制，不过本章讨论的绝大多数细节，都是通用的。

### 组合原子

我们可以用圆括号 `()` 将多个单字符原子组合成一个原子，这么做的效果是 `()` 内的字符串将被当作一整个原子，可以被随后我们要讲解的数量操作符操作。这个语法叫做**分组**或者**组合**（*grouping*）。

另外，`()` 这个操作符，除了用于 *grouping*，还用于**捕获**（*capturing*），前面看到过，后面还会讲。

所以现在你应该可以分清楚，[`er`](https://regexper.com#er)、[`[er]`](https://regexper.com#[er]) 和 [`(er)`](https://regexper.com#(er) 含义各不相同。

> * `er` 是两个原子，`'e'` 和紧随其后的 `'r'`；
> * `[er]` 是一个原子，或者 `'e'` 或者 `'r'`；
> * `(er)` 是一个原子，`'er'`。

下一节中讲到数量操作符的时候，会再次强调这点。

## 数量操作符

数量操作符有这几个：`+` `?` `*` `{n, m}`。

它们是用来限定位于它们之前的原子出现的个数；不加数量操作符代表出现一次且仅出现一次，而加上数量操作符的含义分别是：

* `+` 代表前面的原子必须至少出现一次，即 出现次数 ≧ 1；例如，[`go+gle`](https://regexper.com#go+gle)可以匹配 `google` `gooogle` `goooogle` 等；
* `?` 代表前面的原子最多只可以出现一次，即 0 ≦ 出现次数 ≦ 1；例如，[`colou?red`](https://regexper.com#colou?red)可以匹配 `colored` 或者 `coloured`;
* `*` 代表前面的原子可以不出现，也可以出现一次或者多次，即 出现次数 ≧ 0；例如，[`520*`](https://regexper.com#520*)可以匹配 `52` `520` `52000` `5200000` `520000000000` 等；
* `{n}` 代表前面的原子确定出现 n 次；`{n,}` 代表前面的原子出现至少 n 次；`{n, m}` 代表前面的原子出现至少 `n` 次，至多 `m` 次；例如，[`go{2,5}gle`](https://regexper.com#go%7B2,5%7Dgle)，能匹配 `google` `gooogle` `goooogle` 或 `gooooogle`，但不能匹配 `gogle` 和 `gooooooogle`。

如下例：


```python
with open('assets/sample.txt', 'r') as f:
    str = f.read()

re.findall(r'go+gle', str)
```




    ['google', 'gooogle', 'goooogle', 'goooooogle']




```python
re.findall(r'go{2,5}gle', str)
```




    ['google', 'gooogle', 'goooogle']




```python
re.findall(r'colou?red', str)
```




    ['coloured', 'colored']




```python
re.findall(r'520*', str)
```




    ['520', '52000', '5200000', '520000000', '520000000000']



数量操作符是对它之前的原子进行操作的，换言之，数量操作符的操作元是操作符之前的原子。

上一节提到，要注意区别：`er`、`[er]` 和 `(er)` 各不相同。

> * `er` 是两个原子，`'e'` 之后 `'r'`
> * `[er]` 是一个原子，或者 `'e'` 或者 `'r'`；
> * `(er)` 是一个原子，`'er'`


```python
str = 'error wonderer severeness'

re.findall(r'er', str)
```




    ['er', 'er', 'er', 'er']




```python
re.findall(r'[er]', str)
```




    ['e', 'r', 'r', 'r', 'e', 'r', 'e', 'r', 'e', 'e', 'r', 'e', 'e']




```python
re.findall(r'(er)', str)
```




    ['er', 'er', 'er', 'er']



这里我们还看不出 `er` 和 `(er)` 的区别，但是，加上数量操作符就不一样了——因为数量操作符只对它之前的那**一个**原子进行操作：


```python
re.findall(r'er+', str)
```




    ['err', 'er', 'er', 'er']




```python
re.findall(r'[er]+', str)
```




    ['err', 'r', 'erer', 'e', 'ere', 'e']




```python
re.findall(r'(er)+', str)
```




    ['er', 'er', 'er']



## 或操作符

或操作符 `|` 是所有操作符中优先级最低的，数量操作符的优先级比它高，所以，在 `|` 前后的原子被数量操作符（如果有的话）操作之后才交给 `|` 操作。简单理解就是规则被 `|` 分成若干段，各段里出现一种就匹配成功。

下面例子里，[`begin|began|begun`](https://regexper.com#begin%7Cbegan%7Cbegun) 能够匹配 `begin` 或 `began` 或 `begun`。


```python
str = 'begin began begun begins beginn'
re.findall(r'begin|began|begun', str)
```




    ['begin', 'began', 'begun', 'begin', 'begin']



**注意**：前面讲的集合原子方括号中的 `|` `()` 不会被当作特殊符号，而是被当作字符本身，没有或操作符和分组的作用。

## 匹配并捕获

**捕获**（*capture*）使用的是圆括号 `()`。使用圆括号可以捕获匹配中的特定片段，并将其值暂存进一个带有索引的列表，第一个片段是 `$1`，第二个是 `$2`，依此类推。随后，我们可以在替换的过程中使用 `\1` `\2` 中来引用所保存的值。

比如前面我们讲过的例子：


```python
str = 'The white dog wears a black hat.'

pattern = r'The (white|black) dog wears a (white|black) hat.'
repl = r'The \2 dog wears a \1 hat.'
re.sub(pattern, repl, str)
```




    'The black dog wears a white hat.'




```python
repl = r'The \1 dog wears a \1 hat.'
re.sub(pattern, repl, str)
```




    'The white dog wears a white hat.'



有时，你并不想捕获圆括号中的内容，在那个地方你使用括号的目的只是分组（*grouping*），而非捕获，所以不希望捕获那里的值占用一个 `$` 标号，那么就在圆括号内最开头加上 `?:`：


```python
str = 'The white dog wears a black hat.'
pattern = r'The (?:white|black) dog wears a (white|black) hat.'
re.findall(pattern, str)
```




    ['black']




```python
repl = r'The \1 dog wears a \1 hat.'    # 之前的一处捕获，在替换时可被多次引用
re.sub(pattern, repl, str)
```




    'The black dog wears a black hat.'



实际上在 Python 代码中使用正则表达式匹配和捕获以及随后的替换，有更灵活的方式，因为可以对那些值直接编程，`re.sub()` 中的 `repl` 参数甚至可以接收另外一个函数作为参数，具体可以查阅 Python 的[官方文档](https://docs.python.org/3/library/re.html)。

非捕获匹配，还有几个**预查**（*look ahead*, *look behind*）操作符。

所谓 *look ahead* 和 *look behind*，意思就是匹配一个部分时看看这个部分前后是不是符合某个规则，前后的部分只进行规则检查但不参与匹配：

* `(?=pattern)`：*look ahead positive assert*，例如 [`Windows(?=95|98|NT|2000)`](https://regexper.com#%60Windows(?=95%7C98%7CNT%7C2000)%60) 能匹配 `Windows2000` 中的 `Windows`（注意匹配的只有 `Windows`，而不是 `Windows2000`），但不能匹配 `Windows3.1` 中的 `Windows`；
* `(?!pattern)`：*look ahead negative assert*，例如 [`Windows(?!95|98|NT|2000)`](https://regexper.com#Windows(?=95%7C98%7CNT%7C2000)) 能匹配 `Windows3.1` 中的 `Windows`，但不能匹配 `Windows2000` 中的 `Windows`；
* `(?<=pattern)`：*look behind positive assert*，这是检查匹配部分之前的字符，例如 [`(?<=95|98|NT|2000)Windows`](https://regexper.com#(?%3C=95%7C98%7CNT%7C2000)Windows) 能匹配 `2000Windows` 中的 `Windows`，但不能匹配 `3.1Windows` 中的 `Windows`；
* `(?<!pattern)`：*look behind negative assert*，例如 `(?<!95|98|NT|2000)Windows` 能匹配 `3.1Windows` 中的 `Windows`，但不能匹配 `2000Windows` 中的 `Windows`。

再次提醒，这几个都是非捕获的。

## 控制标记

在 Python 中控制标记（*flag*）是模块 `re` 下的一组布尔值，通过设置其真假会影响正则引擎的匹配和替换逻辑。

另外也可以在 *pattern* 最开始写一个特殊的 `(?)` 结构来只影响当前这一个 *pattern*（这叫 *inline flag*）。

下面是所有标记的简要介绍，供参考：

`A`/`ASCII`，默认为 `False`
* `\d`, `\D`, `\w`, `\W`, `\s`, `\S`, `\b`, 和 `\B` 等只限于 ASCII 字符
* Inline flag：`(?a)`
* Flag in `re` module：`re.A` `re.ASCII`

`I`/`IGNORECASE`，默认为 `False`
* 忽略字母大小写
* Inline flag：`(?i)`
* Flag in `re` module：`re.I` `re.IGNORECASE`

`G`/`GLOBAL`，默认为 `True`
* 找到第一个 match 之后不返回
* Inline flag：`(?g)`
* Flag in `re` module：不能更改，默认为 TRUE

`L`/`LOCALE`，默认为 `False`
* 由本地语言设置决定 `\d`, `\D`, `\w`, `\W`, `\s`, `\S`, `\b`, 和 `\B` 等等的内容
* Inline flag：`(?L)`
* Flag in `re` module：`re.L` `re.LOCALE`

`M`/`MULTILINE`，默认为 `True`
* 使用本标志后，`^` 和 `$` 匹配行首和行尾时，会增加换行符之前和之后的位置。
* Inline flag：`(?m)`
* Flag in `re` module：`re.M` `re.MULTILINE`

`S`/`DOTALL`，默认为 `False`
* 使 `.` 完全匹配任何字符，包括换行；没有这个标志，`.` 匹配除了 `n` `r` 之外的任何字符。
* Inline flag：`(?s)`
* Flag in `re` module：`re.S` `re.DOTALL`

`X`/`VERBOSE`，默认为 `False`
* 当该标志被指定时，Pattern 中的的空白符会被忽略，除非该空白符在圆括号或方括号中，或在反斜杠 `\ ` 之后。这样做的结果是允许将注释写入 Pattern，这些注释会被 Regex 解析引擎忽略。注释用 `#` 号来标识，不过该符号不能在字符串或反斜杠之后。
* Inline flag：`(?x)`
* Flag in `re` module：`re.X` `re.VERBOSE`

## 常用规则

前面快速的过了一遍 *regex* 的主要语法和用法，其实很多常用的 *regex* 规则都不需要自己的写，可以直接在网上找到，比如[这里](https://digitalfortress.tech/tricks/top-15-commonly-used-regex/)，[这里](https://www.keycdn.com/support/regex-cheatsheet)，还有[这里](https://code.tutsplus.com/tutorials/8-regular-expressions-you-should-know--net-6149)。

有个特别经常被问而且特别难写完美的，是用来校验 Email 合法性的规则，有人甚至专门为这个建了个站点叫 [Email Address Regular Expression That 99.99% Works](https://emailregex.com/)，里面有个难得一见的怪兽级 *regex*，你可以去瞻仰一下。

p.s. 大部分情况下并不需要追求那么全面完整，大致够用就可以了，我们在介绍[字符串数据](../p4/6-data-1.ipynb)的一章有一些例子。

## 附：趣事一则

[ELIZA](https://en.wikipedia.org/wiki/ELIZA) 是人类史上第一个“聊天机器人”程序，上个世纪六十年代（1966）实现的，核心就是使用正则表达式匹配对方语句中的关键词汇，再经过替换，形成看似有意义的“回应”——事实上，ELIZA 完全不知道自己在说什么……

几年后，就是七十年代初（1972），另外一个聊天机器人 [Parry](https://en.wikipedia.org/wiki/PARRY) 被写了出来，随后第二年，这两个聊天机器人在 ARPANET “碰面”好好地聊了一下：

> ELIZA: Good Evening. Tell me your problems. Please terminate input with a period or a question mark.
>
> PARRY: People get on my nerves sometimes.
>
> ELIZA: I am not sure I understand you fully.
>
> PARRY: You should pay more attention.
>
> ELIZA: Suppose you should pay more attention.
>
> PARRY: You're entitled to your own opinion.
>
> ELIZA: What makes you think I am entitled to my own opinion?
>
> PARRY: It bothers me just to be around people in general.
>
> ELIZA: Please go on.
>
> PARRY: I went to the track at Bay Meadows a while back.
>
> ELIZA: What does that suggest to you?
> 
> PARRY: I went to the races.
>
> ......

这俩的完整聊天记录可以在[这里](https://tools.ietf.org/html/rfc439)看到，不知道你看了是什么感觉……

不少后人用 Python 重新实现了 ELIZA，比如[这个](https://github.com/jezhiggins/eliza.py)，你可以执行它，然后试着阅读和理解其实现，作为之前我们自己做[练习](../p2/b-oo-4.ipynb)的持续演进。
