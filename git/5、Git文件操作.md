**特别注意：**

千万不要使用Windows自带的**记事本**编辑任何文本文件。原因是Microsoft开发记事本的团队使用了一个非常弱智的行为来保存UTF-8编码的文件，他们自作聪明地在每个文件开头添加了0xefbbbf（十六进制）的字符，你会遇到很多不可思议的问题，比如，网页第一行可能会显示一个“?”，明明正确的程序一编译就报语法错误，等等，都是由记事本的弱智行为带来的。

建议下载[vscode](https://code.visualstudio.com/Download)代替记事本，不但功能强大，而且开源免费！大家就不要下载**Notepad++ **了吧，该作者是个台独，对华充满偏见，实际上是歪曲事实、搬弄是非，发表辱华言论。

#  文件的5种状态

现在请注意，如果你希望后面的学习更顺利，请记住下面这些关于 Git 的概念。 Git 有6种状态，你的文件只能处于其中之一：**untracked（未跟踪）**、**unmodify（未修改）**、 **modified（已修改）**、**staged（已暂存）** 和 **committed（已提交）**。

- **untracked（未跟踪）**：表示此文件在工作区中，但是没有加入Git仓库，不参与版本控制
- **unmodify（未修改）**：表示此文件已经在暂存区或仓库里了，但是未修改
- **modified（已修改）**：表示此文件已经在暂存区或仓库里了，但是被修改过了
- **staged（已暂存）**：表示此文件由 untracked 或 deleted 或 modified 等状态添加到暂存区了，使之包含在准备下次提交的快照中。
- **committed（已提交）**：表示此文件已经安全地保存在仓库中。

下面的图很好的解释了这几种状态的转变：

![img](https://img2018.cnblogs.com/blog/1090617/201810/1090617-20181008212245877-52530897.png)

这会让我们的 Git 项目拥有三个阶段：工作区、暂存区以及 Git 目录。

![工作区、暂存区以及 Git 目录。](https://git-scm.com/book/en/v2/images/areas.png)



- Checkout the project，有两种情况
  - 第一种，将暂存区的文件替换掉工作区的文件，这个操作很危险，会清除工作区中未添加到暂存区的改动。
  - 第二种：将Git仓库中的文件替换掉暂存区以及工作区的文件，这个命令也是极具危险性的，因为不但会清除工作区中未提交的改动，也会清除暂存区中未提交的改动。
- Stage Fixes：将工作区的目录以及各种改动操作暂存到暂存区
- Commit：将暂存区的内容存到仓库中

# 查看文件状态

上面说文件有5种状态，通过如下命令可以查看到文件的状态：

```bash
#查看指定文件状态
git status [filename]

#查看所有文件状态
git status
```

![image-20210417142842842](C:/Users/Administrator/AppData/Roaming/Typora/typora-user-images/image-20210417142842842.png)

# 对文件进行版本控制

## 第一步：使用add命令添加到暂存区

![img](https://images2017.cnblogs.com/blog/63651/201709/63651-20170905231716304-244902919.png)

将untracked状态的文件添加到暂存区，语法格式如下：

```bash
# 添加指定文件到暂存区
$ git add [file1] [file2] ...

# 添加指定目录到暂存区，包括子目录
$ git add [dir]

# 添加当前目录的所有文件到暂存区
$ git add .
```

示例：

![image-20210417144802674](C:/Users/Administrator/AppData/Roaming/Typora/typora-user-images/image-20210417144802674.png)



## 第二步：使用commit提交到仓库

通过add只是将文件或目录添加到了index暂存区，使用commit可以实现将暂存区的文件提交到本地仓库。

```bash
# 提交暂存区到仓库区
$ git commit -m [message]

# 提交暂存区的指定文件到仓库区
$ git commit [file1] [file2] ... -m [message]

# 提交工作区自上次commit之后的变化，直接到仓库区，跳过了add,对新文件无效
$ git commit -a

# 提交时显示所有diff信息
$ git commit -v

# 使用一次新的commit，替代上一次提交
# 如果代码没有任何新变化，则用来改写上一次commit的提交信息
$ git commit --amend -m [message]

# 重做上一次commit，并包括指定文件的新变化
$ git commit --amend [file1] [file2] ...
```

示例：

![image-20210417145036077](C:/Users/Administrator/AppData/Roaming/Typora/typora-user-images/image-20210417145036077.png)

简单解释一下`git commit`命令，`-m`后面输入的是本次提交的说明，可以输入任意内容，当然最好是有意义的，这样你就能从历史记录里方便地找到改动记录。

`git commit`命令执行成功后会告诉你，`1 file changed`：1个文件被改动（我们新添加的readme.txt文件）；`1 insertions`：插入了一行内容（readme.txt有一行内容）。

# 查看文件修改后的差异

git diff用于显示WorkSpace中的文件和暂存区文件的差异

用"git status"只能查看对哪些文件做了改动，如果要看改动了什么，可以用：

```bash
#查看暂存区和工作区的文件差异
git diff [files]
#比较暂存区和最近提交的仓库的文件差异
git diff --cached
#比较仓库与工作空间中的文件差异
git diff [HEAD~n]
```

 ---a表示修改之前的文件，+++b表示修改后的文件



# 忽略文件

有些时候我们不想把某些文件纳入版本控制中，比如数据库文件，临时文件，设计文件等

在主目录下建立".gitignore"文件，此文件有如下规则：

1. 忽略文件中的空行或以井号（#）开始的行将会被忽略。
2. 可以使用Linux通配符。例如：星号（*）代表任意多个字符，问号（？）代表一个字符，方括号（[abc]）代表可选字符范围，大括号（{string1,string2,...}）代表可选的字符串等。
3. 如果名称的最前面有一个感叹号（!），表示例外规则，将不被忽略。
4. 如果名称的最前面是一个路径分隔符（/），表示要忽略的文件在此目录下，而子目录中的文件不忽略。
5. 如果名称的最后面是一个路径分隔符（/），表示要忽略的是此目录下该名称的子目录，而非文件（默认文件或目录都忽略）。

如：

```bash
#为注释
*.txt #忽略所有 .txt结尾的文件
!lib.txt #但lib.txt除外
/temp #仅忽略项目根目录下的TODO文件,不包括其它目录temp
build/ #忽略build/目录下的所有文件
doc/*.txt #会忽略 doc/notes.txt 但不包括 doc/server/arch.txt
```

更多规则参考：https://www.cnblogs.com/kevingrace/p/5690241.html

# 日志与历史

查看提交日志可以使用git log指令，语法格式如下：

```bash
#查看提交日志
git log [<options>] [<revision range>] [[\--] <path>…?]
```

示例：

![img](https://images2017.cnblogs.com/blog/63651/201709/63651-20170906162101757-272912226.png)

"git log --graph"以图形化的方式显示提交历史的关系，这就可以方便地查看提交历史的分支信息，当然是控制台用字符画出来的图形。

"git log -1"则表示显示1行。

使用history可以查看您在bash下输入过的指令：

![img](https://images2017.cnblogs.com/blog/63651/201709/63651-20170906162304663-1459403979.png)

几乎所有输入过的都被记录下来的，不愧是做版本控制的。

**查看所有分支日志**

"git reflog"中会记录这个仓库中所有的分支的所有更新记录，包括已经撤销的更新。

![img](https://images2017.cnblogs.com/blog/63651/201709/63651-20170907101206757-412323808.png)

# 查看文件列表

使用git ls-files指令可以查看指定状态的文件列表，格式如下：

```bash
#查看指定状态的文件
git ls-files [-z] [-t] [-v] (--[cached|deleted|others|ignored|stage|unmerged|killed|modified])* (-[c|d|o|i|s|u|k|m])*
```

示例：

![img](https://images2017.cnblogs.com/blog/63651/201709/63651-20170906213836397-1160093052.png)

# 删除文件

git rm 命令用于删除文件。

如果只是简单地从工作目录中手工删除文件，运行 **git status** 时就会在 **Changes not staged for commit** 的提示。

git rm 删除文件有以下几种形式：

1、将文件从仓库和工作区中删除：

```bash
git rm <file>
```

以下实例从暂存区和工作区中删除readme.txt  文件：

```bash
git rm readme.txt 
```

如果删除之前修改过并且已经放到暂存区域的话，则必须要用强制删除选项 **-f**。

强行从仓库和工作区中删除修改后的 readme.txt  文件：

```bash
git rm -f readme.txt 
```

如果想把文件从跟踪清单中删除，使用 **--cached** 选项即可：

```bash
git rm --cached <file>
```

以下实例从跟踪清单中删除 readme.txt  文件：

```bash
git rm --cached readme.txt
```

## 移除所有未跟踪文件

```bash
#移除所有未跟踪文件
#一般会加上参数-df，-d表示包含目录，-f表示强制清除。
git clean [options] 
```

移除前：![image-20210417150931651](C:/Users/Administrator/AppData/Roaming/Typora/typora-user-images/image-20210417150931651.png)

移除后：![image-20210417150846048](C:/Users/Administrator/AppData/Roaming/Typora/typora-user-images/image-20210417150846048.png)

#

# 撤销操作

三种情况

## 第一种：撤销文件修改

![image-20210417153456073](C:/Users/Administrator/AppData/Roaming/Typora/typora-user-images/image-20210417153456073.png)

## 第二种：撤销add添加到暂存区

![image-20210417153407394](C:/Users/Administrator/AppData/Roaming/Typora/typora-user-images/image-20210417153407394.png)

## 第三种：撤销提交到仓库

撤销提交有两种方式：**使用HEAD指针**和**使用commit id**

在Git中，有一个HEAD指针指向当前分支中最新的提交。当前版本，我们使用"HEAD^"，那么再钱一个版本可以使用"HEAD^^"，如果想回退到更早的提交，可以使用"HEAD~n"。（也就是，HEAD^=HEAD~1，HEAD^^=HEAD~2）

git reset 命令语法格式如下：

```bash
git reset [--soft | --mixed | --hard] [HEAD or commit id]
```

```bash
git reset --hard HEAD^
git reset --hard HEAD~1
git reset --59cf9334cf957535cb328f22a1579b84db0911e5
```

示例：回退到添加f6

回退前：

![img](https://images2017.cnblogs.com/blog/63651/201709/63651-20170906235156351-597175458.png)

回退后：

![img](https://images2017.cnblogs.com/blog/63651/201709/63651-20170906235057710-1820019779.png)

现在又想恢复被撤销的提交可用"git reflog"查看仓库中所有的分支的所有更新记录，包括已经撤销的更新，撤销方法与前面一样。

```bash
git reset --hard HEAD@{7}
git reset --hard e0e79d7
```

**--hard** 参数撤销工作区中所有未提交的修改内容，将暂存区与工作区都回到上一次版本，并删除之前的所有信息提交：

```bash
git reset --hard HEAD
```

**--soft** 参数用于回退到某个版本：

```bash
git reset --soft HEAD
```

撤销提交还有另一种方式：

用reset命令撤销，会被log和reflog记录

用checkout命令，不会被log和reflog记录

```bash
# 会用 HEAD 指向的 master 分支中的全部或者部分文件替换暂存区和以及工作区中的文件
git checkout [HEAD or commit id] . 
或者 
git checkout [HEAD or commit id] [file]
```