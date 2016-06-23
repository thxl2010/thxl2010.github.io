#**最近在做移动端的项目，Hybrid模式，使用Ionic框架，是基于AngularJS的。磕磕绊绊碰到了不少问题，也学到了不少东西。**

#1. Ionic
1. Ionic http://ionicframework.com/
2. 配置 http://ionicframework.com/getting-started/
  1. First, install Node.js. Then, install the latest Cordova and Ionic command-line tools.：` npm install -g cordova ionic`
       Cordova提供了一组设备相关的API，通过这组API，移动应用能够以JavaScript访问原生的设备功能，如摄像头、麦克风等。Cordova还提供了一组统一的JavaScript类库，以及为这些类库所用的设备相关的原生后台代码。
   
   
  2. Follow the Android and iOS platform guides to install required platform dependencies：http://cordova.apache.org/docs/en/latest/guide/platforms/android/index.html
     1. 安装jdk； 
     2. 安装Android SDK  http://developer.android.com/sdk/index.html
     3. 安装 Apache  ant http://ant.apache.org/bindownload.cgi
3. Start a project ： Create an Ionic project using one of our ready-made app templates, or a blank one to start fresh.
  `$ ionic start myApp tabs`
  
   
4. Run it ： 
  ```
$ cd myApp
$ ionic platform add ios
$ ionic build ios
$ ionic emulate ios
   ```

1. 在本地浏览器测试:  `ionic serve`/`ionic serve --address ip`
 
#2. 项目
1. 自定义样式：sass
  首先在项目目录下,运行命令`$ionic setup sass    $ionic serve`
  运行以后, 就会对scss/ionic.app.scss文件监控, 有修改, 会自动编译该文件, 输出到css/ionic.app.css
  所以你每次修改保存scss文件后, 浏览器会看到实时的效果, 非常方便.  
  
2. 跳转链接 
  1. 使用内置浏览器插件：inAppBrowser ：cordova-plugin-inappbrowser
    https://www.thepolyglotdeveloper.com/2014/07/launch-external-urls-ionicframework/
    http://ngcordova.com/docs/plugins/inAppBrowser/
    **安装**：`cordova plugin add cordova-plugin-inappbrowser`
  2. a标签 ： Setting ng-href and then referencing it in window.open works for me.
   ```html
<ion-view title="Test Page">
    <ion-content>
        <div class="list">
            <a class="item" href="#" onclick="window.open('http://www.baidu.com/', '_system', 'location=yes'); return false;">
                Open a Browser
            </a>
            <a class="item" href="#" onclick="window.open('http://www.baidu.com/', '_system', 'location=yes'); return false;">
                Open a Browser
            </a>
            <a class="item" ng-href="{{choice.href}}" onclick="window.open(this.href, '_system', 'location=yes'); return false;">
                Open a Browser
            </a>
        </div>
    </ion-content>
</ion-view>
```  
  
3. angular 返回html片段，自动转义html标签
 **使用**：**ng-bind-html="article.content | trustHtml"**

 ```html
<article class="commodity-details" ng-bind-html="choice.brief | trustHtml"></article>
```

  ```js
// app.js
angular.module('starter', ['ionic', 'starter.controllers', 'starter.services', 'starter.filters'])

// filter.js
angular.module('starter.filters')
    .filter('trustHtml', function ($sce) {
      return function (input) {
        return $sce.trustAsHtml(input);
      }
    });
```  
  
4. 图片轮播:
 1. `ion-slide-box`图片加载显示不出问题 **添加**`ng-if`
  `<ion-slide-box ng-if="recommendedProductDetail.mainPictures">`
 2. 循环显示设置
   1. 设置属性： `does-continue="true"`
   2. js:`$ionicSlideBoxDelegate.loop(true);`  
 3. 一张主图时不显示轮播圆点 **show-pager**
   `<ion-slide-box ng-if="recommendedProductDetail.mainPictures" does-continue="true"
               ng-init="sliderPagerShow=(recommendedProductDetail.mainPictures.length == 1 ? false : true)" show-pager="{{sliderPagerShow}}">`
  
5. filter： currency 
  `<h1 class="title">{{recommendedProductDetail.price / 100 | currency:(recommendedProductDetail.paiceUnit || "￥"):0}}</h1>`  

6. 搜索 点击出现浮层 显示类别
  - 浮动框: $ionicPopover 是一个可以浮在app内容上的一个视图框。
  - 对话框: $ionicPopup ionic 对话框服务允许程序创建、显示弹出窗口。$ionicPopup 提供了3个方法：alert(), prompt(),以及 confirm() 。
  **问题**：1. 浮动框有三角号 2. 都有$ionicBackdrop背景层，阻止下一层的点击
  **解决**：使用模板
7. 模板：使用`ng-include`指令，可以利用ng-include指令在HTML中直接使用内联模板，例如：
  `<div ng-include="'a.html'"></div>`
  
