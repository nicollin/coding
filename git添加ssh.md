# Git添加ssh[window环境]
## 2022-2-16
常规步骤：
1. 下载Git
2. 通过`此电脑`,找到`C:\Users\\{{your_account}}\\.ssh`目录
3. 鼠标右键选中`Git Bash Here`
4. 执行以下指令，会生成`id_rsa`,`id_rsa.pub`两个文件；如果之前已存在这两个文件，执行时会有确认提示，执行后会覆盖原来的ssh密匙、公匙 -> 原来的ssh密匙、公匙失效
```shell
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```
5. 执行以下指令，复制ssh公匙到剪贴板
```shell
clip < ~/.ssh/id_rsa.pub
```
6. 打开`github.com`,依次点击`右上角头像->Settings->SSH and GPG keys->New SSH key(SSH keys)`,任意编辑`Title`,在`Key`栏中粘贴剪贴板中内容，再点击`Add SSH key`添加成功
7. 回到Git的Bash窗口，输入以下指令测试ssh key设置是否有效
```ssh -T git@github.com```

   
参考文档：[关于SSH - GitHub Docs](https://docs.github.com/cn/authentication/connecting-to-github-with-ssh/about-ssh)
[怎样生成SSH公匙](https://gitee.com/help/articles/4181)

## 后记

### 1. 为gitee与github不同邮箱账号创建ssh公匙

默认创建的本机ssh key以`id_rsa`命名，但一个用户在gitee和github两个平台上的注册账户邮箱不同，解决方案如下：
1. 按照对应平台生成各自的ssh key
```shell
ssh-keygen -t rsa -b 4096 -C "your.email@example.com" -f "id_rsa_gitee"
ssh-keygen -t rsa -b 4096 -C "your.email@example.com" -f "id_rsa_github"
```
2. 此时如果直接添加ssh key到对应平台并执行`ssh -T git@gitee.com`会提示ssh key不存在，因为ssh key未声明的别名文件不在可搜索的范围内，因此需要配置config文件说明：
```text
# gitee
Host gitee.com
HostName gitee.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa_gitee

# github
Host github.com
HostName github.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa_github
```
3. 再测试连接是否成功，解决问题

### 2. ssh -T测试连接查看错误信息日志

执行以下指令查看具体的调试信息，再根据信息来调试
```shell
ssh -T -v git@gitee.com
```

### 3. 可能遇到的错误

（1）所有的配置都对，为什么还不能连接成功？
答：第一次连接测试会提示是否将测试网址加入白名单`know_hosts`文件中，如果第一次输入`no`后续会一直提示是否加入白名单，无法输出连接成功的提示语
（2）遇到未知错误怎么办？
答：最简单的方法是删除~/.ssh目录下的所有内容，重新生成ssh key，执行以下指令：
```shell
rm -rf ~/.ssh/*
```
（3）执行ssh-keygen成功、但在.ssh目录下找不到文件
答：问题出在`Git Bash here`上，指在当前目录下打开Git Bash窗口，因此根路径在当前目录。生成的ssh key也会在当前目录下，执行以下指令切换到.ssh目录下
```shell
cd ~/.ssh
```

