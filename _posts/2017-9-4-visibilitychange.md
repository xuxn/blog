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
<html>
<body onload="load()">
	<audio id="audio_id">
		<source src="demo-audio.mp3"/>
		<source src="demo-audio.ogg"/>
		Browser can't support Audio tag.
	</audio>
<script>
	var audioElement = document.getElementById("audio_id");
	function onVisibilityChanged(event) {
	  var hidden = event.target.webkitHidden;
	  if (hidden)
	    audioElement.pause();
	  else
	    audioElement.play();
	}
	function load() {
	  audioElement.play();
	  audioElement.loop = true;
	  document.addEventListener("webkitvisibilitychange", onVisibilityChanged, false);
	}
</script>
</body>
</html>
{% endhighlight %}


### **Page Visibility(页面可见性) --登录同步**

**1.  去淘宝买东西，未登录状态下，进入首页。**

**2.  然后新窗口打开任意页面，登录并成功返回。**

**3.  再次访问刚才打开的首页，发现页面还是未登录状态，实际上用户已经登录了。**

{% highlight ruby %}
//登录信息页
<p id="loginInfo"></p>
(function() {
    if (typeof pageVisibility.hidden !== "undefined") {
        var eleLoginInfo = document.querySelector("#loginInfo");
        var funLoginInfo = function() {
            var username = localStorage.username || sessionStorage.username;
            if (username) {
                eleLoginInfo.innerHTML = '欢迎回来，<strong>' + username + '</strong>';
                sessionStorage.username = username;
            } else {
                eleLoginInfo.innerHTML = '您尚未登录，请<a target="_blank" href="'+ location.href.replace("-1.", "-2.") +'">登录</a>';
            }    
        }
        pageVisibility.visibilitychange(function() {
            if (!this.hidden) funLoginInfo();
        });
        
        funLoginInfo();
        
        // 页面关闭清除localStorage
        window.addEventListener("unload", function() {
            localStorage.removeItem("username");
        })
    } else {
        alert("弹框？？？没错，因为你的这个浏览器不支持Page Visibility API的啦！");    
    }
})();

//登录页面
<form id="loginForm" action="" method="post">
    <p>用户名：<input type="text" name="username" required /></p>
    <p>密码：<input type="password" name="password" required /></p>
    <p><input type="submit" /></p>
</form>

(function() {
    if (typeof window.screenX === "number") {
	    var eleLoginForm = document.querySelector("#loginForm");
        eleLoginForm.addEventListener("submit", function(e) {
            localStorage.username = document.querySelector("input[name='username']").value;
            alert("登录成功！现在可以回到刚才的页面了！");
            this.reset();
            e.preventDefault();
        }, false);
    } else {
        alert("弹框？？？没错，因为你的这个浏览器不支持HTML5表单的啦！");    
    }
})();
{% endhighlight %}

 
 

 