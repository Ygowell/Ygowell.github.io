# Android Unresolved R 
最近碰到一个很奇怪的问题，在Linux下使用Android studio时，导入项目后，发现R文件一直会报错提示Unresoleved R，尝试了网上各种方式，比如：
- Grade Sync.
- Invalidate caches and restart.
- Clean the project and rebuild.
- Alt + Enter import R files.

仍然无效，而我在mac运行居然又没问题，然后我又在Linux下新建一个项目，和该项目比较了下，发现使用的kotlin的版本和gradle的版本是有一定联系，我把gradle
改成3.2.1试了下，果然好了，但目前还没知道真正是什么原因，猜测应该跟本机的gradle版本由关吧，之后弄清楚了再补充。

```
buildscript {
    ext.kotlin_version = '1.3.21'
    repositories {
        google()
        jcenter()
        
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:3.2.1'
        ...
    }
}
```
