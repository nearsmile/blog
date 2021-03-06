# canvas

## 构建

* 绘制
  * 布置画布：通过添加\<canvas>标签，添加canvas元素
  * 获取画布：通过\<canvas>标签的id，获得canvas对象
  * 获得画笔：通过canvas对象的getContext("2d")方法，获得2D环境
* 设置画布的大小:
  * 在\<canvas>标签中设置；
  * 在JS代码中设置canvas的属性
* Canvas是基于状态的绘制
  * 为了让绘制方法不重复绘制, 在每次绘制之前加上beginPath(), 代表下次绘制的起始之处为beginPath()之后的代码
  * beginPath()是绘制设置状态的起始点，它之后代码设置的绘制状态的作用域结束于绘制方法stroke()、fill()或者closePath()
  * 只有调用了stroke()和fill()才确定绘制

## 概念

### 线条属性

* lineCap属性：定义上下文中线的端点
  * butt：默认值，端点是垂直于线段边缘的平直边缘
  * round：端点是在线段边缘处以线宽为直径的半圆
  * square：端点是在选段边缘处以线宽为长、以一半线宽为宽的矩形
* lineJoin属性：
  * lineJoin 定义两条线相交产生的拐角，可将其称为连接
  * 在连接处创建一个填充三角形，可以使用 lineJoin 设置它的基本属性
    * miter：默认值，在连接处边缘延长相接。miterLimit 是角长和线宽所允许的最大比例(默认是 10)
    * bevel：连接处是一个对角线斜角
    * round：连接处是一个圆
  * miterLimit规定了一个自动填充连接点的极限值。如果超过了这个值，会导致lineJoin属性失效，会从 miter 变成 bevel。可以看出来，这个值和线宽以及夹角有关
* 线宽:
  * lineWidth 定义线的宽度(默认值为 1.0)
* 笔触样式
  * strokeStyle 定义线和形状边框的颜色和样式

### 填充颜色

* fillStyle属性用来设置画布上形状的基本颜色和填充
* 填充基本色的方法
  * 使用颜色字符串填充  context.fillStyle = "red";
  * 使用十六进制数字字符串填充  context.fillStyle = "#FF0000";
  * 使用十六进制数字字符串简写形式填充  context.fillStyle = "#F00";
  * 使用rgb()方法设置颜色。 context.fillStyle = "rgb(255,0,0)";
  * 使用rgba()方法设置颜色  context.fillStyle = "rgba(255,0,0,1)"; 此方法最后一个参数传递的是alpha值，透明度范围为1（不透明）~0（透明）
  * 使用hsl()方法设置颜色。 context.fillStyle = "hsl(0,100%,50%)"; HSL即是代表色相（H），饱和度（S），明度（L）三个通道的颜色。
  * 使用hsla()方法设置颜色。 context.fillStyle = "hsla(0,100%,50%,1)";

### 填充渐变形状

* 线性或径向
* 线性渐变创建一个水平、垂直或者对角线的填充图案
* 径向渐变自中心点创建一个放射状填充
* 填充渐变形状分为三步：添加渐变线，为渐变线添加关键色，应用渐变
* 渐变线的起点和终点不一定要在图像内，颜色断点的位置也是一样的。但是如果图像的范围大于渐变线，那么在渐变线范围之外，就会自动填充离端点最近的断点的颜色

```javascript
var grd = context.createLinearGradient(xstart,ystart,xend,yend); // 添加渐变线
// stop传递的是 0 ~ 1 的浮点数，代表断点到(xstart,ystart)的距离占整个渐变色长度是比例。
grd.addColorStop(stop,color); // 为渐变线添加关键色(类似于颜色断点)
context.fillStyle = grd; // 应用渐变
context.strokeStyle = grd;
```

* 绘制矩形的快捷方法
  * fillRect(x,y,width,height)、stroke(x,y,width,height)。
  * 这两个函数可以分别看做rect()与fill()以及rect()与stroke()的组合
* 径向渐变
  * createRadialGradient(x0,y0,r0,x1,y1,r1);方法规定了径向渐变开始和结束的范围，即两圆之间的渐变。

```javascript
var grd = context.createRadialGradient(x0,y0,r0,x1,y1,r1);
grd.addColorStop(stop,color);
context.fillStyle = grd;
context.strokeStyle = grd;
```

* 线性渐变是基于两个端点定义的，但是径向渐变是基于两个圆定义的。

### 填充样式

* 纹理其实就是图案的重复，填充图案通过createPattern()函数进行初始化
* 它需要传进两个参数createPattern(img,repeat-style)，第一个是Image对象实例，第二个参数是String类型，表示在形状中如何显示repeat图案。
* 第一个参数还可以传入一个canvas对象或者video对象
* 可以使用这个函数加载图像或者整个画布作为形状的填充图案。
* 4种图像填充类型
  * 平面上重复：repeat
  * x轴上重复：repeat-x
  * y轴上重复：repeat-y
  * 不使用重复：no-repeat

### 绘制标准圆弧

#### 高级路径

* 标准圆弧：arc()
  * context.arc(x,y,radius,startAngle,endAngle,anticlockwise)
  * 圆心坐标与圆半径。startAngle、endAngle使用的是弧度值，不是角度值。弧度的规定是绝对的
  * anticlockwise表示绘制的方法，是顺时针还是逆时针绘制。它传入布尔值，true表示逆时针绘制，false表示顺时针绘制，缺省值为false。
* 复杂圆弧：arcTo() - 使用切点绘制圆弧
  * `arcTo(x1,y1,x2,y2,radius)`
  * 两个切点的坐标和圆弧半径
* 塞尔曲线 Bézier curve
  * 曲线定义：起始点、终止点、控制点。
  * 贝塞尔曲线是一条由起始点、终止点和控制点所确定的曲线
  * n阶贝塞尔曲线就有n-1个控制点
  * 二次贝塞尔曲线: `context.quadraticCurveTo(cpx,cpy,x,y)`
  * 三次贝塞尔曲线：bezierCurveTo()
    * `context.bezierCurveTo(cp1x,cp1y,cp2x,cp2y,x,y)`
    * 绘制波浪线的神器

#### 图形变换

* 用数学方法调整所绘形状的物理属性 - 坐标变形
* 平移变换：`translate(x,y)`
* 旋转变换：`rotate(deg)`
  * 传入的参数是弧度，不是角度
  * 以坐标系的原点（0，0）为圆心进行的顺时针旋转
  * 通常需要配合使用translate()平移坐标系，确定旋转的圆心, 旋转变换通常搭配平移变换使用的
  * Canvas是基于状态的绘制，每次旋转都是接着上次旋转的基础上继续旋转，所以在使用图形变换的时候必须搭配save()与restore()方法，一方面重置旋转角度，另一方面重置坐标系原点
* 缩放变换：`scale(sx,sy)`
* 一定要以最初状态为最根本的参照物
* 在涉及所有图形变换的时候，都要使用状态保存, 在每次平移之前使用`context.save()`，在每次绘制之后，使用`context.restore()`
* 在使用图形变换的时候，要记得结合使用状态保存
* 变换矩阵
  * `context.transform(a,b,c,d,e,f)`
  * 参数  意义
    a   水平缩放(1)
    b   水平倾斜(0)
    c   垂直倾斜(0)
    d   垂直缩放(1)
    e   水平位移(0)
    f   垂直位移(0)
  * 对一个图形进行变换的时候，只要对变换矩阵相应的参数进行操作，操作之后，对图形的各个定点的坐标分别乘以这个矩阵，就能得到新的定点的坐标

#### 文本

|属性|描述|
|:--:|:--:|
|`font`|设置或返回文本内容的当前字体属性|
|`textAlign`|设置或返回文本内容的当前对齐方式|
|`textBaseline`|设置或返回在绘制文本时使用的当前文本基线|

|方法|描述|
|:--:|:--:|
|`fillText()`|在画布上绘制“被填充的”文本|
|`strokeText()`|在画布上绘制文本（无填充）|
|`measureText()`|返回包含指定文本宽度的对象|

* Canvas上的文本不能使用CSS样式，虽然font属性与CSS的属性相似，但是却不能够交换使用
* 使用font设置字体。 使用fillStyle设置字体颜色。 使用fillText()方法显示字体。
* font属性可以不指定，如果没有指定字体，则默认自动使用 10px 无衬线体
* context.font = "[font-style] [font-variant] [font-weight] [font-size/line-height] [font-family]"
* font-style 属性定义字体的风格; font-variant 属性设置小型大写字母的字体显示文本
* @font-face 能够加载服务器端的字体文件，让客户端显示客户端所没有安装的字体。目前支持加载EOT与TTF文件。
* `context.fillText(String,x,y,[maxlen])`与`context.strokeText(String,x,y,[maxlen])`

##### 文本对齐与度量

* 水平对齐textAlign `context.textAlign="center|end|left|right|start"`
* 垂直对齐textBaseline `context.textBaseline="alphabetic|top|hanging|middle|ideographic|bottom"`
* 文本度量使用measureText()方法实现，这个api在换行显示判断中会有奇效 `context.measureText(text).width`

#### 阴影效果

* context.shadowColor：阴影颜色。
* context.shadowOffsetX：阴影x轴位移。正值向右，负值向左。
* context.shadowOffsetY：阴影y轴位移。正值向下，负值向上。
* context.shadowBlur：阴影模糊滤镜。数据越大，扩散程度越大。

#### 图像合成

* globalCompositeOperation属性设置或返回如何将一个源（新的）图像绘制到目标（已有）的图像上
* 源图像 = 您打算放置到画布上的绘图
* 目标图像 = 您已经放置在画布上的绘图

|值|描述|
|:--:|:--:|
|source-over|默认。在目标图像上显示源图像。|
|source-atop|在目标图像顶部显示源图像。源图像位于目标图像之外的部分是不可见的。|
|source-in|在目标图像中显示源图像。只有目标图像内的源图像部分会显示，目标图像是透明的。|
|source-out|在目标图像之外显示源图像。只会显示目标图像之外源图像部分，目标图像是透明的。|
|destination-over|在源图像上方显示目标图像。|
|destination-atop|在源图像顶部显示目标图像。源图像之外的目标图像部分不会被显示。|
|destination-in|在源图像中显示目标图像。只有源图像内的目标图像部分会被显示，源图像是透明的。|
|destination-out|在源图像外显示目标图像。只有源图像外的目标图像部分会被显示，源图像是透明的。|
|lighter|显示源图像 + 目标图像。|
|copy|显示源图像。忽略目标图像。|
|xor|使用异或操作对源图像与目标图像进行组合。|

#### 裁剪和绘制图像

* Canvas API的图像裁剪功能是指，在画布内使用路径，只绘制该路径内所包含区域的图像，不绘制路径外的图像
* 使用图形上下文的不带参数的clip()方法来实现Canvas的图像裁剪功能。该方法使用路径来对Canvas画布设置一个裁剪区域。因此，必须先创建好路径。创建完整后，调用clip()方法来设置裁剪区域。
* 裁剪是对画布进行的，裁切后的画布不能恢复到原来的大小
* 画布是先裁切完了再进行绘图

#### 绘制图像drawImage()

* 可以引入图像、画布、视频，并对其进行缩放或裁剪
* 三参数：context.drawImage(img,x,y)
* 五参数：context.drawImage(img,x,y,width,height)
* 九参数：context.drawImage(img,sx,sy,swidth,sheight,x,y,width,height)
* 三参数的是标准形式，可用于加载图像、画布或视频；五参数的除了可以加载图像还可以对图像进行指定宽高的缩放；九参数的除了缩放，还可以裁剪。

|参数|描述|
|:--:|:--:|
|img|规定要使用的图像、画布或视频。|
|sx|可选。开始剪切的 x 坐标位置。|
|sy|可选。开始剪切的 y 坐标位置。|
|swidth|可选。被剪切图像的宽度。|
|sheight|可选。被剪切图像的高度。|
|x|在画布上放置图像的 x 坐标位置。|
|y|在画布上放置图像的 y 坐标位置。|
|width|可选。要使用的图像的宽度。（伸展或缩小图像）|
|height|可选。要使用的图像的高度。（伸展或缩小图像）|