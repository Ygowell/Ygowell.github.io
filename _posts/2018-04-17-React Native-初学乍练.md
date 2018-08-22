### 引言
俗话说的好，“师傅引进门，修行靠个人”，怎么入这个门就显得尤为重要了。对于一个新手来说，如何去入React Native这个门呢？当然首先你得去看 [官方文档](http://facebook.github.io/react-native/docs/getting-started.html)，如果你的英文不是很好，也可以先看看 [中文介绍](https://reactnative.cn/docs/0.51/getting-started.html)，不管你理解的如何，你都得亲自动手去尝试，有时你会发现，在尝试中都会发现许多问题的，接下来介绍下我是如何一步一步搭建环境，运行起来的。

### 搭建环境
由于我开发的是Android和使用的是Mac，所以我只介绍下在该环境下是如何搭建的。

#### Homebrew、Node、Watchman安装
首先你需要安装一些“工具”来辅助你搭建环境，官方推荐使用 [Homebrew](https://brew.sh/) 来安装 [Node](https://nodejs.org/zh-cn/) 和 [Watchman](https://facebook.github.io/watchman/)，Watchman其实只是一个Facebook提供来查看版本变化的，跟搭建该项目无关，但能起到一个不错的辅助作用；Node则是我们所需要的，***而且目前只支持5.0及以上版本***，这是我碰到的第一个坑。

#### React Native的命令行工具（react-native-cli）
在安装好以上“工具”后，接下来我们就进入真正核心部分了，我们可以使用Node提供给我们npm来安装，
```
npm install -g react-native-cli
```

#### 下载官方工程
接下来我们去下载官方给我们提供的一个样例工程。
```
react-native init AwesomeProject // AwesomeProject这个工程名是可以随便命名的
```
当时我碰到了第二个坑，

![](https://ws4.sinaimg.cn/large/006tNc79ly1fqfg87bwbcj30wi0kc41x.jpg)

经过一番的奋斗，我是通过切换镜像源来解决的。
 
![](https://ws3.sinaimg.cn/large/006tNc79ly1fqfg7hca7uj30wi0kc43k.jpg )
```
nrm use taobao
```
你也可以使用npm来设置
```
npm config set registry https://registry.npm.taobao.org --global
npm config set disturl https://npm.taobao.org/dist --global
```
接着重新运行下react-native init AwesomeProject，就可以开始下载工程和依赖了，可以从图中看出，最后还提示如何分别在iOS和Android上运行app。

![](https://ws1.sinaimg.cn/large/006tNc79ly1fqfg8mbvzhj30wi0kcn1r.jpg)

#### 真机运行app
一般做Android开发的肯定都会有Android手机，估计大多数人都喜欢直接在真机跑，连接你的手机，然后运行，

```
cd AwesomeProject
react-native run-android
```
![https://ws1.sinaimg.cn/large/006tNc79ly1fqfg5chy3ij30f00qomyr.jpg](https://ws1.sinaimg.cn/large/006tNc79ly1fqfg5chy3ij30f00qomyr.jpg)

What's the hell? (什么鬼？）你肯定会相当失望，怎么一跑起来就飘红，擦，怎么有一种程序直接崩溃的感觉。Relax，其实你并没有错，只是还没有连接上服务而已。

#### 连接服务

##### 方式一
你需要将你的设备通过USB连接到电脑上，然后运行
```
adb reverse tcp:8081 tcp:8081
```
接着你摇一摇你的手机，会出来一个弹窗，
  
![](https://ws2.sinaimg.cn/large/006tNc79ly1fqfgj3682zj30f00qo74q.jpg)
  
点击Reload，就会重新加载，你的第一个React Native工程就大功告成了！Awesome~！
  
![](https://ws3.sinaimg.cn/large/006tNc79ly1fqfgn9xjwqj30f00qo3yr.jpg)
  
##### 方式二
就我而言，连接线太不方便，我选择通过IP关联。首先，你也需要摇一摇，出现如上面所示的弹窗，然后点击Dev Settings, 进入后选择Debug server host...
  
![](https://ws1.sinaimg.cn/large/006tNc79ly1fqfgkmjb9fj30f00qo40d.jpg)
  
接下输入你电脑的IP(例如：192.168.1.11:8081)
  
![](https://ws1.sinaimg.cn/large/006tNc79ly1fqfglxmh72j30f00qoab8.jpg)
  
确定后返回首页，接着摇一摇，点击Reload，Pefect！你又掌握了一种方式。
  
#### 总结
本文主介绍了如何搭建RN的环境及运行RN项目，如果你碰到了其他问题，欢迎留言，你也可以参考官网去寻找解决问题的方法，接下来，我会搭建一个项目来一步一步介绍如何使用和开发RN的，敬请期待~！
