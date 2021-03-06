# 平面向量

平面向量是在二维平面内既有方向(direction)又有大小(magnitude)的量，物理学中也称作矢量，与之相对的是只有大小、没有方向的数量（标量）。

### 表示方法
在计算的时候，一般我们会使用坐标表示法去表示向量。

在直角坐标系内，我们分别取与x轴、y轴方向相同的两个单位向量i、j作为基底。任作一个向量a，由平面向量基本定理可知，有且只有一对实数x、y，使得： a = (x, y) ，我们把(x,y)叫做向量a的（直角）坐标。

其中x叫做a在x轴上的坐标，y叫做a在y轴上的坐标，上式叫做向量的坐标表示。在平面直角坐标系内，每一个平面向量都可以用一对实数唯一表示。

![image](http://vincken.top/collision/image/image6.png)

### 向量的大小

向量的大小其实就是向量的长度，又称向量的模。

![image](http://vincken.top/collision/image/image7.png)

根据勾股定理可得:

```
magnitude = Math.sqrt(x ** 2 + y ** 2)
```
### 单位向量

模等于1个单位长度的向量叫做单位向量

![image](http://vincken.top/collision/image/image8.png)

要从一个给定的向量(x1, y1)不改变方向得到它的单位向量(x2, y2)，做以下运算即可:

```
magnitude = Math.sqrt(x1 ** 2 + y1 ** 2)
x2 = x1 / magnitude
y2 = y1 / magnitude
```
### 向量的加减法
在坐标表示法中，向量加减法很简单。
给定两个向量(x1, y1)和(x2, y2)

```
sum = (x1 + x2, y1 + y2)
```

```
subtraction = (x1 - x2, y1 - y2)
```
### 数量积
已知两个非零向量a、b，那么a·b=|a||b|cosθ（θ是a与b的夹角）叫做a与b的点积、数量积或内积，记作a·b。点积a·b的几何意义是：a的长度|a|与b在a的方向上的投影|b|cosθ的乘积。计算方式如下：

```
dot = x1 * x2 + y1 * y2
```

### 向量积
两个向量a和b的向量积（外积、叉积）是一个向量，记作a×b（这里“×”并不是乘号，只是一种表示方法，与“·”不同，也可记做“∧”）。若a、b不共线，则a×b的模是：∣a×b∣=|a|·|b|·sin〈a，b〉；a×b的方向是：垂直于a和b，且a、b和a×b按这个次序构成右手系。若a、b垂直，则∣a×b∣=|a|*|b|（此处与数量积不同，请注意），若a×b=0，则a、b平行。向量积即两个不共线非零向量所在平面的一组法向量。

# 碰撞检测
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

### 点和已旋转矩形之间的碰撞
这个场景非常场景，我们用鼠标点击一个矩形，实际上就是这种判定，只是一般情况下我们用的是浏览器已经实现的API。那么如果在浏览器未实现的场景中，我们应该如何实现呢？

点与未旋转的矩形之间的判定很简单，实际上用一个包围框就可以判定，也就是满足点在两条对边之间就可以了，有以下关系
```
point.x > rect.left && 
point.x < rect.left + rect.width && 
point.y > rect.top && 
point.y < rect.top + rect.height
```
但是对于已旋转矩形，该公式不能直接使用，所以我们需要反向思考一下。
以当前方向的坐标轴来讲，矩形的确是旋转的。但是假设我们把坐标体系旋转和矩形一样的度数，那么除了矩形之外其它东西都是旋转的，而矩形是未旋转的，且坐标未变，依然可以使用以上公式。

那么建立这个新坐标轴后我们需要求出点的新坐标。我们可以看出，点在新坐标系中与原来的点的坐标实际上是做了旋转，度数为矩形旋转度数的负数。也就是矩形相对点旋转了a度，那么点相对于矩形实际是旋转了-a度。

那么结合原来的点坐标相对于矩形旋转基点的坐标还有旋转矩阵公式我们可以求出点在新坐标轴的坐标。
```
( x*cos(-radian) - y*sin(-radian), x*sin(-radian) + y*cos(-radian) )
```
最后加上旋转基点位置就是新点坐标，在此坐标体系下，矩形并未旋转，所以结合包围框的判定公式，我们可以很简单判断出点是否与矩形碰撞。
demo如下：

[点和已旋转矩形之间的碰撞](http://vincken.top/collision/demo/demo6.html)

接下来我们将来研究如何基于分离轴定理的算法来实现更为精确的碰撞检测。

### 分离轴定理
从概念上来讲，分离轴定理非常容易理解，可以用通俗的语言将其数学模型表述如下：**把受测的两个多边形置于一堵墙前面，用平行于它们的任意一条边的光线照射它们，然后根据其阴影部分是否相交来判断两者有没有发生碰撞。**

如果它们在任意一条边的投影不重叠，就说明它们并未碰撞。换言之，它们需要在所有边都有重叠的部分，才能证明它们发生了碰撞。下图为其中一条边的判定方法图解。

![image](http://vincken.top/collision/image/image4.png)

我们可以发现，由于投影轴的数量和每个多边形的边数有关，所以可能需要在很多轴上进行投影检测。所以实际使用时经常需要和其它算法结合使用来减少计算量。常用的策略有以下这些:
1. 每两个图形只检测一次。
2. 对于两条平行的边只检测一次，比如矩形四条边是两两平行的，所以只需要检测2次。
3. 只要在一条边上的投影不重合，就可以判断它们并未碰撞，立刻结束检测。
4. 物体先进行简单的包围测试，通过后再使用分离轴测试。

接下来我们首先从已旋转的矩形入手，初探分离轴定理的碰撞判定。
### 已旋转矩形之间的碰撞
为了方便计算，我们需要对分离轴定理进行简单的推导，所以这里**拿两个图形的中心点连线在投影轴上的投影长度和两个图形投影半径之和进行对比，如果半径之和小于或者等于中心连线，就可以判定投影发生重合，该边通过检测。**

首先我们需要找出两个图形所有的边缘向量和边缘法向量，因为矩形四条边两两平行，两两垂直所以我们只需要找出两条。根据旋转角度我们可以得到两条边所在的单位向量分别为

```
Vector1(Math.cos(radian), Math.sin(radian))
```

```
Vector2(-Math.sin(radian), Math.cos(radian))
```
然后我们需要计算出两个图形的中心点，利用旋转角度可以模拟出getBoundingClientRect的实现，计算出矩形每个顶点坐标，从而取任意对角两点计算出中心点。

```
center((tl.x - br.x)/2, (tl.y - br.y)/2)
```


然后就到了计算投影长度的步骤，这里需要利用一个定理：**若b为单位向量，则a与b的点积即为a在方向b的投影**。而一个矩形在某条边缘向量上的投影可以看作是该矩形所有投影轴的在这条边缘向量上的投影之和。也就是矩形投影实际上等于长和宽的投影之和。利用该定理我们可以一一求出投影。

这里涉及大量数学中的向量中的运算，为了方便，我们整理一个Vector类作为计算使用。详见demo:

[已旋转矩形demo](http://vincken.top/collision/demo/demo4.html)

### 任意凸多边形的碰撞
此处需要说明，分离轴定理只适用于任意凸多边形，也就是所有内角均小于180度的多边形。凸多边形的各个顶点都是由多边形的中心向外延伸的，比如三角形，矩形，正方形等。

所以，任意凸多边形的碰撞检测和矩形类似，只要知道多边形各个顶点，然后依次构建相邻两个顶点的向量，一一作为投影轴去计算两个多边形的投影，然后判断有无重叠即可。

然后，我们再来研究一个比较特殊的图形，也就是圆形。圆形可以看作是无数条边的正多边形，所以我们无法用常规方法去按照这些边一一去检测，事实上也不需要，我们只需要把圆的投影轴看作一条，也就是圆心与距其最近的多边形顶点之间的连线。图示如下：

![image](http://vincken.top/collision/image/image5.png)

然后把圆形投影轴和多边形的所有投影轴结合起来，去一一检测，就可以得出结果。

由于dom中无法描述创建多边形，此处demo用canvas实现。

[任意凸多边形碰撞demo](http://vincken.top/collision/demo/demo5.html)


# 边界判断

边界判断也是图形处理领域中经常会遇到的一个问题，通常用于判断某一个图形是否超过了边界，从而限制图形始终在容器中。边界判断本质上也是碰撞检测，只不过不是图形的碰撞，而是线的碰撞。下面我们会详细了解这部分。

### 未旋转矩形的边界判断

未旋转矩形的边界判断非常简单，图示如下：

![image](http://vincken.top/collision/image/image9.png)

设容器对象为container，内置矩形为rect，要让矩形始终在容器内部，由矩形的包围框可以得出以下关系：

```
rect.left >= container.left && 
rect.top >= container.top && 
rect.left + rect.width <= container.left + container.width && 
rect.top + rect.height <= container.top + container.height
```
demo如下：

[未旋转矩形的边界判断demo](http://vincken.top/collision/demo/demo7.html)

### 任意形状的边界判断

对于任意形状来讲，问题变得稍微复杂起来了，类似下面这种情况：

![image](http://vincken.top/collision/image/image10.png)

#### 任意边形的边界判断
在任意形状超过边界的时候，该形状肯定至少有两条边会和容器的边出现相交的情况，所以这个问题转换为数学问题就是，**如何判断两条线段相交**。在最坏的情况下，只要把形状所有的边和容器所有的边做相交判断，即可得到结论。

##### 线段相交判定
**1、快速排斥**

快速排斥非常简单，它是线段相交的必要非充分条件。内容是，**以两条线段为对角线的矩形，如果不重合的话，那么两条线段一定不可能相交**。图示如下：

![image](http://vincken.top/collision/image/image11.png)

要判断以两条线段为对角线的矩形是否重合，有以下条件：

- 线段ab的低点低于cd的最高点（可能重合）
- cd的最左端小于ab的最右端（可能重合）
- cd的最低点低于ab的最高点（加上条件1，两线段在竖直方向上重合）
- ab的最左端小于cd的最右端（加上条件2，两直线在水平方向上重合）

综上4个条件，两条线段组成的矩形是重合的。代码表示为：

```
Math.min(a.x, b.x) <= Math.max(c.x, d.x) && 
Math.min(c.y, d.y) <= Math.max(a.y, b.y) && 
Math.min(c.x, d.x) <= Math.max(a.x, b.x) && 
Math.min(a.y, b.y) <= Math.max(c.y, d.y)
```
然而，快速排斥只是初步的判定，主要用于快速筛选。而通过了快速排斥以后，还需要另一个判定才能通过相交的判断。

**2、跨立实验**

跨立实验是两条线段相交的充分必要条件。内容是，**如果两条线段相交，那么必须跨立，就是以一条线段为标准，另一条线段的两端点一定在这条线段的两端**。如下图所示：

![image](http://vincken.top/collision/image/image12.png)

也就是说a b两点在线段cd的两端，c d两点在线段ab的两端。

结合向量积的知识点，由其物理意义知：AB x CD=-CD x AB，(ca x cd)·(cb x cd)<=0 则说明ca cb先对于cd的方向不同，则a b在线段cd的两侧，其它点同理。

结合以上两点，就可以判断两条线段是否相交。

任意边形的边界判断都可以转换为两条线段的相交判断，因为每一条边实际上就是一条线段，在这里不再额外说明。

#### 圆的边界判断

要判断圆是否超过边界，实际上就是判断圆心到容器各边的最短距离是否大于圆的半径，如下图所示：

![image](http://vincken.top/collision/image/image13.png)

要求圆心到容器其中一条边的最短距离，比如c点到tl bl的最短距离c l，直接就是从c开始，作垂直于tl bl的直线，c点到交点的距离就是最短距离。所以转化为数学问题就是，**如何求点到直线的距离**。

由斜截式方程可以得到tl bl所在直线斜率为

```
(tl.y - bl.y) / (tl.x - bl.x)
```
互相垂直的直线斜率互为负倒数，可得c l的斜率为

```
-1 / ((tl.y - bl.y) / (tl.x - bl.x))
```
由直线斜截式方程y = kx + b可得到 

```
c.y = -1 / ((tl.y - bl.y) / (tl.x - bl.x)) * c.x + b
b = c.y + 1 / ((tl.y - bl.y) / (tl.x - bl.x)) * c.x
```

k, b已知，即c l的斜截式方程已知，可有以下关系：


```
l.y = -1 / ((tl.y - bl.y) / (tl.x - bl.x)) * l.x + c.y + 1 / ((tl.y - bl.y) / (tl.x - bl.x)) * c.x
```

而l点经过tl bl，由两点式方程有以下关系：

```
(l.x - bl.x) / (tl.x - bl.x) = (l.y - bl.y) / (tl.y - bl.y)
```
结合以上两条方程，便可求得l点坐标，从而由两点距离公式可求得c l距离。

看到这里是不是有点头晕。其实点到直线的距离公式早已被整理出来，如下图所示：

![image](http://vincken.top/collision/image/image14.png)

其中公式中的直线方程为直线的一般式方程Ax + By + C = 0，已知点的坐标为(x0, y0)

结合场景，由斜截式方程代入一般式方程，有以下关系：

```
A = bl.y - tl.y
B = tl.x - bl.x
C = bl.x * tl.y - tl.x * bl.y
```
由此代入距离公式即可求得距离，与圆半径比较即可。

以下是一个多个形状的边界判断demo

[任意形状边界判断demo](http://vincken.top/collision/demo/demo8.html)

### 总结
对碰撞检测和边界判断的介绍到这里就告一段落了，大部分判断逻辑同样适用于3D领域，只是复杂度会呈指数级增长，有兴趣的可以自行了解下。
