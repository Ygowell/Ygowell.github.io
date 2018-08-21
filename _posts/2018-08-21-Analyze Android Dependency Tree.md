- ### Why
    首先想想我们为什么我需要分析依赖库？    
    现在，我们在开发项目的过程中，不会再“傻傻”地的去自行封装网络库、图片库、工具库等等，而是找一些好的开源库，随着库的数量增多，这样难免不会碰到这些第三方库中又依赖了公共的另一个第三方库，如果使用的库的版本不一样就会导致编译失败，出现类似以下的错误日志：

    ```
    > Conflict with dependency 'com.android.support:support-annotations' in project ':app'. Resolved versions for app (26.1.0) and test app (27.1.1) differ. 
    See https://d.android.com/r/tools/test-apk-dependency-conflicts.html for details.
    
    ```
    这时候就需要去分析使用的库的依赖树，找出使用了相同的库，通过exclude排除一个。

- ### How
    这里有两种方法去分析依赖库
    ##### Gradle View
    这是一个插件，需要去Android Studio的Plugin中下载。   
    然后我们打开gradle view

    ![image](https://raw.githubusercontent.com/Ygowell/MuyPic/master/res/gradle_view1.jpg)
    
    Wait for a moment 就会得到结果，可以分析出哪些库使用重复且版本不对
    
    ![image](https://raw.githubusercontent.com/Ygowell/MuyPic/master/res/gradle_view2.jpeg)
    
    ##### 指令方式
    
    这种方式只需要你在android studio Terminal中输入
    
    ```
    ./gralew app:dependencies
    ```
    ![image](https://github.com/Ygowell/MuyPic/raw/master/res/terminal0.jpeg)
    
    然后你需要去找到complie对应的地方，相对来说看起来比较麻烦点，你也尝试以下方式，先在build.gradle配置下：
    
    ```
    apply plugin: 'project-report'
    ```
    
    然后在Terminal中输入：
    
    ```
    ./gradlew htmlDependencyReport
    ```
    
    然后你可以在/build/reports/project/dependencies找到index.html，然后用浏览器打开查看
    ![image](https://github.com/Ygowell/MuyPic/raw/master/res/terminal.jpeg)

    大功告成，回去继续撸代码！