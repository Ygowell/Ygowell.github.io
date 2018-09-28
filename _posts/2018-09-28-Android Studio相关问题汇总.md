## ssh variant 'simple' does not support setting port?
#### 问题：    
在Android Studio上使用git fecth远程代码时出现的问题

#### 原因：    
主要是因为新版本git在使用ssh方式拉取代码的时候，使用的命令带有端口号导致的

#### 解决方法：     
a. 配置git参数，git config --global ssh.variant ssh    
b. 在Android Studio设置中修改git的SSH executable为Native

## Gradle Sync Failed
#### 问题：    
Gadle's dependency cache may be corrupt (this sometimes occurs after a network connection timeout.)   
Re-download dependencies and sync project (requires network)    
Re-download dependencies and sync project (requires network)

#### 原因：    
可能是你之前下载的gralde文件有问题或者对应的下载版本不正确

#### 解决方法：    
我将./gradle/wrapper/dists中的对应版本，例如gradle-4.4-all删除，然后重新下载；如果不行话，建议改下gradle-wrapper.properties中gradle的版本号
distributionUrl=https\://services.gradle.org/distributions/gradle-4.4.1-all.zip
