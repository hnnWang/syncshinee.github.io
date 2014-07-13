---
layout: post
title: "HandyBUAA开发笔记（一）"
date: 2014-07-13 15:17:57 +0800
comments: true
categories: android
---
这个暑假，原定的通过新版vpn爬课表的开发计划由于学校对移动版vpn的支持一直有问题而搁浅。于是`Ken Yu`学长让我定了一个新的开发计划：社团公告系统。他负责搞后台那部分，手机端的任务由我来制定，并和另外两名同学分配以下。最后，我分到的任务是完成社团公告系统的列表界面。通过这两天的努力，我这部分的任务基本完成了。下面就记录以下我这两天的成果吧～
<!-- more -->
开发记录
---
1. **与SlidingMenu的交互。**
   * 我们的HandyBUAA引入了开源项目[SlidingMenu](https://github.com/jfeinstein10/SlidingMenu)，因此要添加社团公告系统的列表就要SlidingMenu里面添加相应的模块并完成新activity与SlidingMenu的交互。
   * 往SlidingMenu里添加新模块可以直接在xml文件里完成。通过阅读已有的代码，我明白了SlidingMenu与activity交互的机制。
   * 首先为要添加的activity创建一个fragment，这个fragment继承于一个总的MainMenuFragment，在总的fragment里，我们对相应的SlidingMenu模块的点击事件作出判断并响应。在新建的fragment里也进行应有的响应。然后让这个activity实现这个fragment。这样，这个功能就实现了。
2. **列表内容的获取与投放**
   * 要完成数据的创建、获取和投放，首先你要有这个数据类型……所以最先进行的就是定义数据类型，定义数据库组成等等，有点无聊。
   * 投放的时候，首先，把数据从数据库中取出（如果有的话），通过sql语句来完成，将数据取出放在Cursor里，然后再把Cursor中的数据打包放入LinkedList中。之后通过一个ViewAdapter把LinkedList中的数据放到一个名为RefreshListView的由ListView派生的View里面，这个特殊的ListView是由学长们封装好的，它可以支持下拉刷新和加载更多功能。
   * 最后添加数据刷新和点击打开相应公告的详细信息的功能。数据刷新通过一个AsyncTask类实现。点击打开详细信息的功能直接给ListView添加OnItemClickListener方法就可以了。

附截图（由于HandyBUAA不是开源项目，就不附源码了）
![pic](/images/14-07-13/HandyBUAA.png)
![pic1](/images/14-07-13/HandyBUAAsocietynotice.png)