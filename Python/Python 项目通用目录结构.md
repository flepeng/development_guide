---
title: Python 项目通用目录结构.py
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

## 目录结构

关于如何组织一个较好的Python工程目录结构，已经有一些得到了共识的目录结构。

在Stackoverflow的[这个问题](http://stackoverflow.com/questions/193161/what-is-the-best-project-structure-for-a-python-application)上，能看到大家对Python目录结构的讨论。

假设你的项目名为 projectname, 建议的目录结构如下:

```
Projectname/
|-- bin/
|   |-- projectname
|
|-- projectname/
|   |-- __init__.py
|   |-- main.py
|
|-- tests/
|   |-- __init__.py
|   |-- test_main.py
|
|-- docs/
|   |-- conf.py  # 配置文件
|   |-- abc.rst
|
|-- apidoc/
|
|-- setup.py
|-- requirements.txt
|-- README
```

### 项目名或者目录名

0. `目录名`：将目录命名为与您的项目相关的名称。例如，如果您的项目名为“Twisted”，请将其源文件的顶级目录命名为Twisted。当你发布时，你应该包含一个版本号后缀：Twisted-2.5
    1. 如果您的项目是单个 Python 源文件，则将其放入目录中，并将其命名为与您的项目相关的名称。例如，`Twisted/twisted.py`。
    2. 如果您需要多个源文件，请改为创建一个包（`Twisted/twisted/`和`Twisted/twisted/__init__.py`）并将源文件放入其中。例如，`Twisted/twisted/internet.py`

### 顶级目录

1. `源代码存放目录`: 源代码中的所有模块、包都应该放在此目录，不要置于顶层目录，程序的入口最好命名为`main.py`。现在有四种方式：

    - 将源代码放在名为 src 或 lib 的目录中
    - 将源代码放在名为 foo 文件目录中
        Python不像 Java 或 C 那样区分/src、/lib 和 /bin，由于某些人认为 /src 作为顶级目录毫无意义，因此您的顶级目录可以是应用程序的顶级架构。
        - /foo
        - /bar
        - /baz
    - 将源代码放在名为 apps 的目录中
    - 将源代码放在名为 项目名称 的目录中，如上面提到的`Twisted/twisted/`
    
    仁者见仁智者见智吧

2. `bin/`: 存放项目的一些可执行文件，当然你可以起名`script/`之类的也行。
    不要给它们.py扩展名，即使它们是 Python 源文件。除了导入和调用项目中其他地方定义的主函数外，不要在其中放置任何代码。
    （注释：因为在 Windows 上，解释器是由文件扩展名选择的，所以 Windows 用户需要 .py 扩展名。所以，当你为 Windows 打包时，你可能想要添加它。不幸的是，没有简单的 distutils 技巧我知道要自动化这个过程。考虑到在 POSIX 上，.py 扩展名只是一个名，而在 Windows 上，缺少的是一个实际的错误，如果您的用户群包括 Windows 用户，您可能希望选择只拥有 .py到处扩展。）

3. `测试目录`:
    - 将单元测试放在包的子包中（你总是需要至少一个其他文件来进行单元测试）。例如，Twisted/twisted/test/。当然，将其与Twisted/twisted/test/__init__.py. 将测试放在像Twisted/twisted/test/test_internet.py
    - 作为一个独立的顶级目录

3.  `docs/`: 存放一些文档。

4.  `apidoc`: 对于 Epydoc 生成的 API 文档。

### 顶级文件

1.  `setup.py`: 安装、部署、打包的脚本。
    业界标准的写法是用Python流行的打包工具setuptools来管理这些事情。
    这种方式普遍应用于开源项目中。不过这里的核心思想不是用标准化的工具来解决这些问题，而是说，一个项目一定要有一个安装部署工具，
    能快速便捷的在一台新机器上将环境装好、代码部署好和将程序运行起来。
    setup.py可以将这些事情自动化起来，提高效率、减少出错的概率。"复杂的东西自动化，能自动化的东西一定要自动化。"是一个非常好的习惯。
    
2.  `requirements.txt`: 存放软件依赖的外部Python包列表。

3.  `README`: 项目说明文件。需要说明以下几个事项:

    - 软件定位，软件的基本功能。
    - 运行代码的方法: 安装环境、启动命令等。
    - 简要的使用说明。
    - 代码目录结构说明，更详细点可以说明软件的基本原理。
    - 常见问题说明。
    - 参考：https://github.com/pallets/flask/blob/master/README.rst

4.  `Makefile`: docker 文件

5.  `.gitignore`: git 文件


### 开源涉及的文件

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

除此之外，有一些方案给出了更加多的内容。比如`LICENSE.txt`,`ChangeLog.txt`文件等，我没有列在这里，因为这些东西主要是项目开源的时候需要用到。


## 如何解决多次目录下数据的导入？

比如：main.py中导入docs文件中的conf.py中的函数

```
from docs improt conf     # 这样的导入是不成功的
```

因为from导入的是该目录即foo文件下的文件夹，docs与foo文件夹是一级的目录导入不成功

解决方案

```python
import os,sys


path = os.path.join(os.path.dirname(os.path.abspath(__file__))) 

# 临时添加搜索路径，这种方法导入的路径会在python程序退出后失效。
sys.path.append(path)
```


