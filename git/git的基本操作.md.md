# git的基本操作
>前提：已完成 SSH 密钥添加至 github

## 一、
### 1. 建立本地仓库
- 其实只要提交一次就是建立好了

### 2. 关联远程仓库
#### （1）新建github仓库
- 先新建一个 **New repository**，并且不要勾选`Initialize this repository with a README`
- 获取该仓库的 http 网址
#### （2）首先查看是否有已关联的远程仓库
若有且不是想要的，就删除
- 查看所有已关联的远程仓库：`git remote -v`
- 删除现有关联仓库：`git remote remove origin`
#### （3）关联远程仓库
- 用 http 网址进行关联：`git remote add origin 正确的仓库URL`
- 使用点击式操作来提交、推送、上传分支

## 二、怎么将项目上传到建好的仓库里面
### 1. 初始化本地仓库
```git
cd /path/to/your/project  # 进入你的项目目录
git init  # 初始化为Git仓库
```
### 2. 添加仓库地址
```git
git remote add origin <repository-url>
```

## 三、怎么克隆仓库的项目到本地
> 还可以克隆特定上传节点的

### 1. 克隆整个仓库
```git
git clone <repository-url>
```
### 2. 找到提交的哈希值 
插件 `git graph` 可以找到

### 3. 切换到特定的提交
先进入到仓库目录，然后使用 `git checkout` 来切换到想要的节点
```git
cd <repository-directory>
git checkout 6c5cfd5
```

### 4. 建立新的分支（可选）
`new-branch-name` 为新分支的名字
```git
git checkout -b new-branch-name
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjQ1MTU4NzQsLTMwOTM0ODQxMiwtOTcyND
UyOTVdfQ==
-->