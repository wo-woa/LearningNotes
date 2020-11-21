## 将已有的项目添加到github

1. git init 初始化本地仓库
2. git add .   添加全部已经修改的文件
3. git commit -m 提交说明      将修改后的文件提交到本地仓库
4. git remote add origin 远程仓库地址
5. git push -u origin master



## 分支操作

查看分支：git branch

创建分支：git branch <name>

切换分支：git checkout <name>

创建+切换分支：git checkout -b <name>

合并某分支到当前分支：git merge <name>

删除分支：git branch -d <name>



## 同时推送到GitHub和Gitee

https://www.cnblogs.com/poloyy/p/12215199.html

1. 从github上拉取项目到本地
2. gitee创建同一个项目[![img](https://img2018.cnblogs.com/i-beta/1896874/202001/1896874-20200119171043102-666253483.png)](https://img2018.cnblogs.com/i-beta/1896874/202001/1896874-20200119171043102-666253483.png)

3. 推荐使用在同一个远程仓库下添加另一个远程仓库的地址，然后推送 only一次推送 

   git remote set-url --add origin 码云项目地址 

4. 推送 **git push** 



## 同时使用两个github账户

https://www.cnblogs.com/xjnotxj/p/5845574.html

https://blog.csdn.net/IT_xiao_bai/article/details/88563103

**生成两个SSH key**

`ssh-keygen -t rsa -C "one@gmail.com"`

**创建config文件并配置**

继续在.ssh目录下创建config文件，在config文件中添加以下内容：

```
# one(one@gmail.com)
Host one.github.com
HostName github.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa_one
User one
    
# two(two@gmail.com)
Host two.github.com
HostName github.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa_two
User two
```

这里说明一下配置各字段的含义

```
Host myhost（这里是自定义的host简称，以后连接远程服务器就可以用命令ssh myhost）
HostName 主机名可用ip也可以是域名(如:github.com或者bitbucket.org)
Port 服务器open-ssh端口（默认：22,默认时一般不写此行）
PreferredAuthentications   配置登录时用什么权限认证--可设为publickey,password publickey,keyboard-interactive等
IdentityFile 证书文件路径（如~/.ssh/id_rsa_*)
User 登录用户名(如：git)
```

**部署SSH key**

这里以配置github为例，分别登陆两个github账号，进入Settings –> SSH and GPG keys，点击"new SSH key"， 把下面两个公钥（.pub文件）的内容分别添加到相应的github账号中，其中Title为自定义的名字，Key为.pub文件的内容，最后点击“Add SSH key”即可。

**远程测试**

输入以下命令进行测试

```shell
ssh -T git@one.github.com
ssh -T git@two.github.com
```

**使用**

1、clone代码到本地

（1）原来的写法为：

```shell
git clone git@github.com: one的用户名/learngit.git
```

（2）现在的写法为：

```shell
git clone git@one.github.com: one的用户名/learngit.git
git clone git@two.github.com: two的用户名/learngit.git
```

2、为仓库设置局部的用户名和邮箱：

```shell
# 取消全局 用户名/邮箱 配置
git config --global --unset user.name
git config --global --unset user.email
    
# 单独为每个repo设置 用户名/邮箱
git config user.name "one_name" ; git config user.email "one_email"
git config user.name "two_name" ; git config user.email "two_email"
```



## GitHub Actions使用

阮一峰的GitHub Actions 入门教程 [链接](http://www.ruanyifeng.com/blog/2019/09/getting-started-with-github-actions.html)

github actions 简易入门及自动部署博客实践 [链接](https://zhuanlan.zhihu.com/p/93829286)



简单来说就是github给你一条小服务器，能完成一些脚本定时跑批，或者接收到push执行自动部署，类似jenkins

实践自动部署

```yaml
name: deploy to aliyun oss
on: [push]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    # 切代码到 runner
    - uses: actions/checkout@v1
      with:
        submodules: true
    # 下载 git submodule
    - uses: srt32/git-actions@v0.0.3
      with:
        args: git submodule update --init --recursive
    # 使用 node:10
    - name: use Node.js 10.x
      uses: actions/setup-node@v1
      with:
        node-version: 10.x
    # npm install
    - name: npm install and build
      run: |
        npm install
        npm run build
      env:
        CI: true
    # 设置阿里云OSS的 id/secret，存储到 github 的 secrets 中
    - name: setup aliyun oss
      uses: manyuanrong/setup-ossutil@master
      with:
        endpoint: oss-cn-beijing.aliyuncs.com
        access-key-id: ${{ secrets.OSS_KEY_ID }}
        access-key-secret: ${{ secrets.OSS_KEY_SECRET }}
    - name: cp files to aliyun
      run: ossutil cp -rf .vuepress/dist oss://shanyue-blog/
```

java程序每天跑批

```yaml
name: Bilibili

on:
   push:
     branches: main
   gollum:
   workflow_dispatch:
   schedule:
       - cron: '10 20 * * *'
jobs:
  start:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    - name: Set up JDK 1.8
      uses: actions/setup-java@v1
      with:
        java-version: 1.8
    - name: Cache local Maven repository
      uses: actions/cache@v2
      with:
        path: ~/.m2/repository
        key: ${{ runner.os }}-maven-${{ hashFiles('**/pom.xml') }}
        restore-keys: |
          ${{ runner.os }}-maven-
    - name: Build with Maven
      env:
        BILI_JCT: ${{ secrets.BILI_JCT }}
        DEDEUSERID: ${{ secrets.DEDEUSERID }}
        SESSDATA: ${{ secrets.SESSDATA }}
        SCKEY: ${{ secrets.SCKEY }}
        DINGTALK: ${{secrets.DINGTALK}}
      run:
        mvn compile exec:java -Dexec.mainClass="top.srcrs.BiliStart" -Dexec.args="${BILI_JCT} ${SESSDATA} ${DEDEUSERID} ${SCKEY} ${DINGTALK}"
```

