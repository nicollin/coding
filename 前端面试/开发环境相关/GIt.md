# Git

[Git官网](https://git-scm.com/)

1. Git服务商
- 国内：
  - [coding.net](https://coding.net/)
  - [gitee.com](https://gitee.com/)
- 国外：
  - [github.com](https://github.com/)

2. Git命令

Q：常用的Git命令有哪些？如何使用Git多人协作开发？

1.1 常用Git命令：
- git clone <项目远程地址>：默认下载远程master分支的最新代码
- git status：查看本地仓库距上一次提交之后文件的修改情况
- git diff <文件名>：查看单个文件的差异
- git branch：查看本地的所有分支
- 
- 将修改内容提交到远程服务器：
```shell
git add . // 将所有暂存区的文件加入到提交区
git commit -m 'xxx' // 提交到本地仓库并附上提交信息
git push origin master
```
- 多人协作时，同步别人提交的内容：
```shell
git pull origin master
```

1.2 如何多人协作开发
- 新建独立分支：git checkout -b <branchname>
- 同步master分支的内容：git merge master
- 切换分支：git checkout <branchname>
- 在自己的分支上修改了内容，将分支提交到远程服务器：
```shell
git add .
git comomit -m 'xxx'
git push orgin <branchname>
```
- 将独立开发的分支合并到master分支
```shell
git checkout master
git merge <branchname>
git push origin master
```
