---
categories: learn
layout: post-detail
title:  "Arguments"
date:   2017-04-24 16:40:04 +0800
---

#  Arguments

### **argeuments特性**

**1. 每个函数都有一个arguments属性，表示函数的实参集合，这里的实参是重点，就是执行函数时实际传入的参数的集合。**

**2. arguments不是数组而是一个对象，但它和数组很相似，所以通常称为类数组对象，以后看到类数组其实就表示arguments。**

**3. arguments有length属性，可以用arguments[length]显示调用。**

 

在js中 不需要明确指出参数名，就能访问它们，例如：


{% highlight ruby %}
function test() {
        var s = "";
        for (var i = 0; i < arguments.length; i++) {
            alert(arguments[i]);
            s += arguments[i] + ",";
        }
        return s;
    }
test("name", "age")

输出结果：

name,age
{% endhighlight %}

### **Array.prototype.slice.call(arguments)**

**call的作用----可以改变函数运行的作用域**

{% highlight ruby %}
var obj1={
	name:"java"
} 
window.name="javascript";
	var func=function(){
	console.log(this.name);
}
func();  //javasript
func.call(obj1); //java
{% endhighlight %}

那么首先我们猜测Array.prototype.slice是一个方法，将它call(arguments)之后，Array.prototype.slice中的this就指向了arguments对象了


**Array.slice是一个数组复制函数，接受两个参数(strat,[end])，从下标为start复制到下标为end**

{% highlight ruby %}
var numArr=[1,2,3,4,5];
console.log(numArr.slice(0,4)); //[1,2,3,4]
console.log(numArr.slice()); //[1,2,3,4,5]
console.log(numArr.slice(1)) //[2,3,4,5]
{% endhighlight %}

slice函数的内部实现

{% highlight ruby %}
Array.prototype.slice = function(start,end){
       var result = new Array();
       start = start || 0;
       end = end || this.length; //this指向调用的对象，当用了call后，能够改变this的指向，也就是指向传进来的对象，这是关键
       for(var i = start; i < end; i++){
            result.push(this[i]);
       }
       return result;
}
{% endhighlight %}

注意当中的result.push(this[i]),当Array.prototype.slice.call(arguments)后，就变成了result.push(arguments[i])，这样就将arguments转成了一个实实在在的数组了。



### **callee属性，返回正被执行的 Function 对象，也就是所指定的 Function 对象的正文，callee 属性是 arguments 对象的一个成员，仅当相关函数正在执行时才可用。**

callee 属性的初始值就是正被执行的 Function 对象，这允许匿名的递归函数。

{% highlight ruby %}
var sum = function (n) {
       if (1 == n) {
            return 1;
       } else {
       return n + arguments.callee(n - 1);
    }
}
alert(sum(6));
{% endhighlight %}


### **一个给对象添加属性的函数**

思考一下：首先得传入一个实参，这个实参是要被添加属性的对象。

剩下的参数是属性键值对，可以用对象表示：

{% highlight ruby %}
var obj={};//先看执行函数
function addProp(){
     var obj=[].shift.call(arguments);   //shift()函数是将数组的第一个值删除，并返回,这里是得到obj
     var propArray=[].slice.call(arguments); //此时的argument[0]已被删除，所以就剩下propArray=[{name:"gao",age:"18"}{setName:function(name){this.name=name;}]
     for(var i in propArray){
      //prop[0]={name:"gao",age:"18"}
      //prop[1]={setName:function(name){this.name=name;}
        for(var j in propArray[i]){
             obj[j]=propArray[i][j];
            //obj[name]="gao"
             //obj[age]="18"
             //obj[setName]=function(name){this.name=name;}
        }
     }
}
 
//执行函数
addProp(obj,{name:"gao",age:"18"},{setName:function(name){
     this.name=name;
}});
console.log(obj.name);  //gao
obj.setName("xiang");
console.log(obj.name);  //xiang
{% endhighlight %}


****

通俗一点就是，arguments此对象大多用来针对同个方法多处调用并且传递参数个数不一样时进行使用，根据arguments的索引来判断执行的方法。