Title: Pelican文档（3）——撰写内容
Date: 2020-9-23 18:00
Author: SunHao
Tags: pelican, documentation
Category: Documentation
Translate: True
Summary: 这是Pelican英文文档中文翻译的第**一**部分。Pelican是一个基于Python的静态网页生成器，它的名字来源于calepin的相同字母异序词，它在法语中是“笔记本”的意思。

# 文章和页面
Pelican把“文章”看成是依赖时序的内容，比如说blog中的投稿，所以它将每一篇文章和一个时间相联系。

“页面”背后的意思是他们通常不是临时的，而且被用于那些不会经常修改的内容（比如说“About”或者“Contact”页面）。

# 文件元数据
Pelican尽量做到足够智能以便从文件系统（比如说关于你文章的分类）中获取它需要的信息，但是有些信息还是需要你在文件中以元数据的形式给出。

如果你是用reStructuredText格式撰写你的内容，你可以在文本文件里用下面的语法提供这些元数据信息（文件后缀名为`.rst`）：

```
My super title
##############

:date: 2010-10-03 10:20
:modified: 2010-10-04 18:40
:tags: thats, awesome
:category: yeah
:slug: my-super-post
:authors: Alexis Metaireau, Conan Doyle
:summary: Short version for index and feeds
```

Author和tag列表可以用分号隔开，这样你的作者和标签内容就可以包含逗号了：

```
:tags: pelican, publishing tool; pelican, bird
:authors: Metaireau, Alexis; Doyle, Conan
```

Pelican包含一个支持HTML标签`abbr`的reStructureedText的扩展，你可以在你的投稿中写上下面的内容启用它：

```
This will be turned into :abbr:`HTML (HyperText Markup Language)`.
```

你也可以使用Markdown语法（文件后缀名为`.md`，`markdwon`，`mkd`或者`.mdown`）。生成Markdown首先要求你明确安装了[Python-Markdown](https://github.com/Python-Markdown/markdown)包，这可以通过`pip install Markdwon`命令完成。

Pelican也支持[Markdown Extensions](https://python-markdown.github.io/extensions/)，如果他们没有被包含在默认的`Markdown`包内你可以单独安装，他们可以用`MARKDOWN`设置进行配置和加载。

Markdwon投稿的元数据语法应该遵循以下格式：

```
Title: My super title
Date: 2010-12-03 10:00
Modified: 2010-12-05 19:30
Category:Python
Tags:pelican, publishing
Slug: my-super-post
Authors: Alexis Metaireau, Conan Doyle
Summary: Short version for index and feeds

This is the content of my super blog post.
```

你也可以为了自己使用方便定义自己的元数据关键词（只要他们和保留的元数据关键词不冲突）。下面的表包含了所有的保留元数据关键词：

|元数据|描述|
|----|----|
|`title`|文章或页面的标题|
|`date`|公开时间（`YYYY-MM-DD HH:SS`）|
|`modified`|修改时间（`YYYY-MM-DD HH:SS`）|
|`tags`|内容的标签，用逗号分开|
|`keywords`|内容的关键词，用逗号分开（只适用于HTML内容）|
|`category`|内容的分类（只能有一个）|
|`slug`|URLs和翻译的标识|
|`author`|内容的作者，只能写一个|
|`authors`|内容的作者，可以写多个|
|`summary`|内容所在页面的简单描述|
|`lang`|内容的语言ID（`en`，`fr`等）|
|`traslation`|如果内容是作者的翻译（`true`或`false`）|
|`status`|内容的状态：`draft`，`hidden`或`published`|
|`template`|用来生成内容的模板名字（没有拓展）|
|`save_as`|以相对路径保存内容|
|`url`|这一文章/页面的URL|

读者可以通过插件添加额外的格式（比如说[AsciiDoc](https://www.methods.co.nz/cgi-sys/suspendedpage.cgi)），详情参考[pelican-plugins](https://github.com/getpelican/pelican-plugins)仓库。

Pelican也能够处理以`.html`和`.htm`为后缀的HTML文件。Pelican以一种非常直接的方式解释HTML，从`meta`标签中读取元数据，从`title`标签中读取标题，从`body`标签中读取主体：

```
<html>
    <head>
        <title>My super title</title>
        <meta name="tags" content="thats, awesome" />
        <meta name="date" content="2012-07-09 22:28" />
        <meta name="modified" content="2012-07-10 20:14" />
        <meta name="category" content="yeah" />
        <meta name="authors" content="Alexis Métaireau, Conan Doyle" />
        <meta name="summary" content="Short version for index and feeds" />
    </head>
    <body>
        This is the content of my super blog post.
    </body>
</html>
```

HTML中有一个元数据比较特殊：标签可以用`tags`元数据定义，就像Pelican的标准用法一样，也可以用`keywords`元数据定义，就像HTML的标准用法一样。这两种用法可以交替使用。

需要注意的是，除了标题，所有元数据都不是必须的：如果时间没有给出，并且`DEFAULT_DATE`被设置为`'fs'`，Pelican将会采用文件的“mtime”时间戳，而分类可以用文件所在的文件夹决定。比如说一个位于`python/foobar/myfoobar.rst`的文件将会被赋予`foobar`的分类。如果你想按照其他的方式组织你的文件，而文件夹不是一个合适的分类名，你可以在设置里把`USE_FOLDER_AS_CATEGORY`设为`False`。当句法分析时间在页面元数据中给出，Pelican支持W3C的[suggested subset ISO 8601](https://www.w3.org/TR/NOTE-datetime)。

所以标题是唯一要求的元数据。如果你觉得这样很烦，别担心。除了每次都在元数据里给出标题，你还可以用内容的源文件名作为标题。比如说，一个文件名为`Publishing via Pelican.md`的Markdown文件将会自动获得标题*Publishing via Pelican*。如果你更喜欢这种方式，可以在设置文件里加上这样的内容：

```
FILENAME_METADATE = `(?P<title>.*)`
```

`modified`应该是你最后一次更新文章的时间，而且是你没有指明`date`时的默认值。你可以在内容里显示`modified`。当你修改文章后把`modiified`设为当前时间，订阅读者的相关订阅条款会自动更新。

`authors`是一个以逗号分隔的文章作者的列表，如果只有一个作者你可以用`author`。

如果你在一次投稿中不明确给出摘要，`SUMMARY_MAX_LENGTH`设置可以被用来设置从文章开头截取多少字作为摘要。

你还可以从通过`FILENAME_METADATA`设置从文件名中获取其他的元数据。文件名中配对的数据将会被设为元数据对象。`FILENAME_METADATA`的默认值是只从文件名中获取日期。比如说，如果你想从文件名同时获取日期和slug，你可以将其设置为`'(?P<date>\d{4}-\d{2}-\d{2})_(?P<slug>.*)'`。

请记住，文件内可用的元数据对从文件名中获取的元数据具有优先权。

# 页面
如果你在content文件夹内创建了一个名为`pages`的文件夹，所有其中的文件都会被用来生成静态页面，比如说**About**或**Contact**页面。（见下面的文件系统配置例子。）

你可以用`DSIPLAY_PAGES_ON_MENU`设置来控制是否将这些页面都显示在主页的导航菜单中。（默认值是`True`。）

如果你不想把某些页面显示在导航栏中，就在内容的元数据中添加`status: hidden`。这对于为你的站点创建错误页面很有用。

# 静态内容
静态内容是除了文章和页面之外的内容，它们不需要处理而是直接复制到output文件夹中。你可以用`pelicanconf.py`文件中的`STATIC_PATHS`设置控制复制哪些静态文件。Pelican的默认设置是`image`目录，但是其他的必须要手动添加。如此之外其他需要明确关联的静态文件包括（见下文）

## 相同文件夹内的混合内容
从Pelican3.5开始，静态文件可以安全地和页面源文件共享同一个目录，而不会把页面源文件暴露在生成的站点中。任意一个这样的目录都要同时被加入`STATIC_PATHS`和`PAGE_PATH`（或`STATIC_PATHS`和`ARTICLE_PATHS`）中。Pelican将会正常地识别和处理页面源文件，如果其他文件都在一个保存静态文件的单独目录内，Pelican会把剩余的文件复制到输出中。

注意：把静态文件和内容源文件放在相同的源目录内并不能保证他们最终将会出现在生成的站点中的同一位置。做到这一点的最简单方法时用`{attach}`链接语法（下面会说）。除此之外，`STATIC_SAVE_AS`，`PAGE_SAVE_AS`和`ARTICLE_SAVE_AS`设置（和对应的`*_URL`设置）能够被用来设置将不同种类的文件保存在一起，就像他们在Pelican早期版本中的作用一样。

## 内部内容之间的链接
从Pelican3.1开始，可以将站点内的链接在内容文件夹内进行链接，而不用在生成的站点的文件夹内进行链接。这使得我们从当前的投稿链接到另一个可能与之相关的投稿更加简单（不需要知道被链接的内容在被生成的站点内的位置）。

为了链接内部的内容（`content`目录内的文件），可以使用下面的语法链接到目标文件：`{filename}path/to/file`。注意：用来分隔目录层级的反斜杠`/`在所有操作系统内都一样，包括Windows。

比如说，一个Pelican项目可能具有如下的结构：

```
website/
├── content
│   ├── category/
│   │   └── article1.rst
│   ├── article2.md
│   └── pages
│       └── about.md
└── pelican.conf.py
```
在这个例子里，`article.rst`可以这么写：

```
The first article
#################

:date: 2012-12-01 10:02

See below intra-site link examples in reStructuredText format.

`a link relative to the current file <{filename}../article2.md>`_
`a link relative to the content root <{filename}/article2.md>`_
```

而`article2.md`可以这么写：

```
Title: The second article
Date: 2012-12-01 10:02

See below intra-site link examples in Markdown format.

[a link relative to the current file]({filename}category/article1.rst)
[a link relative to the content root]({filename}/category/article1.rst)
```

## 链接到静态文件
你可以用`{static}path/to/file`链接到静态内容。被这一语法链接的文件将会自动被复制到输出目录里，即使包含他们的源目录没有被包含在项目设置文件`pelicanconf.py`中的`STATIC-PATH`里。

比如说，你个项目的内容目录可能有如下的结构：

```
content
├── images
│   └── han.jpg
├── pdfs
│   └── menu.pdf
└── pages
    └── test.md
```

`test.md`可以被写成：

```
![Alt Text]({static}/images/han.jpg)
[Our Menu]({static}/pdfs/menu.pdf)
```

站点生成时将会把`han.jpg`复制到`output/images/han.jpg`，把`menu.pdf`复制到`output/pdfs/menu.pdf`。

如果你使用`{static}`链接一个文章或页面，将会链接到它的源代码。

## 绑定静态文件
从Pelican3.5开始，静态文件可以通过`{attach}path/to/file`将静态文件“绑定”到一个页面或文章。这一命令和`{static}`的语法很像，但是它把静态文件和链接它的文件放在输出目录的相同位置。如果最初的静态文件位于链接它的源文件的一个子目录里，这一关系将会在输出中保留。否则它将会和链接它的文件位于同一目录下。

这一命令只对静态文件有效。

比如说，一个项目的内容目录可能具有如下的结构：

```
content
├── blog
│   ├── icons
│   │   └── icon.png
│   ├── photo.jpg
│   └── testpost.md
└── downloads
    └── archive.zip
```

`pelicanconf.py`将会包括：

```
PATH = 'content'
ARTICLE_PATHS = ['blog']
ARTICLE_SAVE_AS = '{date:%Y}/{slug}.html'
ARTICLE_URL = '{date:%Y}/{slug}.html'
```

`testpost.md`将会包括：

```
Title: Test Post
Category: test
Date: 2014-10-31

![Icon]({attach}icons/icon.png)
![Photo]({attach}photo.jpg)
[Downloadable File]({attach}/downloads/archive.zip)
```

生成站点时产生的输出目录具有如下的结构：

```
output
└── 2014
    ├── archive.zip
    ├── icons
    │   └── icon.png
    ├── photo.jpg
    └── test-post.html
```

注意，用`{attach}`链接的文件最终或者会和输出的文章位于同一目录，或者位于输出文章所在目录的子目录中。

如果一个文件被链接多次，`{attach}`的改变位置的特性只有在这些链接的第一个中才会被处理。第一次链接之后，Pelican将会把`attach`当成`{static}`处理。这避免了破坏之前处理的链接。

**当同时有多个文件都链接同一个文件时一定要小心**：因为第一次链接将会决定被链接文件的最终位置，而Pelican并不会区分文件被处理的顺序，被链接到多个文件的文件有可能在不同的编译之后位于不同的位置。（这在现实中是否会发生取决于操作系统，文件系统，Pelican的版本和项目中增加、修改或删除的文件。）那么链接到文件之前旧的位置的链接就会失效。所以明智的做法是只有当链接到同一个文件的多个链接文件位于同一目录里时才用`{attach}`。在这一条件下，文件的输出位置不会在将来的编译中改变。对于没有这么做的情况，可以考虑用`{static}`而不是`{attach}`，并且用项目的`STATIC_SAVE_AS`和`STATIC_URL`设置决定输出文件的位置。

注意：当使用`{attach}`的时候，`*_URL`/`*_SAVE_AS`设置里的所有父目录都要互相匹配。见[URL设置](https://docs.getpelican.com/en/stable/settings.html#url-settings)。

## 链接作者，分类，索引和标签
你可以用`{author}name`，`{category}foobar`，`{index}`和`{tag}tagname`语法链接作者，分类，索引和标签。

## 极不推荐的内部链接语法
为了和更早的版本相比较，Pelican在内部链接中除了花括号(`{}`)仍然支持竖括号（`||`）。比如说：`|filename|an_article.rst`，`|tag|tagname`，`|category|foobar`。将`||`换成`{}`是为了避免和Markdown扩展或reST命令的冲突。相似地，Pelican仍然支持用`{filename}`链接静态内容。语法变为`{static}`是为了既能在生成的文章和页面之间，也能在他们的静态源文件之间链接。

对旧语法的支持最终会被删除。

## 包含其他文件
Markdown和reStructuredText都维持提供了机制。

下面是**reStructuredText**用[the include directive](https://docutils.sourceforge.io/docs/ref/rst/directives.html#include)命令的几个例子：

```
.. include:: file.rst
```

包含一个文件里由两个标识符界定的部分，比如说C++文件（按照行号分隔也是可以的）：

```
.. include:: main.cpp
    :code: C++
    :start-after: // begin
    :end-before: // end
```

包含一个未经处理的HTML文件（或者一个inline SVG）并且将它不经处理直接输出：

```
.. raw:: html
    :file: table,html
```

至于**Markdown**，必须要依赖于拓展，比如说用[mdx_include plugin](https://github.com/neurobin/mdx_include)：


    ```html
    {! template.html !}
    ```

# 导入一个存在的站点
使用简单的脚本就可以将你在Wordpress，Tumblr，Dotclear和RSS feeds的站点导入Pelican。见[Importing an existing site](https://docs.getpelican.com/en/stable/importer.html#import)。

# 翻译
翻译文章是可能的，为此你需要为你的文章/页面添加一个`lang`元数据，并且设置好`DEFAULT_LANG`（它的默认值是英语[en]）。这些都设置好了之后，只有拥有默认语言的文章将会被列出来，而且每一篇文章都会有一个可用的翻译版本的列表。

注意：Pelican的核心功并不能为每一中语言的翻译版本都生成子站点（例如`example.com/de`），这样的高级功能可以用[i18n_subsites plugin](https://github.com/getpelican/pelican-plugins/tree/master/i18n_subsites)实现。

Pleican默认使用文章的URL“slug”来确定是是否两篇或多篇文章是彼此的翻译版本。（这可以通过`ARTICLE_TRANSLATION_ID`设置改变。）slug可以在文件的元数据中设置；如果没有明确设置，Pelican将会从文章的标题中自动生成slug。

下面是两个例子，一个是英语一个是法语。

英语文章：

```
Foobar is not dead
##################

:slug: foobar-is-not-dead
:lang: en

That's true, foobar is still alive!
```

法语文章：

```
Foobar n'est pas mort !
#######################

:slug: foobar-is-not-dead
:lang: fr

Oui oui, foobar est toujours vivant !
```

尽管文章不同，但是你可以看出来两文章的slug是相同的，所以它被当做是标识符。如果你不喜欢这样明确定义，你必须保证翻译文章和原文章的标题一致，因为slug将会根据文章标题自动生成。

如果你不希望某一篇文章的原版本被`DEFAULT_LANG`探测为翻译版本，你可以用`translation`元数据标明哪一篇是翻译版本：

```
Foobar is not dead
##################

:slug: foobar-is-not-dead
:lang: en
:translation: true

That's true, foobar is still alive!
```

# 语法高亮
Pelican能够为你的代码块提供带颜色的语法高亮。为了做到这一点你可以在你的内容文件里采取以下约定。

对于reStructuredText，使用`code-block`指令来指明需要高亮的代码类型（在这些例子里，我们使用`python`）：

```
.. code-block:: python

   print("Pelican is a static site generator.")
```

对于Markdown，需要使用[CodeHilite extension](https://python-markdown.github.io/extensions/code_hilite/#syntax)，需要在代码块的上方将语言标识符和代码使用相同的缩进：

```
There are two ways to specify the identifier:

    :::python
    print("The triple-colon syntax will *not* show line numbers.")

To display line numbers, use a path-less shebang instead of colons:

    #!python
    print("The path-less shebang syntax *will* show line numbers.")
```

知名的标识符（如`python`，`ruby`）应该和[list of available lexers](https://pygments.org/docs/lexers/)中给出的一致。

当使用reStructuredText时，代码块指令的可用参数如下：

|Option|Valid values|Description|
|-|-|-|
|anchorlinenos|N/A|If present wrap line numbers in <a> tags.|
|classprefix|string|String to prepend to token class names|
|hl_lines|numbers|List of lines to be highlighted, where line numbers to highlight are separated by a space. This is similar to `emphasize-lines` in Sphinx, but it does not support a range of line numbers separated by a hyphen, or comma-separated line numbers.|
|lineanchors|string|Wrap each line in an anchor using this string and -linenumber.|
|linenos|string|If present or set to “table” output line numbers in a table, if set to “inline” output them inline. “none” means do not output the line numbers for this table.|
|linenospecial|number|If set every nth line will be given the ‘special’ css class.|
|linenostart|number|Line number for the first line.|
|linenostep|number|Print every nth line number.|
|lineseparator|string|String to print between lines of code, ‘n’ by default.|
|linespans|string|Wrap each line in a span using this and -linenumber.|
|noblockground|N/A|If set do not output background color for the wrapping element.|
|nowrap|N/A|If set do not wrap the tokens at all.|
|tagsfile|string|ctags file to use for name definitions.|
|tagurlformat|string|format for the ctag links.|

注意，根据不同的版本，Pygments模块可能不支持上说所有的选项。各个选项的更多细节可以参考[Pygments documentation](https://pygments.org/docs/formatters/)的*HtmlFormatter*章节。

比如说，下面的代码块启用了行号，从153开始，并且在Pygments CSS类之前设置了前缀*pgcss*使得名字更加特别从而避免了可能的CSS冲突：

```
.. code-block:: identifier
    :classprefix: pgcss
    :linenos: table
    :linenostart: 153

   <indented code block goes here>
```

也可以在Pelican的设置文件里设置`PYGMENTS_RST_OPTIONS`变量来包含你想在每一个代码块中自动应用的选项。

比如说，如果你想在每一个代码块中都启用行号和一个CSS前缀，你可以将这一变量设置为：

```
PYGMENTS_RST_OPTIONS = {'classprefix': 'pgcss', 'linenos': 'table'}
```

如果在特定代码块中标明了别的设置，那么这一设置将会覆盖设置文件里的默认设置。

# 发布草稿
如果你想把某一文章或页面作为草稿发布（比如说在正式发布前让朋友检查一下），你可以在元数据里添加`Status: draft`。这篇文章会输出到`drafts`文件夹并且不会在主页和任何分类、标签下被列出来。

如果你的文章都想默认作为草稿发布（避免在文章完成前不小心发布）你可以在`DEFAULT_METADATA`添加：

```
DEFAULT_METADATA = {
    'status': 'draft',
}
```

发布一篇默认状态时`draft`的文章，可以在文章的元数据中更新`Status: published`。



