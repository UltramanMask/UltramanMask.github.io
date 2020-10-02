Title: Pelican文档（4）——发布你的站点
Date: 2020-9-23 18:00
Author: SunHao
Tags: pelican, documentation
Category: Documentation
Translate: True
Summary: 这是Pelican英文文档中文翻译的第**一**部分。Pelican是一个基于Python的静态网页生成器，它的名字来源于calepin的相同字母异序词，它在法语中是“笔记本”的意思。

# 站点生成
一旦Pelican安装完成并且你拥有了一些内容（比如Markdown或reST格式的文件），你可以通过`pelican`命令将你的内容转化为HTML，你也可以指明你的内容的路径和（可选择地）你的设置文件的路径：

```
pelican /path/to/your/content/ [-s path/to/your/settings.py]
```

以上命令将会用默认的主题生成一个简单的站点并且将其保存在`output/`文件夹内。默认主题由不包含任何装饰的简单HTML组成，你可以以它为基础创建自己的主题。

当处理单个文章或页面时，可以只生成这一内容对应的文件。为了做到这一点，使用`--write-selected`参数，比如说：

```
pelican --write-selected output/posts/my-post-title.html
```

注意你需要指明的是生成的*output*文件的路径，而不是源文件的路径。为了决定输出文件的内容和位置，可以使用`--debug`标志。如果需要，`--write-selected`后面可以跟多个以逗号分开的路径，或者也可以在设置文件中配置。（见[Writing only selected content](https://docs.getpelican.com/en/stable/settings.html#writing-only-selected-content)）

你也可以告诉Pelican留意你的更改，而不是每次修改后都要手动再运行。为了做到这一点，在运行`pelican`时加上`-r`或者`--autoload`选项。再非Windows环境下，这一选项也可以和`-l`或`--listen`选项结合使用，以在自动重生成站点的同时将其部署到[http://localhost:8000](http://localhost:8000):

```
pelican --autoreload --listen
```

Pelican还有很多其他可用的选项，你可以用以下命令查看所有选项：

```
pelican --help
```

## 预览生成的站点
Pelican生成的文件是静态文件，事实上你不需要任何特殊的东西在预览他们，你可以用你的浏览器直接打开生成的HTML文件：

```
firefox output/index.html
```

因为上述方式可能在定位你的CSS和其他相关内容是出问题，所以运行Pelican内置的网页服务器经常能提供更可靠的预览体验：

```
pelican --listen
```

一旦启动了网页服务器，你就可以在[http://localhost:8000](http://localhost:8000)预览你的站点。

# 部署
当你生成你的站点并且在本地开发环境中预览之后，你就准备好将它部署到产品中了。你可以用任何你之前可能定义过的与产品有关的设置重新生成你的站点（比如说analytics feeds等）：

```
pelican contents -s publishconf.py
```

如果你想在`pelicanconf.py`的基础上建立你的发布配置，你可以在`publishconf.py`中加入以下代码把`pelicanconf.py`中的设置引进其中：

```
from pelicanconf import *
```

如果你已经用`pelican-quickstart`生产了`publishconf.py`，那么这条代码已经默认包含了。

部署站点的具体步骤依赖于你把你的网站托管在什么平台上。如果你有运行Ngnix或Apache的服务器的SSH权限，你可以用`rsync`工具转化你的站点文件：

```
rsync -avc --delete output/ host.example.com:/var/www/your-site/
```

还有很多其他的部署选项，其中有一些在你第一次用`pelican-quickstart`配置站点时就已经设置好了。在[Tips](https://docs.getpelican.com/en/stable/tips.html)页面你看以看到将网站托管到Github Pages的一些细节。

# 自动化
尽管Pelican命令是生成站点的经典方式，自动工具可以让你的站点生成更加生产线化。在`pelican-quickstart`过程中的一个问题是你是否像自动化你的站点生成和发布。如果你的回答是的话，在你的项目的根目录下会生成`tasks.py`和`Makefile`，这些文件以及在`pelican-quickstart`过程中的其他问题的答案给出的信息只是一个起点，而你需要进一步地定制以使其更符合你的使用习惯。如果你发现这些自动化工具中的一个或多个作用有限，你随时可以把这些文件删除而不会影响到经典的`pelican`命令。

接下来是一些能够“封装”`pelican`命令的自动化工具，他们能够简化站点生成、预览和上传的步骤。

## Inboke
[Invoke](https://www.pyinvoke.org/)的优点是它是用Python写的，所以它的使用范围非常广。它的缺点是它需要单独安装。使用下面的命令安装Invoke，如果你的环境要求则需要前缀`sudo`：

```
python -m pip install invoke
```

打开在你的项目根目录下胜场的`task.py`。你将会看到很多命令，其中任何一个都能被重命名、删除和/或按你的喜爱定制。使用out-of-the-box配置，你可以通过以下命令生成你的站点：

```
invoke build
```

如果你喜欢在每一次改变被探测时都让Pelican自动生成你的站点（这在本地测试时非常方便），使用下面的命令：

```
invoke regenerate
```

为你的站点提供服务器，从而使它能在[http://localhost:8000/](http://localhost:8000/)：

```
invoke serve
```

为了使你的浏览器能在每次发生改变时重载你的站点，首先`python -m pip install livereload`，然后使用下面的命令：

```
invoke livereload
```

在`pelican-quickstart`过程中如果你对你是否像用SSH上传你的站点这一问题的回答是“是”的话，你可以通过以下命令用SSH发布你的站点：

```
invoke publish
```

这些只是一些默认命令，所以你可以自由的探索`task.py`，看看其他可用的命令。更重要的是，一定要按照你自己的需要和偏爱定制`task.py`。

## Make
在`pelican-quickstart`过程中如果你对相关问题回答是的话，一个`Makefile`也被自动生成了。这一方式的优点是`make`命令在大多数POSIX系统中内置了，所以不需要额外安装任何东西就能使用它。缺点是非POSIX系统（比如说Windows）没有包含`make`，而在这些系统中安装它并不是一个简单的任务。

如果你想通过`make`用`pelicanconf.py`中的设置生成你的站点，运行：

```
make html
```

使用`pelicanconf.py`中的设置发布你的站点，运行：

```
make publish
```

如果你喜欢在每一次改变被探测时都让Pelican自动生成你的站点（这在本地测试时非常方便），使用下面的命令：

```
make regenerate
```

为你的站点提供服务器，从而使它能在[http://localhost:8000/](http://localhost:8000/)：


```
make serve
```

通常你需要在两个终端里分别运行`make regenerate`和`make serve`，但是也可以在一个终端里运行他们两个：

```
make deserve
```

上述命令将会同时运行Pelican里的重生成命令和将输出预览在[http://localhost:8000/](http://localhost:8000/)的命令。


当你准备发布你的站点时，你可以通过你在`pelican-quickstart`的提问环节中选择方式上传。比如说，你可以使用rsync（SSH）：

```
make rsync_upload
```

就是这样，现在你的站点应该就活了！

（默认的`Makefile`和`devserver.sh`脚本使用`python`和`pelican`来完成他们的任务。如果你想用不同的命令，比如说`python3`，你可以分别设置`PY`和`PELICAN`环境变量覆盖默认的命令名。
）
