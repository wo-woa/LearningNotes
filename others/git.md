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