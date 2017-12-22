---
categories: learn
layout: post-detail
title:  "[Javascript]visibilitychange"
date:   2017-09-04 15:00:04 +0800
---

### **[Javascript]visibilitychange**


### **HTML5 API --- 页面可见性改变(visibilitychange)事件在当前网页在可见和不可见之间变换的时候调用，这样便可适合在标签不可见的时候减少网络请求、服务器压力等。**

### **目前页面可见性API有两个属性**

document.hidden: Boolean值，表示当前页面可见还是不可见

document.visibilityState: 返回当前页面的可见状态

### **改变网页标题的文字**

{% highlight ruby %}
(function() {
    var OriginTitile = document.title, titleTime;
    document.addEventListener('visibilitychange', function() {
        if (document.hidden) {
            document.title = '悄悄的我走了！';
            clearTimeout(titleTime);
        } else {
            document.title = '(つェ⊂)轻轻地我来了!';
            titleTime = setTimeout(function() {
                document.title = OriginTitile;
            },2000);
        }
    });
})();
{% endhighlight %}

### **当应用程序或浏览器标签页切换到后台时就停止播放音乐，从后台切换回来时又开始播放音乐**

{% highlight ruby %} 
<audio src="http://www.w3school.com.cn/i/horse.ogg" controls="controls" id="audio">浏览器不支持</audio>
 
//控制音频
var audio=document.getElementById("audio");
audio.play();
audio.loop=true;

function changeAudio(event){
    var target=event.target;
    if(target.hidden){
        audio.pause();
    }else{
        audio.play();
    }
}

document.addEventListener('visibilitychange',changeAudio,false);
  
{% endhighlight %}


### **登录同步**

**1.  去淘宝买东西，未登录状态下，进入首页。**

**2.  然后新窗口打开任意页面，登录并成功返回。**

**3.  再次访问刚才打开的首页，发现页面还是未登录状态，实际上用户已经登录了。**

{% highlight ruby %}
//登录信息页
<p id="loginInfo"></p>
(function() {
    //返回后显示登录用户名
    var loginInfo=document.getElementById("loginInfo"); 

    function changePage(){ 
        var username=localStorage.name;
        if(username){
            loginInfo.innerHTML='欢迎回来，'+username;
        }else{
            var href=location.href.replace("index","login"); 
            loginInfo.innerHTML='您尚未登录，请<a href="'+href+'" target="_blank">登录</a>';

        } 
    }

    //首次进入页面
    changePage();

    //切换页面回来
    document.addEventListener('visibilitychange',function(){
        if(!document.hidden){
            changePage();
        }
    },false);


    //关闭页面的时候清楚缓存
    window.addEventListener("unload",function(){
        localStorage.removeItem('name'); 
    },false);
})();

//登录页面
<form id="loginForm">
    <table>
        <tr><td>用户名</td><td><input type="text" placeholder="请输入用户名" name="username"></td></tr>
        <tr><td>密码</td><td><input type="password" placeholder="请输入密码" name="password"></td></tr>
        <tr><td>邮箱</td><td><input type="email" placeholder="请输入邮箱" name="email"></td></tr>
        <tr><td colspan="2"><button type="submit" value="登录" name="submit">登录</button></td></tr>
    </table>
</form>

(function() {
    var loginForm=document.getElementById("loginForm"); 
    loginForm.addEventListener("submit",function(e){
        localStorage.name=loginForm.elements[0].value;
        alert("返回首页，您已登录");
        this.reset(); 
        e.preventDefault();
    })
})();
{% endhighlight %}



 
 

 