#Flexbox学习之一
xuxn.github.io布局想要实现左右1:2的布局，用到了```flex:1 0 0```和```flex:2 0 0```，这样的写法其实是Flexbox的3个属性，依次是```flex-grow，flex-shrink，flex-basis```，flex-basis是Flexbox项目的基准值，决定着其他两个属性。
**1. flex-grow（扩展比例）**
> * 注意flex-grow值不依赖于其他项，容器里一个flex-grow为4的项不一定是另一个flex-grow为2的项的2倍。
> * flex-grow属性定义项目的放大比例，默认为0，即如果存在剩余空间，也不放大

* 计算 每个项目实际分配的像素值
```
可用空间 = (容器大小 - 所有相邻项目flex-basis的总和)
```
* 计算每个项目可扩展的大小
```
可扩展空间 = (可用空间/所有相邻项目flex-grow的总和)
```
* 计算每项伸缩大小(即每个项目宽度)
```
每项伸缩大小 = (伸缩基准值 + (可扩展空间 x flex-grow值))
```
**2. flex-shrink（收缩）**
>* 应用于当项目超过容器计算值时就会将项目压缩
>* flex-shrink属性定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小

* 计算每项收缩比例值乘以伸展基准值并累加
```
所有项目总和 = (项目一伸缩值 x 项目一伸缩基准值 + 项目二伸缩值 x 项目二伸缩基准值 + 项目三伸缩值 x 项目三伸缩基准值)
```
* 计算每项收缩因数
```
每项收缩因数 = 每项伸缩值 x 每项伸缩基准值/所有项目总和 0.286
```
* 计算每项移除空间
```
每项移除空间 = 每项收缩因数(0.286) x 负可用空间(-200px) 57.142向下舍入为57
```

**3. flex-basis（伸缩基准值）**
>* 要使每个项目是另一个项目的精准倍数只能将flex-basis设置为0
>* 默认值为auto，可以设为跟width或height属性一样的值（比如350px），则项目将占据固定空间
>* auto：首先检索该子元素的主尺寸，如果主尺寸不为 auto，则使用值采取主尺寸之值；如果也是 auto，则使用值为 content。
>* content：指根据该子元素的内容自动布局。有的用户代理没有实现取 content 值，等效的替代方案是 flex-basis 和主尺寸都取 auto。
>* 百分比：根据其包含块（即伸缩父容器）的主尺寸计算。如果包含块的主尺寸未定义（即父容器的主尺寸取决于子元素），则计算结果和设为 auto 一样。

**举一个不同的值之间的区别**
```
<div class="parent">
    <div class="item-1"></div>
    <div class="item-2"></div>
    <div class="item-3"></div>
</div>

<style type="text/css">
    .parent {
        display: flex;
        width: 600px;
    }
    .parent > div {
        height: 100px;
    }
    .item-1 {
        width: 140px;
        flex: 2 1 0%;
        background: blue;
    }
    .item-2 {
        width: 100px;
        flex: 2 1 auto;
        background: darkblue;
    }
    .item-3 {
        flex: 1 1 200px;
        background: lightblue;
    }
</style>
```
* 1.主轴上父容器总尺寸为 600px
* 2.子元素的总基准值是：0% + auto + 200px = 300px，其中
```
- 0% 即 0 宽度
- auto 对应取主尺寸即 100px
```
* 3.故剩余空间为 600px - 300px = 300px
* 4.伸缩放大系数之和为： 2 + 2 + 1 = 5
* 5.剩余空间分配如下
```
- item-1 和 item-2 各分配 2/5，各得 120px
- item-3 分配 1/5，得 60px
```
* 6.各项目最终宽度为：
```
- item-1 = 0% + 120px = 120px
- item-2 = auto + 120px = 220px
- item-3 = 200px + 60px = 260px
```
* 7.当 item-1 基准值取 0% 的时候，是把该项目视为零尺寸的，故即便声明其尺寸为 140px，也并没有什么用，形同虚设
* 8.而 item-2 基准值取 auto 的时候，根据规则基准值使用值是主尺寸值即 100px，故这 100px 不会纳入剩余空间
**4. flex**
> * 默认值为0 1 auto
> * ```flex:1;```让所有弹性盒模型对象的子元素都有相同的长度

* 当 flex 取值为 none，则计算值为 0 0 auto
```
.item {flex: none;}
.item {
    flex-grow: 0;
    flex-shrink: 0;
    flex-basis: auto;
}
```
* 当 flex 取值为 auto，则计算值为 1 1 auto
```
.item {flex: auto;}
.item {
    flex-grow: 1;
    flex-shrink: 1;
    flex-basis: auto;
}
```
* 当 flex 取值为一个非负数字，则该数字为 flex-grow 值，flex-shrink 取 1，flex-basis 取 0%
```
.item {flex: 1;}
.item {
    flex-grow: 1;
    flex-shrink: 1;
    flex-basis: 0%;
}
```
* 当 flex 取值为一个长度或百分比，则视为 flex-basis 值，flex-grow 取 1，flex-shrink 取 1
```
.item-1 {flex: 0%;}
.item-1 {
    flex-grow: 1;
    flex-shrink: 1;
    flex-basis: 0%;
}
.item-2 {flex: 24px;}
.item-1 {
    flex-grow: 1;
    flex-shrink: 1;
    flex-basis: 24px;
}
```
* 当 flex 取值为两个非负数字，则分别视为 flex-grow 和 flex-shrink 的值，flex-basis 取 0%
```
.item {flex: 2 3;}
.item {
    flex-grow: 2;
    flex-shrink: 3;
    flex-basis: 0%;
}
```





