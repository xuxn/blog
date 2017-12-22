---
categories: learn
layout: post-detail
title:  "手机滑动事件"
date:   2017-12-22 14:00:04 +0800
---

# 手机滑动事件

移动端触屏滑动的效果其实就是图片轮播，需要用到核心的touch事件。

### **知识储备：**

#### **1. 四种touch事件：**

touchstart: //手指放到屏幕上时触发

touchmove: //手指在屏幕上滑动式触发

touchend: //手指离开屏幕时触发

touchcancel: //系统取消touch事件的时候触发 

#### **2. TouchEvent对象：**

每一个touch事件的触发都会产生一个TouchEvent对象，以下是TouchEvent对象三个比较常用的重要属性：

touches: //当前位于屏幕上的所有手指的一个列表 

targetTouches: //特定于事件目标的Touch对象的数组 

changeTouches: //表示自上次触摸以来发生了什么改变的Touch对象的数组 

#### **3. TouchEvent对象属性:**

clientX: //触摸目标在视口中的x坐标

clientY: //触摸目标在视口中的y坐标

identifier: //标识触摸的唯一ID

pageX: //触摸目标在页面中的x坐标

pageY: //触摸目标在页面中的y坐标

screenX: //触摸目标在屏幕中的x坐标

screenY: //触摸目标在屏幕中的y坐标

target: //触摸的DOM节点目标


![git install](/blog/img/2017-12-22/touchEvent.png)


### **原理：**

1：当开始一个touchstart事件的时候，获取此刻手指的横坐标startX和staerY,并且坚定touchmove和touchend事件。

{% highlight ruby %}
//定义touchstart的事件处理函数
start:function(event){
　　var touch = event.targetTouches[0]; //touches数组对象获得屏幕上所有的touch，取第一个touch
　　startPos = {x:touch.pageX,y:touch.pageY,time:+new Date}; //取第一个touch的坐标值
　　isScrolling = 0; //这个参数判断是垂直滚动还是水平滚动
　　this.slider.addEventListener('touchmove',this,false);
　　this.slider.addEventListener('touchend',this,false);
},
{% endhighlight %}

2：当触发touchmove事件的时候，再获取此时手指的横坐标moveEndX和纵坐标moveEndY，通过两次获取的坐标差值来判断手指在手机屏幕上的滑动方向，最后再为横向滚动的时候添加一些效果。

{% highlight ruby %}
//移动
move:function(event){
　　//当屏幕有多个touch或者页面被缩放过，就不执行move操作
　　if(event.targetTouches.length > 1 || event.scale && event.scale !== 1) return;
　　var touch = event.targetTouches[0];
　　endPos = {x:touch.pageX - startPos.x,y:touch.pageY - startPos.y};
　　isScrolling = Math.abs(endPos.x) < Math.abs(endPos.y) ? 1:0; //isScrolling为1时，表示纵向滑动，0为横向滑动
　　if(isScrolling === 0){
　　　　event.preventDefault(); //阻止触摸事件的默认行为，即阻止滚屏
　　　　this.slider.className = 'cnt';
　　　　this.slider.style.left = -this.index*600 + endPos.x + 'px';
　　}
},
{% endhighlight %}

3：定义手指从屏幕上拿起的事件，通过判断拿起时两次获取的坐标差值的正负来判断是向左还是向右滑动，添加相应的效果，最后解绑touchmove和touchend事件。

{% highlight ruby %}
//滑动释放
end:function(event){
　　var duration = +new Date - startPos.time; //滑动的持续时间
　　if(isScrolling === 0){ //当为水平滚动时
　　　　this.icon[this.index].className = '';
　　　　if(Number(duration) > 10){ 
　　　　　　//判断是左移还是右移，当偏移量大于10时执行
　　　　　　if(endPos.x > 10){
　　　　　　　　if(this.index !== 0) this.index -= 1;
　　　　　　}else if(endPos.x < -10){
　　　　　　　　if(this.index !== this.icon.length-1) this.index += 1;
　　　　　　}
　　　　}
　　　　this.icon[this.index].className = 'curr';
　　　　this.slider.className = 'cnt f-anim';
　　　　this.slider.style.left = -this.index*600 + 'px';
　　}
　　//解绑事件
　　this.slider.removeEventListener('touchmove',this,false);
　　this.slider.removeEventListener('touchend',this,false);
},
{% endhighlight %}

### **案例：**

<http://prototype.ui.sh.ctriptravel.com/git/Platform/h5/v7.9/one-life/dest/track-card.html> 

{% highlight ruby %}
function touch (event){  
    var event = event || window.event;   
    switch(event.type){  
        case "touchstart":    
            var finger = event.targetTouches[0];    
            startPos = {x:finger.pageX,y:finger.pageY,time:+new Date};   
            this.addEventListener('touchmove',touch,false);
            this.addEventListener('touchend',touch,false); 
            break; 
        case "touchmove":   
            var finger=event.targetTouches[0];
            endPos={x:finger.pageX-startPos.x,y:finger.pageY-startPos.y}
            liWidth=this.getElementsByTagName("li")[0].clientWidth; 
            //判断是上下滑动还是左右滑动,1是上下，0是左右
            isScrolling = Math.abs(endPos.x) < Math.abs(endPos.y)?1:0;
            if(isScrolling === 0){
                event.preventDefault(); 
                this.style.left = -liWidth*index+endPos.x+"px";
            } 
            break;   
        case "touchend":  
            if(isScrolling === 0){  
                this.getElementsByTagName("li")[index].className=" ";
                sliderDot[index].className=" ";

                //判断向左还是右 
                if(endPos.x < -10){
                    //向左滑动
                    if(index !== this.getElementsByTagName("li").length -1) index+=1;
                }else if(endPos.x > 10){
                    //向右滑动
                    if(index !== 0) index -=1;
                } 
                if(index<this.getElementsByTagName("li").length-1){
                    liWidth=this.getElementsByTagName("li")[index].clientWidth;
                }
                this.getElementsByTagName("li")[index].className="active";
                sliderDot[index].className="curr";
                index===0?this.style.left=0:this.style.left =  -index*liWidth/16-0.3 + 'rem'; 
            }
            this.removeEventListener('touchmove',this,false);
            this.removeEventListener('touchend',this,false);
            break;  
    }      
}   
index=0;
slider=document.getElementById("slider");
slider.addEventListener("touchstart",touch,false);
sliderDots=document.getElementById('dots');
sliderDot= sliderDots.getElementsByTagName('span'); 
{% endhighlight %}

 

 