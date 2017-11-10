---
layout: default
---

### [](header-3) 基向量

> 设变换坐标系T的基向量在原坐标系中为a,b,c，构成的变换矩阵为M，则坐标系中的任意向量x乘以矩阵M之后变换到坐标系T中了

![subdivide]({{ "/assets/img/algebra_01.png" | absolute_url }})

> 可以看出变换后的列向量每一个分量都是在变换坐标系T中各坐标轴的投影

### [](header-3) LookAt矩阵

> 使用矩阵的好处之一是如果你定义了一个坐标空间，里面有3个相互垂直的轴，你可以用这三个轴外加一个平移向量来创建一个矩阵，你可以用这个矩阵乘以任何向量来变换到那个坐标空间。

![subdivide]({{ "/assets/img/algebra_02.png" | absolute_url }})

> R是右向量，U是上向量，D是方向向量，P是摄像机位置向量。注意，位置向量是相反的，因为我们最终希望把世界平移到与我们自身移动的相反方向。使用这个LookAt矩阵坐标观察矩阵可以很高效地把所有世界坐标变换为观察坐标LookAt矩阵就像它的名字表达的那样：它会创建一个观察矩阵looks at(看着)一个给定目标。

![subdivide]({{ "/assets/img/algebra_03.png" | absolute_url }})

> -P是因为需要用a-P作为变换坐标系的输入向量

> *需要注意的是，OpenGL是右手坐标系，摄像机观察物体是从原点往-Z轴方向观察的*

![subdivide]({{ "/assets/img/algebra_04.png" | absolute_url }})

### [](header-3) 投影矩阵

> 详细的推导过程还是参考![这个网站](http://www.songho.ca/opengl/gl_projectionmatrix.html)比较好

