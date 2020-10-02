Title: Pelican文档（2）——安装Pelican
Date: 2020-9-15 14:20
Author: SunHao
Tags: pelican, documentation
Category: Documentation
Translate: True
Summary: 这是Pelican英文文档中文翻译的第**二**部分。Pelican是一个基于Python的静态网页生成器，它的名字来源于calepin的相同字母异序词，它在法语中时“笔记本”的意思。

Pelican在Python3.6+上运行效果最好，更早版本的Python已经不被支持。

你可以通过几种不同的方式安装Pelican。最简单的方式是[pip](https://pip.pypa.io/en/stable/)：

```python -m pip install pelican```

或者如果你计划使用Markdown：

```python -m pip install "pelican[markdown]"```

（注意有些操作系统会要求你在上述命令前加上`sudo`前缀以为整个系统安装Pelican。）

尽管以上是最简单的方式，我们最推荐的方式是在安装Pelican之前用[virtualenv](https://virtualenv.pypa.io/en/latest/)为Pelican创建一个虚拟环境。假设你已经安装了[virtualenv](https://virtualenv.pypa.io/en/latest/)，你可以打开一个新的终端并且为Pelican创建一个虚拟环境：

```
virtualenv ~/virtualenvs/pelican
cd ~/virtualenvs/pelican
source bin/activate
```
一旦虚拟环境被创建并激活，Pelican可以通过上述`python -m pip install pelican`命令安装。如果你有项目的源代码，你也可以选择用distutils方法安装Pelican：

```
cd path-to-Pelican-source
python setup.py install
```

如果你安装了Git并且比起稳定发行版你更想要安装最新但不稳定的版本，使用以下命令：

    python -m pip install -e "git+https://github.com/getpelican/pelican.git#egg=pelican"


一旦Pelican安装完成，你可以运行`pelican --help`查看基本用法。更多的细节可以查阅[发布](https://docs.getpelican.com/en/stable/publish.html)章节。

# 可选的包
如果你计划用[Markdown](https://pypi.org/project/Markdown/)作为标记格式，你可以安装支持Markdown的Pelican版本：

```
python -m pip install "pelican[markdwon]"
```

你可以在设置文件里开启印刷增强，但首先你要安装[Typogrify](https://pypi.org/project/typogrify/)库：

```
python -m pip install typogrify
```

# 依赖关系
当Pelican安装完成时，下面的相关Python包就已经在不需要你参与的情况下自动安装了：

+ feedgenerator, to generate the Atom feeds
+ jinja2, for templating support
* pygments, for syntax highlighting
* docutils, for supporting reStructuredText as an input format
* pytz, for timezone definitions
* blinker, an object-to-object and broadcast signaling system
* unidecode, for ASCII transliterations of Unicode text utilities
* MarkupSafe, for a markup-safe string implementation
* python-dateutil, to read the date metadata

# 快速建立你的站点
一旦Pelican安装完成，你可以通过`pelican-quickstart`命令创建一个框架项目，这一命令在运行时会向你询问一些关于你的站点的问题：

```
pelican-quickstart
```

如果在激活的虚拟环境中运行，`pelican-quickstart`将会在`$VIRTUAL_ENV/.project`中寻找一个关联项目路径。如果那个文件存在并包含一个有效的目录路径，新的Pelican项目将会被保存在那个位置。否则默认保存在当前的工作目录。在初次调用时指定新项目路径可以使用：

```
pelican-quickstart --path /your/desired/directory
```

一旦你回答完所有问题，你的项目将会按照以下的结构组成（除了*pages*——在下面的圆括号里——如果你计划创建不依赖时序的内容可以自己添加。）：

```
yourproject
├─content
│  └─(pages)
├─output
├─task.py
├─makefile
├─pelicanconf.py    #Main settings file
├─publishconf.py    #Settings to use when ready to publish
```

下一步就是在已经为你创建的*content*文件夹里添加内容了。

