# 课程练习 #1

本章是课程第一阶段的学习里程碑，第一个大课程练习，是对整个第一阶段学习内容的综合应用和自我检验：**我们来实现一个简单的对话机器人**。

## 理解问题

在我们想编程实现一个什么时，一定要先问下自己：这个东西的实质是什么（*What is its nature*）？

对话机器人的实质是什么呢？Siri 是个著名的对话机器人，我们可以和她闲聊，也可以问她问题和请她帮忙设个闹钟啊啥的：

![](assets/siri.png)

对话机器人能够读取我们对它说的话（输入），理解其含义，然后做出合理的回应，然后把回应展示（输出）给我们。对话的主题可以各种各样，以 Siri 为例，主要可以分几类：
* 通过特定指令让 Siri 操作手机完成特定任务（设闹钟、发邮件等）；
* 提出特定领域的问题（*query*），Siri 搜索知识库并给出结果（问天气、问比赛结果、查地址等）；
* 自由的闲聊，这个是最难的，因为缺乏上下文限定，计算机很难准确理解我们人类的真实意图。

为了完成这些对话，Siri 包含了几个关键的部件：
* 一个**语音识别**（*speech recognition*）引擎来把我们说的话转换成文字；
* 一个**对话系统**（*dialog system*）来理解我们说的话的含义，并且尝试寻找合适的回应；
* 一个巨大并且不断增长的**知识库**（*knowledge base*）后台，同时也是**语料库**（*text corpus*），帮助 Siri 找到并构造回应；
* 将回应呈现反馈给我们，其中有文字有图片，也有从文字自动生成的语音，后面这个需要一个**文本合成语音**（*text-to-speech, TTS*）的引擎。

这里面每一个都是人工智能领域的大课题，相对来说 *speech recognition* 和 *TTS* 这一对引擎比较成熟一些，虽然还有很多提升空间，比如 Siri 自动合成的语音平滑圆润，却还是缺乏真人的生动感（感情色彩），不过对于 Siri 这样的应用，效果已经足够好了。

对话系统（*dialog system*）则是人工智能重要领域“**自然语言处理**（*natural language processing, NLP*）”中一个主要应用场景，其终极目标是创造能够与人自由对话（*free talk*）的计算机程序。

自由对话已经被证明是极具挑战的，迄今为止，无论是 Apple 的 Siri，还是微软的 Cortana、微软小冰，或者 Google Now，或者小米的小爱同学，都还只是迈出了第一步，离终极目标还差得很远。不过对于特定场景或者特定目的的对话，这些试验产品都正在“开始变得有用”，其背后的技术进步目前已经开始应用在一些非常重要的系统中，比如大型客服系统。

## 设计

我们自己要尝试实现一个对话机器人，当然不可能做到像 Siri 那么完整，目前能有一点对话系统的感觉就好，同时我们还希望搭好一个架子，让我们可以不断迭代加强这个系统的能力，可以这么去思考：
* 简化输入和输出环节，用户直接输入文字，机器人直接回应文字；
* 对系统进行功能分解，除了高度简化的输入和输出层，主要划分为“理解”和“回应”两个引擎；
* 构造一个体系，让我们可以随时实现更好的“理解”和“回应”引擎并且无缝的替换。

为了进一步降低难度，我们可以从程序发起的对话开始，也就是把“程序提问题-用户回应-程序再回应”作为一个基本对话单元，这比由用户自由提出话题要容易。

程序设计不是画图纸，是像小孩玩沙子一样，一边尝试一边思考，在必要时才会画图纸。在做更复杂的思考和设计之前可以先随便的玩一下，看看在 Python 里怎么实现“程序提问题-用户回应-程序再回应”这样的基本单元，结合我们之前学过的知识，我们很快可以写出下面这样的代码：


```python
print("Hi, what is your name?")
name = input()
print(f"Hello {name}!")
```

    Hi, what is your name?
    

     Neo
    

    Hello Neo!
    

是不是已经有点对话的样子了？我们可以再写一个类似的，这次引入一点逻辑判断和条件分支：


```python
print("How are you today?")
feeling = input()
if 'good' in feeling:
    print("I'm feeling good too")
else:
    print("Sorry to hear that")
```

    How are you today?
    

     Good
    

    Sorry to hear that
    

在这里用各种输入测试几次，会发现一个小 bug：如果用户输入 *good* 或者 *I'm good* 这种，代码运行是如我们所想，但是如果用户输入的是 *Good*，程序会不认，而输出 *Sorry to hear that*，这显然不合理，源于我们代码中的不严谨，在判断时未考虑大小写问题。修正也很容易，改成下面这样就好了：


```python
print("How are you today?")
feeling = input()
if 'good' in feeling.lower():
    print("I'm feeling good too")
else:
    print("Sorry to hear that")
```

    How are you today?
    

     Good
    

    I'm feeling good too
    

继续，这次我们引入一点随机性来增加乐趣：


```python
import random

print("What's your favorite color?")
favcolor = input()
colors = ['red', 'orange', 'yellow', 'green', 'blue', 'indigo', 'purple']
print(f"You like {favcolor.lower()}? My favorite color is {random.choice(colors)}")
```

    What's your favorite color?
    

     Blue
    

    You like blue? My favorite color is purple
    

这里用了来自 `random` 模块中的 `choice` 函数，从列表 `colors` 中随机抽选一个元素。

经过这一番尝试，我们可以有一些收获：
* 每一轮交互都是 `print()` 一句话，然后用 `input()` 获取用户输入，然后围绕输入内容进行计算，给出一个相关的输出，并 `print()` 出来；
* 在识别和处理输入时要当心大小写问题；
* 如果细心的话还会发现体验上的一些细节，比如程序 `print()` 输出太快了，如果能停顿一下再输出感觉会更像对话。

最重要的，我们知道基本的交互模式之后，只要愿意，我们可以重复上面的过程，写出好多好多交互剧本，和用户聊上一整天，但是那样好像很枯燥，而且要写好多重复的代码，所以现在我们可以停下来思考怎么把这个过程变得更方便和高效。

回忆我们在前面几章所讲的一些编程的基本思维，尤其是“分而治之”和“责任分离”，把问题分解，每个子问题用一个程序模块来解决，每个程序模块力争把一个问题解决好，并且给出清晰的接口供其他模块使用。

在对话机器人这个课题中，我们可以建立这么几个抽象模块：
1. 每轮交互可以用一个 bot 对象来实现，不同的剧本实现为不同的 Bot 类；
2. 每轮交互中的共性功能，比如输入输出的 `print-input-print` 流程，可以在一个公共的 Bot 父类中处理；
2. 理解用户输入并给出回应是核心的逻辑，每个 Bot 类需要实现这个逻辑，但接口应该在 Bot 父类中统一。

下面我们就来尝试构建这一设计的代码实现。

## 构建

先来看公共父类 `Bot`：


```python
import time

class Bot:

    wait = 1

    def __init__(self):
        self.q = ''
        self.a = ''

    def _think(self, s):
        return s

    def run(self):
        time.sleep(Bot.wait)
        print(self.q)
        self.a = input()
        time.sleep(Bot.wait)
        print(self._think(self.a))
```

这个定义中我们可以看到前面尝试的成果已经很好的集成进来：
* `run()` 方法就是一轮交互 `print-input-print` 流程的实现；
* 前面我们提到的停顿一下的功能被加了进来，用的是 Python `time` 模块中的 `sleep()` 方法，这个方法让解释器休息指定的秒数；
* 用一个类变量（*class variable*）`wait` 来管理程序说每句话之前要停顿的秒数；
* 实例变量（*instance variable*）`self.q` 和 `self.a` 分别代表这一轮对话中程序提出的问题和用户的回答，是两个字符串；
* `_think()` 方法就是以用户输入来计算程序回答的核心算法，在 `run()` 方法中被调用，这个方法一般来说只在 `Bot` 类内部使用，外部最好不要直接访问，所以名字前面我们加了下划线 `_`。

有了这个父类，我们就可以一股脑把前面尝试的几轮对话都实现为它不同的子类：


```python
class HelloBot(Bot):
    def __init__(self):
        self.q = "Hi, what is your name?"

    def _think(self, s):
        return f"Hello {s}"
```


```python
class GreetingBot(Bot):
    def __init__(self):
        self.q = "How are you today?"

    def _think(self, s):
        if 'good' in s.lower() or 'fine' in s.lower():
            return "I'm feeling good too"
        else:
            return "Sorry to hear that"
```


```python
import random

class FavoriteColorBot(Bot):
    def __init__(self):
        self.q = "What's your favorite color?"

    def _think(self, s):
        colors = ['red', 'orange', 'yellow', 'green', 'blue', 'indigo', 'purple']
        return f"You like {s.lower()}? My favorite color is {random.choice(colors)}"
```

可以看到，具体对话逻辑的实现就两件事：
1. 在 `__init__()` 方法中设定程序发问的问题是什么；
2. 在 `_think()` 方法中根据用户输入来计算程序回应的内容。

下面我们需要一个主流程把上面的这些 `Bot` 串起来，这是我们的对话系统的主类定义（我们给它起名叫 `Garfield`，你可以用你喜欢的名字）：


```python
class Garfield:
    
    def __init__(self, wait=1):
        Bot.wait = wait
        self.bots = []

    def add(self, bot):
        self.bots.append(bot)

    def _prompt(self, s):
        print(s)
        print()

    def run(self):
        self._prompt("This is Garfield dialog system. Let's talk.")
        for bot in self.bots:
            bot.run()
```

这里面有点新东西，我们稍微解释一下：
* 对话系统可以包含很多个对话 *bot*，我们把它们放在一个实例变量 `self.bots` 中，这是一个列表，就是我们在[介绍循环的一章](p1-5-structure-4.md)提过的一种数据容器，初始化为空列表 `[]`；
* 提供 `add()` 方法来往系统中加入 *bot*，加入方法是用列表的 `append()` 方法，如上代码中 `add(self, bot)` 方法定义；
* 系统中各个 *bot* 的聊天延迟 `wait` 统一设定，在初始化对话系统时通过修改类变量 `Bot.wait` 的值来实现；
* `run()` 方法是对话系统运行主方法，它先打印一行提示，然后开始一个一个的运行我们加入的 *bot*，即循环调用每个 `bot` 的 `run()` 方法。

然后我们就可以初始化一个 `Garfield` 实例来具体运行了：


```python
# 创建一个聊天延时 1s 的对话系统
garfield = Garfield(1)
# 向其中加入我们已经定义好的各个对话 bot 的对象实例
garfield.add(HelloBot())
garfield.add(GreetingBot())
garfield.add(FavoriteColorBot())
# 运行之
garfield.run()
```

    This is Garfield dialog system. Let's talk.
    
    Hi, what is your name?
    

     Neo
    

    Hello Neo
    How are you today?
    

     Good
    

    I'm feeling good too
    What's your favorite color?
    

     Red
    

    You like red? My favorite color is yellow
    

这样我们的对话系统雏形就运行起来了，还有点像回事吧？

以后我们可以定义更多的 *bot*，每个代表了一轮不一样的对话，然后加入到 `Garfield` 的对象实例，就可以运行起来了。

你可以把这一节的代码一起放进一个源代码文件保存起来，然后在命令行用 `python garfield.py` 来运行一下。

## 增量式优化

有了上面的第一版之后，我们可以随时“**增量式**（*progressively*）”更新和优化这个对话系统，比如：
* 如果我们对交互的流程和样式不满意，就优化增强 `Bot` 父类的 `run()` 方法；
* 如果我们需要更多对话内容，就定义更多不一样的 `Bot` 子类；
* 如果想增加对话系统级的操作，比如提供系统帮助指令，可以修改 `Garfield` 和 `Bot` 类的 `run()` 方法来实现。

这里我们给出两个增强的例子。第一个是给程序输出的对话加上颜色，以区分用户输入的内容，这个修改只涉及 `Bot` 父类的 `run` 方法（这就是责任分离的好处）。

要给 Python `print()` 输出的文字加上颜色和其他格式，可以使用命令行特定的格式串，自己做起来有点复杂，我们用一个第三方的模块 `termcolor`，首先打开你的命令行界面运行 `pip install termcolor`，然后修改 `Bot` 父类定义如下：


```python
from time import sleep
from termcolor import colored

class Bot:

    wait = 1

    def __init__(self):
        self.q = ''
        self.a = ''

    def _think(self, s):
        return s

    def _format(self, s):
        return colored(s, 'blue')

    def run(self):
        sleep(Bot.wait)
        print(self._format(self.q))
        self.a = input()
        sleep(Bot.wait)
        print(self._format(self._think(self.a)))
```

我们增加了一个内部方法 `_format` 来对程序输出的文字进行格式化，在其中调用 `termcolor` 提供的 `colored` 函数来对字符串上色。程序的其他部分不需要改变，所有 `Bot` 子类继承父类自动得到这一新特性，你可以重新运行程序来检验一下。

第二个例子是做一个新的 `Bot` 子类，可以对用户输入的一个四则运算表达式进行计算求值，也就是把对话系统变成一个计算器，你可以试着自己做一做，提示是：可以借助第三方库来进行表达式求值，比如这个 [simpleeval](https://pypi.org/project/simpleeval/)。

下面是我们的参考答案。

首先在命令行界面执行 `pip install simpleeval`，然后增加一个 `Bot` 子类的定义如下：


```python
from simpleeval import simple_eval

class CalcBot(Bot):
    def __init__(self):
        self.q = "Through recent upgrade I can do calculation now. Input some arithmetic expression to try:"

    def _think(self, s):
        result = simple_eval(s)
        return f"Done. Result = {result}"
```

然后创建 `Garfield` 实例试试：


```python
garfield = Garfield(1)
garfield.add(HelloBot())
garfield.add(CalcBot())
garfield.run()
```

    This is Garfield dialog system. Let's talk.
    
    Hi, what is your name?
    

     Neo
    

    Hello Neo
    [34mThrough recent upgrade I can do calculation now. Input some arithmetic expression to try:[0m
    

     24*42
    

    [34mDone. Result = 1008[0m
    

是不是比想象的还要简单啊？

## 作业

课程练习 #1 的目标是在上述成果基础上完成下面两个作业。

**作业一**：改进最后这个 `CalcBot`，让它可以反复执行，用户可以一直输入算术表达式求值，直到用户输入 `x` `q` `exit` 或者 `quit` 为止，才跳到下一个 *bot* 执行。

提示：这个需求改变了 `run()` 方法的基本流程，所以需要在 `CalcBot` 中重新实现一个 `run()` 方法，覆盖掉父类中 `run()` 的实现；也可以通过修改父类 `Bot` 实现，但要注意，不要影响已经完成的功能。

**作业二**：创建一个自己设计的 bot 并将其整合到整个系统中。

提示：这个新的 bot 应该是 Bot 类的子类；可以用上面示范的方法将其装载到对话系统中（`Garfield` 或者自行定义的类）；这个 bot 的话题应该和已有 bot 都不一样，而且实现机制越独特得分越高。

## 小结

我们在本章实例中运用的程序设计方法叫做 *UDB*（[Understand, Design and Build](https://lob.com/blog/understand-design-build-a-framework-for-problem-solving)）：
* *Understand*：深入理解问题，探求问题的本质，聚焦关键问题；
* *Design*：通过初步尝试了解问题的基本解决方案，并作出责任分离的模块化设计，包括各个模块的职责、实现思路以及将各模块粘合起来的方法；
* *Build*：编写代码实现每个模块，一边构建一边测试；尽快完成第一个版本，然后增量化持续改进。

对于所有要解决问题的探索，这个方法基本都适用，格外适合编程，差别仅在问题的规模（难度和复杂度），你可以多多体会这个方法的思维模式，平时多尝试。

本章还运用了前面学到的几乎所有东西，如果需要可以回到前面重新温习，直到可以顺利的完成本章的各个部分。
