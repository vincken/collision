# 碰撞检测(2)
### 概述
在第一部分我们研究了一些简单图形之间的碰撞，这些技巧适用于许多情况，而且实现起来相对简单些，然后，它们并不适用于检测已旋转矩形乃至任意多边形之间的碰撞。
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

首先我们需要找出两个图形所有的边缘向量，因为矩形四条边两两平行，所以我们只需要找出两条。根据旋转角度我们可以得到两条边所在的单位向量分别为

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


然后就到了计算投影长度的步骤，这里需要利用一个定理：**若b为单位向量，则a与b的点积即为a在方向b的投影**。而一个矩形在某条边缘向量上的投影可以看作是该矩形所有投影轴的在这条边缘向量上的投影之和。利用该定理我们可以一一求出投影。

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

### 总结
对基于分离轴定理的碰撞检测到这里就告一段落了，该定理同样适用于3D领域，只是投影轴的条数会呈指数级增长，有兴趣的可以自行了解下。