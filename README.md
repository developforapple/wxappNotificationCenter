# wxappNotificationCenter
在微信小程序中实现了一套类似于iOS通知中心的功能。做到1对多发消息，传值
http://www.jianshu.com/p/c83211bd9cf0


微信小程序中各个界面之间的传值和通知比较蛋疼。所以模仿了iOS中的通知中心，在微信小程序中写了一套类似的通知中心。

通知中心可以做到：1对多发消息，传递object。使用十分简洁。

使用时，在需要接收消息的界面注册一个通知名。然后在需要发消息的界面post这个通知名就可以了。可以在多个界面注册同一个通知名。这样就可以1对多发消息。

使用方法：

1：在app.js中引用notification.js

    var notificationCenter = require('/utils/notification.js'); //这里请改为你的绝对路径

2：在app.js中添加：

    App({
       onLaunch: function (){
              this.notificationCenter = notificationCenter.center();
        },
        notificationCenter:null,
    })


3: 接收通知的page.js中注册
PageA.js:

var app = getApp();
Page({
  onLoad:function(options){
  app.notificationCenter.register("一个通知名称",this,"didReceviceAnyNotification");
  },
  didReceviceAnyNotification:function(notification){
    console.log("接收到了通知：",notification);
    var _this = notification._this; //不要直接使用 this
    var name = notification.name;
  },
})


4: 发出通知的page.js中
PageB.js 任意函数

var app = getApp();
Page({
  anyFunction:function(){
    app.notificationCenter.post("通知名称",{
        //任意通知object
    })   ;
  },
})
