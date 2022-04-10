# 1 K8s 安装

## 1.1 环境准备

### 1.1.1 阿里云环境
| 公网IP        | 私网IP         | 用户名 | 端口 | 密码         |
| ------------- | -------------- | ------ | ---- | ------------ |
| 112.124.11.65 | 172.20.151.134 | root   | 22   | Fhaw****@65# |
| 120.26.82.188 | 172.20.151.135 | root   | 22   | Fhaw****@188# |
| 121.43.153.211| 172.20.151.136 | root   | 22   | Fhaw****@211# |

### 1.1.2 集群网段划分原则

1. 主机节点网段  172.20.151.0/24
2. Service网段     10.96.0.0/16
3. Pod网段           10.244.0.0/16

### 1.1.3 Github

Github账号：ydsl01  ydsl01@163.com  wqy**********









# 附件

## 附件1 git命令大全

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

# 删除分支
git checkout master         # 切换到master分支
git branch -d x0854         # 删除本地分支
git push origin -d x0854    # 删除远程分支

*********** 实战20220402 ***********
# 带token摘取git内容，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为ydsl01的token，有效期到20230402
git clone https://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@github.com/ydsl01/kubernetes-study-documents-51.git

```

