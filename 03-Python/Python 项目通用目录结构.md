---
title: Python 项目通用目录结构
subtitle: 
author: Lepeng
catalog: true
comments: true
header-img: img/header_img/archive-bg.jpg
tags:
- Python
categories:
- Python
date: 2021-07-30
link: 
- https://stackoverflow.com/questions/193161/what-is-the-best-project-structure-for-a-python-application
- https://blog.ionelmc.ro/2014/05/25/python-packaging/
---

# 目录结构

Stackoverflow 上有人提出了[这个问题](http://stackoverflow.com/questions/193161/what-is-the-best-project-structure-for-a-python-application)上，大家对Python目录结构有很多自己的看法，也有很多共识，结合这么多年的开发经验，我总结了自己的项目结构。

假设你的项目名为 projectname, 建议的目录结构如下:

```
Projectname/
|-- bin/   # 放使用命令行 执行的脚本
|   |-- projectname.exe
|
|-- lib/  # C-language libraries
|
|-- projectname/  
|   |-- __init__.py
|   |-- main.py
|   |-- tests/
|   |   |-- __init__.py
|   |   |-- test_main.py
|
|-- docs/
|   |-- conf.py  # 配置文件
|   |-- abc.rst
|
|-- apidoc/
|
|-- setup.py
|-- requirements.txt
|-- README.md
|-- LICENSE.txt
```

# 项目名或者目录名

0.  `目录名`：将目录命名为与你的项目相关的名称。例如，项目名为`Twisted`，将源文件的目录命名为 `Twisted`。当你发布时，你应该包含一个版本号后缀：Twisted-2.5
    1.  如果您的项目是单个 Python 源文件，则将其放入目录中，并将其命名为与您的项目相关的名称。例如，`Twisted/twisted.py`。
    2.  如果您需要多个源文件，建议创建一个包（`Twisted/twisted/` 和 `Twisted/twisted/__init__.py`）并将源文件放入其中。例如 `Twisted/twisted/main.py`


# 顶级目录

1. `源代码存放目录`: 源代码中的所有模块、包都应该放在此目录而不要置于顶层目录，程序的入口建议命名为 `main.py`。源代码目录有多种命名方式：
    - 将源代码目录命名`项目名称`，如上面提到的 `Twisted/twisted/`
    - 将源代码目录命名为`src`
    - Python 不像 Java 或 C 那样区分/src、/lib 和 /bin，某些人认为 /src 作为顶级目录毫无意义，因此这个目录可以是应用程序的顶级架构，如下：
        - /foo
        - /bar
        - /baz
    - 将源代码目录命名 `apps` 
    这个需要名字需要仁者见仁智者见智，不同的情况使用不同的命名。

2.  `bin/` 或者 `script/`: 存放项目的一些可执行文件。
    里面的文件不要给它们 `.py` 扩展名，即使它们是 Python 源文件。除了导入和调用项目中其他地方定义的主函数外，不要在其中放置任何代码。
    注释：因为在 Windows 上，解释器是由文件扩展名选择的，所以 Windows 用户需要 .py 扩展名。所以，当你为 Windows 打包时，你可能想要添加它。不幸的是，没有简单的 distutils 技巧我知道要自动化这个过程。考虑到在 POSIX 上，.py 扩展名只是一个名，而在 Windows 上，缺少的是一个实际的错误，如果您的用户群包括 Windows 用户，您可能希望选择只拥有 .py到处扩展。

3.  `docs/`: 存放一些文档。

4.  `apidoc`: 对于 Epydoc 生成的 API 文档。

5.  `lib`: 存放 c 语言相关的库。


# 二级目录

1. `测试目录`:
    将单元测试放在包的子包中（你总是需要至少一个文件来进行单元测试）。例如，`Twisted/twisted/test/`。比如将测试代码放在`Twisted/twisted/test/test_internet.py`


# 顶级文件

1.  `setup.py`: 安装、部署、打包的脚本。
    业界标准的写法是用 Python 流行的打包工具 setuptools 来管理这些事情。这种方式普遍应用于开源项目中。
    不过这里的核心思想不是用标准化的工具来解决这些问题，而是说，一个项目一定要有一个安装部署工具，能快速便捷的在一台新机器上将环境装好、代码部署好和将程序运行起来。
    setup.py可以将这些事情自动化起来，提高效率、减少出错的概率。"复杂的东西自动化，能自动化的东西一定要自动化。"是一个非常好的习惯。
    
2.  `requirements.txt`: 存放软件依赖的外部Python包列表。

3.  `README.md`: 项目说明文件。需要说明以下几个事项:
    - 软件定位，软件的基本功能。
    - 运行代码的方法: 安装环境、启动命令等。
    - 简要的使用说明。
    - 代码目录结构说明，更详细点可以说明软件的基本原理。
    - 常见问题说明。
    - 参考：https://github.com/pallets/flask/blob/master/README.rst

4.  `Makefile`: docker 文件

5.  `.gitignore`: git 文件


# 开源涉及的文件和目录

如果是开源项目，可能会增加如下顶级目录和顶级文件夹。

```
Projectname/
|-- .tx/        # 如果你使用Transifex进行国际化的翻译工作，创建此目录
|   |--config   # Transifex的配置文件
|
|-- projectname/
|   |--__init__.py
|
|-- AUTHORS       # 作者清单
|-- INSTALL       # 安装说明
|-- LICENSE       # 版权声明
|-- MANIFEST.in   # 装箱清单文件
|-- MAKEFILE      # 编译脚本
```

除此之外，有一些方案给出了更加多的内容。比如`LICENSE.txt`,`ChangeLog.txt`文件等。


# 如何解决多次目录下数据的导入？

比如：`main.py` 中导入 `docs/conf.py` 中的函数

```
from docs improt conf     # 这样的导入是不成功的
```

因为 `from` 导入的是该目录即 `src` 文件下的文件夹，docs 与 src 都是顶级目录，所以导入不成功。解决方案

```python
import os,sys


path = os.path.join(os.path.dirname(os.path.abspath(__file__))) 

# 临时添加搜索路径，这种方法导入的路径会在python程序退出后失效。
sys.path.append(path)
```

