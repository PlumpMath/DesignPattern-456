## 1. MVC设计模式
### 1.1 概述

MVC设计模式的目的是将数据模型和视图分离开来，并以控制器作为连接两者的桥梁以实现解耦

![mvc](img/mvc.png)

| 意义   | 说明                                       |
| :--- | :--------------------------------------- |
| M    | Model，表示模型层，数据模型或业务模型，就是我们要显示给用户查看的内容    |
| V    | View，表示视图层，就是用户直接看到的界面，例如：Activity，Fragment，自定义View，还有XML布局文件 |
| C    | Controler，表示控制层，用于模型和视图间切换数据，对应于Activity业务逻辑，数据处理和UI处理 |

而J2EE中的MVC分别表示：

- M：javabean
- V：jsp
- C：servlet

三角关系的问题就是维护问题。在MVC，当你有变化的时候你需要同时维护三个对象和三个交互，这显然让事情复杂化了

Android中经常会出现数千行的Activity代码，究其原因，Android中纯粹作为View的各个XML视图功能太弱，Activity基本上都是View和Controller的合体，既要负责视图的显示又要加入控制逻辑，承担的功能过多，代码量大也就不足为奇。所以更贴切的目前常规的开发说应该是View-Model 模式，大部分都是通过Activity的协调，连接，和处理逻辑的

Activity中存在两部分内容：业务相关和界面相关，V中的内容较少，而C中的内容较多

解决方案：将Activity中的业务部分拆分----MVP，将Activity中的界面部分拆分----MVVM

### 1.2 案例分析
Android中的ListView就用到了MVC设计模式

- M：数据的集合，List<UserInfo>
- V：ListView
- C：Adapter，控制数据如何显示在ListView上

而Activity控制层和视图层都有

数据模型，业务层，业务逻辑，数据层，表现层，控制层：事件路由

## 2. MVP设计模式

MVP模式可以分离显示层和逻辑层，它们之间通过接口进行通信，降低耦合。

![mvp模式](img/mvp.png)

| 意义        | 说明                                    |
| :-------- | :------------------------------------ |
| Model     | 数据的存取，提供数据的存取功能，依然是实体模型               |
| View      | 用户界面，对应于Activity和xml，负责View的绘制以及与用户交互 |
| Presenter | 交互中间人，负责完成View于Model间的交互和业务逻辑         |

利用MVP的设计模型可以把部分的逻辑代码从Fragment和Activity业务的逻辑移出来，在Presenter中持有View（Activity或者Fragment）的引用，然后在Presenter调用View暴露的接口对视图进行操作，这样有利于把视图操作和业务逻辑分开来。MVP能够让Activity成为真正的View而不是View和Control的合体，Activity只做UI相关的事。但是这个模式还是存在一些不好的地方，比较如说：

- Activity需要实现各种跟UI相关的接口，同时要在Activity中编写大量的事件，然后在事件处理中调用presenter的业务处理方法，View和Presenter只是互相持有引用并互相做回调,代码不美观。

- 这种模式中，程序的主角是UI，通过UI事件的触发对数据进行处理，更新UI就要考虑线程的问题。而且UI改变后牵扯的逻辑耦合度太高，一旦控件更改（比较TextView 替换 EditText等）牵扯的更新UI的接口就必须得换。

- 复杂的业务同时会导致presenter层太大，代码臃肿的问题。

切断的View和Model的联系，让View只和Presenter（原Controller）交互，减少在需求变化中需要维护的对象的数量。MVP定义了Presenter和View之间的接口，让一些可以根据已有的接口协议去各自分别独立开发，以此去解决界面需求变化频繁的问题

MVP模式是MVC模式的一个演化版本，MVP全称Model-View-Presenter。目前MVP在Android应用开发中越来越重要了。解耦view和model层

在Android中，业务逻辑和数据存取是紧紧耦合的，很多缺乏经验的开发者很可能会将各种各样的业务逻辑塞进某个Activity、Fragment或者自定义View中，这样会使得这些组件的单个类型臃肿不堪。如果不将具体的业务逻辑抽离出来，当UI变化时，你就需要去原来的View中抽离具体业务逻辑，这必然会很麻烦并且易出错。

对于view层和presenter层的通信，我们是可以通过接口实现的，具体的意思就是说我们的activity，fragment可以去实现实现定义好的接口，而在对应的presenter中通过接口调用方法。

将activity中的业务部分拆分—mvp，使用接口实现view和presenter的通信和隔离，这种方式有一个缺点，就是接口会非常多

将activity中的界面相关内容拆分—mvvm

### 2.1 使用MVP的好处

MVP模式会解除View与Model的耦合，有效的降低View的复杂性。同时又带来了良好的可扩展性、可测试性，保证系统的整洁性和灵活性。

MVP模式可以分离显示层与逻辑层，它们之间通过接口进行通信，降低耦合。理想化的MVP模式可以实现同一份逻辑代码搭配不同的显示界面，因为它们之间并不依赖于具体，而是依赖于抽象。这使得Presenter可以运用于任何实现了View逻辑接口的UI，使之具有更广泛的适用性，保证了灵活度。

让UI界面和数据分离

### 2.2 MVP模式的三个角色

- Presenter – 交互中间人

Presenter主要作为沟通View与Model的桥梁，它从Model层检索数据后，返回给View层，使得View与Model之间没有耦合，也将业务逻辑从View角色上抽离出来。负责完成View于Model间的交互和业务逻辑

- View – 用户界面

View通常是指Activity、Fragment或者某个View控件，它含有一个Presenter成员变量。通常View需要实现一个逻辑接口，将View上的操作转交给Presenter进行实现，最后，Presenter 调用View逻辑接口将结果返回给View元素。对应于Activity和xml，负责View的绘制以及与用户交互

- Model – 数据的存取

Model 角色主要是提供数据的存取功能。Presenter 需要通过Model层存储、获取数据，Model就像一个数据仓库。更直白的说，Model是封装了数据库DAO或者网络获取数据的角色，或者两种数据方式获取的集合。

## 3. MVVM设计模式
![mvvm模式](../assets/designpattern20)

| 意义        | 说明                                       |
| :-------- | :--------------------------------------- |
| Model     | 实体模型，代表基本业务逻辑                            |
| View      | 对应于Activity和xml，负责View的绘制以及与用户交互         |
| ViewModel | 将view和model联系在一起，起到桥梁的作用，负责完成View于Model间的交互,负责业务逻辑 |

MVVM的目标和思想MVP类似，利用数据绑定(Data Binding)、依赖属性(Dependency Property)、命令(Command)、路由事件(Routed Event)等新特性，打造了一个更加灵活高效的架构。

ViewModel大致上就是MVP的Presenter和MVC的Controller了，而View和ViewModel间没有了MVP的界面接口，而是直接交互，用数据“绑定”的形式让数据更新的事件不需要开发人员手动去编写特殊用例，而是自动地双向同步。比起MVP，MVVM不仅简化了业务与界面的依赖关系，还优化了数据频繁更新的解决方案，甚至可以说提供了一种有效的解决模式。

可以直接在layout布局xml中绑定数据，分离视图与业务逻辑，低耦合，可重用，独立开发，可测试

View和Model进行双向绑定（data binding），View的变化可以自动的反应在ViewModel，ViewModel的数据变化也会自动反应到View上，data binding框架解决了数据绑定的问题

- 数据驱动

在MVVM中，以前开发模式中必须先处理业务数据，然后根据的数据变化，去获取UI的引用然后更新UI，也是通过UI来获取用户输入，而在MVVM中，数据和业务逻辑处于一个独立的View Model中，ViewModel只要关注数据和业务逻辑，不需要和UI或者控件打交道。由数据自动去驱动UI去自动更新UI，UI的改变又同时自动反馈到数据，数据成为主导因素，这样使得在业务逻辑处理只要关心数据，方便而且简单很多。

- 低耦合度

MVVM模式中，数据是独立于UI的，ViewModel只负责处理和提供数据，UI想怎么处理数据都由UI自己决定，ViewModel 不涉及任何和UI相关的事也不持有UI控件的引用，即使控件改变（TextView 换成 EditText）ViewModel 几乎不需要更改任何代码，专注自己的数据处理就可以了，如果是MVP遇到UI更改，就可能需要改变获取UI的方式，改变更新UI的接口，改变从UI上获取输入的代码，可能还需要更改访问UI对象的属性代码等等。

- 更新 UI

在MVVM中，我们可以在工作线程中直接修改View Model的数据（只要数据是线程安全的），剩下的数据绑定框架帮你搞定，很多事情都不需要你去关心。

- 团队协作

MVVM的分工是非常明显的，由于View和View Model之间是松散耦合的。一个是处理业务和数据，一个是专门的UI处理。完全有两个人分工来做，一个做UI（xml 和 Activity）一个写ViewModel，效率更高。

- 可复用性

一个View Model复用到多个View中，同样的一份数据，用不同的UI去做展示，对于版本迭代频繁的UI改动，只要更换View层就行，对于如果想在UI上的做AbTest 更是方便的多。

- 单元测试

View Model里面是数据和业务逻辑，View中关注的是UI，这样的做测试是很方便的，完全没有彼此的依赖，不管是UI的单元测试还是业务逻辑的单元测试，都是低耦合的。

通过上面对MVVM的简述和其他两种模式的对比，我们发现MVVM对比MVC和MVP来说还是存在比较大的优势，虽然目前Android开发中可能真正在使用MVVM的很少.

通过一系列的学习，消化和参考。目前，MVC，MVP都已经应用到项目开发中了。
关于MVVM目前是只闻其声，不见其下楼。MVVM算是舶来品，从Web前端的Argular而来。Argular有个databind的属性领我们羡慕不已，瞬间感觉高大上啊！

其实，还有WPF，这个落寞巨人微软的背影。这个有个点感觉很牛逼就是，界面开发和业务开发完全分开实现。界面开发除了数据是假的，业务操作绑定，显示全是真的，业务流程的定义和界面设计进行了解耦，二者通过接口文档，默认命名规则进行约定实现。最后界面显示和业务处理拼合起来就好了。

我觉得除了databind （现在有robobinding可以参考），还需要 有事件订阅与发布 的模块（推荐使用使用facebook的flux），相关技术用到RxJava，RxAndroid的模块。

而后再加上代码自动生成，通过web服务器的数据库，直接生成业务处理代码与接口，Android端也自动生成业务处理类。着多牛逼啊，然后Android开发失业。web开发失业。数据库设计和界面开发笑到了最后。

这都是我们对于mvvm的自嗨和幻想+YY，Google大兄弟目前还没有推荐的技术实现。感觉Google处境很尴尬，现在Facebook 推出的 React Native 如日中天，大有替代Java 原生编程的趋势。如果真取代了，Android Application 将是 Node.js JavaScript的乐园。我们原生应用的开发工程师可要下岗了。目前已经在不断蚕食Android原生开发的份额，通过Cordova这个内奸，Html5家族由原来的jQuery Mobile 到 Augular.js 和ionic，再到 React 和 React Native，来势汹汹。JS是如日中天，蓬勃发展。再回来讨论MVVM竟然是前端带来的技术。遇见的未来竞争会更激烈。广大Android原生开发的从业者受的的冲击会更大。

## 4. Demo

用MVC、MVP和MVVM分别实现登录界面的功能

![](img/login.png)

### 4.1 MVC

布局文件

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:paddingBottom="@dimen/activity_vertical_margin"
    android:paddingLeft="@dimen/activity_horizontal_margin"
    android:paddingRight="@dimen/activity_horizontal_margin"
    android:paddingTop="@dimen/activity_vertical_margin"
    >

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:gravity="center"
        android:orientation="horizontal">

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Username:" />

        <EditText
            android:id="@+id/username"
            android:layout_width="200dp"
            android:layout_height="wrap_content" />
    </LinearLayout>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="20dp"
        android:gravity="center"
        android:orientation="horizontal">

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Password:" />

        <EditText
            android:id="@+id/password"
            android:layout_width="200dp"
            android:layout_height="wrap_content" />
    </LinearLayout>

    <Button
        android:id="@+id/login"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center"
        android:layout_marginTop="20dp"
        android:onClick="login"
        android:text="login" />
</LinearLayout>
```
MainActivity.java
```java
package cn.itcast.architecture;

import android.app.Activity;
import android.app.ProgressDialog;
import android.os.SystemClock;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.text.TextUtils;
import android.view.View;
import android.widget.EditText;
import android.widget.Toast;

import cn.itcast.architecture.bean.User;
import cn.itcast.architecture.net.UserLoginNet;

public class MainActivity extends AppCompatActivity {
    /*

    流程：
    1、界面展示
    2、用户的输入
    3、按钮点击
    4、判断用户输入
    5、显示滚动条
    6、一系列耗时工作
    7、隐藏
    8、提示用户

    MainActivity中处理所有代码
    按照MVC模式拆分

    存在问题：
    Activity中存在两部分内容：业务相关+界面相关
    V中的内容相对较少而C中的内容很多
    解决方案：
    1、如果将Activity中的业务部分拆分——MVP
    2、如果将Activity中的界面相关内容拆分——MVVM
     */

    private EditText mUsername;
    private EditText mPassword;
    private ProgressDialog dialog;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        mUsername = (EditText) findViewById(R.id.username);
        mPassword = (EditText) findViewById(R.id.password);
        dialog = new ProgressDialog(this);
    }

    /**
     * 按钮点击
     * @param view
     */
    public void login(View view) {
        final String username = mUsername.getText().toString();
        final String password = mPassword.getText().toString();
        final User user=new User();
        user.username=username;
        user.password=password;
        boolean userInfo = checkUserInfo(user);
        if(userInfo){
            dialog.show();
            new Thread(){
                @Override
                public void run() {
                    UserLoginNet net=new UserLoginNet();

                    if(net.sendUserLoginInfo(user)){
                        runOnUiThread(new Runnable() {
                            @Override
                            public void run() {
                                // 登陆成功
                                dialog.dismiss();
                                Toast.makeText(MainActivity.this, "欢迎回来"+username, Toast.LENGTH_SHORT).show();
                            }
                        });

                    }else{
                        //登陆失败
                        runOnUiThread(new Runnable() {
                            @Override
                            public void run() {
                                // 登陆成功
                                dialog.dismiss();
                                Toast.makeText(MainActivity.this, "用户名或密码输入有误", Toast.LENGTH_SHORT).show();
                            }
                        });
                    }

                }
            }.start();
        }else{
            Toast.makeText(MainActivity.this, "用户名或密码不能为空", Toast.LENGTH_SHORT).show();
        }
    }

    /**
     * 判断用户输入
     * @param user
     * @return
     */
    private boolean checkUserInfo(User user) {
        if(TextUtils.isEmpty(user.username)||TextUtils.isEmpty(user.password)){
            return false;
        }
        return true;
    }
}
```
UserLoginNet.java，模拟网络请求
```java
package cn.itcast.architecture.net;

import android.os.SystemClock;
import android.widget.Toast;

import cn.itcast.architecture.bean.User;

public class UserLoginNet {

    /**
     * 发送用户输入数据
     * @param user
     * @return
     */
    public boolean sendUserLoginInfo(User user){
        SystemClock.sleep(2000);
        if("itcast".equals(user.username)&&"wuhan".equals(user.password)){
           return true;
        }else{
            //登陆失败
           return false;
        }
    }
}
```

```java
package cn.itcast.architecture.bean;

public class User {
    public String username;
    public String password;
}
```

### 4.2 MVP

MainActivity.java

```java
package cn.itcast.mvp;

import android.app.ProgressDialog;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.widget.EditText;
import android.widget.Toast;

import cn.itcast.mvp.bean.User;
import cn.itcast.mvp.presenter.UserLoginPresenter;

public class MainActivity extends AppCompatActivity implements IUserLoginView{

    private EditText mUsername;
    private EditText mPassword;
    private ProgressDialog dialog;

    private UserLoginPresenter presenter;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        mUsername = (EditText) findViewById(R.id.username);
        mPassword = (EditText) findViewById(R.id.password);
        dialog = new ProgressDialog(this);

        presenter = new UserLoginPresenter(this);
    }

    /**
     * 按钮点击
     * @param view
     */
    public void login(View view) {
        final String username = mUsername.getText().toString();
        final String password = mPassword.getText().toString();
        final User user=new User();
        user.username=username;
        user.password=password;

        boolean userInfo = presenter.checkUserInfo(user);

        if(userInfo){
            dialog.show();
            presenter.login(user);
        }else{
            Toast.makeText(MainActivity.this, "用户名或密码不能为空", Toast.LENGTH_SHORT).show();
        }
    }

    /**
     * 登陆成功
     */
    public void success(){
        runOnUiThread(new Runnable() {
            @Override
            public void run() {
                // 登陆成功
                dialog.dismiss();
                Toast.makeText(MainActivity.this, "欢迎回来"+mUsername.getText().toString(), Toast.LENGTH_SHORT).show();
            }
        });
    }

    /**
     * 登陆失败
     */
    public void failed(){
        runOnUiThread(new Runnable() {
            @Override
            public void run() {
                // 登陆失败
                dialog.dismiss();
                Toast.makeText(MainActivity.this, "用户名或密码输入有误", Toast.LENGTH_SHORT).show();
            }
        });
    }
}
```
IUserLoginView.java
```java
package cn.itcast.mvp;

public interface IUserLoginView {
    /**
     * 登陆成功
     */
    void success();

    /**
     * 登陆失败
     */
    void failed();
}
```
UserLoginPresenter.java
```java
package cn.itcast.mvp.presenter;

import android.text.TextUtils;
import android.widget.Toast;

import cn.itcast.mvp.IUserLoginView;
import cn.itcast.mvp.MainActivity;
import cn.itcast.mvp.bean.User;
import cn.itcast.mvp.net.UserLoginNet;

public class UserLoginPresenter {
    // Activity 和 Fragment 通用性不好
    // 提高通用性：放置参数为通用（抽象类或接口，实际开发中接口更通用）
    private IUserLoginView view;
    public UserLoginPresenter(IUserLoginView view){
        this.view=view;
    }

    /**
     * 判断用户输入
     *
     * @param user
     * @return
     */
    public boolean checkUserInfo(User user) {
        if(TextUtils.isEmpty(user.username)||TextUtils.isEmpty(user.password)){
            return false;
        }
        return true;
    }

    /**
     * 用户登录
     * @param user
     */
    public void login(final User user){
        new Thread(){
            @Override
            public void run() {
                UserLoginNet net=new UserLoginNet();
                if(net.sendUserLoginInfo(user)){
                    // 登陆成功
                    view.success();
                }else{
                    //登陆失败
                    view.failed();
                }
            }
        }.start();
    }
}
```
UserLoginNet.java
```java
package cn.itcast.mvp.net;

import android.os.SystemClock;
import cn.itcast.mvp.bean.User;

public class UserLoginNet {

    /**
     * 发送用户输入数据
     * @param user
     * @return
     */
    public boolean sendUserLoginInfo(User user){
        SystemClock.sleep(2000);
        if("itcast".equals(user.username)&&"wuhan".equals(user.password)){
           return true;
        }else{
            //登陆失败
           return false;
        }
    }
}
```
User.java
```java
package cn.itcast.mvp.bean;

public class User {
    public String username;
    public String password;
}
```
### 4.3 MVVM

MVVM模式使用到Data Binding，需要在build.gradle文件中配置

```grad
apply plugin: 'com.android.application'

android {
    compileSdkVersion 25
    buildToolsVersion "25.0.2"

    defaultConfig {
        applicationId "cn.itcast.mvvm"
        minSdkVersion 14
        targetSdkVersion 25
        versionCode 1
        versionName "1.0"
    }
    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
    }
    dataBinding{
        enabled=true // 设置dataBinding为可用
    }
}

dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    testCompile 'junit:junit:4.12'
    compile 'com.android.support:appcompat-v7:25.3.0'
}
```

布局文件

```xml
<?xml version="1.0" encoding="utf-8"?>
<layout  xmlns:android="http://schemas.android.com/apk/res/android">
    <data>
        <variable
            name="event"
            type="cn.itcast.mvvm.UserEvent" />
    </data>
<LinearLayout
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:paddingBottom="@dimen/activity_vertical_margin"
    android:paddingLeft="@dimen/activity_horizontal_margin"
    android:paddingRight="@dimen/activity_horizontal_margin"
    android:paddingTop="@dimen/activity_vertical_margin"
    >

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:gravity="center"
        android:orientation="horizontal">

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Username:" />

        <EditText
            android:id="@+id/username"
            android:layout_width="200dp"
            android:layout_height="wrap_content"
            android:addTextChangedListener="@{event.usernameWatcher}"/>
    </LinearLayout>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="20dp"
        android:gravity="center"
        android:orientation="horizontal">

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Password:" />

        <EditText
            android:id="@+id/password"
            android:layout_width="200dp"
            android:layout_height="wrap_content"
            addTextChangedListener="@{event.passwordWatcher}"/>
    </LinearLayout>

    <Button
        android:id="@+id/login"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center"
        android:layout_marginTop="20dp"
        android:onClick="login"
        android:text="login" />
</LinearLayout>
</layout>
```
MainActivity.java
```java
package cn.itcast.mvvm;

import android.app.ProgressDialog;
import android.databinding.DataBindingUtil;
import android.os.Bundle;
import android.support.v7.app.AppCompatActivity;
import android.text.TextUtils;
import android.view.View;
import android.widget.EditText;
import android.widget.Toast;

import cn.itcast.mvvm.bean.User;
import cn.itcast.mvvm.databinding.ActivityMainBinding;
import cn.itcast.mvvm.net.UserLoginNet;


public class MainActivity extends AppCompatActivity {
    /*

    流程：
    1、界面展示
    2、用户的输入
    3、按钮点击
    4、判断用户输入
    5、显示滚动条
    6、一系列耗时工作
    7、隐藏
    8、提示用户

    MainActivity中处理所有代码
    按照MVC模式拆分

    存在问题：
    Activity中存在两部分内容：业务相关+界面相关
    V中的内容相对较少而C中的内容很多
    解决方案：
    1、如果将Activity中的业务部分拆分——MVP
    2、如果将Activity中的界面相关内容拆分——MVVM
     */

    private ProgressDialog dialog;
    private User user;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        ActivityMainBinding binding = DataBindingUtil.setContentView(this, R.layout.activity_main);
        user = new User();
        UserEvent event=new UserEvent(user);
        binding.setEvent(event);

        dialog = new ProgressDialog(this);
    }

    /**
     * 按钮点击
     *
     * @param view
     */
    public void login(View view) {


        boolean userInfo = checkUserInfo(user);
        if(userInfo){
            dialog.show();
            new Thread(){
                @Override
                public void run() {
                    UserLoginNet net=new UserLoginNet();

                    if(net.sendUserLoginInfo(user)){
                        runOnUiThread(new Runnable() {
                            @Override
                            public void run() {
                                // 登陆成功
                                dialog.dismiss();
                                Toast.makeText(MainActivity.this, "欢迎回来"+user.username, Toast.LENGTH_SHORT).show();
                            }
                        });

                    }else{
                        //登陆失败
                        runOnUiThread(new Runnable() {
                            @Override
                            public void run() {
                                // 登陆成功
                                dialog.dismiss();
                                Toast.makeText(MainActivity.this, "用户名或密码输入有误", Toast.LENGTH_SHORT).show();
                            }
                        });
                    }

                }
            }.start();
        }else{
            Toast.makeText(MainActivity.this, "用户名或密码不能为空", Toast.LENGTH_SHORT).show();
        }
    }

    /**
     * 判断用户输入
     *
     * @param user
     * @return
     */
    private boolean checkUserInfo(User user) {
        if(TextUtils.isEmpty(user.username)||TextUtils.isEmpty(user.password)){
            return false;
        }
        return true;
    }
}
```
UserEvent.java
```java
package cn.itcast.mvvm;

import android.text.Editable;
import android.text.TextWatcher;

import cn.itcast.mvvm.bean.User;

public class UserEvent {

    private User user;
    public UserEvent(User user){
        this.user=user;
    }

    public TextWatcher usernameWatcher=new TextWatcher() {
        @Override
        public void beforeTextChanged(CharSequence s, int start, int count, int after) {

        }

        @Override
        public void onTextChanged(CharSequence s, int start, int before, int count) {

        }

        @Override
        public void afterTextChanged(Editable s) {
            user.username=s.toString();
        }
    };

    public TextWatcher passwordWatcher=new TextWatcher() {
        @Override
        public void beforeTextChanged(CharSequence s, int start, int count, int after) {

        }

        @Override
        public void onTextChanged(CharSequence s, int start, int before, int count) {

        }

        @Override
        public void afterTextChanged(Editable s) {
            user.password=s.toString();
        }
    };
}
```
UserLoginNet.java
```java
package cn.itcast.mvvm.net;

import android.os.SystemClock;

import cn.itcast.mvvm.bean.User;

public class UserLoginNet {

    /**
     * 发送用户输入数据
     * @param user
     * @return
     */
    public boolean sendUserLoginInfo(User user){


        SystemClock.sleep(2000);

        if("itcast".equals(user.username)&&"wuhan".equals(user.password)){
           return true;

        }else{
            //登陆失败
           return false;
        }
    }
}
```
User.java
```java
package cn.itcast.mvvm.bean;

public class User {
    public String username;
    public String password;
}
```