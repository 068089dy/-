---
layout: post
title:  "android侧滑栏学习（drawerlayout）"
category: "note"
keywords: ["drawerlayout", "侧滑栏","android"]
description: "android侧滑栏学习（drawerlayout）"
tags: ["drawerlayout", "侧滑栏","android"]
---

## android原生的侧滑栏使用步骤：
### 1.首先定义一个ActionBarDrawerToggle
{% highlight java %}
private ActionBarDrawerToggle drawerToggle;
{%endhighlight%}
### 2.下面是与侧滑相关的AppCompatActivity中的方法：
{% highlight java %}
@Override
public boolean onOptionsItemSelected(MenuItem item) {
    if (drawerToggle.onOptionsItemSelected(item)){
        return true;
    }
    return super.onOptionsItemSelected(item);
}
{%endhighlight%}
由drawerToggle来控制布局的展开与隐藏

### 3.然后定义主界面布局和侧滑栏布局

主界面布局要用DrawerLayout
{% highlight java %}
private DrawerLayout drawerLayout;//主界面
private LinearLayout linearLayout;//侧滑栏
{%endhighlight%}
下面是布局的xml代码，
{% highlight xml %}
<android.support.v4.widget.DrawerLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/drawerlayout"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context="com.example.dy.tabdemo.MainActivity">

    <LinearLayout
        android:id="@+id/ll_menu"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:layout_gravity="start"
        android:orientation="vertical"
        android:background="#FFFFFF"
        tools:layout_editor_absoluteY="8dp"
        tools:layout_editor_absoluteX="8dp">

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Menu Content"
            android:textColor="#000000"
            />

    </LinearLayout>

</android.support.v4.widget.DrawerLayout>
{%endhighlight%}
其中ll_menu布局中layout_gravity属性设置为start或left可以使ll_menu布局显示在屏幕左侧，如图：
![img](https://raw.githubusercontent.com/068089dy/068089dy.github.io/master/media/img/study-drawerlayout/1.png)
同理，设置为end或right则会显示在右侧，如图：
![img](https://raw.githubusercontent.com/068089dy/068089dy.github.io/master/media/img/study-drawerlayout/2.png)
### 4.初始化布局
{% highlight java %}
drawerLayout = (DrawerLayout) findViewById(R.id.drawerlayout);
linearLayout = (LinearLayout) findViewById(R.id.ll_menu);

drawerToggle = new ActionBarDrawerToggle(this, drawerLayout, R.string.app_name, R.string.app_name);
drawerLayout.setDrawerListener(drawerToggle);


getSupportActionBar().setDisplayHomeAsUpEnabled(true);    //在toolbar左侧显示一个返回按钮
drawerToggle.syncState();   //让toolbar左侧的菜单按钮点击时有一个动态的效果

DrawerLayout.LayoutParams layoutParams = (DrawerLayout.LayoutParams) linearLayout.getLayoutParams();
layoutParams.width = getScreenSize()[0]/4*3;    //设置linearlayout的宽度为屏幕的3/4
{%endhighlight%}
### 5.getScreenSize()方法如下：
{% highlight java %}
//获取屏幕长宽
public int[] getScreenSize(){
    int screenSize[] = new int[2];
    DisplayMetrics displayMetrics = new DisplayMetrics();
    this.getWindowManager().getDefaultDisplay().getMetrics(displayMetrics);
    screenSize[0] = displayMetrics.widthPixels;
    screenSize[1] = displayMetrics.heightPixels;
    return screenSize;
}
{%endhighlight%}
完毕，效果图：
![img](https://raw.githubusercontent.com/068089dy/068089dy.github.io/master/media/img/study-drawerlayout/3.gif)
