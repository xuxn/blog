---
categories: learn
layout: post-detail
title:  "区别style、runtimeStyle、currentStyle、getComputedStyle"
date:   2017-04-24 18:04:04 +0800
---

#  区别style、runtimeStyle、currentStyle、getComputedStyle

在JavaScript中，通过document.getElementById(id).style.XXX就可以获取到XXX的值，但是这样做只能取到通过内嵌方式设置的样式值，即style属性里面设置的值。


 
### **style**
只能获取或修改内嵌样式增改top、left等，IE里直接写数值，Firefox等要加”px”

语法：
document.getElementById(id).style.XXX



### **runtimeStyle**

运行时的样式，如果与style的属性重叠，将覆盖style的属性

runtimeStyle属性不是必需的，它的存在甚至远没有currentStyle有意义，因为由于IE布局、呈现原理限制，style属性里的定义，总有一些是无法和currentStyle同步的。什么意思呢？比如即使写上100个style="height: 1px"，也是没有任何效果的。这时style的height虽然是1px，而currentStyle的height仍然是表格实际的高度。

firefox不提供runtimeStyle和currentStyle

语法：

obj.runtimeStyle.att

obj.runtimeStyle[att]



### **currentStyle**

currentStyle 指 style 和 runtimeStyle 的结合！ 通过currentStyle就可以获取到通过内联或外部引用的CSS样式的值了（仅限IE）


语法：

obj.currentStyle.att

obj.currentStyle[att] 



###  **getComputedStyle**

注意: getComputedStyle是firefox中的， currentStyle是ie中的


语法：

window.getComputedStyle(obj, pseudoElt)[att]

window.getComputedStyle(obj, pseudoElt).att

window.getComputedStyle(obj, pseudoElt).getPropertyValue(att)

window.getComputedStyle(obj, pseudoElt).getPropertyCSSValue(att)

document.defaultView.getComputedStyle(obj,pseudoElt)[att]

document.defaultView.getComputedStyle(obj,pseudoElt).att

document.defaultView.getComputedStyle(obj,pseudoElt).getPropertyValue(att)

document.defaultView.getComputedStyle(obj,pseudoElt).getPropertyCSSValue(att)

getComputedStyle的语法带上getPropertyValue或getPropertyCSSValue才算是标准，其中pseudoElt是指伪元素，如:after, :before, :marker, :line-marker之类的，如果不用伪类，则填null就可以了。


getPropertyValue和getPropertyCSSValue的区别就是getPropertyValue返回的是一个string，而getPropertyCSSValue返回的是一个CSS2Properties对象


{% highlight ruby %}
var mydiv = document.getElementById('mydiv');
if(mydiv.currentStyle) {
      var width = mydiv.currentStyle['width'];
      alert('ie:' + width);
} else if(window.getComputedStyle) {
      var width = window.getComputedStyle(mydiv , null)['width'];
      alert('firefox:' + width);
} 
{% endhighlight %}

 