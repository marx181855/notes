### 疑难解答

Q：输入`git add readme.txt`，得到错误：`fatal: not a git repository (or any of the parent directories)`。

A：Git命令必须在Git仓库目录内执行（`git init`除外），在仓库目录外执行是没有意义的。

---------

Q：输入`git add readme.txt`，得到错误`fatal: pathspec 'readme.txt' did not match any files`。

A：添加某个文件时，该文件必须在当前目录下存在，用`ls`或者`dir`命令查看当前目录的文件，看看文件是否存在，或者是否写错了文件名。

-----
Q：git bash的中文乱码问题


A：
- 1 一般情况

    - 1.1 git status命令
    
    - git status，显示的文件目录如果存在中文字符，则中文字符则会转成八进制显示。


- ![](images/Git的FAQ.md-0.PNG)
      
    - 1.2 设置

    - 在bash右键打开 `Options` ，点击 `Text` ，在 Locale处选择 `zh_CN`，在Character set处选择 `UTF-8 `。

    - 然后在 git bash 终端输入命令：git config --global core.quotepath false 即可。

![](images/Git的FAQ.md-1.PNG)

- 2 特殊情况

- 如果在上面设置了之后，出现中文混乱的编码字符，而不是正常的中文或者八进制的字符，那就要将 git 完全卸载。

    - 2.1 流程

    - 首先卸载掉git，然后删掉git安装的目录，最后在 `C:\Users\Administrator` 路径，删掉所有的文件，然后重装git，再按照上面的一般情况去设置即可。
    
-----

