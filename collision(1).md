# 碰撞检测(1)
### 概述
在很多可视化交互场景中，都会用到各种形式的碰撞检测。比如说在游戏中，漫天的刀光剑影席卷而来，到底是精确命中，还是擦肩而过呢？当然我们能够看到这一切，不过视图只是数据的反应，本质上是数学上的判断。碰撞检测的算法有很多，我们将从基础出发，去探讨一下其中一些较为简单的2D平面上的检测原理。
### 未旋转矩形之间的碰撞
无论是什么类型的碰撞，首先我们需要找出刚好能够碰撞的临界情况，然后问题就解决了第一步。

矩形中的碰撞临界图示如下：

![image](http://vincken.top/collision/image/image1.png)

图中就是两个矩形能刚好碰撞在一起的情形。能够很清楚地看出，**当两个矩形的中心点的x坐标之差等于两个矩形的宽的一半之和，y坐标之差等于高的一半之和时，刚好发生碰撞**。

我们在简单分析一下就能发现，当中心点的 x坐标之差小于等于宽的一半之和，而且y坐标之差小于等于高的一半之和时，矩形肯定发生了碰撞。

我们设两个中心点分别为centerPoint1, centerPoint2，宽分别为width1，width2，高分别为height1，height2。有以下公式：

```
Math.abs(centerPoint1.x - centerPoint2.x) <= Math.abs(width1 - width2)
而且
Math.abs(centerPoint1.y - centerPoint2.y) <= Math.abs(height1 - height2)
```
在未旋转矩形中，中心点坐标公式为

```
x = left + width/2
y = top + height/2
```


下面是一个可视化demo，你可以随意拖动两个矩形，发生碰撞后会在页面中有相应提示。

[矩形碰撞demo](http://vincken.top/collision/demo/demo1.html)
### 圆与圆之间的碰撞
圆与圆之间的碰撞也非常简单，临界如下:
![image](http://vincken.top/collision/image/image2.png)

设圆中心点坐标分别为centerPoint1, centerPoint2，半径分别为r1, r2，直角边分别为side1，side2。图中可以看出，当两个圆刚好发生碰撞时，可以得出以下公式：

```
side1 = Math.abs(centerPoint1.x - centerPoint2.x);
side2 = Math.abs(centerPoint1.y - centerPoint2.y);
由勾股定理得：
Math.sqrt(Math.pow(side1, 2) + Math.pow(side2, 2)) === r1 + r2
```

可视化demo如下：

[圆形碰撞demo](http://vincken.top/collision/demo/demo2.html)

### 圆形与矩形的碰撞

上面我们已经探讨了矩形之间和圆形之间的碰撞，那么圆形与矩形之间的碰撞呢？
临界图示如下：
![image](http://vincken.top/collision/image/image3.png)

图中可以看到，**当矩形上离圆心最近的点等于圆的半径时，圆与矩形刚好发生碰撞**。所以我们最重要的点在于找出矩形离圆心最近的点坐标。

我们需要把临界分成四种情况，分别是圆心在矩形左侧、右侧，上下侧。首先来看水平方向，也就是x轴方向，当圆心在矩形左侧时，最近点x坐标与矩形x坐标相等，当圆心在矩形右侧时，最近点x坐标等于矩形x坐标加上矩形的宽，当圆心在矩形左侧和右侧之间时，最近点x坐标等于圆心的x坐标。垂直方向，也就是y轴方向同理。

我们设矩形离圆心最近的点为closestPoint，矩形为rect，属性为x（x坐标），y（y坐标），w（宽），h（高），圆为circle，属性为x（圆心x坐标），y（圆心y坐标），r（半径），在水平方向（x轴方向）上，可以得出：

```
if(circle.x < rect.x){
    closestPoint.x = rect.x;
}else if(circle.x > rect.x + rect.w){
    closestPoint.x = rect.x + rect.w;
}else{
    closestPoint.x = circle.x;
}
```
在垂直方向上（y轴方向），可以得出：

```
if(circle.y < rect.y){
    closestPoint.y = rect.y;
}else if(circle.y > rect.y + rect.h){
    closestPoint.y = rect.y + rect.h;
}else{
    closestPoint.y = circle.y;
}
```

再结合两点间距离公式，可以得到

```
Math.sqrt(Math.pow(closestPoint.x - circle.x, 2) + Math.pow(closestPoint.y - circle.y, 2)) = circle.r
```

可视化demo如下：

[圆形与矩形碰撞demo](http://vincken.top/collision/demo/demo3.html)

以上就是一些简单图形之间的碰撞检测，下一期我们将深入探讨一些较为复杂的碰撞场景，比如旋转后的矩形之间的碰撞，由此延伸到任何凸多边形的碰撞。


