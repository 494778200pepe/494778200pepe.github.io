---
layout: post
title:  "Android Support Library"
date:   2019-10-28 14:57:00 +0800
categories: Android
tags: Android样式的开发
author: pepe
description: 『 Android Support Library 』
---

Support Library的版本号其实都在developer里面 

[Support Library Revision Archive](https://developer.android.google.cn/topic/libraries/support-library/rev-archive.html)

[Recent Support Library Revisions](https://developer.android.google.cn/topic/libraries/support-library/revisions.html)

各种jar包的存放路径：
```
C:\Users\Administrator\.gradle\caches\modules-2\files-2.1\com.android.support
```
各个版本Gradle的存放路径：
```
C:\Users\Administrator\.gradle\caches\modules-2\files-2.1\com.android.tools.build\gradle
```
也有说是在：`[sdk]\extras\android\m2repository\com\android\support\appcompat-v7\25.0.0\  `


## **各个版本的Android Support Library介绍**

Android 各个Support Library支持的最低版本如下：

![support1]({{ site.baseurl }}/assets/images/android/theme/support1.png)

目前为止Android Support Library包含的依赖包有：

![support2]({{ site.baseurl }}/assets/images/android/theme/support2.png)

### **1、V4 Support Libraries**
这个包是为Android 2.3(API版本为9)及以上的版本设计的(Support V4首次发布是在2011年，它支持的最低版本是Android 1.6即API Level 4，V4的名字也是根据其支持的最低API版本来的，随着系统的迭代Android 1.6的设备已经很少了，官方在Support Library 24.2.0版本的时候移除了对Android 2.2(API Level 8)及以下版本的支持，所以从Android Support Library 24.2.0开始，V4包支持的最低版本是Android 2.3即API Level 9)，它包含大部分高版本中有而低版本中没有的API，包括application components、user interface features、accessibility、data handling、network connectivity、programming utilities，下面是对V4中的一些关键API的介绍：

**App Components：**

	。Fragment:一个专为解决Android碎片化的类，通过它可以让同一个程序适配不同的屏幕。

	。NotificationCompat:支持更丰富的通知形式；

	。LocalBroadcastManager:适合于应用内的消息传递。

**User Interface：**

	。ViewPager:一个可以管理子view的viewgroup，用户可以在各个view之间自由切换，这个在很多应用中都有使用到；

	。PagerTitleStrip:一个关于当前页面、上一个页面和下一个页面的一个非交互的指示器。它经常作为ViewPager控件的一个子控件被被添加在XML布局文件中。

	。PagerTabStrip:一个关于当前页面、上一个页面和下一个页面的一个可交互的指示器。它经常作为ViewPager控件的一个子控件被被添加在XML布局文件中。

	。DrawerLayout:抽屉

	。SlidingPaneLayout:用于实现两列面板的切换，在UI最上层的使用提供了一个水平的，多个面板的布局。左边的面板可以看作是一个内容列表或者是浏览，右边的面板的任务是显示详细的内容。

**Accessibility：**

	。ExploreByTouchHelper：帮助自定义View实现accessibility的帮助类；

	。AccessibilityEventCompat、AccessibilityNodeInfoCompat、AccessibilityNodeProviderCompat、AccessibilityDelegateCompat：Accessibility的适配类

**Content：**

	。Loader：异步加载数据；

	。FileProvider：应用间的私有文件共享。

关于V4的更多API介绍可以参见：[Support V4 Libraries API References](https://developer.android.com/intl/zh-cn/reference/android/support/v4/app/package-summary.html)

在Android Support Library 24.2.0及之后的版本中，为了增强效率和减小APK的大小起见，Android将V4包从一个独立的依赖包拆分成v4 compat library、v4 core-utils library、v4 core-ui library、v4 media-compat library和v4 fragment library这5个包，考虑到V4的向后兼容，你在工程中依赖V4这个依赖包时默认是包含拆分后的5个包的，但为了节省APK大小，建议在开发过程中根据实际情况依赖对应的V4包，移除不必要的V4包。

拆分后的5个V4包如下：

![support3]({{ site.baseurl }}/assets/images/android/theme/support3.png)

**v4 compat library**
兼容一些 Framework API，如 Context.getDrawable() 和 View.performAccessibilityAction()，大小为 602k，在AS中的依赖方式如下：
```
    compile 'com.android.support:support-compat:24.2.1'
```
	
**v4 core-utils library**
提供一系列核心的工具类，如 AsyncTaskLoader 和 PermissionChecker，大小为 90k，在AS中的依赖方式如下：

    compile 'com.android.support:support-core-utils:24.2.1'
**v4 core-ui library**
提供一系列核心的 UI，如 ViewPager、 NestedScrollView，大小为 240k，在AS中的依赖方式如下：
```
    compile 'com.android.support:support-core-ui:24.2.1'
```

**v4 media-compat library**
android.media 兼容库，包括 MediaBrowser 和 MediaSession，大小为 248k，在AS中的依赖方式如下：
```
    compile 'com.android.support:support-media-compat:24.2.1'
```

**v4 fragment library**
跟fragment相关部分，大小为 136k。V4这个子库依赖了其他4个子库，所以我们一旦依赖这个库就会自动导入其他4个子库，这跟直接依赖整个support-v4效果类似，在AS中的依赖方式如下：
```
	compile 'com.android.support:support-fragment:24.2.1'
```

拆包并不一定代表能够真的解决问题，V4各子包的依赖关系如下，可见即使拆包之后，要用到V4中的某个API时，依赖包并没有减小多少：

![support4]({{ site.baseurl }}/assets/images/android/theme/support4.png)

### **2、V7 Support Libraries**
V7和V4一样，同样包含多个依赖包，但和V4不同的是，V7下的多个子包并不是后面拆分开来的，而是最初发布时就以各个独立库的形式发布的。它是针对Android 2.3(API Level 9)及以上的版本谷歌提供了一系列的support包（和V4包的命名一样，V7最初支持的最低版本是Android 2.1即API Level 7，所以称其为V7，同样在Android Support Library 24.2.0将V7支持的最低版本改为Android 2.3即API Level 9了），这些support包各自对应着特定的功能，每一个都可以单独地被引用。

**v7 appcompat library**
这个包支持对Action Bar接口的设计模式、Material Design接口的实现等，核心类有ActionBar、AppCompatActivity、AppCompatDialog、ShareActionProvider等，在AS中的依赖方式如下：
```
    compile 'com.android.support:appcompat-v7:24.2.1'
```

注意：这个包需要依赖android-support-v4。

**v7 cardview library**
支持cardview控件，使用Material Design语言设计，卡片式的信息展示，在电视App中有广泛的使用，在AS中的依赖方式如下：
```
    compile 'com.android.support:cardview-v7:24.2.1'
```

**v7 gridlayout library**
一个支持GridLayout布局的support包，在AS中的依赖方式如下：
```
    com.android.support:gridlayout-v7:24.2.1
```

**v7 mediarouter library**
一个用于设备间音频、视频交换显示的support包，在AS中的依赖方式如下：
```
    com.android.support:mediarouter-v7:24.2.1
```

**v7 palette library**
该库提供了palette类，使用这个类可以很方便提取出图片中主题色。比如在音乐App中，从音乐专辑封面图片中提取出专辑封面图片的主题色，然后将播放界面的背景色设置为封面的主题色，随着播放音乐的改变，播放界面的背景色也会巧妙的跟着改变，从而提供更好的用户体验。，在AS中的依赖方式如下：
```
    com.android.support:palette-v7:24.2.1
```

**v7 recyclerview library**
核心类是RecyclerView，用于替换ListView、GridView，具体可以查阅RecyclerView方面的资料，在AS中的依赖方式如下：
```
    com.android.support:recyclerview-v7:24.2.1
```

**v7 Preference Support Library**
一个用于支持各种控件存储配置数据的support包，比如CheckBoxPreference和ListPreference，在AS中的依赖方式如下：
```
    com.android.support:preference-v7:24.2.1
```

### **3、V8 Support Library**

V8 Support Library支持的最低SDK版本是Android 2.3即API Level 9。

v8 renderscript library
一个用于渲染脚本的support包，在AS中按照如下方式配置即可正常使用：
```
    defaultConfig {
        renderscriptTargetApi 18
        renderscriptSupportModeEnabled true
    }
```

### **4、V13 Support Library**
这个包的作用主要是为Android3.2（API Level 13）及以上的系统提供更多地Framgnet特性支持，使用它的原因在于，android-support-v4中虽然也对Fragment做了支持，由于要兼容低版本，导致他是自行实现的 Fragment 效果，在高版本的 Fragment 的一些特性丢失了，而对于 v13以上的 sdk 版本，我们可以使用更加有效，特性更多的代码，在AS中的依赖方式如下：
```
    com.android.support:support-v13:24.2.1
```

### **5、Multidex Support Library**
该support包用于使用多dex技术编译APP，当一个应用的方法数超过65536个时需要使用multidex配置，关于multidex的更多信息，可以参见如何编译超过65K方法数的应用，在AS中的依赖方式如下：
```
    compile 'com.android.support:multidex:1.0.0'
```

### **6、Annotations Support Library**
一个支持注解的support包，在AS中的依赖方式如下：
```
    compile 'com.android.support:support-annotations:24.2.1'
```

### **7、Design Support Library**
一个用于支持Design Patterns的support包，它提供了Material Desgin设计风格的控件，在AS中的依赖方式如下：
```
    com.android.support:design:24.2.1
```

### **8、Custom Tabs Support Library**
一个提供了在应用中添加和管理custom tabs的support包，在Google IO 2015中有介绍，在AS中的依赖方式如下：
```
    compile 'com.android.support:customtabs:24.2.1'
```

### **9、Percent Support Library**
一个提供了百分比布局的support包，通过这个包可以实现百分比布局，在AS中的依赖方式如下：
```
    com.android.support:percent:24.2.1
```





参考:

[Android Support Library详细介绍 - 简书](https://www.jianshu.com/p/a5aa5f209895)

[Android Support 包里究竟有什么 - Jsoh的博客专栏 - CSDN博客](https://blog.csdn.net/u010015108/article/details/52459890)

Android Support 库传记 - 云在千峰
http://blog.chengyunfeng.com/?p=1047

Android Support 库：SortedList - 云在千峰
http://blog.chengyunfeng.com/?p=1058