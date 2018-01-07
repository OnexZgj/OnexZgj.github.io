---
layout: post
title: 用Materail Design设计实现悬浮的Tablayout
date: 2017-12-27 15:13:20 +0300
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: how-to-start.jpg # Add image post (optional)
tags: [Tablayout, Float]
---
首先看看效果图：




![onex.gif](http://upload-images.jianshu.io/upload_images/5249989-ee168e845126dc42.gif?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


今天就实现一个这样的效果：悬浮的tablayout的效果，带你一步步的实现这个效果：

###常规实现方式：
- 整体采用的是ScrollView的滑动，通过监听ScrollView的滑动，然后根据一个特定的高度进行控制预先设置好的View的隐藏和显示，俗称障眼法，然后达到悬浮的效果

- 整体还是监听ScrollView的滑动，通过Window动态的添加的View，实现悬浮的效果，为什么要用ScrollView而不用ListView或者RecycleView这些滑动控件，因为ScrollView可以很精确的判断出当前滑动的距离，而其他的控件会出现一个惯性滑动，不好测量，所以一般都会使用ScrollView。

###Demo的实现过程
通过Materail Design新控件去实现悬浮的Tablayout，进行实现大体的思想是：
1. 使用了可折叠的Toolbar,即CollapsingToolbarLayout，它可以帮助我们进行折叠Toolbar

2. 使用类似于LinearLayout的AppBarLayout使得Tablayout和Toolbar进行垂直放置。

3. 使用NestedScrollView实现整体的UI的上下滑动

4. 在NestedScrollView 使用ViewPager和Tablayout,实现左右滑动和布局的切换

```
<?xml version="1.0" encoding="utf-8"?>
<android.support.design.widget.CoordinatorLayout xmlns:android="http://schemas.android.com/apk/res/android"
                                                 xmlns:app="http://schemas.android.com/apk/res-auto"
                                                 xmlns:tools="http://schemas.android.com/tools"
                                                 android:id="@+id/coordinator_Layout"
                                                 android:layout_width="match_parent"
                                                 android:layout_height="match_parent" 
                                                 android:fitsSystemWindows="true">

    <android.support.design.widget.AppBarLayout
        android:id="@+id/app_bar_layout"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:fitsSystemWindows="true">

        <android.support.design.widget.CollapsingToolbarLayout
            android:id="@+id/collapsingToolbarLayout"
            android:layout_width="match_parent"
            android:layout_height="300dp"
            android:background="@mipmap/taylor"
            app:contentScrim="#46a8ba"
            app:expandedTitleMarginEnd="48dp"
            app:expandedTitleMarginStart="48dp"
            app:expandedTitleTextAppearance="@style/transparentText"
            app:collapsedTitleTextAppearance="@style/ToolBarTitleText"
            app:layout_scrollFlags="scroll|exitUntilCollapsed">
            

            <LinearLayout
                android:id="@+id/head_layout"
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                android:orientation="vertical"
                app:layout_collapseMode="pin"
                app:layout_collapseParallaxMultiplier="0.7">

                <com.youth.banner.Banner
                    xmlns:app="http://schemas.android.com/apk/res-auto"
                    android:id="@+id/banner"
                    android:layout_marginRight="20dp"
                    android:layout_marginLeft="20dp"
                    android:layout_marginTop="50dp"
                    android:layout_width="match_parent"
                    android:layout_height="220dp" />

            </LinearLayout>
            <!--Toolbar放在下面不然会被挡住-->
            <android.support.v7.widget.Toolbar
                android:id="@+id/tb_atf_toolbar"
                android:layout_width="match_parent"
                android:layout_height="?attr/actionBarSize"
                app:layout_collapseMode="pin"
                app:popupTheme="@style/ThemeOverlay.AppCompat.Light"
                app:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar" />
        </android.support.design.widget.CollapsingToolbarLayout>


        <android.support.design.widget.TabLayout
            android:id="@+id/toolbar_tab"
            android:layout_width="match_parent"
            android:layout_height="48dp"
            android:layout_gravity="bottom"
            android:background="#ffffff"
            android:fillViewport="false"
            app:layout_scrollFlags="scroll"
            app:tabIndicatorColor="#0835f8"
            app:tabIndicatorHeight="2.0dp"
            app:tabSelectedTextColor="#0835f8"
            app:tabTextColor="#151515">

            <android.support.design.widget.TabItem
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                android:icon="@mipmap/v5" />
   
            <android.support.design.widget.TabItem
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                android:text="Android" />

            <android.support.design.widget.TabItem
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                android:text="ios" />

            <android.support.design.widget.TabItem
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                android:icon="@mipmap/v8"/>

            <android.support.design.widget.TabItem
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                android:text="JavaEE" />
            
        </android.support.design.widget.TabLayout>

    </android.support.design.widget.AppBarLayout>


    <android.support.v4.widget.NestedScrollView
        android:id="@+id/nsv"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:fillViewport="true"
        android:scrollbars="none"
        app:layout_behavior="@string/appbar_scrolling_view_behavior">

        <android.support.v4.view.ViewPager
            android:id="@+id/main_vp_container"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            app:layout_behavior="@string/appbar_scrolling_view_behavior" />

    </android.support.v4.widget.NestedScrollView>

</android.support.design.widget.CoordinatorLayout>

```
使用到的CollapsingToolbarLayout的折叠和展开时的toolbar字体的颜色的主题的
```
    <style name="transparentText">
        <item name="android:textColor">@android:color/transparent</item>
    </style>

    <style name="ToolBarTitleText">
        <item name="android:textColor">@color/white</item>
    </style>
```

以上是布局文件源码：如果你对这些控件或者属性不是很熟悉的话，可以查看我的另外一篇文件 [MaterialDesign（完整） 带你体验艺术般交互体验]( https://www.jianshu.com/p/c3fb5e2b1c80)里面详细介绍了这些控件和属性。

但是只有这些布局文件，是不会出现gif图中的效果，我们还需要设置和控制Toolbar的字体的显示和隐藏，状态栏的切换等。具体实现如下：
```
        appBarLayout.addOnOffsetChangedListener(new AppBarLayout.OnOffsetChangedListener() {
            @Override
            public void onOffsetChanged(AppBarLayout appBarLayout, int verticalOffset) {
                if (verticalOffset <= -headLayout.getHeight() / 2) {
                    collapsingToolbarLayout.setTitle("Future OneX");
                    toolbar.setVisibility(View.VISIBLE);
                    toolbar.setNavigationIcon(R.mipmap.v7);
                    //使用下面两个CollapsingToolbarLayout的方法设置展开透明->折叠时你想要的颜色
                    collapsingToolbarLayout.setExpandedTitleColor(getResources().getColor(android.R.color.transparent));
                    collapsingToolbarLayout.setCollapsedTitleTextColor(getResources().getColor(R.color.white));

                    window.setStatusBarColor(getResources().getColor(R.color.fuck));
                } else {
                    toolbar.setVisibility(View.GONE);
                    collapsingToolbarLayout.setTitle("");
                    window.setStatusBarColor(Color.TRANSPARENT);
                }
            }
        });
```
通过监听AppBarLayout的测量变化，去控制Toolbar的隐藏和显示，和collapsingToolbarLayout的标题的颜色和文字，当展开时，没有文字，并且透明，当折叠后，设置状态栏颜色和文字颜色等等。

在Demo中，我们使用轮播图使用的是第三方的[banner](https://github.com/youth5201314/banner) 有兴趣可以看看 ，


demo已上传到GitHub https://github.com/OnexZgj/MaterialDesign
