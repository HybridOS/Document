repo
==========
Android使用Git作为代码管理工具，开发了Gerrit进行代码审核以便更好的对代码进行集中式管理，还开发了Repo命令行工具，对Git部分命令封装，将百多个Git库有效的进行组织。要想克隆和管理这百多个Git库，还真不是一件简单的事情。

如果了解了Repo的实现，参考《Using Repo and Git》 http://source.android.com/source/git-repo.html , 建立一个本地的 android 版本库镜像还是
不难的：
    下载 repo bootstrap 脚本
    $ curl http://android.git.kernel.org/repo >~/bin/repo
    $ chmod a+x ~/bin/repo
    $ export PATH=$PATH:~/bin

提供--mirror参数调用repo init，建立 git 版本库克隆

查看可切换的分支
=============
cd .repo/manifests
git branch -a | cut -d / -f 3


以 gingerbread-release 分支为例
repo init -b gingerbread-release 
repo sync (not needed if your local copy is up to date)
repo start gingerbread-release --all 
查看当前的分支
repo branches



1.    下载 repo 的地址: http://android.git.kernel.org/repo ，可以用 wget http://android.git.kernel.org/repo 或者 curl http://android.git.kernel.org/repo>~/bin/repo  来下载 repo , 

由于官网被墙，目前可以使用的repo脚本下载方法如下（两者选一）：
$ git clone git://git.omapzoom.org/git-repo.git
$ git clone git://aosp.tuna.tsinghua.edu.cn/android/git-repo.git/ 

Repo脚本授权：chmod a+x ~/bin/repo 
2.    用repo sync 在抓去 android source code 的时候，会经常出现一些错误导致 repo sync 中断，每次都要手动开始。可以用如下的命令，来自动重复：   $?=1;   while [$? -ne 0 ] ; do  repo sync ; done

一下脚本借鉴别人，用个脚本更爽 

reposync.sh
#!/bin/bash  
echo "======start repo sync======"  
repo sync  
while [ $? == 1 ]; do  
echo "======sync failed, re-sync again======"  
sleep 3  
repo sync  
done
3.     repo help [ command ] , 显示command 的详细的帮助信息内容

4.    repo init -u URL ,  在当前目录安装 repository ，会在当前目录创建一个目录 ".repo"  -u 参数指定一个URL，从这个URL 中取得repository 的 manifest 文件。   repo init -u git://android.git.kernel.org/platform/manifest.git

              可以用 -m 参数来选择 repository 中的某一个特定的 manifest 文件，如果不具体指定，那么表示为默认的 namifest 文件 (default.xml)    repoinit -u git://android.git.kernel.org/platform/manifest.git -m dalvik-plus.xml

             可以用 -b 参数来指定某个manifest 分支。

              repo init -ugit://android.git.kernel.org/platform/manifest.git -b release-1.0

             可以用命令: repo help init 来获取 repo init 的其他用法

       4. repo sync [project-list]

           下载最新本地工作文件，更新成功，这本地文件和repository 中的代码是一样的。可以指定需要更新的project ，如果不指定任何参数，会同步整个所有的项目。

          如果是第一次运行 repo sync ，则这个命令相当于 git clone ，会把 repository 中的所有内容都拷贝到本地。如果不是第一次运行 repo sync ，则相当于 git remote update ;  git rebaseorigin/branch .  repo sync 会更新 .repo 下面的文件。如果在merge 的过程中出现冲突，这需要手动运行  git  rebase --continue

     5. repo update[ project-list ]

     上传修改的代码，如果你本地的代码有所修改，那么在运行 repo sync 的时候，会提示你上传修改的代码，所有修改的代码分支会上传到 Gerrit (基于web 的代码review 系统), Gerrit 受到上传的代码，会转换为一个个变更，从而可以让人们来review 修改的代码。

      6. repo diff [ project-list ]

       显示提交的代码和当前工作目录代码之间的差异。

      7. repo download  target revision

       下载特定的修改版本到本地，例如:  repo downloadpltform/frameworks/base 1241 下载修改版本为 1241 的代码

      8. repo start newbranchname

       创建新的branch分支。 "." 代表当前工作的branch 分支。

      9.  repo prune [project list]

       删除已经merge 的 project

     10. repo foreach [ project-lists] -c command

      对每一个 project 运行 command 命令

     11. repo status

      显示 project 的状态
      

最近在使用Android的repo和git的过程中遇到了很多莫名奇妙的问题，现在记录一下，便于自己以后的查用。

1.repo sync中遇到error:......checkout ....接一串hashnumber

解决方法：进到它说提示的目录中，用git status显示文件，将修改过的文件删除掉，再重新repo sync

2.repo sync中遇到：contains uncommitted changes

解决方法：进到它说提示的目录中，使用git reset --hard命令

3. 怎么对repo下的所有project执行git命令

解决方法：repo forall -c git checkout -b    //该条命令会对repo下的project执行切换branch的命令

4. 怎么切换到你想要的branch

解决方法：git checkout branchName，比如 git checkout testBranch

git
=============

Showing and Following Renames or Copies
========================================
git log --name-status -M   即　--find-renames[=n] (-M[n])
git log --follow  bar :: o have Git follow a file past a rename, only works when you give a single file to follow
git log --find-copies[=n] (-C[n]) :: does the same for detecting copies
  -CC (or --find-copies-harder) will consider all files in a commit as potential 
  sources of copying, while plain -C considers only files that changed in that commit.

Searching for Changes: The “pickaxe”
====================================
git log -S string  :: lists commits that changed the number of occurrences of string in at least one file
git log -G pattern  :: does the same with a regular expression.
ADD --name-status, Git shows only those files that triggered the listing
ADD --pickaxe-all, then Git shows all files touched by the listed commits.

Showing Diffs
=============
git log -p ::shows the “patch” or “diff” associated with each commit
-m ::Shows each pairwise diff between the merge and its parents.
-c  ::Shows the differences with all parents simultaneously in a merged format
--cc :: Implies -c and further simplifies the diff by showing only conflicts

Comparing Branches
==================
git log --cherry-pick  :: takes this into account by omitting commits that have identical diffs.
git log master...other    得到 E,D,C, 3,2,1
git log --cherry-pick master...other 得到 E,C,3,1
$ git log --cherry-mark --left-right master...other
< e5feb479 E
< 070e87e5 D
= 9b0e3dc5 C
> 6f70a016 3
= 0badfe94 2
> 15f47204 1

保存临时更改
=========
暂存一些未完成的更改

$ git stash

临时存储所有修改的已跟踪文件

$ git stash pop

重新存储所有最近被stash的文件

$ git stash list

列出所有被stash的更改

$ git stash drop

放弃所有最近stash的更改

查阅历史
=================
浏览并检查项目文件的发展

$ git log

列出当前分支的版本历史

$ git log --follow [file]

列出文件的版本历史，包括重命名

$ git diff [first-branch]...[second-branch]

展示两个不同分支之间的差异

$ git show [commit]

Git 工具 - 搜索
==============

Git 提供了一个 grep 命令，你可以很方便地从提交历史或者工作目录中查找一个字符串或者正则表达式。 我们用 Git 本身源代码的查找作为例子。

默认情况下 Git 会查找你工作目录的文件。 你可以传入 -n 参数来输出 Git 所找到的匹配行行号。

输出元数据以及特定commit的内容变化
```
$ git grep -n gmtime_r
compat/gmtime.c:3:#undef gmtime_r
compat/gmtime.c:8:      return git_gmtime_r(timep, &result);
compat/gmtime.c:11:struct tm *git_gmtime_r(const time_t *timep, struct tm *result)
compat/gmtime.c:16:     ret = gmtime_r(timep, result);
compat/mingw.c:606:struct tm *gmtime_r(const time_t *timep, struct tm *result)
compat/mingw.h:162:struct tm *gmtime_r(const time_t *timep, struct tm *result);
date.c:429:             if (gmtime_r(&now, &now_tm))
date.c:492:             if (gmtime_r(&time, tm)) {
git-compat-util.h:721:struct tm *git_gmtime_r(const time_t *, struct tm *);
git-compat-util.h:723:#define gmtime_r git_gmtime_r
```

例如，你可以使用 --count 选项来使 Git 输出概述的信息，仅仅包括哪些文件包含匹配以及每个文件包含了多少个匹配。
```
$ git grep --count gmtime_r
compat/gmtime.c:4
compat/mingw.c:1
compat/mingw.h:1
date.c:2
git-compat-util.h:2
```

如果你想看匹配的行是属于哪一个方法或者函数，你可以传入 -p 选项：
```
git grep -p -n  binder_main_lock  
drivers/android/binder.c:49:static DEFINE_MUTEX(binder_main_lock);
drivers/android/binder.c=426=static inline void binder_lock(const char *tag)
drivers/android/binder.c:429:   mutex_lock(&binder_main_lock);
drivers/android/binder.c=433=static inline void binder_unlock(const char *tag)
drivers/android/binder.c:436:   mutex_unlock(&binder_main_lock);

```

你还可以使用 --and 标志来查看复杂的字符串组合，也就是在同一行同时包含多个匹配。 比如，我们要查看在旧版本 1.8.0 的 Git 代码库中定义了常量名包含 “LINK” 或者 “BUF_MAX” 这两个字符串所在的行。

这里我们也用到了 --break 和 --heading 选项来使输出更加容易阅读。

$ git grep --break --heading \
    -n -e '#define' --and \( -e LINK -e BUF_MAX \) v1.8.0


Git 日志搜索
===========
或许你不想知道某一项在 哪里 ，而是想知道是什么 时候 存在或者引入的。 git log 命令有许多强大的工具可以通过提交信息甚至是 diff 的内容来找到某个特定的提交。

例如，如果我们想找到 ZLIB_BUF_MAX 常量是什么时候引入的，我们可以使用 -S 选项来显示新增和删除该字符串的提交。
例如，如果我们想找到 ZLIB_BUF_MAX 常量是什么时候引入的，我们可以使用 -S 选项来显示新增和删除该字符串的提交。
```
$ git log -SZLIB_BUF_MAX --oneline
e01503b zlib: allow feeding more than 4GB in one go
ef49a7a zlib: zlib can only process 4GB at a time
```
如果我们查看这些提交的 diff，我们可以看到在 ef49a7a 这个提交引入了常量，并且在 e01503b 这个提交中被修改了。
如果你希望得到更精确的结果，你可以使用 -G 选项来使用正则表达式搜索。

行日志搜索
行日志搜索是另一个相当高级并且有用的日志搜索功能。 这是一个最近新增的不太知名的功能，但却是十分有用。 在 git log 后加上 -L 选项即可调用，它可以展示代码中一行或者一个函数的历史。

例如，假设我们想查看 zlib.c 文件中git_deflate_bound 函数的每一次变更，我们可以执行 git log -L :git_deflate_bound:zlib.c。 Git 会尝试找出这个函数的范围，然后查找历史记录，并且显示从函数创建之后一系列变更对应的补丁。
```    
$ git log -L :git_deflate_bound:zlib.c
commit ef49a7a0126d64359c974b4b3b71d7ad42ee3bca
Author: Junio C Hamano <gitster@pobox.com>
Date:   Fri Jun 10 11:52:15 2011 -0700

    zlib: zlib can only process 4GB at a time

diff --git a/zlib.c b/zlib.c
--- a/zlib.c
+++ b/zlib.c
@@ -85,5 +130,5 @@
-unsigned long git_deflate_bound(z_streamp strm, unsigned long size)
+unsigned long git_deflate_bound(git_zstream *strm, unsigned long size)
 {
-       return deflateBound(strm, size);
+       return deflateBound(&strm->z, size);
 }


commit 225a6f1068f71723a910e8565db4e252b3ca21fa
Author: Junio C Hamano <gitster@pobox.com>
Date:   Fri Jun 10 11:18:17 2011 -0700

    zlib: wrap deflateBound() too

diff --git a/zlib.c b/zlib.c
--- a/zlib.c
+++ b/zlib.c
@@ -81,0 +85,5 @@
+unsigned long git_deflate_bound(z_streamp strm, unsigned long size)
+{
+       return deflateBound(strm, size);
+}
+
```

如果 Git 无法计算出如何匹配你代码中的函数或者方法，你可以提供一个正则表达式。 例如，这个命令和上面的是等同的：

git log -L '/unsigned long git_deflate_bound/',/^}/:zlib.c

。 你也可以提供单行或者一个范围的行号来获得相同的输出。

git log 查看提交记录，参数：
-n      (n是一个正整数)，查看最近n次的提交信息

-- fileName     fileName为任意文件名，查看指定文件的提交信息。(注：文件名应该放到参数的最后位置，通常在前面加上--并用空格隔开表示是文件。)

$ git log file1 file2   查看file1文件file2文件的提交记录
$ git log file/         查看file文件夹下所有文件的提交记录


--branchName    branchName为任意一个分支名字，查看莫个分支上的提交记录。同上，需要放到参数中的最后位置处。(注：如果分支名与文件名相同，系统会提示错误，可通过--选项来指定给定的参数是分支名还是文件名。)例：在当前分支中有一个名为v1的文件，同时还存在一个名为v1的分支,则：

$ git log v1 -- 此时的v1代表的是分支名字
$ git log -- v1 此时的v1代表的是名为v1的文件
$ git log v1 -- v1


tagName或branchame               查询指定标签/分支中的提交记录信息

$ git log v1.0..        查询从v1.0以后的提交历史记录(不包含v1.0)
$ git log test..master  查询master分支中的提交记录但不包含test分支记录
$ git log master..test  查询test分支中的提交记录但不办含master分支记录
$ git log master...test 查询master或test分支中的提交记录。
$ git log test --not master　　屏蔽master分支


根据commit查询日志    

$ git log commit  　　查询commit之前的记录，包含commit
$ git log commit1 commit2 查询commit1与commit2之间的记录，包括commit1和commit2
$ git log commit1..commit2 同上，但是不包括commit1

其中，commit可以是提交哈希值的简写模式，也可以使用HEAD代替。HEAD代表最后一次提交，HEAD^为最后一个提交的父提交，等同于HEAD～1，HEAD～2代表倒数第二次提交



git log format
=====================
--pretty        按指定格式显示日志信息,可选项有：oneline,short,medium,full,fuller,email,raw以及format:<string>,默认为medium，可以通过修改配置文件来指定默认的方式
常见的format选项：
选项     说明
%H      提交对象（commit）的完整哈希字串
%h      提交对象的简短哈希字串
%T      树对象（tree）的完整哈希字串
%t      树对象的简短哈希字串
%P      父对象（parent）的完整哈希字串
%p      父对象的简短哈希字串
%an     作者（author）的名字
%ae     作者的电子邮件地址
%ad     作者修订日期（可以用 -date= 选项定制格式）
%ar     作者修订日期，按多久以前的方式显示
%cn     提交者(committer)的名字
%ce     提交者的电子邮件地址
%cd     提交日期
%cr     提交日期，按多久以前的方式显示
%s      提交说明

$ git log --pretty=format:"%an %ae %ad %cn %ce %cd %cr %s" --graph



--mergs 查看所有合并过的提交历史记录

--no-merges     查看所有未被合并过的提交信息

--author=someonet       查询指定作者的提交记录

$ git log --author=gbyukg
--since，--affter       仅显示指定时间之后的提交(不包含当前日期)

--until，--before       仅显示指定时间之前的提交(包含当前日期)

$ git log --before={3,weeks,ago} --after={2010-04-18}
--grep  通过提交说明信息过滤提交日志

$ git log --grep=hotfix 该命令会列出所有包含hotfix字样的提交信息说明的提交记录
注意：如果想同时使用--grep和--author，必须在附加一个--all-match参数。

-S      通过查询文件的变更内容来检索出指定提交日志 注：-S后没有"="，与查询内容之间也没有空格符

$ git log --Snew
-p      查看提交时的补丁信息

$ git log -p --no-merges -2

我们常用 -p 选项 展开显示每次提交的内容差异，用 -2 则仅显示最近的两次更新：

$ git log -p -2



--stat  列出文件的修改行数

--sortstat      只显示--stat中最后行数修改添加移除的统计

--graph 以简单的图形方式列出提交记录



比较提交 - Git Diff
=========================
你可以用 git diff 来比较项目中任意两个版本的差异。

$ git diff master..test
上面这条命令只显示两个分支间的差异，如果你想找出‘master’,‘test’的共有 父分支和'test'分支之间的差异，你用3个‘.'来取代前面的两个'.' 。

$ git diff master...test
git diff 是一个难以置信的有用的工具，可以找出你项目上任意两点间 的改动，或是用来查看别人提交进来的新分支。

哪些内容会被提交(commit)

你通常用git diff来找你当前工作目录和上次提交与本地索引间的差异。

$ git diff
上面的命令会显示在当前的工作目录里的，没有 staged(添加到索引中)，且在下次提交时 不会被提交的修改。

如果你要看在下次提交时要提交的内容(staged,添加到索引中),你可以运行：

$ git diff --cached
上面的命令会显示你当前的索引和上次提交间的差异；这些内容在不带"-a"参数运行 "git commit"命令时就会被提交。

$ git diff HEAD
上面这条命令会显示你工作目录与上次提交时之间的所有差别，这条命令所显示的 内容都会在执行"git commit -a"命令时被提交。

更多的比较选项

如果你要查看当前的工作目录与另外一个分支的差别，你可以用下面的命令执行:

$ git diff test
这会显示你当前工作目录与另外一个叫'test'分支的差别。你也以加上路径限定符，来只 比较某一个文件或目录。

$ git diff HEAD -- ./lib 
上面这条命令会显示你当前工作目录下的lib目录与上次提交之间的差别(或者更准确的 说是在当前分支)。

如果不是查看每个文件的详细差别，而是统计一下有哪些文件被改动，有多少行被改 动，就可以使用‘--stat' 参数。

$>git diff --stat
 layout/book_index_template.html                    |    8 ++-
 text/05_Installing_Git/0_Source.markdown           |   14 ++++++
 text/05_Installing_Git/1_Linux.markdown            |   17 +++++++
 text/05_Installing_Git/2_Mac_104.markdown          |   11 +++++
 text/05_Installing_Git/3_Mac_105.markdown          |    8 ++++
 text/05_Installing_Git/4_Windows.markdown          |    7 +++
 .../1_Getting_a_Git_Repo.markdown                  |    7 +++-
 .../0_ Comparing_Commits_Git_Diff.markdown         |   45 +++++++++++++++++++-
 .../0_ Hosting_Git_gitweb_repoorcz_github.markdown |    4 +-
 9 files changed, 115 insertions(+), 6 deletions(-)
有时这样全局性的查看哪些文件被修改，能让你更轻轻一点。

--abbrev-commit 仅显示 SHA-1 的前几个字符，而非所有的 40 个字符。

--relative-date 使用较短的相对时间显示（比如，“2 weeks ago”）。

--name-only 仅在提交信息后显示已修改的文件清单。

--name-status 显示新增、修改、删除的文件清单。

GIT Blame
用来查看文件的每个部分修改详情

$git blame index.php
