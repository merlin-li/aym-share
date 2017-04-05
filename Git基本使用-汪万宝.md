<font face="微软雅黑">

# Git基本使用


## 一、Git环境配置

### 1、全局配置

#### 1.1 文件修改

window下打开当前用户所在的文件夹，或直接Win+R运行%USERPROFILE%，找到文件.gitconfig，Mac下在$HOME/.gitconfig里，内容如下：

```
[user]
    name = aym
    email = aym@aixuedai.com
[alias]
    st = status
    co = checkout
    br = branch
    ci = commit
    pl = pull --no-ff
    ps = push
    lg = log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit
[core]
    autocrlf = true
[push]
default = upstream
```

#### 1.2 命令修改
```
$ git config --global --list // 查看全局配置
$ git config --global user.name aym // 修改提交名
$ git config --global alias.br branch // 修改简写

$ git config --unset alias.co // 删除配置项
```
> 如果不加--global 修改的是当前项目git配置


### 2、让Git记住用户名与密码(Win客户端)

1.） 在Windows用户变量中添加一个HOME环境变量，值为%USERPROFILE%，如下图：

![img](http://pic002.cnblogs.com/images/2011/1/2011070615112192.jpg)

2.） 在"开始>运行"中打开%Home%，新建一个名为_netrc的文件。

3.） 用记事本打开_netrc文件，输入Git服务器名、用户名、密码保存，如下：

```python
machine dev.guang.com
login aiyoumi
password 123456
```

> 可放多个不同登录信息的Git项目，中间空一行即可。


<br>
## 二、Git常用命令

### 2.1、仓库管理
---

#### 1、初始化
```
$ git init
```
>目录下生成.git隐藏文件夹，图标显示git可控
.git目录下有一个hooks，里面可以对预提交信息做一些规范和限制。

#### 2、添加/修改仓库地址
>1. **添加关联地址**
```
$ git remote add origin http://dev.guang.com:9800/root/aicai-front.git
```

>2. **修改关联地址**
```
$ git remote set-url origin http://dev.guang.com:9800/root/aicai-front.git
```

#### 3、查看当前仓库地址
> 
```
$ git remote -v
```

#### 4、创建本地仓库

```
$ git clone http://dev.guang.com:9800/root/aicai-front.git
```
> 会在当前目录下自动生成aicai-front的仓库目录，如果自定义文件夹名，后面空格后加上名称

<br>
### 2.2、分支管理
---

#### 1、查看分支
>
```
$ git br    // 查看本地
```
>
```
$ git br -r // 查看远端
```
>
```
$ git br -a // 查看所有
```

#### 2、查看分支状态（推荐）

```
$ git br -vv
```
> 可全面查看本地分支状态，远端对应关系，落后/优先多少，最后一次提交信息及hash值

#### 3、创建分支

>1. **创建本地分支**
> 本地分支名可不与远程一致，简单一点，方便Tab键补全，只要.gitconfig里的[push]项设置default = upstream即可，不然提交时，会提示本地与远程不一致
```
$ git br <branch> <origin-branch> // 创建分支，并关联远端
```
> > 或：
```
$ git co -b <branch> <origin-branch> // 创建分支并立即切换过去
```

>2. **创建远程分支**（本地分支推送到远端）
```
$ git push origin -u <branch>:<origin-branch> // -u：创建远程分支并关联，origin-branch不用带origin/路径
```

#### 4、切换分支
> 
```
$ git co <branch>
```

#### 5、合并分支

>1. **合并本地分支**
```
$ git merge <branch> --no-ff  // --no-ff：关闭Fast-Foward合并，这样可以生成merge提交，保留分支合并信息
```

>2. **合并远端本地**
```
$ git merge <origin-branch> --no-ff  // 推荐这样使用，因为从远端merge能保证永远是最新的
```

>3. **取消合并**
```
$ git merge --abort
```

#### 6、修改分支

>1. **修改分支名称**
```
$ git br -m <branchName>
```

>2. **修改当前分支远端关联**
```
$ git br --set-upstream-to <origin-branch>
```

>3. **修改指定分支远端关联**
```
$ git br --set-upstream <branch> <origin-branch>
```

#### 7、删除分支

>1. **删除本地分支**
```
$ git br -d <branch>
```

>2. **强制删除本地分支**
```
$ git br -D <branch>  // 未被合并的分支被删除的时候需要强制
```

>3. **删除远端分支**
```
$ git push origin --delete <branch>
```
> > 或：
```
$ git push origin :<branch>  // 推送一个空分支到远端，即删除
```

#### 8、更新分支

> 从远端拉取最新的分支信息，更新本地缓存
```
$ git fetch -p   // 更新被删除的分支
```


<br>
### 2.3、查看管理
---

#### 1、查看当前状态
>
```
$ git st  // 查看当前工作状态
```

#### 2、查看提交记录

>1. **查看全部提交记录**
```
$ git lg
```

>2. **查看单个文件所有记录**
```
$ git lg <fileName>
```

>3. **查看单个文件修改详情**
```
$ git lg -p <fileName>  // 可查看比较文件历史修改记录
```

>4. **查看单条记录详情**
```
$ git show <commit-id>  // 日志的hash值
```

>5. **查找所有包括指定字符串文件**
```
$ git grep -n <string>  // -n：显示字符串所在的行数
```

#### 3、查看比较文件

>1. **查看比较文件**（与暂存区比较）
> 如果当前文件没被修改，需要查看历史修改情况，用上面的git lg -p fileName
```
$ git diff <fileName>  // 比较当前文件和暂存区文件差异，应用在提交前确认的场景
```

>2. **比较两次提交记录**
```
$ git diff <commit-id1> <commit-id2> // 比较两次提交之间的差异
```

#### 4、查看历史操作记录
> 每一次当前HEAD发生改变（包括切换branch, pull, 添加新commit）一个新的纪录就会被添加到reflog
```
$ git reflog
```


<br>
### 2.4、操作管理
---

#### 1、更新

>1. **非快进更新**
```
$ git pull --no-ff
```

>2. **基于rebase**
> 这个命令会迫使git将远程分支上的变更同步到本地，然后将尚未推送的提交重新应用到这个最新版本，就好象它们刚刚发生一样，这样就可以避免合并以及随之而来的丑陋的合并信息
```
$ git pull --rebase   // 避免产生无用的合并信息
```
如果冲突了：
```
$ git add -A   // 强制添加到
$ git rebase --continue
$ git push
```

>3. **临时更新**（暂存）
```
$ git stash      // 先放入暂存区
$ git pull
$ git stash pop  // 恢复显示工作内容
```
管理暂存：
```
$ git stash list    // 显示暂存列表
$ git stash clear   // 清除暂存列表
```

#### 2、提交

>1. **多个修改，只提交部分** 
```
$ git add <file1> <file2>   // 只添加要提交的到本地暂存区
$ git ci -m 'commit info'   // -m处不加a
$ git push
```

>2. **提交全部** 
```
$ git add .               // 有新文件时，一定要
$ git ci -am 'commit info'
$ git push
```

>3. **追加提交（未push到远端）** 
将最近一次的变更追加到最新的提交，同时也可以编辑提交信息，不产生新的提交记录。
```
$ git ci --amend   // 如果已push到远端，不建议追加，容易跟自己冲突
$ git push
```

#### 3、提取
> 从别的分支同步一个commit到本分支
```
$ git cherry-pick <commit-id>
```


<br>
### 2.5、恢复管理
---

#### 1、恢复本地

>1. **已修改，未暂存** 
```
$ git co <fileName|directory>  // 恢复单个文件或目录
```
```
$ git co .  // 恢复全部
```

>2. **已暂存，未提交** 
```
$ git reset HEAD^   // 向前回滚一条记录，相当于HEAD~1
$ git co .
```

>3. **恢复前N条**
```
$ git reset HEAD~n   // 默认省略--soft参数，属于软恢复，n>=1整数
```

>4. **硬恢复** 
```
$ git reset --hard HEAD~n|<commit-id>   // 不保留当前修改，要当心，确认当前修改没有用了
```

#### 2、恢复远端

>1. **单次回滚** 
git revert 会产生一个新的与之前commit相反的操作，来抵消之前错误的提交
```
$ git revert <commit-id>   // 也可以多次操作，达到恢复多个的目的
$ git push
```

>2. **指定位置** 
```
$ git reset --hard <commit-id>   // 恢复到当前位置
$ git push -f      // 要加-f强制推送
```

>3. **挑选模式** 
```
$ git rebase -i <commit-id>  // 会打开编辑，剔除挑选记录
$ git push -f      // 要加-f强制推送
```


<br>
### 2.6、其它
---

#### 1、临时忽略文件

>1. **忽略跟踪** 
.gitignore只能忽略那些原来没有被跟踪的文件，如果文件已被纳入版本管理中，则修改.gitignore是无效的。
```
$ git update-index --assume-unchanged <file>
```

>2. **恢复跟踪** 
```
$ git update-index --no-assume-unchanged <file>
```

>3. **查看被忽略的跟踪** 
```
$ git ls-files -v|grep '^h'
```

>4. **另一个类似的功能** 
```
$ git update-index --skip-worktree <file>  // 忽略

$ git update-index --no-skip-worktree <file>  // 恢复
```


#### 2、清理

>1. **清理工作树** 
```
$ git clean -fd    // 移除未跟踪的文件和目录，如果加上-n参数来先看看会删掉哪些文件
```

>2. **垃圾回收** 
Git 往磁盘保存对象时默认使用的格式叫松散对象(loose object)格式，当手动执行git gc 命令，或推送至远程服务器时，Git会将这些对象打包至一个叫packfile的二进制文件以节省空间并提高效率。
```
$ git gc --auto
$ git repack -d -l  
```

#### 3、帮助
>
```
$ git help
```

</font>
<body style="width:85%; border: 1px solid #eee;"></body>