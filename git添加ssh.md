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

## 未完待续
1. gitee与github不同邮箱账号ssh公匙
2. 可能遇到的错误
3. ssh -T 报错信息查看
