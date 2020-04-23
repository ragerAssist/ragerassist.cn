---
title: 原创：命令行备忘录 cheat 的安装与配置
date: 2020-04-23 14:27:06
updated: 2020-04-23 22:53:26
categories: 工具
mathjax: false
tags:
  - 工具
  - 命令行
  - 备忘录
  - Windows
  - Windows PowerShell
---

对于经常使用命令行的人来说，记忆那些很长很复杂但又经常用到的命令行指令无疑是一件令人痛苦的事情，因此有时候我们希望能有那样一个工具，来帮助我们记忆这些指令，以减轻我们记忆上的负担。

对于这个问题，一种解决办法是将这些指令统一记录在一个文档中，当你忘记了某个指令时，再打开这个文档进行查找；随着文档中记录的指令不断增加，你可能会想要将这些指令分开到不同的文档中，并给这些文档起一个对应的名字，就比如git、vim、diff等，并且你很可能会将这些文档放在同一个文件夹中以便于统一管理。那么具体怎么操作呢？

<!-- more -->

我想可以这样来进行：假设你现在已经有了一个存放着各种指令文档的文件夹`C:\Users\惠普\Desktop\cheatsheets`，当你忘记了某个指令时，你可以执行以下命令来查找你所需要的指令：

``` powershell
> cd C:\Users\惠普\Desktop\cheatsheets	# 切换到存放有备忘录的文件夹
> cat git	# 将文件git中的内容输出到终端
# To set your identity:
git config --global user.name <name>
git config --global user.email <email>

# To set your editor:
git config --global core.editor <editor>
......
```

当然，为了提高效率，你还可以创建一个变量`$cheat`来存放该文件夹的路径，并将其放入Windows PowerShell的配置文件中`$PROFILE.CurrentUserAllHosts`，这样就可以更加方便的查找我们所需要的指令：

``` powershell
> cat $cheat\git
```

说到这里，你可能已经想要自己实现一个这样的软件，来更好的管理这些备忘录了，但我们无需这么做，虽然那将会很有趣，因为已经有人实现了这样的一个软件，那个软件就是**cheat**，该项目开源在GitHub上，这是该项目的地址 <https://github.com/cheat/cheat>。

我在这篇文章中将向你们讲述这个软件在Windows系统上的安装与配置的过程，因为在Windows系统中安装这个软件并不是那么方便的。

---

**1、**在cheat的下载地址 <https://github.com/cheat/cheat/releases> 下载Windows版本的cheat：

![](1.png)

**2、**将解压后的文件夹放入任意一个磁盘，此时，你可以选择重命名该文件夹，就比如`cheat`，然后将子文件夹`dist`中的二进制文件`cheat-windows-amd64.exe`重命名为`cheat.exe`，然后你也可以选择给它换个地方：

![](2.png)

**3、**将该可执行文件`cheat.exe`的路径添加进环境变量中，这样你就可以在命令行下使用该软件了，具体步骤就不在此处赘述了。

**4、**<font color="red">手动</font>创建一个配置文件，你可以在命令行下执行如下命令：

``` powershell
> cd D:\cheat
> cheat --init > conf.yml	# 初始化一个配置文件并将其重定向到文件D:\cheat\conf.yml中
```

**5、**创建一个环境变量来指定该配置文件的路径，在运行cheat时，cheat将会根据该路径寻找配置文件，若找到，则将加载该配置文件，其中环境变量的名字必须为`CHEAT_CONFIG_PATH`，值则为你刚刚创建的配置文件的路径：

![](3.png)

**6、**接下来，你需要创建一个文件夹`cheatsheets`来存放备忘录，备忘录分为两种，一种是社区(community)提供的，另一种则是用户自己(personal)创建的，因此你还需要在文件夹`cheatsheets`下创建两个子文件夹`community`和`personal`，然后你可以选择将文件夹`cheatsheets`放在文件系统中的任何一个位置，当然，为了方便管理，你最好将其与`cheat.exe`和`conf.yml`放在一起：

![](4.png)

**7、**打开配置文件`conf.yml`，下拉到文件底部，将与`community`所对应的`path`的值设置为你刚刚创建的`community`文件夹的路径，同样地，将与`personal`对应的`path`的值设置为`personal`文件夹的路径：

![](5.png)

> 注意：`path:`与其后面的值(路径)之间必须要留一个空格

**8、**至此，cheat已经可以正常使用了，但是，此时你的`cheatsheets`文件夹中还没有任何一个备忘录，你可以选择创建一个自己的备忘录，并将其放入子文件夹`personal`中，也可以选择暂时使用社区提供的备忘录。如果你打算创建一个自己的备忘录，你应该遵循cheat的文档中给你的[指导](https://github.com/cheat/cheat)；如果你打算暂时使用社区提供的备忘录，则需要自己手动下载它们，下面将给出下载方法。

**9、** 你可以在这里下载社区提供的备忘录 <https://github.com/cheat/cheatsheets>：

![](6.png)

单击图中的`Download ZIP`，将所有社区提供的备忘录下载到本地，解压缩后，将文件夹中的所有的备忘录移动到目录`...\cheatsheets\community`中，你可以选择将其中的文件夹`.github`删除，也可以留着；此时，打开cmd或者Windows PowerShell，在其中执行命令`cheat git`，你将会看到如下输出：

![](7.png)

> 注：你也可以使用git将整个仓库克隆到文件夹`community`中，以后当该仓库有更新时，可以方便的通过`git pull`来对该文件夹下的备忘录进行更新；此外，如果你觉得你并不需要所有这些社区提供的备忘录，比如你只想要备忘录`git`，你也可以选择只下载该文件，然后将其放入文件夹`community`中

---

**说在最后**：除了**cheat**，你也可以试一试另一个命令行备忘录工具**navi**，该工具给我们提供了一个交互式的命令行界面，你可以浏览备忘录并执行其中的命令，而不是将备忘录中的所有内容直接输出到终端，同样地，navi开源在GitHub上，这是仓库地址 <https://github.com/denisidoro/navi>；遗憾的是，navi并没有提供Windows可执行文件，目前，你只能在 *nix系统以及mac系统上使用该软件。

此外，你还可以试一试命令行下的模糊查找工具**fzf**，该软件可以与cheat整合，使得cheat能提供更强大的功能，这是仓库地址 <https://github.com/junegunn/fzf>。
