---
categories: learn
layout: post-detail
title:  "HTML5 的 Notification API"
date:   2017-09-07 18:00:04 +0800
---

### **HTML5 的 Notification API**


HTML5 Web Notification通知是属于桌面性质的通知，有点类似于显示器右下角蹦出的QQ弹框，杀毒提示之类的，跟浏览器是脱离的，消息是置顶的。

### **1、申请权限**

出于安全考虑，要发送桌面消息，需要先申请用户授权。Notification对象提供了一个静态的方法——requestPermission()，它接收一个回调函数作为参数，并把返回值传递给回调函数作为参数：

{% highlight ruby %}
Notification.requestPermission(function(status){
    if(Notification.permission !== status){
        Notification.permission = status;
    }
}); 
{% endhighlight %}

返回值为字符串，有以下三个值：default、granted、denied

其中granted表示用户允许通知，denied表示用户拒绝你，default表示用户目前还没有管你。

默认为default，也就是需要询问。

Notification.permission是一个静态属性。表示是否允许通知，值就是上面的granted, denied, 或default.

默认情况下，Notification.permission的值是'default'。

如果选择了拒绝，无论再怎么刷新，都不会再有提示。要在Chrome浏览器的地址栏左侧，点击控制中心图标。


![Notification](/blog/img/2017-9-8/1.jpg)

将通知改为询问

![Notification](/blog/img/2017-9-8/2.jpg)

### **2、创建消息**

用户授权以后，就可以通过new构造显示通知创建一条桌面提醒了：

{% highlight ruby %}
var n = new Notification(title, options);  
{% endhighlight %}

 
其中title是必须参数，表示通知的标题内容，options是可选参数，对象。

常用的options有：

body提示主体内容，字符串，会在标题的下面显示。 

icon字符串, 通知面板右侧那个图片地址。

renotify布尔值，新通知出现的时候是否替换之前的。如果设为true，则表示替换，表示当前标记的通知只会出现一个。

silent布尔值，通知出现的时候，是否要有声音。默认false, 表示无声。

sound字符串，音频地址，表示通知出现要播放的声音资源。 

### **3、定义事件**

Notification对象有四个事件，分别是onshow()、onclick()、onclose()、onerror()，对应在消息显示、被点击、被关闭和出错的时候被触发。

通常情况下，只需要处理点击事件就够了，比如点击消息后跳转到某一特定的页面。


{% highlight ruby %}
var button = document.getElementById('button'), text = document.getElementById('text');
var options={
    body:"祝福老薛，一往情深彼此都给了同一个人，最幸福莫过于兜兜转转，你还在我身边要好好的，一直走下去。不想再寻觅 那就再爱一次吧 !",
    icon:'http://image.zhangxinxu.com/image/study/s/s128/mm1.jpg',
    sound:'http://www.w3school.com.cn/i/horse.ogg',
    silent:true 
}
var title="下一首歌曲";
button.addEventListener("click",function(){
    Notification.requestPermission(function(){
        if(Notification.permission=='granted'){
            var n = new Notification(title, options);  
            n.onclick=function(){ 
                text.innerHTML="歌曲已播放";
                n.close();
            }
        }else{ 
            Notification.permission=='default';
            console.log("您已拒绝发送通知");
        }
    })
},false);
{% endhighlight %}


 
 

 