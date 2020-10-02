Title: Pelican文档（1）——快速开始
Date: 2020-9-14 10:20
Author: SunHao
Tags: pelican, documentation
Category: Documentation
Translate: True
Summary: 这是Pelican英文文档中文翻译的第**一**部分。Pelican是一个基于Python的静态网页生成器，它的名字来源于calepin的相同字母异序词，它在法语中是“笔记本”的意思。

我们强烈建议通读所有的文档，但是对实在没有耐心的人，接下来是一些快速开始的步骤。

# 安装
在Python 3.6+版本中安装Pelican（如果需要也可以选择同时安装Markdown）可以通过在你喜爱的终端里运行以下命令，有可能需要`sodo`给与权限：
    
```python -m pip install "pelican[markdown]"```

# 创建一个项目
首先给你的项目选择一个名称，并为你的站点创建一个命名合适的目录，然后跳转到这个目录：

    mkdir -p ~/projects/yoursite
    cd ~/projects/yoursite

通过`pelican-quickstart`命令创建一个框架程序，这一命题的运行方式是询问一些关于你的站点的问题：
    
    pelican-quickstart

那些有默认值的问题都将默认值标记在了括号里，你可以自由地选择用敲击回车键的方式接受这些默认值。当想你询问URL前缀时，请键入你的域名（例如`https://example.com`）。

# 创建一篇文章
在你创建一些内容之前Pelican是无法运行的。有你喜爱的文本编辑器按一下的内容创建你的第一篇文章：
    
    Title: My First Review
    Date: 2010-12-03 10:00
    Category: Review

    Following is a review of my favorite mechanical keyboard.

给予这篇示例文章Markdown的格式，并且保存为`~/prjects/yoursite/content/keyboard-review.md`

# 生成你的站点
在你的站点的根目录下运行`pelican`命令来生成你的站点：

    pelican content

你的站点现在已经在`output`目录下生成了。（你也许会看到一个和反馈有关的警告，但是这是在本地运行时的正常现象，并且可以暂时忽略。）

# 预览你的站点
打开一个新的终端，进入你的项目的根目录，运行以下命令启动Pelican的web服务器：

    pelican --listen

用你的浏览器访问http://localhost:8000/ 来预览你的站点。

继续阅读文档的其他章节可以获得更多细节，并且浏览Pelican wiki的指导页面可以获取社区中的相关教程。