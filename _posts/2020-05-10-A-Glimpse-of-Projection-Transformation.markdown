# A Glimpse of Projection Transformation

## Projection in Computer Graphics
- A 3D projection or graphical projection maps points in three-dimensions onto a two-dimensional plane
- Projections transform points in n-space to m-space, where m < n.

三维投影是将三维空间中的点映射到二维平面上的方法。因为屏幕是二维, 因此三维物体呈现到屏幕就会用到投影.，因此三维投影的应用相当广泛，计算机图形学，工程学和工程制图中。

所谓的投影矩阵，指的是一个“降维”的转换过程。

## Two types of projection
- Orthographic projection
- Perspective projection

正交透视本质区别: 正交投影不会带来近大远小的现象  而透视投影会有这个现象

正交投影更多的会用在工程制图中

## Frustum

![image](https://raw.githubusercontent.com/lumixraku/NotesForGraphics/master/images/project0.jpg)


在了解计算机如何做透视之前 需要知道 Frustum.

左边透视投影,  相机理解为一个点.  从这个点出发4条线, 构成四棱锥

四棱锥中某一个深度到另一个深度构成的区域 叫做 Frustum  (中文: 平截头体 OR 视椎体)  同时定义了 近平面 和 远平面

在图形学中， 只有在这个frustum 区域内的物体才可见。

可以访问这个感受一下 [webgl-3d-perspective](https://webglfundamentals.org/webgl/lessons/webgl-3d-perspective.html)

正交投影则是长方体,  可以理解为透视投影的特殊情况, 此刻相机无限远.  那么近平面和远平面就一样大,  所以场景中物体不论和相机有多远都一样

## Orthographic Projection

A simple way to understand "Drop Z coordinates"

实际上是这么做的


![image](https://raw.githubusercontent.com/lumixraku/NotesForGraphics/master/images/project7.jpg)

先确定视野范围(图中的矩形空间)  然后标准化( 规则观察体 )


所以得到的矩阵变换如下

1. 平移到原点
2. 再缩放到标准立方体 (视锥体xyz 都缩放到 [-1, 1] 的范围)

![image](https://raw.githubusercontent.com/lumixraku/NotesForGraphics/master/images/project.jpg){:width="736px"}


相关实现 https://github.com/lumixraku/ThreePlay/blob/master/pureWebGL/Orthographic.html


## Perspective Projection
回顾一下欧几里得中平行线的性质: 同一平面内两条平行线永远不会相交

由于透视会近大远小,  因此平行线不再不行.

计算机如何做透视?

先挤压视体, 使其成为一个长方体, 那么问题就变为了正交投影.

![image](https://raw.githubusercontent.com/lumixraku/NotesForGraphics/master/images/project5.jpg){:width="736px" height="536px"}

这个"挤压"要保持下面的性质
- 近平面的任何点在挤压前后不会变
- 远平面的 z 值不会发生改变
- 远平面的中心点位置不会改变


![image](https://raw.githubusercontent.com/lumixraku/NotesForGraphics/master/images/project6.jpg){:width="736px" height="536px"}

PS: 前面提到的挤压是一种笼统的说法, 实际上做的不是挤压. 根据我上面的3条规定, 们只知道近平面和远平面 z 值不变, 中间 z 是否变不确定.


![image](https://raw.githubusercontent.com/lumixraku/NotesForGraphics/master/images/project8.jpg){:width="736px" height="536px"}



根据相似三角形, 已经可以推出部分矩阵了

![image](https://raw.githubusercontent.com/lumixraku/NotesForGraphics/master/images/project9.jpg){:width="736px" height="536px"}

现在只剩第三行不知道了

根据近平面的点变换之后不会变, 也就是(x, y, n, 1) => (x, y, n, 1). 另外根据其次坐标变换一种表达形式 (nx, ny, n^2, n)

就可以得到第三行应该是 (0, 0, A, B) 的形式.
(因为得到的结果是 n^2 也就是 xy 的部分都是0 )

![image](https://raw.githubusercontent.com/lumixraku/NotesForGraphics/master/images/project2.jpg){:width="736px" height="536px"}

PS:  注意这里最后 (0,0, A, B)那一块是简写   只写了透视矩阵的第三行  最后的 n^2 是说透视 矩阵✖️ (x, y, z, 1) 得到的第三行的值

另外再根据变换后远平面的 z 值不改变, 就可以得到一个方程组.

![image](https://raw.githubusercontent.com/lumixraku/NotesForGraphics/master/images/project3.jpg){:width="736px" height="536px"}

那么透视投影矩阵就是
```
n    0    0    0
0    n    0    0
0    0   n+f  -nf
0    0    1    0
```
也就是说透视的效果是和选取的远平面近平面有关的值.

相关实现 https://github.com/lumixraku/ThreePlay/blob/master/pureWebGL/Perspective.html
