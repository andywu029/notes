# 初始化操作

&emsp;&emsp;安装好 Git 后，在资源管理器的空白处单击鼠标右键打开菜单，点击 `Git Bash Here` 打开 Git 命令行窗口，在窗口中可直接使用 Linux 命令操作：

![](./images/02/01.png)

&emsp;&emsp;然后使用 <font color=green>***`git init`***</font> 命令来初始化本地库：

![](./images/02/02.png)

> <font color=orange>**注意：**</font>`.git` 目录中存放的是本地库相关核心配置文件，不要随意删除和修改。

&emsp;&emsp;`.git` 目录说明：

+ <font color=skyblue>**hooks 目录：**</font>脚本文件的目录
+ <font color=skyblue>**info 目录：**</font>保存了不希望在 `.gitignore` 文件中管理的忽略模式的全局可执行文件
+ <font color=skyblue>**logs 目录：**</font>日志目录
+ <font color=skyblue>**objects 目录：**</font>存储所有数据内容
+ <font color=skyblue>**refs 目录：**</font>存储指向数据（分支）的提交对象的指针
+ <font color=skyblue>**config 文件：**</font>包含了项目特有的配置选项
+ <font color=skyblue>**description 文件：**</font>仅供 `GitWeb` 程序使用
+ <font color=skyblue>**HEAD 文件：**</font>指向当前分支

&emsp;&emsp;为了区分不同开发人员的身份信息，可以设置签名信息，格式如下：

+ 用户名：yxbb
+ Email：yxbb@126.com

> <font color=orange>**注意：**</font>这里的签名信息和登录远程库的账号和密码没有任何关系。

&emsp;&emsp;设置签名信息的具体命令如下：

+ <font color=skyblue>**项目级别/仓库级别：**</font>仅在当前目录的本地 Git 仓库范围内有效
  + <font color=green>***`git config user.name yxbb`***</font>
  + <font color=green>***`git config user.email yxbb@126.com`***</font>
  + 签名信息保存位置：<font color=red>**`./.git/config`**</font> 文件中

![](./images/02/03.png)

+ <font color=skyblue>**系统用户级别：**</font>登录当前操作系统的用户范围
  + <font color=green>***`git config --global user.name yxbb`***</font>
  + <font color=green>***`git config --global user.email yxbb@126.com`***</font>
  + 签名信息保存位置：<font color=red>**`~/.gitconfig`**</font> 文件中

![](./images/02/04.png)

> <font color=orange>**关于级别优先级说明：**</font>
>
> + 就近原则：项目级别优先于系统用户级别
> + 如果只有系统用户级别的签名，则采用系统用户级别的签名信息
> + 二者都不存在是不允许的

# Git 基本操作

## 查看状态

&emsp;&emsp;查看状态使用 <font color=green>***`git status`***</font> 命令，用来查看工作区、暂存区的状态。首先在仓库里面创建一个`demo01.txt` 文件，然后使用 <font color=green>***`git status`***</font> 查看状态，会提示 <font color=red>**Untracked files（有未追踪文件）**</font>：

![](./images/02/05.png)

## 添加到暂存区

&emsp;&emsp;添加到暂存区就是将工作区的 "新增/修改" 添加到暂存区，相关联的命令如下：

+ 添加到暂存区：`git add <file name>`

![](./images/02/06.png)

+ 恢复，不放到暂存区： `git rm --cached <file name>`

![](./images/02/07.png)

## 提交到本地库

&emsp;&emsp;提交到本地库是指将暂存区的内容提交到本地库，可以使用 `git commit [-m "提交说明信息"] <file name>`：

![](./images/02/08.png)

## 查看版本历史记录

&emsp;&emsp;通过 <font color=green>***`git log`***</font> 命令可以查看详细的日志信息：

![](./images/02/09.png)

> <font color=orange>**注意：**</font>如果内容太长，多屏显示控制方式如下：
>
> + 空格键：向下查看
> + b：向上查看
> + q：退出查看

&emsp;&emsp;可以使用 <font color=green>***`git log --pretty=oneline`***</font> 命令以漂亮的格式显示（每条日志只显示一行）：

![](./images/02/10.png)

&emsp;&emsp;可以使用 <font color=green>***`git log --oneline`***</font> 以简约的格式显示：

![](./images/02/11.png)

&emsp;&emsp;可以使用 <font color=green>***`git reflog`***</font> 命令显示回滚版本步数（<font color=red>**推荐，HEAD@{回滚对应版本，底层操作需要移动多少步}**</font>）：

![](./images/02/12.png)

## 前进后退版本

&emsp;&emsp;可以通过 <font color=red>**HEAD**</font> 指针来移动回滚版本：

 ![](./images/02/13.png)

+ 基于索引值操作（<font color=red>**推荐方法**</font>）
  + 命令： `git reset --hard <局部索引值>`
  + 举例： `git reset --hard be8ad5d`
+ 使用 <font color=red>**^ 符号**</font>：只能后退
  + 命令： `git reset --hard HEAD^`
  + <font color=red>**一个 ^ 表示后退一步，n个表示后退 n 步**</font>
+ 使用 <font color=red>**~ 符号**</font>：只能后退
+ 命令： <font color=green>***`git reset --hard HEAD~n`***</font>
  + <font color=red>**n指定步数，表示后退 n 步**</font>

&emsp;&emsp;要想恢复被删除的文件，前提是删除文件前，此文件已经提交到本地库。删除文件后，可以使用 `git reset --hard <历史记录索引值>` 命令，需要注意两点：

+ 删除操作已经提交到本地库：指针位置指向历史记录
+ 删除操作尚未提交到本地库：无法恢复

## 对比文件差异

&emsp;&emsp;如果需要将工作区和暂存区进行比较，可以使用 `git diff <文件名>` 命令。假设在 `demo01.txt` 中添加了一段内容，然后使用 <font color=green>***`git diff demo01.txt`***</font> 命令查看：

![](./images/02/14.png)

&emsp;&emsp;如果需要将工作区中的文件和本地库历史记录比较，可以使用  `git diff <本地库中历史版本> <文件名>` 命令：

![](./images/02/15.png)

> <font color=orange>**注意：**</font>不带文件名比较多个文件。

# Git 分支管理

## 基础知识

&emsp;&emsp;Git 分支是指在版本控制过程中，使用多条线同时推进多个任务：

![](./images/02/16.png)

&emsp;&emsp;使用 Git 分支的好处：

+ 同时并行推进多个功能开发，提高开发效率
+ 各个分支在开发过程中，如果某一个分支开发失败，不会对其它分支有任何影响。失败的分支删除重新开始即可

## Git 分支操作

&emsp;&emsp;常用的命令：

|                   操作说明                   | 操作指令                                                     |
| :------------------------------------------: | ------------------------------------------------------------ |
|                   查看分支                   | `git branch -v`                                              |
|                   创建分支                   | `git branch 新分支名`                                        |
| 删除分支（删除的分支不是当前正在打开的分支） | `git branch -d 分支名`                                       |
|                   切换分支                   | `git checkout 分支名`                                        |
|                   合并分支                   | 首先切换到接受修改的分支上，`git checkout 需要接受的分支名`；然后执行 `git merge 有新内容的分支名` 命令。 |

&emsp;&emsp;出现冲突的表现如下：

![](./images/02/17.png)

&emsp;&emsp;出现冲突后的解决方法如下：

+ 第一步：编辑文件，删除特殊符号
+ 第二步：把文件修改到满意为止，保存退出
+ 第三步：`git add 文件名`
+ 第四步：`git commit -m "日志信息"`（<font color=red>**此时 `commit` 后面一定不要有文件名**</font>）

## 分支管理机制

1. 创建分支

![](./images/02/18.png)

![](./images/02/19.png)

2. 切换分支

+ 创建两个分支，当前指向 `hot_fix`

![](./images/02/20.png)

+ 在 `hot_fix` 更新了新的版本

![](./images/02/21.png)

+ 由 `hot_fix`分支切换回 `master` 分支

![](./images/02/22.png)

+ 在 `master` 分支上做了更新

![](./images/02/23.png)