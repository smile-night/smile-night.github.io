---
title: ReactNative开发笔记
date: 2018-05-13 23:37:20
tags:
---

一：环境构建
==============
1: node+git，不多说(node版本必须是8+)
2：python2,目前还不支持python3
3：Java Development Kit [JDK] 1.8
4：Android Studio，这里推荐大家直接下载android-studio-bundle，这里面直接包含了Android SDK，配置更方便 
5：模拟器Genymotion，或者其他的安卓模拟器都OK
6：react native的版本0.54.2

二：调试
==============
1：说明：
  1)	react-native-cli去创建项目
  2)	项目最好放在磁盘根目录，否则经常会因为目录层级太深导致出错
2：新建项目：
  1)	全局安装react-native-cli和yarn: npm install -g yarn react-native-cli
  2)	创建项目：react-native init yourProjectName
  3)	打开模拟器进入yourProjectName文件夹，在package.json文件夹的scripts写如下配置：
  4)	"scripts": {
  5)	"start": "node node_modules/react-native/local-cli/cli.js start",
  6)	"dev": "react-native run-android"
  7)	}
  8)	运行npm run dev,这个时候在模拟器中就可以看到你的APP的运行状态，这个命令主要做了两个操作：
  9)	起服务，服务端口号为8081
  10)	在模拟器上安装开发包，并启动APP
  11)	服务起来之后，在浏览器中打开：http://localhost:8081/index.bundle?platform=android；如果看到一个页面的JS代码，代码就是正常的，如果代码有错误，浏览器提示是错误信息
  12)	如果是模拟器，点开APP中的菜单按钮，出现调试选择框，选择Dev Settings，填写自己电脑的IP和端口号，详细步骤如下：
    <!-- ![images](RN_1.png) -->
    ![avatar](RN_1.png)
    ![avatar](RN_2.png)
    ![avatar](RN_3.png)
  13)	下面我们开始调试页面，选择远程调试选项，然后再浏览器地址栏输入：http://localhost:8081/debugger-ui/就可以看到我们程序中打印的东西了
  ![avatar](RN_4.png)
  ![avatar](RN_5.png)

三：上述过程中的异常指南
==============
  1)	第一次项目安装失败，如果提示错误：
  ![avatar](RN_6.png)
  解决步骤：
    (1)	在Android/app/src/main目录下创建一个空的assets文件夹
    (2)	react-native bundle --platform android --dev false --entry-file index.js --bundle-output android/app/src/main/assets/index.android.bundle --assets-dest android/app/src/main/res/
    (3)	重新启动项目
  2)	如果开启Enable Live Reload,按HOME键退出，然后再进来，周期函数会加载多次
  3)	要修改安装之后的APP名字：android/app/src/main/res/values/strings.xml，可以看到里面的app_name,进行修改即可
  4)	如果在本地调试时，浏览器出现如下的提示：关闭浏览器重新打开就可以了~
  ![avatar](RN_7.png)

四：升级打怪（coding）
==============
1）路由：react-navigation
  React Native中，使用react-navigation跳转路由，是叠加一个视图层，所以，返回并不会刷新页面，如果我们要做页面刷新，可以通过路由传参的方式，如：
  ![avatar](RN_8.png)
在打开的weather页面
  ![avatar](RN_9.png)
2）状态管理：mobx
  a)	npm install mobx mobx-react –save
  b)	如果运行起来发现@observer装饰器不识别，则npm install babel-plugin-transform-decorators-legacy --save-dev
  c)	如果使用过vue开发的小伙伴，对vuex肯定会非常熟悉，mobx的状态管理器的使用方法推荐：
  ![avatar](RN_10.png)
  在子路由中的用法：
  ![avatar](RN_11.png)
  然后在constructor中，使用this.props.loginStore.xxx就可以调用了，是不是非常方便呀~
3）存储：react-native-storage
4）热更新：pushy或code push
  （1）这里我采用的是pushy，其中要注意的点：
    a) 上传新的更新包的时候，需要在pushy后台，把之前的安装包给删掉，否则，不生效
    b) 修改了JS进行热更新，一定要使用markSuccess,否则，下次还会提示更新
5）数据请求：fetch


五：从VUE开发到React Native的转变
==============
（1）条件渲染
  Vue: v-if=”show”
  React Native: this.state.show ? xxxxxx : null
（2）列表渲染
  Vue: v-for=”item in items”
  React Native:
  ![avatar](RN_12.png)
（3）React Native中开发和Vue中的区别
    1）	样式：
      View是块状元素，样式中不能含有和字体相关的属性，比如color，lineHeight等，文字必须包含在Text标签中，View下的Text标签是行内块元素，Text中的Text是行内元素。
    2）	事件：
      (1) 并不是任意标签的元素都具有点击事件的，在React Native中，具有点击事件的标签有：Touch开头的标签和Text， Button等。
    (2) 事件绑定this的几种方式：官方推荐b)这种方式，也是性能最好的方式。
      a) 在调用的时候使用bind绑定this
      ![avatar](RN_13.png)
      b) 在调用的时候使用bind绑定this
      ![avatar](RN_14.png)
      c) 在调用的时候使用箭头函数绑定this
      ![avatar](RN_15.png)
      d) 使用属性初始化器语法绑定this
      ![avatar](RN_16.png)

六：项目开发中碰到的一些坑
==============
1）	在JS中，new Date(‘yyyy-mm-dd’)这种写法是OK的，在RN的调试模式下也是OK的，但是，在React Native的正式包中，这个是不行的
2）	Webview使用本地的html文件，要放在android/app/src/main/assets文件夹下
3）	开发的过程中，Reload的时候，有时候storage不走success也不走catch，这个时候你只要把设备或者模拟器中的APP关掉重新打开就可以了
4）	删除文件，经常会碰到一些文件删除不了的报错，这个时候，直接删除android/app下的build文件夹就可以，或者你这么做，在package.json文件中（如下），然后直接npm run clean
![avatar](RN_17.png)


七：总结
==============
由于之前没有React和安卓的开发经验，刚开始踩了很多坑，这里是以0.54.2的RN版本为例希望能够给刚从vue的开发模式转过来或者是刚开始学习React Native的小伙伴一些帮助，如果有叙述不准确或者接下来大家想知道的一些技术方向都可以发送到我的邮箱simplenight_cool@yeah.net，跟大家一起进步哦~













