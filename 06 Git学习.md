# 1 Git服务安装

## 1.1 容器化安装

+ 安装网址：<https://docs.gitlab.com/ee/install/docker.html>

+ 创建挂载目录

```shell
export GITLAB_HOME=/srv/gitlab
# rm -rfv $GITLAB_HOME     # 清空数据重新初始化
mkdir -p $GITLAB_HOME/config
mkdir -p $GITLAB_HOME/logs
mkdir -p $GITLAB_HOME/data
```

+ 在selinux关闭的情况下(二选一)

```shell

docker run --detach \
  --hostname gitlab.example.com \
  --publish 443:443 --publish 80:80 --publish 22:22 \
  --name gitlab \
  --restart always \
  --volume $GITLAB_HOME/config:/etc/gitlab \
  --volume $GITLAB_HOME/logs:/var/log/gitlab \
  --volume $GITLAB_HOME/data:/var/opt/gitlab \
  --shm-size 256m \
  gitlab/gitlab-ee:latest

```

+ 在selinux开启的情况下(二选一)

```shell
 docker run --detach \
  --hostname gitlab.example.com \
  --publish 8443:443 --publish 8880:80 --publish 8822:22 \
  --name gitlab \
  --restart always \
  --volume $GITLAB_HOME/config:/etc/gitlab:Z \
  --volume $GITLAB_HOME/logs:/var/log/gitlab:Z \
  --volume $GITLAB_HOME/data:/var/opt/gitlab:Z \
  --shm-size 256m \
  gitlab/gitlab-ee:latest

```

+ 查看启动日志

```shell
docker logs -f gitlab
```

+ 查看初始密码（用户名为root）

```shell
[root@Docker1 ~]# docker exec -it gitlab grep 'Password:' /etc/gitlab/initial_root_password
Password: 2eIIr+azQwg/7spjNCkFW4g1Tpkk4xenqMcNBkvVgYg=
[root@Docker1 ~]#

```

+ 访问URL: 

   <http://192.166.1.241/>

  <https://192.166.1.241:8443>

 ## 1.2 配置git（可不做）

+ 进入docker 或直接配置

```shell
docker exec -it gitlab /bin/bash

# docker exec -it gitlab editor /etc/gitlab/gitlab.rb
```

+ 配置文件： /etc/gitlab/gitlab.rb

```shell
# For HTTP
external_url 'http://10.1.2.93:8880'     ### 此处端口影响gitlab的侦听端口，建议使用侦听端口作映射
# For ssh
gitlab_rails['gitlab_shell_ssh_port'] = 8822                                     
```

+ 重启docker

```shell
docker restart gitlab
```

## 1.3 创建用户

界面操作（略）

http://192.166.1.241/admin/users

## 1.4 生成ssh-rsa

+ 在本地机器（客户端）上，执行ssh-keygen，生成  ~/.ssh/id_rsa.pub  和 ~/.ssh/id_rsa
+ 将 ~/.ssh/id_rsa.pub的内容拷贝到 用户 --> Perferences  --> SSH Keys

# 2 Git实验

## 2.1 实例一 远程已有仓库

在界面 上创建仓库（略）

### 2.1.1 Git全局设置（设置一次即可）

```shell
[root@Docker1 golang]# git config --global user.name "X4033"
[root@Docker1 golang]# git config --global user.email "X4033@fh.com"
[root@Docker1 channel-go]# git config --list
user.name=X4033
user.email=X4033@fh.com
core.repositoryformatversion=0
core.filemode=true
core.bare=false
core.logallrefupdates=true
remote.origin.url=git@192.166.1.241:X4033/channel-go.git
remote.origin.fetch=+refs/heads/*:refs/remotes/origin/*
branch.main.remote=origin
branch.main.merge=refs/heads/main
branch.test1.remote=origin
branch.test1.merge=refs/heads/test1
[root@Docker1 channel-go]#
```

### 2.1.2 拉取远程，建立分支

```shell
##########################  clone远程仓库到本地
[root@Docker1 golang]# git clone git@192.166.1.241:X4033/channel-go.git
Cloning into 'channel-go'...
The authenticity of host '192.166.1.241 (192.166.1.241)' can't be established.
ECDSA key fingerprint is SHA256:yeWpBCYFRntZb8prLgT3qnPsyJCNMP8V3+GNBauUiqw.
ECDSA key fingerprint is MD5:17:3f:c3:a3:3e:7e:d2:10:37:eb:03:22:9d:7d:92:0c.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '192.166.1.241' (ECDSA) to the list of known hosts.
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Receiving objects: 100% (3/3), done.
[root@Docker1 golang]# 
[root@Docker1 golang]# cd channel-go/
[root@Docker1 channel-go]# ls
README.md
[root@Docker1 channel-go]# git branch		# 查看本地分支
* main
[root@Docker1 channel-go]# git branch  -a	# 查看所有分支
* main
  remotes/origin/HEAD -> origin/main
  remotes/origin/main
[root@Docker1 channel-go]# git checkout -b "test1"     #建立test1分支（本地）
Switched to a new branch 'test1'
[root@Docker1 channel-go]# git branch
  main
* test1
[root@Docker1 channel-go]# git branch  -a
  main
* test1
  remotes/origin/HEAD -> origin/main
  remotes/origin/main
[root@Docker1 channel-go]# git status
# On branch test1
nothing to commit, working directory clean
[root@Docker1 channel-go]#
[root@Docker1 channel-go]# git push origin test1    #推送test1分支到远程，创建远程分支
Total 0 (delta 0), reused 0 (delta 0)
remote:
remote: To create a merge request for test1, visit:
remote:   http://gitlab.example.com/X4033/channel-go/-/merge_requests/new?merge_request%5Bsource_branch%5D=test1
remote:
To git@192.166.1.241:X4033/channel-go.git
 * [new branch]      test1 -> test1
[root@Docker1 channel-go]# git push origin test1
Everything up-to-date
[root@Docker1 channel-go]#

```

### 2.1.3 合并到远程

```shell
[root@Docker1 channel-go]# git add .
[root@Docker1 channel-go]# git commit -m "01.go"
[test1 47a7507] 01.go
 1 file changed, 14 insertions(+)
 create mode 100644 01.go
[root@Docker1 channel-go]# 
##########################  推送到远程分支
[root@Docker1 channel-go]# git push -u origin test1	
Counting objects: 4, done.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 457 bytes | 0 bytes/s, done.
Total 3 (delta 0), reused 0 (delta 0)
remote:
remote: To create a merge request for test1, visit:
remote:   http://gitlab.example.com/X4033/channel-go/-/merge_requests/new?merge_request%5Bsource_branch%5D=test1
remote:
To git@192.166.1.241:X4033/channel-go.git
   ccf75da..04d8b6d  test1 -> test1
Branch test1 set up to track remote branch test1 from origin.
[root@Docker1 channel-go]#
```

> 1. <font color=red>开发者</font>访问 http://192.166.1.241/X4033/channel-go/-/merge_requests/new?merge_request%5Bsource_branch%5D=test1 ，申请合并到主分支；
> 2. <font color=red>管理员</font>访问 http://192.166.1.241/X4033/channel-go/-/merge_requests/1 ， 进行合并操作；<font color=orange>在合并时不要删除源分支，否则开发者的分支就没了。</font>

### 2.1.4 同步远程主分支到本地

> 此操作同步共同开发者合并的代码；通过界面操作在main分支下增加一个名为ingress-controller-1.yml.yaml的文件；

```shell
[root@Docker1 channel-go]# git fetch   #从远程获取代码库
remote: Enumerating objects: 6, done.
remote: Counting objects: 100% (6/6), done.
remote: Compressing objects: 100% (4/4), done.
remote: Total 4 (delta 0), reused 1 (delta 0), pack-reused 0
Unpacking objects: 100% (4/4), done.
From 192.166.1.241:X4033/channel-go
   ccf75da..db521c1  main       -> origin/main
[root@Docker1 channel-go]# ls
01.go  README.md
[root@Docker1 channel-go]# 
##########################  将远程仓库里主分支的数据合并到本地分支
[root@Docker1 channel-go]# git merge origin test1 
Updating 04d8b6d..db521c1
Fast-forward
 ingress-controller-1.yml.yaml | 682 +++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
 1 file changed, 682 insertions(+)
 create mode 100644 ingress-controller-1.yml.yaml
[root@Docker1 channel-go]#
[root@Docker1 channel-go]# ls
01.go  ingress-controller-1.yml.yaml  README.md
[root@Docker1 channel-go]#
[root@Docker1 channel-go]# git pull origin test1
From 192.166.1.241:X4033/channel-go
 * branch            test1      -> FETCH_HEAD
Already up-to-date.
[root@Docker1 channel-go]#

```

### 2.1.5 回退到历史commit

```shell
########################### 先使用git log查看历史版本
[root@Docker1 channel-go]# git log
commit 8c0af9a50158f9f170682a26463a32de52213e41
Author: X4033 <X4033@fh.com>
Date:   Sat Apr 23 18:36:12 2022 +0800

    01.go_update01

commit db521c1a793fdcf29c1ffe3d91ab2401dd21e11a
Author: X4033 <x4033@fh.com>
Date:   Sat Apr 23 10:10:10 2022 +0000

    Upload New File

commit 84457a539b1875bb35d53c09aadeb1e51d234d33
Merge: ccf75da 04d8b6d
Author: X4033 <x4033@fh.com>
Date:   Sat Apr 23 10:01:47 2022 +0000

    Merge branch 'test1' into 'main'

    01.go

    See merge request X4033/channel-go!1

commit 04d8b6dd8ee8ab8d91bad9250978ec4c71f73310
Author: X4033 <X4033@fh.com>
Date:   Sat Apr 23 17:40:02 2022 +0800

    01.go

commit ccf75dabd3c9342bc07eadb2fe2653de303ad41f
Author: X4033 <x4033@fh.com>
Date:   Fri Apr 22 16:27:58 2022 +0000

    Initial commit
[root@Docker1 channel-go]#
########################### git reset强制恢复到历史版本
[root@Docker1 channel-go]# git reset --hard  04d8b6dd8ee8ab8d91bad9250978ec4c71f73310
HEAD is now at 04d8b6d 01.go
[root@Docker1 channel-go]# ls
01.go  README.md
[root@Docker1 channel-go]# 
[root@Docker1 channel-go]# vim 01.go
########################### 修改文件后，提交远程；需提前把远程分支删除，否则会rejected
[root@Docker1 channel-go]# git add .
[root@Docker1 channel-go]# git commit -m "01.go_update02"
[test1 ddddd6f] 01.go_update02
 1 file changed, 1 insertion(+), 1 deletion(-)
[root@Docker1 channel-go]# ls
01.go  README.md
[root@Docker1 channel-go]# 
[root@Docker1 channel-go]# git push -u origin test1
Counting objects: 5, done.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 296 bytes | 0 bytes/s, done.
Total 3 (delta 1), reused 0 (delta 0)
remote:
remote: To create a merge request for test1, visit:
remote:   http://gitlab.example.com/X4033/channel-go/-/merge_requests/new?merge_request%5Bsource_branch%5D=test1
remote:
To git@192.166.1.241:X4033/channel-go.git
 * [new branch]      test1 -> test1
Branch test1 set up to track remote branch test1 from origin.
[root@Docker1 channel-go]#
```

> 1. 此时不能同步远程到本地，否则文件内容恢复；
> 2. 应先删除远程同名分支；
> 3. 修改文件后，合并到远程，并提交merge申请，同步到主分支中；
> 4. 再同步远程分支到本地；




# 3 Git其它示例

## 3.1  删除掉本地不存在的远程分支

多人合作开发时，如果远程的分支被其他开发删除掉，在本地执行 `git branch --all` 依然会显示该远程分支，可使用下列的命令进行删除：

```shell
# 使用 pull 命令，添加 -p 参数
$ git pull -p

# 等同于下面的命令
$ git fetch -p
$ git fetch --prune origin
```

## 3.2 带token摘取git内容

```shell

# 带token摘取git内容，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为ydsl01的token，有效期到20230402
git clone https://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@github.com/ydsl01/kubernetes-study-documents-51.git

```



# 附件

## 附件1 git命令实践（朱）

```shell
# Git全局设置（设置一次即可）
git config --global user.name "zhu"
git config --global user.email "zhu@admin.com"

# 推送现有文件夹
cd existing_folder
git init
git remote add origin git@61.155.203.87:zhu/fhops.git
git add .
git commit -m "Initial commit"
git push -u origin master

# 创建一个新仓库（未测试）
git clone git@61.155.203.87:zhu/fhops.git
cd fhops
touch README.md
git add README.md
git commit -m "add README"
git push -u origin master

# 推送现有的Git仓库（未测试）
cd existing_repo
git remote rename origin old-origin
git remote add origin git@61.155.203.87:zhu/fhops.git
git push -u origin --all
git push -u origin --tags

*********** 实战 ***********
# 拉取整个项目（首次初始化使用）
git clone git@61.155.203.87:zhu/fhops.git

# 查看分支
git branch                  # 查看本地分支
git branch -a               # 查看所有分支

git checkout -b x0854       # 创建分支（x0854）
git status                  # 查看状态

# 推送新分支
git add .                   # 保存本地修改
git commit -m "xxxx"        # 添加描述信息
git push -u origin x0854    # 推送分支名称（x0854）

# 从master获取
git fetch                   # 获取更新
git merge origin master     # 获取最新master数据
git pull					# 获取最新master数据

# 删除分支
git checkout master         # 切换到master分支
git branch -d x0854         # 删除本地分支
git push origin -d x0854    # 删除远程分支

```

## 附件2 git命令大全

### git config

配置 Git 的相关参数。

Git 一共有3个配置文件：

> 1. 仓库级的配置文件：在仓库的 `.git/.gitconfig`，该配置文件只对所在的仓库有效。
> 2. 全局配置文件：Mac 系统在 `~/.gitconfig`，Windows 系统在 `C:\Users\<用户名>\.gitconfig`。
>  3. 系统级的配置文件：在 Git 的安装目录下（Mac 系统下安装目录在 `/usr/local/git`）的 `etc` 文件夹中的 `gitconfig`。

```shell
# 查看配置信息
# --local：仓库级，--global：全局级，--system：系统级
$ git config <--local | --global | --system> -l

# 查看当前生效的配置信息
$ git config -l

# 编辑配置文件
# --local：仓库级，--global：全局级，--system：系统级
$ git config <--local | --global | --system> -e

# 添加配置项
# --local：仓库级，--global：全局级，--system：系统级
$ git config <--local | --global | --system> --add <name> <value>

# 获取配置项
$ git config <--local | --global | --system> --get <name>

# 删除配置项
$ git config <--local | --global | --system> --unset <name>

# 配置提交记录中的用户信息
$ git config --global user.name <用户名>
$ git config --global user.email <邮箱地址>

# 更改Git缓存区的大小
# 如果提交的内容较大，默认缓存较小，提交会失败
# 缓存大小单位：B，例如：524288000（500MB）
$ git config --global http.postBuffer <缓存大小>

# 调用 git status/git diff 命令时以高亮或彩色方式显示改动状态
$ git config --global color.ui true

# 配置可以缓存密码，默认缓存时间15分钟
$ git config --global credential.helper cache

# 配置密码的缓存时间
# 缓存时间单位：秒
$ git config --global credential.helper 'cache --timeout=<缓存时间>'

# 配置长期存储密码
$ git config --global credential.helper store
```

### git clone

从远程仓库克隆一个版本库到本地。

```bash
# 默认在当前目录下创建和版本库名相同的文件夹并下载版本到该文件夹下
$ git clone <远程仓库的网址>

# 指定本地仓库的目录
$ git clone <远程仓库的网址> <本地目录>

# -b 指定要克隆的分支，默认是master分支
$ git clone <远程仓库的网址> -b <分支名称> <本地目录>
```

### git init

初始化项目所在目录，初始化后会在当前目录下出现一个名为 .git 的目录。

```shell
# 初始化本地仓库，在当前目录下生成 .git 文件夹
$ git init
```

### git status

查看本地仓库的状态。

```shell
# 查看本地仓库的状态
$ git status

# 以简短模式查看本地仓库的状态
# 会显示两列，第一列是文件的状态，第二列是对应的文件
# 文件状态：A 新增，M 修改，D 删除，?? 未添加到Git中
$ git status -s
```

### git remote

操作远程库。

```shell
# 列出已经存在的远程仓库
$ git remote

# 列出远程仓库的详细信息，在别名后面列出URL地址
$ git remote -v
$ git remote --verbose

# 添加远程仓库
$ git remote add <远程仓库的别名> <远程仓库的URL地址>

# 修改远程仓库的别名
$ git remote rename <原远程仓库的别名> <新的别名>

# 删除指定名称的远程仓库
$ git remote remove <远程仓库的别名>

# 修改远程仓库的 URL 地址
$ git remote set-url <远程仓库的别名> <新的远程仓库URL地址>
```

### git branch

操作 Git 的分支命令。

```shell
# 列出本地的所有分支，当前所在分支以 "*" 标出
$ git branch

# 列出本地的所有分支并显示最后一次提交，当前所在分支以 "*" 标出
$ git branch -v

# 创建新分支，新的分支基于上一次提交建立
$ git branch <分支名>

# 修改分支名称
# 如果不指定原分支名称则为当前所在分支
$ git branch -m [<原分支名称>] <新的分支名称>
# 强制修改分支名称
$ git branch -M [<原分支名称>] <新的分支名称>

# 删除指定的本地分支
$ git branch -d <分支名称>

# 强制删除指定的本地分支
$ git branch -D <分支名称>
```

### git checkout

检出命令，用于创建、切换分支等。

```shell
# 切换到已存在的指定分支
$ git checkout <分支名称>

# 创建并切换到指定的分支，保留所有的提交记录
# 等同于 "git branch" 和 "git checkout" 两个命令合并
$ git checkout -b <分支名称>

# 创建并切换到指定的分支，删除所有的提交记录
$ git checkout --orphan <分支名称>

# 替换掉本地的改动，新增的文件和已经添加到暂存区的内容不受影响
$ git checkout <文件路径>
```

### git cherry-pick

把已经提交的记录合并到当前分支。

```shell
# 把已经提交的记录合并到当前分支
$ git cherry-pick <commit ID>
```

### git add

把要提交的文件的信息添加到暂存区中。当使用 git commit 时，将依据暂存区中的内容来进行文件的提交。

```csharp
# 把指定的文件添加到暂存区中
$ git add <文件路径>

# 添加所有修改、已删除的文件到暂存区中
$ git add -u [<文件路径>]
$ git add --update [<文件路径>]

# 添加所有修改、已删除、新增的文件到暂存区中，省略 <文件路径> 即为当前目录
$ git add -A [<文件路径>]
$ git add --all [<文件路径>]

# 查看所有修改、已删除但没有提交的文件，进入一个子命令系统
$ git add -i [<文件路径>]
$ git add --interactive [<文件路径>]
```

### git commit

将暂存区中的文件提交到本地仓库中。

```shell
# 把暂存区中的文件提交到本地仓库，调用文本编辑器输入该次提交的描述信息
$ git commit

# 把暂存区中的文件提交到本地仓库中并添加描述信息
$ git commit -m "<提交的描述信息>"

# 把所有修改、已删除的文件提交到本地仓库中
# 不包括未被版本库跟踪的文件，等同于先调用了 "git add -u"
$ git commit -a -m "<提交的描述信息>"

# 修改上次提交的描述信息
$ git commit --amend
```

### git fetch

从远程仓库获取最新的版本到本地的 tmp 分支上。

```shell
# 将远程仓库所有分支的最新版本全部取回到本地
$ git fetch <远程仓库的别名>

# 将远程仓库指定分支的最新版本取回到本地
$ git fetch <远程主机名> <分支名>
```

### git merge

合并分支。

```shell
# 把指定的分支合并到当前所在的分支下
$ git merge <分支名称>
```

### git diff

比较版本之间的差异。

```shell
# 比较当前文件和暂存区中文件的差异，显示没有暂存起来的更改
$ git diff

# 比较暂存区中的文件和上次提交时的差异
$ git diff --cached
$ git diff --staged

# 比较当前文件和上次提交时的差异
$ git diff HEAD

# 查看从指定的版本之后改动的内容
$ git diff <commit ID>

# 比较两个分支之间的差异
$ git diff <分支名称> <分支名称>

# 查看两个分支分开后各自的改动内容
$ git diff <分支名称>...<分支名称>
```

### git pull

从远程仓库获取最新版本并合并到本地。
 首先会执行 `git fetch`，然后执行 `git merge`，把获取的分支的 HEAD 合并到当前分支。

```shell
# 从远程仓库获取最新版本。
$ git pull
```

### git push

把本地仓库的提交推送到远程仓库。

```shell
# 把本地仓库的分支推送到远程仓库的指定分支
$ git push <远程仓库的别名> <本地分支名>:<远程分支名>

# 删除指定的远程仓库的分支
$ git push <远程仓库的别名> :<远程分支名>
$ git push <远程仓库的别名> --delete <远程分支名>
```

### git log

显示提交的记录。

```bash
# 打印所有的提交记录
$ git log

# 打印从第一次提交到指定的提交的记录
$ git log <commit ID>

# 打印指定数量的最新提交的记录
$ git log -<指定的数量>
```

### git reset

还原提交记录。

```shell
# 重置暂存区，但文件不受影响
# 相当于将用 "git add" 命令更新到暂存区的内容撤出暂存区，可以指定文件
# 没有指定 commit ID 则默认为当前 HEAD
$ git reset [<文件路径>]
$ git reset --mixed [<文件路径>]

# 将 HEAD 的指向改变，撤销到指定的提交记录，文件未修改
$ git reset <commit ID>
$ git reset --mixed <commit ID>

# 将 HEAD 的指向改变，撤销到指定的提交记录，文件未修改
# 相当于调用 "git reset --mixed" 命令后又做了一次 "git add"
$ git reset --soft <commit ID>

# 将 HEAD 的指向改变，撤销到指定的提交记录，文件也修改了
$ git reset --hard <commit ID>
```

### git revert

生成一个新的提交来撤销某次提交，此次提交之前的所有提交都会被保留。

```shell
# 生成一个新的提交来撤销某次提交
$ git revert <commit ID>
```

### git tag

操作标签的命令。

```shell
# 打印所有的标签
$ git tag

# 添加轻量标签，指向提交对象的引用，可以指定之前的提交记录
$ git tag <标签名称> [<commit ID>]

# 添加带有描述信息的附注标签，可以指定之前的提交记录
$ git tag -a <标签名称> -m <标签描述信息> [<commit ID>]

# 切换到指定的标签
$ git checkout <标签名称>

# 查看标签的信息
$ git show <标签名称>

# 删除指定的标签
$ git tag -d <标签名称>

# 将指定的标签提交到远程仓库
$ git push <远程仓库的别名> <标签名称>

# 将本地所有的标签全部提交到远程仓库
$ git push <远程仓库的别名> –tags
```

### git mv

重命名文件或者文件夹。

```shell
# 重命名指定的文件或者文件夹
$ git mv <源文件/文件夹> <目标文件/文件夹>
```

### git rm

删除文件或者文件夹。

```shell
# 移除跟踪指定的文件，并从本地仓库的文件夹中删除
$ git rm <文件路径>

# 移除跟踪指定的文件夹，并从本地仓库的文件夹中删除
$ git rm -r <文件夹路径>

# 移除跟踪指定的文件，在本地仓库的文件夹中保留该文件
$ git rm --cached
```



# END

