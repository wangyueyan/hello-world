Activity的简介

Activity是Android四大组件之一，它用于展示界面。Activity是一个应用程序组件，提供一个屏幕，用户可以用来交互为了完成某项任务。Activity中所有操作都与用户密切相关，是一个负责与用户交互的组件，可以通过setContentView(View)来显示指定控件。

在一个android应用中，一个Activity通常就是一个单独的屏幕，它上面可以显示一些控件也可以监听并处理用户的事件做出响应。Activity之间通过Intent进行通信。

使用流程

定义类继承Activity
在AndroidManifest.xml的<application>节点中声明<activity>
     <intent-filter>
    <action android:name="android.intent.action.MAIN" />
    <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>
Intent-filter子节点：
添加意图过滤，可以通过隐式意图启动。可以在桌面生成快捷方式，应用程序的入口。
跳转方式

显式跳转：在可以引用到那个类, 并且可以引用到那个类的字节码时可以使用，一般用于自己程序的内部，显式跳转不可以跳转到其他程序的页面中。
隐式跳转：可以在当前程序跳转到另一个程序的页面，隐式跳转不需要引用到那个类,但是必须得知道那个界面的action和category。
Activity之间通过Intent进行通信。Intent，用于描述一个页面的信息,同时也是一个数据的载体。
你可以用startActivity(),或startActivityForResult()(如果你想要Activity返回数据)传递一个Intent来开启一个Activity(或者让它做一些其他东西)。
Intent除了可以激活组件，还可以通过封装的Bundle对象来携带数据。所以在启动一个Activity的时候，同时还可以传递数据，然后在新的Activity中可以获得意图对象以获取其中Bundle保存的数据。
Intent可传递的数据类型有: 八大基本数据类型，String,数组,ArrayList<String>, Bundle数据捆, 实现序列化接口的 Javabean。
隐式跳转打开浏览器界面：1. 配置文件 2.程序代码

<intent-filter>
<action android:name="android.intent.action.VIEW" />
<category android:name="android.intent.category.DEFAULT" />
<category android:name="android.intent.category.BROWSABLE" />
            <data android:scheme="http" />
            <data android:scheme="https" />
            <data android:scheme="about" />
            <data android:scheme="javascript" />
</intent-filter>
    //跳转到浏览器界面代码
    public void skip2Browser(View view){
        //创建一个Intent对象
        Intent intent = new Intent();
        //设置Action
        intent.setAction("android.intent.action.VIEW");
        //设置category
        intent.addCategory("android.intent.category.BROWSABLE");
        //设置参数
        intent.setData(Uri.parse("http://www.baidu.com"));
        //启动Activity
        startActivity(intent);
    }
生命周期

Activity有三种状态：

当它在屏幕前台时,响应用户操作的Activity, 它是激活或运行状态。
当它上面有另外一个Activity，使它失去了焦点但仍然对用户可见时, 它处于暂停状态。
当它完全被另一个Activity覆盖时则处于停止状态。
当Activity从一种状态转变到另一种状态时，会调用其生命周期方法。

Activity lifecycle
startActivity开启一个Activity时, 生命周期的过程是: onCreate ---onStart(可见不可交互) ---onResume(可见可交互)。
点击back键关闭一个Activity时, 生命周期的过程是: onPause(部分可见不可交互) ---onStop(完全不可见) ---onDestroy(销毁)。
当开启一个新的Activity(以对话框形式)，新的activity把后面的activity给盖住一部分时，后面的activity的生命周期执行的方法是:onPause(部分可见, 不可交互)。
注意：指定Activity以对话框的形式显示, 需在activity节点追加以下主题android:theme="@android:style/Theme.Dialog"。
当把新开启的Activity(以对话框形式)给关闭时, 后面的activity的生命周期执行的方法是: onResume(可见，可交互)。
当开启一个新的activity把后面的activity完全盖住时, 生命周期的方法执行顺序是: onPause ---onStop(完全不可见)。
当把新开启的activity(完全盖住)给关闭时, 生命周期的方法执行顺序是: onRestart---onStart ---onResume(可见, 可交互)。
实际应用场景：onResume 可见, 可交互，在该方法中可进行刷新数据操作；onPause 可见，但是不能响应用户操作，在该方法中可进行操作暂停；onCreate 初始化布局以及一些大量的数据；onDestroy 把数据给释放掉, 节省内存。
保存Activity信息

onSaveInstanceState：在Activity被动的摧毁或停止的时候调用，用于保存运行数据，可以将数据存在在Bundle中。onPause之后执行。被动消耗，指被系统回收，不是主动调用finish方法。
onRestoreInstanceState：该方法在Activity被重新绘制的时候调用，例如改变屏幕方向，savedInstanceState为onSaveInstanceState保存的数据。
Activity重新创建，恢复数据，onStart之后执行。如果activity停止之后，进程在后头很容易被杀死，然后重新启动，就会执行恢复数据方法。
