## git基础概念

## 什么是git？
Git是一个免费的开源分布式版本控制系统，它可以快速高效地处理从小型到大型的所有项目。
**官方网站：https://git-scm.com/**

## git的优势
- 本地版本控制
- 重写提交声明
- 可以回滚
- 分支系统

## git的仓库相关
- github：面向开源及私有软件项目的托管平台，只支持git作为唯一的版本库格式进行托管(网站，所有人都可以使用，国内类似网站“码云”)，github上可以创建开源仓库和私有仓库，但是私有需要收费才可以使用。
- gitlab：用于仓库管理系统的开源项目，使用Git作为代码管理工具，并在此基础上搭建起来的web服务（企业可以自己搭建只提供公司内部使用）。

## git的三种状态

- 已修改（modified）
- 已暂缓（staged）
- 已提交（commited）

## git的简单使用



（1） 初始化git项目（默认master分支）

~~~bash
git init
~~~

会生成`.git`隐藏目录，存放git版本控制的文件

（2） 将工作区增加到暂缓区

~~~bash
git add 项目文件
~~~

（3）从暂缓区提交到本地仓库

~~~bash
git commit -m "提交说明"
或者
git commit
~~~

（4）将暂缓区删除，回到工作区

~~~bash
git rm --cached
或者
git checkout -- 指定文件
~~~

（5） 查看提交的日志

~~~bash
git log	# 查看所有提交的日志
git log -1	# 查看最近的日志，-1表示最近提交的1次
git log --pretty=oneline # 查看简洁的日志
git log --pertty=format:"%h-%an,%ar:%s" # 自定查看日志
~~~

（6） 显示工作目录和暂缓区状态

~~~bash
git status
~~~

（7）删除操作

~~~bash
git rm 指定文件 # 将本地仓库的指定文件删除，删除之后放在和暂缓区
# 彻底删除，在指定git rm之后，再提交
git commit -m "彻底删除"
~~~

操作系统中的删除，是对工作区的文件进行操作

（8）撤销`git rm`删除

~~~bash
# 恢复到工作区
git reset HEAD 指定文件
# 撤销操作 
git checkout -- 指定文件
~~~

（9）重命名操作

~~~bash
git mv 原文件名 修改后文件名	# git会认为我们把文件删除了
~~~

`git mv`之后可以执行`git reset HEAD 原文件名`恢复到工作区，再执行`git checkout -- 原文件名`恢复原文件，会发现原文件名和修改后文件名的文件都存在

操作系统的重名跟修改文件一样，使用`git add`到暂缓区，然后`git commit`到本地仓库

（10）重写提交说明

~~~bash
# 正常方法
git commit --amend -m "修改的提交说明"
# 另一种方法
# 重新提交一次
~~~

（11）忽略文件

创建一个`.gitignore`的文件，并将需要忽略即不被git管理的文件名写入到文件中

~~~bash
a.txt	# 忽略a.txt文件
~~~

通配符的使用：

`*`：代表任意字符

`!`：代表非

~~~bash
*.txt	# 匹配所有以.txt结尾的文件
!b.txt	# 匹配不是名为b.txt的文件
dir/	# 匹配dir目录下的所有文件
dir/*.txt	# 匹配dir目录下所有.txt结尾的文件
dir/*/*.txt	# 匹配dir目录下的下一级目录下的.txt结尾的文件
dir/**/*.txt	# 匹配任意级别目录下以.txt结尾的文件

~~~



## 设置用户名和邮箱

（1）给整个计算机设置（不推荐）

~~~bash
git config --global	user.name
git config --global	user.email

git config --global --unset user.name
git config --global --unset user.email
~~~

（2）给当前用户设置（推荐）

~~~bash
git config --system user.name
git config --system user.email
# 配置保存在~.gitconfig

git config --system --unset user.name
git config --system --unset user.email
~~~

（3）给当前项目设置

~~~bash
git config --local user.name
git config --local user.email

git config --local --unset user.name
git config --local --unset user.email
~~~

优先级：（3）>（2）>（1）

**注意：设置了用户名和邮箱的配置之后，只对之后提交的有效，不影响之前提交的文件的用户名和邮箱**

## git的分支操作

**什么是分支？**一条commit链，一条工作记录线

分支名（master）：只想当前的提交（commit）

HEAD：指向当前分支（HEAD->分支名）

（1）查看分支

~~~bash
git branch
~~~

（2）创建分支

~~~bash
git branch 分支名
~~~

（3）切换分支

~~~bash
git checkout 分支名
~~~

（4）删除分支

~~~bash
git branch -d 分支名
git branch -D 分支名	# 强行删除
~~~

不能删除的情况：

- 当前分支不能删除自己
- 如果分支有未合并的内容时，删除分支前建议先合并分支

注意：

- 如果在分支A中进行了写操作，但操作实在工作中进行的，即没有`add`和`commit`，在master中能够看到该操作，如果分支A中进行了写操作，并且提交（`commit`）到了本地仓库，则master中无法观察到此操作
- 如果在分支A中进行了写操作，但操作实在工作中进行的，即没有`add`和`commit`。删除分支A是可以成功的。

（5）创建新分支并切换

~~~bash
git checkout -b 分支名
~~~

（6）合并分支

~~~bash
git merge new_branch
~~~

如果一个分支考前，另一个分支靠口，靠后的分支进行合并，则不会产生冲突，master可以同个靠后的分支直接赶上前面的分支，称为fast forward

fast forward：

- 两个分支fast forward 归于一点commit
- 没有分支信息（丢失分支信息）

git在merge时，默认使用fast forward，也可以禁止：git merge --no-ff：

- 两个分支fast forward，不会归于一点commit（主动合并的分支，会前进一步）
- 分支信息完整（不丢失分支信息）

分支合并冲突：

如果发生冲突，需要解决冲突，修改文件中提示冲突的部分，然后`git add`和`git commit`。这里的`git add`是告知`git`冲突已经解决了。

注意：master在merge时，如果遇到冲突并解决，则解决冲突后会进行2次提交，1次是最终提交，1次是将另外分支的提交信息commit也拿过来。

在合并时，如果一方落后，另一方 前进。则落后放可以直接通过merge合并到前进方。



（7）查看分支

~~~bash
git branch -v
~~~




## 补充
svn：增量，每一个版本只包含修改的部分
git：全量，每一个版本都包含了全部的文件，即使没有修改也都会复制一份，保证了数据的完整性