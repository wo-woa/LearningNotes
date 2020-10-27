# 创建pipeline及相关脚本编写

https://www.cnblogs.com/shuoer/p/9627072.html

- 创建pipeline
- 配置中pipeline初始选择script，不要选择from scm，scm是从远端获取脚本，例如git，手动选择git中脚本的位置。如果有问题还需要重新提交比较麻烦，故采用script。
- 运行。运行成功如图：

![这里写图片描述](https://img-blog.csdn.net/20180629145948435?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NkbmlnaHQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

*演示pipeline参数化构建–（即添加输入输出界面）执行sh脚本及从svn拉取源代码*

```
#!/usr/bin/env groovy
pipeline{
    agent any
  environment {
    REVISION = "0.0.${env.BUILD_ID}"
  }
    options{
        disableConcurrentBuilds()
        skipDefaultCheckout()
        timeout(time: 1, unit: 'HOURS')
        timestamps()
    }
    parameters{
        string(name: 'PERSON', defaultValue: '测试-字段输入', description: '请输入中文')
        booleanParam(name: 'YESORNO', defaultValue: true, description: '测试-布尔值')
        choice(name: 'Server_addr', choices: '10.114.24.192:7722\n10.129.14.147:7722\n10.114.4.208:6005\n127.0.0.1:6005', description: '被测系统ip及端口')
    }
    stages{
        stage ('Initialize') {
          steps {
              sh '''
              echo "任务初始化..."
              echo "构建版本revision:${REVISION}"
              '''
              sh '''
              echo "项目检出...."
              '''

checkout([$class: 'SubversionSCM',
          additionalCredentials: [],
          excludedCommitMessages: '',
          excludedRegions: '',
          excludedRevprop: '',
          excludedUsers: '',
          filterChangelog: false,
          ignoreDirPropChanges: false,
          includedRegions: '',
          locations: [[credentialsId: 'e522721e-4a9a-467c-b154-acb803d8afb0',
                       depthOption: 'infinity',
                       ignoreExternalsOption: true,
                       remote: 'svn://您的svn/FunFunLeBaseLib']],
          workspaceUpdater: [$class: 'UpdateUpdater']])

          }
        }

    stage ('Build') {
      steps {
        echo '构建阶段....'
        sh 'mvn clean package -e -f FunFunLeBaseLib/pom.xml'
      }
    }
    stage ('Deploy') {
      steps {
      echo '发布阶段....'
        script {
          currentBuild.displayName = "${REVISION}"
        }
        sh 'mvn deploy  -Drevision=${REVISION} -e -f FunFunLeBaseLib/pom.xml'
      }
    }
    }

post {
        always {
            echo '构建结束...'
        }
        success {
            echo '恭喜您，构建成功！！！'
        }
        failure {
            echo '抱歉，构建失败！！！'
        }
        unstable {
            echo '该任务已经被标记为不稳定任务....'
        }
        changed {
            echo ''
        }
    }

}
```

注意：

​	credentialsId 是你在jenkins添加的一个凭据，添加以后会有一个唯一标识给你的。



# Credentials添加证书从git上拉代码

### 1.Add domain

在jenkins界面，依次点击： Credentials -> System -> Add domain： 
Domain Name: 填写你git服务器的地址，如 github.xxx.com 
Description: 随便写一点描述，如 This is the Credential for github

![img](https://images2018.cnblogs.com/blog/1366086/201807/1366086-20180722150124623-1662900129.png)

![img](https://images2018.cnblogs.com/blog/1366086/201807/1366086-20180722150204554-1559933974.png)

 

### 2.Add Credentials

进入页面后，可以选择 Username with password 或者 SSH Username with private key, 根据你的情况选择，这里我们选择 Username with private key：

Username: 随便起一个名字，以便在创建 Job 的时候使用该 Credential 
Private Key：可以指定文件，也可以使用默认的 ~/.ssh，当然也可以直接将私钥复制粘贴到此处。 
Passphrase: 如果你在创建 ssh key 的时候输入了 Passphrase 那就填写相应的Passphrase，为空就不填写 
ID: 空 
Description： 空

![img](https://images2018.cnblogs.com/blog/1366086/201807/1366086-20180722145826353-492799712.png)

 

点击 ok 后 Credential 就创建好了。

### 3.Job配置

如果你再新建 Job 就可以看到我们的 Credential 选项了：

![这里写图片描述](https://img-blog.csdn.net/20160714171626853)