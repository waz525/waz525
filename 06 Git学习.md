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

 ## 1.2 配置git（可不做）

+ 进入docker 或直接配置

```shell
docker exec -it gitlab /bin/bash

# docker exec -it gitlab editor /etc/gitlab/gitlab.rb
```

+ 配置文件： /etc/gitlab/gitlab.rb

```shell
# For HTTP
external_url 'http://192.166.1.241/'     ### 此处端口影响gitlab的侦听端口，建议使用侦听端口作映射
# For ssh
gitlab_rails['gitlab_shell_ssh_port'] = 8822                                     
```

+ 重启docker

```shell
docker restart gitlab
```

## 1.3 创建用户

用root用户访问如下URL，界面操作（略）

http://192.166.1.241/admin/users

## 1.4 生成ssh-rsa，配置用户公钥

+ 在本地机器（客户端）上，执行ssh-keygen，生成  ~/.ssh/id_rsa.pub  和 ~/.ssh/id_rsa
+ 将 ~/.ssh/id_rsa.pub的内容拷贝到 用户 --> Perferences  --> SSH Keys

## 1.5 利用docker-compose启动容器

### 1.5.1 docker-compose.yml

```shell
[root@Docker1 gitlab]# pwd
/home/gitlab
[root@Docker1 gitlab]# ls
config  data  docker-compose.yml  logs
[root@Docker1 gitlab]#
[root@Docker1 gitlab]# cat docker-compose.yml
version: '2.0'
services:
  gitlab:
    image: gitlab/gitlab-ee:latest
    hostname: 192.166.1.241
    # restart: always
    container_name: gitlab2
    ports:
      - "8053:443"
      - "8052:80"
      - "60022:22"
    volumes:
      - /home/gitlab/config:/etc/gitlab
      - /home/gitlab/logs:/var/log/gitlab
      - /home/gitlab/data:/var/opt/gitlab
    shm_size: 256m

[root@Docker1 gitlab]#
```

### 1.5.2 启动容器

> 在docker-compose.yml的目录下执行 `docker-compose up -d`

```shell
[root@Docker1 gitlab]# ls
config  data  docker-compose.yml  logs
[root@Docker1 gitlab]# docker-compose up -d      # -d 是放后台运行
[+] Running 2/2
 ⠿ Network gitlab_default  Created         0.4s
 ⠿ Container gitlab2       Started         0.8s
[root@Docker1 gitlab]#
[root@Docker1 gitlab]# docker ps
CONTAINER ID        IMAGE                     COMMAND             CREATED              STATUS                                 PORTS                                                                NAMES
56a07f00c4c6        gitlab/gitlab-ee:latest   "/assets/wrapper"   About a minute ago   Up About a minute (health: starting)   0.0.0.0:60022->22/tcp, 0.0.0.0:8052->80/tcp, 0.0.0.0:8053->443/tcp   gitlab2
[root@Docker1 gitlab]#
```

### 1.5.3 停止容器

>  在docker-compose.yml的目录下执行 `docker-compose down`

```shell
[root@Docker1 gitlab]# ls
config  data  docker-compose.yml  logs
[root@Docker1 gitlab]# docker-compose down
[+] Running 2/2
 ⠿ Container gitlab2       Removed        11.0s
 ⠿ Network gitlab_default  Removed        0.4s
[root@Docker1 gitlab]#
[root@Docker1 gitlab]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
[root@Docker1 gitlab]#
```



### 1.5.4 带端口的git clone

```shell
[root@Docker1 golang]# git clone ssh://git@192.166.1.241:60022/X4033/channel-go.git
Cloning into 'channel-go'...
The authenticity of host '[192.166.1.241]:60022 ([192.166.1.241]:60022)' can't be established.
ECDSA key fingerprint is SHA256:BP+o+BZrRIJFO0Rufh2T8h/3HW/UwZAlYZ8PWS4l+PY.
ECDSA key fingerprint is MD5:e1:c9:fe:de:84:b4:db:69:cc:4d:50:9a:d0:1a:3d:a0.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '[192.166.1.241]:60022' (ECDSA) to the list of known hosts.
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Receiving objects: 100% (3/3), done.
[root@Docker1 golang]# ls
channel-go
[root@Docker1 golang]# 
```



# 2 Git实验

## 2.1 实例一：远程已有仓库

在界面 上创建仓库（略）

### 2.1.1 Git全局设置（设置一次即可）

```shell
[root@Docker1 golang]# git config --global user.name "X4033"
[root@Docker1 golang]# git config --global user.email "X4033@fh.com"
[root@Docker1 golang]# git config --list
user.name=X4033
user.email=X4033@fh.com
[root@Docker1 golang]#
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
[root@Docker1 channel-go]# git add --all .
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

## 2.2 实验二：命令行创建新项目

```shell
############################# 1. 创建一个空目录
[root@Docker1 golang]# mkdir interface-go
[root@Docker1 golang]# cd interface-go/
[root@Docker1 interface-go]# ls
############################# 2. 初始化git本地库
[root@Docker1 interface-go]# git init
[root@Docker1 interface-go]# cp /home/golang/golang/21.go .
[root@Docker1 interface-go]# ll
total 4
-rw-r--r-- 1 root root 654 Apr 24 09:00 21.go
[root@Docker1 interface-go]#
############################# 3. 提交一个commit
[root@Docker1 interface-go]# git add .
[root@Docker1 interface-go]# git commit -m "Initial commit"
[master (root-commit) a35fb5b] Initial commit
 1 file changed, 31 insertions(+)
 create mode 100644 21.go
[root@Docker1 interface-go]# 
############################# 4. 查看本地分支名
[root@Docker1 interface-go]# git branch
* master
[root@Docker1 interface-go]#
############################# 5. 添加远程仓库
[root@Docker1 interface-go]# git remote add origin git@192.166.1.241:X4033/interface-go.git
[root@Docker1 interface-go]# git remote -v
origin  git@192.166.1.241:X4033/interface-go.git (fetch)
origin  git@192.166.1.241:X4033/interface-go.git (push)
[root@Docker1 interface-go]#
############################# 6. 同步到远程仓库，网页上可以看到新项目
[root@Docker1 interface-go]# git push -u origin master
Counting objects: 3, done.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 586 bytes | 0 bytes/s, done.
Total 3 (delta 0), reused 0 (delta 0)
remote:
remote:
remote: The private project X4033/interface-go was successfully created.
remote:
remote: To configure the remote, run:
remote:   git remote add origin git@gitlab.example.com:X4033/interface-go.git
remote:
remote: To view the project, visit:
remote:   http://gitlab.example.com/X4033/interface-go
remote:
remote:
remote:
To git@192.166.1.241:X4033/interface-go.git
 * [new branch]      master -> master
Branch master set up to track remote branch master from origin.
[root@Docker1 interface-go]#
[root@Docker1 interface-go]# git config --list
user.name=X4033
user.email=X4033@fh.com
core.repositoryformatversion=0
core.filemode=true
core.bare=false
core.logallrefupdates=true
remote.origin.url=git@192.166.1.241:X4033/interface-go.git
remote.origin.fetch=+refs/heads/*:refs/remotes/origin/*
branch.master.remote=origin
branch.master.merge=refs/heads/master
[root@Docker1 interface-go]#
```










## 2.3 Git其它示例

### 2.3.1  删除掉本地不存在的远程分支

多人合作开发时，如果远程的分支被其他开发删除掉，在本地执行 `git branch --all` 依然会显示该远程分支，可使用下列的命令进行删除：

```shell
# 使用 pull 命令，添加 -p 参数
$ git pull -p

# 等同于下面的命令
$ git fetch -p
$ git fetch --prune origin
```

### 2.3.2 带token摘取git内容

```shell

# 带token摘取git内容，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为ydsl01的token，有效期到20230402
git clone https://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@github.com/ydsl01/kubernetes-study-documents-51.git

```

### 2.3.3 界面上删除项目

> 1. 进入项目中
> 2. 左侧菜单栏Settings->General->Advanced->Expend
> 3. 划到最下方有`Remove project`
> 4. 输入需要删除的项目名即可

# 3 gitrunner CI/CD

https://blog.csdn.net/downanddusk/article/details/124130814

https://zhuanlan.zhihu.com/p/441581000

https://cloud.tencent.com/developer/article/1908063?from=article.detail.1429143

https://cloud.tencent.com/developer/article/1429143

## 3.1 使用Shell命令打包

### 3.1.1增加 gitlab-runner-shell

```shell
docker run -d --name gitlab-runner --restart always \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v /srv/gitlab-runner/config:/etc/gitlab-runner \
  gitlab/gitlab-runner
  
docker run -d --name gitlab-runner-shell --restart always \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v /srv/gitlab-runnerr-shell/config:/etc/gitlab-runner \
  gitlab/gitlab-runner
########################################################################

[root@Docker1 gitlab-runner]# docker run -d --name gitlab-runner-shell --restart always \
>   -v /var/run/docker.sock:/var/run/docker.sock \
>   -v /srv/gitlab-runnerr-shell/config:/etc/gitlab-runner \
>   gitlab/gitlab-runner
3829a6906001ff72749600ef8b4b5f64035342b484da9b3b095d35fe57c54d75
[root@Docker1 gitlab-runner]# docker ps
CONTAINER ID        IMAGE                           COMMAND                  CREATED             STATUS                PORTS                                                          NAMES
3829a6906001        gitlab/gitlab-runner            "/usr/bin/dumb-ini..."   6 seconds ago       Up 4 seconds                                                                         gitlab-runner
e84b12290f7b        myjenkins-blueocean:2.332.2-1   "/sbin/tini -- /us..."   2 days ago          Up 2 days             0.0.0.0:8080->8080/tcp, 0.0.0.0:50000->50000/tcp               jenkins-blueocean
58d40eaaae8c        docker:dind                     "dockerd-entrypoin..."   2 days ago          Up 2 days             2375/tcp, 0.0.0.0:2376->2376/tcp                               jenkins-docker
2c859ce66a53        gitlab/gitlab-ee:latest         "/assets/wrapper"        2 weeks ago         Up 2 days (healthy)   0.0.0.0:22->22/tcp, 0.0.0.0:80->80/tcp, 0.0.0.0:443->443/tcp   gitlab
[root@Docker1 gitlab-runner]#

```

```shell
 docker exec -it gitlab-runnerr gitlab-runner register
 
########################################################################

[root@Docker1 gitlab-runner]#  docker exec -it gitlab-runner-shell gitlab-runner register
Runtime platform                                    arch=amd64 os=linux pid=13 revision=f761588f version=14.10.1
Running in system-mode.

Enter the GitLab instance URL (for example, https://gitlab.com/):
http://192.166.1.241/                #gitlab的url
Enter the registration token:
EDusszQ9uyihjoy86Lkj                 #令牌，在下面的页面里查看
Enter a description for the runner:
[3829a6906001]:
Enter tags for the runner (comma-separated):
mRunner                              #标签，随便定
Enter optional maintenance note for the runner:

Registering runner... succeeded                     runner=EDusszQ9
######################
# git-runner的类型，根据实际需要选择；查看https://docs.gitlab.com/runner/executors/index.html
Enter an executor: parallels, docker-ssh, shell, ssh, virtualbox, docker+machine, docker-ssh+machine, custom, docker, kubernetes:
shell  
Runner registered successfully. Feel free to start it, but if it's running already the config should be automatically reloaded!
[root@Docker1 gitlab-runner]# 
######################如果状态为未连接，则需要verify；如果还解决不了就gitlab-runner start
[root@Docker1 gitlab-runner]# docker exec -it gitlab-runner-shell /bin/sh -c "gitlab-runner verify"
Runtime platform                                    arch=amd64 os=linux pid=28 revision=f761588f version=14.10.1
Running in system-mode.

Verifying runner... is alive                        runner=5byH15fx
[root@Docker1 gitlab-runner]#
#############################
```

需要的token、runner状态等信息，请查看 ：<http://192.166.1.241/X4033/interface-go/-/settings/ci_cd#js-runners-settings

![image-20220512235450225](06 Git学习.assets/image-20220512235450225.png)



### 3.1.2 编辑.gitlab-ci.yml文件内容

```yaml
# 创建一个名为pages的任务
pages:
  # 要执行的脚本
  script:
    # 执行脚本
    - echo "Compiling the code..."
    - pwd
    - ls
    - echo "Compile complete."
  # 告诉 Runner 这个名为pages的任务，只在master分支上执行
  only:
    - master
  # GitLab CI有三个默认阶段：构建(build)、测试(test)、部署(deploy)。
  # 告诉 Runner 这个名为pages的任务，只在测试阶段执行
  stage: deploy
  # 告诉 Runner 我们应用哪个标签，与Runner标签一致
  tags:
    - gitlab-runner-shell


```

### 3.1.3 执行流水线（自动触发）

![image-20220515170812823](06 Git学习.assets/image-20220515170812823.png)

本实验只有一项pages任务：

```shell
Running with gitlab-runner 14.10.1 (f761588f)
  on gitlab-runner-shell HomiEkTh
Preparing the "shell" executor
Using Shell executor...
Preparing environment
Running on 87d44f6a806e...
Getting source from Git repository
Executing "step_script" stage of the job script
$ echo "Compiling the code..."
Compiling the code...
$ pwd
/home/gitlab-runner/builds/HomiEkTh/0/X4033/interface-go
$ ls
Dockerfile
README.md
simpleHttpServer.go
$ echo "Compile complete."
Compile complete.
Job succeeded
```







# 4 Jenkins+Git

## 4.1 Docker安装Jenkins

> https://www.jenkins.io/doc/book/installing/docker/

```shell
############################# 1. 创建一个名为jenkins的网络
[root@Docker1 ~]# docker network create jenkins
55efd6686075d99bb30fc03e750480628427765efe6203ca3786ad3681253510
############################# 2. 获取远程jenkins版本包
[root@Docker1 ~]# docker pull jenkins/jenkins:2.332.2-jdk11
Trying to pull repository docker.io/jenkins/jenkins ...
2.332.2-jdk11: Pulling from docker.io/jenkins/jenkins
dbba69284b27: Pull complete
7a9cfc1d8bc9: Pull complete
714fad1b5f60: Pull complete
1c33b24d149a: Pull complete
ae5685b28918: Pull complete
559d6c32b276: Pull complete
59d799692e51: Pull complete
37c3ff68dde1: Pull complete
0b319955f911: Pull complete
633e98b0c3ff: Pull complete
afdef6b9454b: Pull complete
c9fc43dc7764: Pull complete
978ba18a6875: Pull complete
8a7228030cbd: Pull complete
02faeb6b9508: Pull complete
4feb05403fe4: Pull complete
c81d57bc88d8: Pull complete
Digest: sha256:97fbf50e6ba97c25efaf64590b4ed6fcc64b551b1e09c5125a1752ab284a73ad
Status: Downloaded newer image for docker.io/jenkins/jenkins:2.332.2-jdk11
[root@Docker1 ~]#
############################# 3. 启动docker:dind容器（docker in docker）
[root@Docker1 ~]# docker run --name jenkins-docker --rm --detach \
>   --privileged --network jenkins --network-alias docker \
>   --env DOCKER_TLS_CERTDIR=/certs \
>   --volume jenkins-docker-certs:/certs/client \
>   --volume jenkins-data:/var/jenkins_home \
>   --publish 2376:2376 \
>   docker:dind --storage-driver overlay2
Unable to find image 'docker:dind' locally
Trying to pull repository docker.io/library/docker ...
dind: Pulling from docker.io/library/docker
df9b9388f04a: Pull complete
c60e3bd692ff: Pull complete
36df9e333c70: Pull complete
335507422d7f: Pull complete
9b18125cc2d2: Pull complete
cf930f6823a9: Pull complete
7048a2a9629e: Pull complete
dd0af8a24b3d: Pull complete
d4c69def9f22: Pull complete
2998284497aa: Pull complete
67900e1c727f: Pull complete
Digest: sha256:210076c7772f47831afaf7ff200cf431c6cd191f0d0cb0805b1d9a996e99fb5e
Status: Downloaded newer image for docker.io/docker:dind
8b9fc22ee9092f7f9e40115959b8e20595bbd7b034c9246ee0202de20accf533
[root@Docker1 ~]# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                              NAMES
8b9fc22ee909        docker:dind         "dockerd-entrypoin..."   30 seconds ago      Up 29 seconds       2375/tcp, 0.0.0.0:2376->2376/tcp   jenkins-docker
[root@Docker1 ~]#
[root@Docker1 ~]# mkdir /home/jenkins
[root@Docker1 ~]# cd /home/jenkins/
[root@Docker1 jenkins]# ls
[root@Docker1 jenkins]# 
############################# 4. 编辑Dockerfile
[root@Docker1 jenkins]# vim Dockerfile
[root@Docker1 jenkins]# cat Dockerfile
FROM jenkins/jenkins:2.332.2-jdk11
USER root
RUN apt-get update && apt-get install -y lsb-release
RUN curl -fsSLo /usr/share/keyrings/docker-archive-keyring.asc \
  https://download.docker.com/linux/debian/gpg
RUN echo "deb [arch=$(dpkg --print-architecture) \
  signed-by=/usr/share/keyrings/docker-archive-keyring.asc] \
  https://download.docker.com/linux/debian \
  $(lsb_release -cs) stable" > /etc/apt/sources.list.d/docker.list
RUN apt-get update && apt-get install -y docker-ce-cli
USER jenkins
RUN jenkins-plugin-cli --plugins "blueocean:1.25.3 docker-workflow:1.28"
[root@Docker1 jenkins]#
############################# 5. 构建容器
[root@Docker1 jenkins]# docker build -t myjenkins-blueocean:2.332.2-1 .
Sending build context to Docker daemon  2.56 kB
Step 1/8 : FROM jenkins/jenkins:2.332.2-jdk11
 ---> fd576e09d155
Step 2/8 : USER root
 ---> Using cache
 ---> 77de53c8bc40
STEP 3/8: RUN apt-get update && apt-get install -y lsb-release
Get:1 http://security.debian.org/debian-security bullseye-security InRelease [44.1 kB]
Get:2 http://deb.debian.org/debian bullseye InRelease [116 kB]
Get:3 http://deb.debian.org/debian bullseye-updates InRelease [39.4 kB]
Get:4 http://deb.debian.org/debian bullseye/main amd64 Packages [8182 kB]
Get:5 http://security.debian.org/debian-security bullseye-security/main amd64 Packages [126 kB]
Get:6 http://deb.debian.org/debian bullseye-updates/main amd64 Packages [2596 B]
Fetched 8510 kB in 4min 41s (30.3 kB/s)
Reading package lists...
Reading package lists...
Building dependency tree...
Reading state information...
The following additional packages will be installed:
  distro-info-data libmpdec3 libpython3-stdlib libpython3.9-minimal
  libpython3.9-stdlib media-types python3 python3-minimal python3.9
  python3.9-minimal
Suggested packages:
  python3-doc python3-tk python3-venv python3.9-venv python3.9-doc binutils
  binfmt-support
The following NEW packages will be installed:
  distro-info-data libmpdec3 libpython3-stdlib libpython3.9-minimal
  libpython3.9-stdlib lsb-release media-types python3 python3-minimal
  python3.9 python3.9-minimal
0 upgraded, 11 newly installed, 0 to remove and 3 not upgraded.
Need to get 5157 kB of archives.
After this operation, 20.0 MB of additional disk space will be used.
Get:1 http://deb.debian.org/debian bullseye/main amd64 libpython3.9-minimal amd64 3.9.2-1 [801 kB]
.
.
.
Setting up libpython3.9-stdlib:amd64 (3.9.2-1) ...
Setting up libpython3-stdlib:amd64 (3.9.2-3) ...
Setting up python3.9 (3.9.2-1) ...
Setting up python3 (3.9.2-3) ...
running python rtupdate hooks for python3.9...
running python post-rtupdate hooks for python3.9...
Setting up lsb-release (11.1.0) ...
Processing triggers for libc-bin (2.31-13+deb11u3) ...
--> 3335f1564bb
STEP 4/8: RUN curl -fsSLo /usr/share/keyrings/docker-archive-keyring.asc   https://download.docker.com/linux/debian/gpg
--> 7b94cdd9e29
STEP 5/8: RUN echo "deb [arch=$(dpkg --print-architecture)   signed-by=/usr/share/keyrings/docker-archive-keyring.asc]   https://downlo                                                            ad.docker.com/linux/debian   $(lsb_release -cs) stable" > /etc/apt/sources.list.d/docker.list
--> 9386359baef
STEP 6/8: RUN apt-get update && apt-get install -y docker-ce-cli
Hit:1 http://deb.debian.org/debian bullseye InRelease
Hit:2 http://deb.debian.org/debian bullseye-updates InRelease
Get:3 https://download.docker.com/linux/debian bullseye InRelease [43.3 kB]
Hit:4 http://security.debian.org/debian-security bullseye-security InRelease
Get:5 https://download.docker.com/linux/debian bullseye/stable amd64 Packages [9412 B]
Fetched 52.8 kB in 1s (37.3 kB/s)
Reading package lists...
Reading package lists...
Building dependency tree...
Reading state information...
.
.
.
Unpacking docker-scan-plugin (0.17.0~debian-bullseye) ...
Setting up docker-scan-plugin (0.17.0~debian-bullseye) ...
Setting up docker-ce-cli (5:20.10.14~3-0~debian-bullseye) ...
--> 2cc791ae60c
STEP 7/8: USER jenkins
--> be45ea9c487
STEP 8/8: RUN jenkins-plugin-cli --plugins "blueocean:1.25.3 docker-workflow:1.28"
Done
COMMIT myjenkins-blueocean:2.332.2-1
--> 1df13058e89
Successfully tagged localhost/myjenkins-blueocean:2.332.2-1
9c9221c302b519d2d115d0e04939c1ccf1dd35104ee90e76e2e400776c24095a
[root@Docker1 jenkins]# docker images
REPOSITORY                                TAG                 IMAGE ID            CREATED             SIZE
myjenkins-blueocean                       2.332.2-1           9c9221c302b5        14 hours ago        771 MB
<none>                                    <none>              1df13058e892        15 hours ago        769 MB
docker.io/jenkins/jenkins                 2.332.2-jdk11       fd576e09d155        2 weeks ago         464 MB
docker.io/docker                          dind                a072474332af        2 weeks ago         235 MB
docker.io/redis                           latest              0e403e3816e8        7 weeks ago         113 MB
docker.io/mysql                           5.7                 8b94b71dcc1e        7 weeks ago         448 MB
docker.io/busybox                         latest              beae173ccac6        3 months ago        1.24 MB
docker.io/gitlab/gitlab-ee                latest              935605e8a1c8        4 months ago        2.54 GB
docker.io/registry                        2                   b8604a3fe854        5 months ago        26.2 MB
docker.io/centos                          7                   eeb6ee3f44bd        7 months ago        204 MB
docker.io/zangredrock/jeecg-boot-system   latest              0b72216377f2        18 months ago       245 MB
docker.io/progrium/consul                 latest              09ea64205e55        6 years ago         69.4 MB
[root@Docker1 jenkins]#
############################# 6. 启动myjenkins-blueocean容器
[root@Docker1 jenkins]# docker run --name jenkins-blueocean --rm --detach \
>   --network jenkins --env DOCKER_HOST=tcp://docker:2376 \
>   --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 \
>   --publish 8080:8080 --publish 50000:50000 \
>   --volume jenkins-data:/var/jenkins_home \
>   --volume jenkins-docker-certs:/certs/client:ro \
>   myjenkins-blueocean:2.332.2-1
b830d2301af39c761c78c830513b225d83c02e8935e01e5350fb49336ca6992a
[root@Docker1 jenkins]# docker ps
CONTAINER ID        IMAGE                           COMMAND                  CREATED             STATUS              PORTS                                              NAMES
b830d2301af3        myjenkins-blueocean:2.332.2-1   "/sbin/tini -- /us..."   7 seconds ago       Up 6 seconds        0.0.0.0:8080->8080/tcp, 0.0.0.0:50000->50000/tcp   jenkins-blueocean
8b9fc22ee909        docker:dind                     "dockerd-entrypoin..."   41 hours ago        Up 41 hours         2375/tcp, 0.0.0.0:2376->2376/tcp                   jenkins-docker
[root@Docker1 jenkins]#
############################# 7. 查看管理员密码
[root@Docker1 jenkins]# docker exec -it jenkins-blueocean   cat /var/jenkins_home/secrets/initialAdminPassword
d8aed159dd164cfe88ce932a505f1491
[root@Docker1 jenkins]# 
```

> 访问URL：http://192.166.1.241:8080/
>
> X4033/ad****

```shell
## 重启机器后，重启dind和jenkins ##
####################################################
##启动dind

docker run --name jenkins-docker --rm --detach \
  --privileged --network jenkins --network-alias docker \
  --env DOCKER_TLS_CERTDIR=/certs \
  --volume jenkins-docker-certs:/certs/client \
  --volume jenkins-data:/var/jenkins_home \
  --publish 2376:2376 \
  docker:dind --storage-driver overlay2

####################################################
##启动jenkins

docker run --name jenkins-blueocean --rm --detach \
   --network jenkins --env DOCKER_HOST=tcp://docker:2376 \
   --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 \
   --publish 8080:8080 --publish 50000:50000 \
   --volume jenkins-data:/var/jenkins_home \
   --volume jenkins-docker-certs:/certs/client:ro \
   myjenkins-blueocean:2.332.2-1
```





## 4.2 自动拉取gitlab代码打包镜像发布

学习资料：

<https://blog.csdn.net/qq_17686345/article/details/119394590>

https://blog.csdn.net/weixin_43618209/article/details/119727236

http://t.zoukankan.com/ifme-p-12901473.html

### 4.2.1 配置jenkins的gitlab权限

+ 在gitlab创建一个应用，填写应用名称和重定向地址，地址的规则是：jenkinsde地址/securityRealm/finishLogin；再勾选api这个选项，提交，然后就会创建出一个应用

![image-20220508002702272](06 Git学习.assets/image-20220508002702272.png)

![image-20220507172707405](06 Git学习.assets/image-20220507172707405.png)

+ 进入jenkins，系统管理=>插件管理，安装 Gitlab Authentication 插件重启之后，进入：系统管理 => 全局安全配置 

  

![image-20220507173746705](06 Git学习.assets/image-20220507173746705.png)

![image-20220507173935289](06 Git学习.assets/image-20220507173935289.png)

​		配置完后点击底部的保存按钮保存配置信息；｛配置完了，jankins就能单点登录了｝

### 4.2.2 配置jenkins的私钥与gitlab的部署密钥

+ gitlab的部署公钥（此处略，请查看 1.4）

+ jankins配置全局凭据：系统管理 => Manage Credentials =>“全局” =>添加一些凭据

![image-20220508004302049](06 Git学习.assets/image-20220508004302049.png)

![image-20220508004409804](06 Git学习.assets/image-20220508004409804.png)

添加完成后，可以查看相关信息，（点击ID可选删除），（另外此处配置的密钥，也可以使用用户名密码方式）

![image-20220508004447105](06 Git学习.assets/image-20220508004447105.png)

### 4.2.3 配置jenkins的构建项目

#### 4.2.3.1 jenkins的源码管理

+ 新建任务==>源码管理

![image-20220508005744334](06 Git学习.assets/image-20220508005744334.png)



![image-20220508010210357](06 Git学习.assets/image-20220508010210357.png)

分支默认为``*/master``，保存后效果：

![image-20220508010335584](06 Git学习.assets/image-20220508010335584.png)



#### 4.2.3.2 设置构建shell

在interface-go工程中，配置=>构建=>增加构建步骤=>执行shell，填入如下内容：

```shell
Githash=`git rev-parse --short HEAD` 
#docker stop demohttp 
#docker rm demohttp 
echo "--------------Building Docker Image-------------" 
echo $Githash 
docker build -t demohttp:$Githash . 
docker tag demohttp:$Githash demohttp:latest 
echo "--------------Launching Container---------------"
docker run --name demo -d -p 12802:12802 demohttp:latest
```

注意：软件工程中包含一个Dockerfile，如下：

```dockerfile
# build step
FROM worden525/centos7-golang 
COPY simpleHttpServer.go /
RUN cd / && go build --tags netgo simpleHttpServer.go
CMD [ "/simpleHttpServer" ]

# create real app image

FROM alpine
COPY --from=0  /simpleHttpServer /
CMD [ "/simpleHttpServer" ]
```



#### 4.2.3.3 手动构建

工程下，左侧执行"**构建**"，在**控制台输出**页输出如下：

```shell
Started by user X4033
Running as SYSTEM
Building in workspace /var/jenkins_home/workspace/interface-go
The recommended git tool is: NONE
using credential 2374d77e-8742-4939-9d88-50280cbf829d
 > git rev-parse --resolve-git-dir /var/jenkins_home/workspace/interface-go/.git # timeout=10
Fetching changes from the remote Git repository
 > git config remote.origin.url git@192.166.1.241:X4033/interface-go.git # timeout=10
Fetching upstream changes from git@192.166.1.241:X4033/interface-go.git
 > git --version # timeout=10
 > git --version # 'git version 2.30.2'
using GIT_SSH to set credentials 
 > git fetch --tags --force --progress -- git@192.166.1.241:X4033/interface-go.git +refs/heads/*:refs/remotes/origin/* # timeout=10
 > git rev-parse refs/remotes/origin/master^{commit} # timeout=10
Checking out Revision 673e9c84bf1740bf164692addc57f61115bffafc (refs/remotes/origin/master)
 > git config core.sparsecheckout # timeout=10
 > git checkout -f 673e9c84bf1740bf164692addc57f61115bffafc # timeout=10
Commit message: "Dockerfile alpine"
 > git rev-list --no-walk eccb33fedcf345eea80e22c59b06cd901a7b84fa # timeout=10
[interface-go] $ /bin/sh -xe /tmp/jenkins11704309756371992938.sh
+ git rev-parse --short HEAD
+ Githash=673e9c8
+ echo --------------Building Docker Image-------------
--------------Building Docker Image-------------
+ echo 673e9c8
673e9c8
+ docker build -t demohttp:673e9c8 .
Sending build context to Docker daemon  92.67kB

Step 1/7 : FROM worden525/centos7-golang
 ---> 672804159bb1
Step 2/7 : COPY simpleHttpServer.go /
 ---> Using cache
 ---> 4656bd2d1616
Step 3/7 : RUN cd / && go build --tags netgo simpleHttpServer.go
 ---> Using cache
 ---> 87bb7619b0b0
Step 4/7 : CMD [ "/simpleHttpServer" ]
 ---> Using cache
 ---> b3ec702787fe
Step 5/7 : FROM alpine
 ---> 0ac33e5f5afa
Step 6/7 : COPY --from=0  /simpleHttpServer /
 ---> 07c70e0f1d4b
Step 7/7 : CMD [ "/simpleHttpServer" ]
 ---> Running in cbd916b04b97
Removing intermediate container cbd916b04b97
 ---> 31b76c31afbb
Successfully built 31b76c31afbb
Successfully tagged demohttp:673e9c8
+ docker tag demohttp:673e9c8 demohttp:latest
+ echo --------------Launching Container---------------
--------------Launching Container---------------
+ docker run --name demo -d -p 12802:12802 demohttp:latest
67bd1d6cd5f9677c060588bac693e4263357d43b4bb4c150e28cb474b8b64c18
Finished: SUCCESS
```



### 4.2.4 参数化构建

#### 4.2.4.1 安装 Build With Parameters 插件

系统管理=>插件管理，搜索并安装**Build With Parameters**

#### 4.2.4.2 配置参数

在**配置**工程时，选择“**参数化构建过程**”，增加image_name、tag、container_name、port参数。

![image-20220510210851801](06 Git学习.assets/image-20220510210851801.png)

**执行shell**里配置如下：

```shell
Githash=`git rev-parse --short HEAD` 
docker stop ${container_name}
docker rm ${container_name}
echo "--------------Building Docker Image-------------" 
echo $Githash 
docker build -t ${image_name}:$Githash . 
docker tag demohttp:$Githash ${image_name}:${tag} 
echo "--------------Launching Container---------------"
docker run --name  ${container_name} -d -p ${port}:${port} ${image_name}:${tag}
docker save ${image_name}:${tag} > ${image_name}_${tag}_`date +%Y%m%d%H%M%S`.tar
```

**保存**后，左侧**构建**菜单变为**Build With Parameters**

#### 4.2.4.3 手动构建

![image-20220510213012232](06 Git学习.assets/image-20220510213012232.png)

**控制台日志**：

```shell
Started by user X4033
Running as SYSTEM
Building in workspace /var/jenkins_home/workspace/interface-go
The recommended git tool is: NONE
using credential 2374d77e-8742-4939-9d88-50280cbf829d
 > git rev-parse --resolve-git-dir /var/jenkins_home/workspace/interface-go/.git # timeout=10
Fetching changes from the remote Git repository
 > git config remote.origin.url git@192.166.1.241:X4033/interface-go.git # timeout=10
Fetching upstream changes from git@192.166.1.241:X4033/interface-go.git
 > git --version # timeout=10
 > git --version # 'git version 2.30.2'
using GIT_SSH to set credentials 
 > git fetch --tags --force --progress -- git@192.166.1.241:X4033/interface-go.git +refs/heads/*:refs/remotes/origin/* # timeout=10
 > git rev-parse refs/remotes/origin/master^{commit} # timeout=10
Checking out Revision 673e9c84bf1740bf164692addc57f61115bffafc (refs/remotes/origin/master)
 > git config core.sparsecheckout # timeout=10
 > git checkout -f 673e9c84bf1740bf164692addc57f61115bffafc # timeout=10
Commit message: "Dockerfile alpine"
 > git rev-list --no-walk 673e9c84bf1740bf164692addc57f61115bffafc # timeout=10
[interface-go] $ /bin/sh -xe /tmp/jenkins10008831914781652038.sh
+ git rev-parse --short HEAD
+ Githash=673e9c8
+ docker stop httpdemo
httpdemo
+ docker rm httpdemo
httpdemo
+ echo --------------Building Docker Image-------------
--------------Building Docker Image-------------
+ echo 673e9c8
673e9c8
+ docker build -t demohttp:673e9c8 .
Sending build context to Docker daemon  12.38MB

Step 1/7 : FROM worden525/centos7-golang
 ---> 672804159bb1
Step 2/7 : COPY simpleHttpServer.go /
 ---> Using cache
 ---> 4656bd2d1616
Step 3/7 : RUN cd / && go build --tags netgo simpleHttpServer.go
 ---> Using cache
 ---> 87bb7619b0b0
Step 4/7 : CMD [ "/simpleHttpServer" ]
 ---> Using cache
 ---> b3ec702787fe
Step 5/7 : FROM alpine
 ---> 0ac33e5f5afa
Step 6/7 : COPY --from=0  /simpleHttpServer /
 ---> Using cache
 ---> 07c70e0f1d4b
Step 7/7 : CMD [ "/simpleHttpServer" ]
 ---> Using cache
 ---> 31b76c31afbb
Successfully built 31b76c31afbb
Successfully tagged demohttp:673e9c8
+ docker tag demohttp:673e9c8 demohttp:1.0
+ echo --------------Launching Container---------------
--------------Launching Container---------------
+ docker run --name httpdemo -d -p 12802:12802 demohttp:1.0
9ccc6d336f3dca5a61707cf2ecc5138ad8539c42ed3593ff3605bb90930be638
+ date +%Y%m%d%H%M%S
+ docker save demohttp:1.0
Finished: SUCCESS
```

![image-20220510225305687](06 Git学习.assets/image-20220510225305687.png)



### 4.2.5 Push Over SSH插件

http://t.zoukankan.com/ifme-p-12901473.html





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

### git ls-remote

在远程存储库中列出引用

```shell
# 列出远程存储库中所有的引用
$ git ls-remote origin 

# 列出远程存储库中指定refs路径下的引用
$ git ls-remote origin  <refs路径>

```


# END

