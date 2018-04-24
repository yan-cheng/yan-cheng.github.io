# Graphviz使用教程

[官方地址](http://www.graphviz.org)

## 一、简介
Graphviz是贝尔实验室开发的一个开源的工具包，是一个对抽象图和网络作视觉化呈现的软件。它使用一个特定的领域特定语言(DSL)，即dot作脚本语言，然后使用布局引擎来解析此脚本，并完成自动布局。
Graphviz提供丰富的导出格式，如web中常用的图片格式和SVG格式等，其他文件格式里常用的PDF格式和Postscript格式等，或者在浏览器里呈现。此外，Graphviz也有着交互图形接口，附加工具，库，以及语言绑定，可以查看[这里](http://www.graphviz.org/content/resources)获取更多外部使用信息。

Graphviz也有一定的局限，比如绘制时序图(序列图)就很难实现。Graphviz的节点出现在画布上的位置事实上是不确定的，依赖于所使用的布局算法，而不是在脚本中出现的位置，这可能使刚开始接触Graphviz的开发人员有点不适应。Graphviz的强项在于自动布局，当图中的顶点和边的数目变得很多的时候，才能很好的体会这一特性的好处。

## 二、安装与使用

### 安装
- Windows平台安装步骤：
```
# 下载，在http://www.graphviz.org/Download_windows.php下载msi文件
# 安装，双击msi文件，然后一直next。默认情况下，dot可执行文件的位置位于"c:\Program Files\Graphviz*\bin\dot.exe"或"c:\Program Files"
# 配置环境变量，将graphviz安装目录下的bin文件夹，比如"c:\Program Files\Graphviz2.40\bin"，添加到Path环境变量中
# 验证，进入windows命令行界面，输入"dot -version"
```
- Linux平台安装步骤：
```
# 在http://www.graphviz.org/Download_source.php下载源码包，linux各发布版也有对应的安装方式
$ tar -zxvf graphviz-2.40.1.tar.gz
$ python setup.py install
# 配置环境变量，默认dot可执行文件的位置位于"/usr/local/bin/graphviz/dot"或"/usr/bin/dot"
$ setenv GRAPHVIZ_DOT /usr/local/bin/graphviz/dot
$ export GRAPHVIZ_DOT
# 验证，进入shell界面，输入"dot -version"
```
- Ubuntu安装步骤：
```
$ sudo apt-get install graphviz, graphviz-dev  # graphiviz-dev是pygraphviz所依赖的
```
### dot 命令行指令
将dot文件转换其他格式
```
dot -T<type> -o<outfile> <infile.dot>
# 更多其他的可选控制选项，比如'-Gsize="7,8" -Nshape=box -Efontsize=8'，输入"$ man dot"获得更多信息
```
```shell
# 支持转换成以下格式： bmp canon cmap cmapx cmapx_np dot eps fig gd gd2 gif gtk gv ico imap imap_np ismap jpe jpeg jpg pdf pic plain plain-ext png pov ps ps2 svg svgz tif tiff tk vml vmlz vrml wbmp x11 xdot xdot1.2 xdot1.4 xlib
$ dot -Tpng -o tree.png tree.dot  # png格式
$ dot -Tsvg -o tree.svg tree.dot  # 矢量图格式
$ dot -Tpng -o tree.pdf tree.pdf  # pdf格式
$ dot -Tps -o tree.ps tree.ps  # Postscript格式
$ dot -Tplain -o tree.plain tree.plain  # plain格式，记录各结点和边的坐标信息
```
### 图形化编辑
#### 使用gvedit
打开gvedit，新建一个.gv或者点.dot的文件并输入DOT文本，在工具栏graph下选择layout（快捷键f5）即可在窗口中看到图片，graph下选择settings（快捷键shift+f5）可以进行设置，在设置里也可以看出有多种处理DOT文本的工具可以选择（默认dot），也可以选择多种导出的文件类型（默认.png）。

#### 使用vim
在shell里运行下面指令，它会同时打开两个窗口，一个是vim，另一个就是graphviz的预览窗口，当你在vim中编辑dot代码完毕，存盘后，预览窗口就会更新显示出该dot文件的生成结果。
```
$ vimdot test.dot 
```

```
# 设置F8为快捷键，直接调用命令生成图片并打开
map <f8> :w<CR>:!dot -Tpng -o %<.png % && start %<.png<CR>
```
#### 使用Sublime
第一步：下载安装Graphviz的zip压缩包
第二步：打开Preferences -> Packages Settings-> Packages Control -> Settings User，来确认一下installed_packages没有GraphVizPreview。并且增加`"remove_orphaned": false`防止Sublime Text 把手动安装的插件包给删除了。
```
{
    "bootstrapped": true,
    "in_process_packages": [ ], 
    "installed_packages": [ "EncodingHelper", "Package Control", "Theme - Spacegray" ],
    "remove_orphaned": false
}
```
第三步：打开Preferences -> Browse Packages...进入到Sublime Text的插件包下Packages。
第四步：解压Graphviz的zip文件到Packagas下，并且更改文件夹SublimeGraphvizPreview-master为GraphVizPreview。
第五步：将F7/Cmd-B映射为调用dot引擎去绘制当前脚本，重启Sublime Text。
![](http://ww1.sinaimg.cn/mw690/8aef0dd3jw1exy8n4e8icj20m80fzq56.jpg)


### 和Python交互
- 安装Python语言捆绑包，使用Python生成dot脚本
```
$ sudo pip install pygraphviz
```
- sklearn提供了生成dot文件的接口
```
from sklearn.tree import export_graphviz    # 导入的是一个函数
# tree表示已经训练好的模型，即已经调用过DecisionTreeClassifier实例的fit(X_train, y_train)方法
export_graphviz(tree, out_file='tree.dot', 
        feature_names=['petal length', 'petal width'])1
```

## 三、布局
Graphviz中包含了众多的布局器：

- **dot** 默认布局方式，采用层次(hierarchical)结构来绘制有向图。边会朝向同一个方向(上到下，左到右)，同时避免边的交叉，减少边的长度。
<img src="http://www.graphviz.org/Gallery/directed/cluster.png" width="300">
- **neato** 使用"spring-model"布局方式，当图很大时(结点超过100个)的默认布局方式。neato试图计算出全局能量函数(global energy function)的最小值，这相当于统计多维标度分析(MDS，multi-dimensional scaling)。解决办法是stress majorization，可以采用Kamada-Kawai算法，也可以使用最快梯度下降算法。用于无向图。
<img src="http://www.graphviz.org/Gallery/undirected/ER.png" width="300">
- fdp 同neato类似，也使用"spring-model"布局方式，但通过减少forces(force-based)而不是energy(energy-based)得到布局。fdp实现了Fruchterman-Reingold启发式算法，包括一个处理更大的图和聚集无向图(larger graphs)的多重网格求解程序(multigrid solver)。用于无向图。
<img src="http://www.graphviz.org/Gallery/undirected/fdpclust.png" width="300">
- sfdp dfp的多尺度版本，适合很大的图的布局。
<img src="http://www.graphviz.org/Gallery/undirected/200910_viz_matrix_188w.png" width="300">
- twopi 径向布局(radial layouts)，结点被置于同心圆上，圆的半径取决于与根节点的距离，整体呈辐射状。
<img src="http://www.graphviz.org/Gallery/twopi/twopi2.png" width="300">
- circo 圆环布局(circular layout)，这种布局适合多个环形结构的图，比如某些电信网络。
<img src="http://www.graphviz.org/Gallery/undirected/honda-tokoro.circo.png" width="300">

## 四、dot
dot脚本定义三种对象：图，结点和边。主图(最外层的)可以是无向图或者有向图。一个主图里可以再定义子图。dot用来绘制有向图。neato用来绘制无向图。

### 基础
dot 绘制有向图，它绘制一个图有四个主要阶段。绝大多数的层级式绘图都使用该通用绘图方法。
1. dot的布局是层级的(hierarchical)，它的布局过程需要图是无环的(acyclic)。因此，绘图的第一步是通过调转内部环的有向边的方向来破坏掉图中的存在的环。
2. 第二步是将结点分派到离散的级别(rank)或水平(level)上。在一个从上到下的有向图布局里，级别决定了Y坐标。边的跨度超过一个级别时，边会被切割成一串“虚拟的”结点和单位长度的边。
3. 第三步是对同一级别里的结点排序，同时尽量避免边的交叉。
4. 第四步是设置结点的X坐标，同时尽量使边的长度最小。最后，绘制各边的样条线。

### 绘图语法
#### dot语法
以下是dot语言的抽象语法。终端(terminals)用**粗体**表示，直接量字符(literal characters)用单引号(')括起来，括号(和)表示分组(grouping)，方括号[和]表示可选项(optional items)，竖线|表示分隔多个选择(alternatives)。缩写"stmt"表示语句(statement)，"attr"表示属性(attribution)。

|-|-|
|-|-|
|graph|[**strict**] (**digraph** \| **graph**) id '{' stmt-list '}'|
|stmt-list|[stmt [';'] [stmt-list ] ]|
|stmt|attr-stmt \| node-stmt \| edge-stmt \| subgraph \| id '=' id|
|attr-stmt|(**graph** \| **node** \| **edge**) attr-list|
|attr-list|'[' [a-list ] ']' [attr-list]|
|a-list|id '=' id [','] [a-list]|
|node-stmt|node-id [attr-list]|
|node-id|id [port]|
|port|port-location [port-angle] \| port-angle [port-location]|
|port-location|':' id \| ':' '(' id ',' id ')'|
|port-angle|'@' id|
|edge-stmt|(node-id \| subgraph) edgeRHS [attr-list]|
|edgeRHS|edgeop (node-id \| subgraph) [edgeRHS]|
|subgraph|[**subgraph** id] '{' stmt-list '}' \| **subgraph** id|
- `id` 是可以下面的任意一种：任何的不以数字开头的字母数字字符串(可带下划线)，比如"node_12"；数字，比如0.2；有引号字符串，比如"print\nsomething"；
- `edgeop` 在有向图中是->，在无向图中是--；
- dot语言支持C++风格注释：`/* */` 和 `//`
- 语句末尾的分号(;)增加可读性，可以省去。但在有些很少见的例子中，没有分号隔开会引起句法解析错误，这个时候不能省去。比如，在没有分号分隔的情况下，一个有名字的但没有主体的子图出现在一个匿名的子图前，这个序列会被解析成一个有头和主体的子图。
- 复杂的属性可能会包含特殊字符，比如逗号，空格等等，这些字符在dot语言解析中是由意义的，这时要进行转义。
下面是一个例子：
```
digraph G {
    graph [center=1,size ="7,4",rankdir=LR,bgcolor="#808080",ordering=out ] 
    edge [fontname=Helvetica,fontsize=15,color=indigo];
    node [shape=circle,width=0.3,height=0.3,color=white];
    main [shape=doublecircle]; /* this is a comment */
    main -> parse [weight=8,arrowhead=odot];
    execute[style=dashed,style=diagonals];
    parse -> execute[dir=both];
    main -> init [shape=box,style=dotted];
    cleanup [shape=diamond,style=rounded];
    main -> cleanup[style=dashed];
    cleanup -> cleanup;
    execute -> {make_string,printf}
    init -> make_string;
    edge [color=red];
    main -> printf [style=bold,label="100 times"];
    make_string [label="make a\nstring"];
    node [shape=box,style=filled,color=".7 .3 1.0"];
    execute -> compare;
}
```
#### 绘图属性

##### **结点形状(Node Shapes)**

- 默认的，结点的形状是`[shape=ellipse, width=.75, height=.5]`，标签是结点名字。当绘制图时，一个结点的实际大小会比需要的大小或呈现文本标签所需大小要大一些，但如果指定`fixedsize=true`，宽和高则会被强制确定。
- 结点的形状包括：`box`(矩形)，`ellipse`(椭圆形)，`circle`(圆形)，`record`(类别框)，`point`(点)，`plaintext`(无框线)，`polygon`(多边形)，`egg`(蛋形)，`triangle`(三角形)，`diamond`(菱形)，`parallelogram`(平行四边形)，`trapezium`(梯形)，`hexagon`(六边形)，`octagon`(八边形)，`doublecircle`(双圆形)，`tripleoctagon`(三重八边形)，`invtriangle`(倒三角形)，`invtrapezium`(倒梯形)。
- 结点形状分为两类：基于多边形的和基于record的。除了形状`record`和`Mrecord`是基于record外，其他都是基于多边形的。
- 对于多边形`polygon`(polygon-based)，通过设置多边形的形状参数可得到很多没有预定义的形状，参数如下：

|参数|取值|说明|
|:-|:-|:-|
|regular|true|多边形形状将强制设为所指定的形状，不会被拉伸调整|
|peripheries|1,2,...|设置多边形的外围圈数，比如，一个双圆形的外围圈数为2|
|orientation|1,...60,...|指定多边形的顺时针旋转角度，使用度数度量角度|
|sides|1,2,...|设置多边形的边数，边数为1时是椭圆|
|skew|.4,-.6|控制多边形歪斜尺度的一个浮点数，一般设定范围在-1.0-1.0。当为正时，把多边形的上部向右侧歪斜。比如，skew可以将一个矩形形状变成平行四边形|
|distortion|.5,-.7|控制多边形从上到下缩小的参数，一般设定范围在-1.0-1.0。当为正时，上部比下部大。比如，distortion可以将一个矩形形状变成梯形|

下面是一个例子：
```
digraph G {
    a [shape=polygon,color=lightblue,style=filled,sides=4,peripheries=3,label="demo"];
    b [shape=polygon,color=lightblue,style=filled,sides=4,peripheries=3,orientation=45,label="demo"];
    c [shape=polygon,color=lightblue,style=filled,sides=4,peripheries=3,orientation=45,skew=.3,label="demo"];
    d [shape=polygon,color=lightblue,style=filled,sides=4,peripheries=3,orientation=45,skew=.3,distortion=.5,label="demo"];
    a -> b;
    b -> c;
    c -> d;
} 
```
- 基于record的结点形成其他结点形状的类，这种结点包括record和Mrecord。这两种结点，除了后面的一个带有圆角，这两个形状是一样的。
这些结点描绘一系列的递归的域(field)，而域被绘成水平的或垂直的矩形。递归的结构由结点的label给出，它的模式如下：
```
# boxLabel的第一个字符串给出域的名字，并作为该矩形的一个端口(port)名字；第二个字符串给出域的label，可以包含z转义序列
# 括号((,))，竖线(|)，尖括号(<)必须进行转义
# 字符之间的空格一般被解释为分隔符，如果空格出现在文本中，也需要被转义

rlabel    →  field ('|' field)*
field     →  boxLabel | ''rlabel''
boxLabel  →  [ '<' string '>' ] [ string ]
```
下面是一个例子：
```
digraph structs {
    node [shape=record];
    struct1 [shape=record,label="<f0> left|<f1> mid\ dle|<f2> right"];
    struct2 [shape=record,label="<f0> one|<f1> two"];
    struct3 [shape=record,label="hello\nworld |{ b |{c|<here> d|e}| f}| g | h"];
    struct1 -> struct2;
    struct1 -> struct3;
}
```

##### 标签(labels)
- 结点的标签默认是结点的名字，边默认没有标签。
- 边的标签位于边的正中央。dot会试图避免边的标签和边或结点发生交叉。如果`decorate=true`，边的标签和边会被一条线连接起来。可以使用`headlabel`和`taillabel`给边指定更多的标签，这两个便签位于边的两端，这两个标签的特征由`labelfontname`，`labelfontsize`和`labelfontcolor`三个属性决定。由于边的标签可能会与边或结点交叉，可以使用`labelangle`和`labeldistance`来调整在图中的位置。在发生交叉时，`labelangle`设置一个角度(度度量)，以调整标签旋转交叉发生点的角度，`labeldistance`设置一个距离缩放倍数，以调整标签距离交叉发生点的距离。
- 图和集群子图也可以有标签。图的标签默认在图的正下方，集群的标签默认在封闭的方框里的左上角。可以设定`labelloc=t/b/l/r`来改变标签的位置。`t/b/l/r`分别对应上/下/左/右。
- 多行标签可以通过转义序列`\n, \l, \r `来实现
- 默认的字体是黑色的`14-point Times-Roman`，通过指定`fontname`，`fontsize`，`fontcolor`来更换字体。

##### 图形样式(Graphics Styles)
- `color` 该属性指定结点和边的颜色。使用颜色有几条建议：
  1. 不要使用太多明亮的颜色
  2. 当结点颜色饱和或很深时，使用`fontcolor=white,fontname=Helvetica`会让标签更清晰

颜色有下面三种表示方法：

|表示方法|举例|说明|
|-|-|-|
|颜色名|"orchid"|通用的颜色名字|
|色调、饱和度、亮度三元组|"0.8396,0.4862,0.8549"|三个范围在0-1的浮点数|
|红蓝绿(RGB)三元组|#DA70D6|三个范围在00-FF的十六进制数，前面加上#|
- `style` 该属性控制着结点和边的很多图形特征，具体如下：

控制结点边界和边的：

|值|含义|
|-|-|
|solid|实线|
|dashed|虚线|
|dotted|点线|
|bold|粗线|
|invis|透明|

控制结点内部的：

|值|含义|
|-|-|
|filled|填充，填充颜色选择顺序为`fillcolor`->`color`->`lightgrey`|
|diagonals|边角带对角线|
|rounded|边角圆滑|
- 控制箭头的属性有`dir`，`arrowhead`，`arrowtail`和`arrowsize`。`dir` 属性控制边的箭头，该属性只设置绘制箭头的方式，并不改变边的指向方向，有四个选项，分别是**forward**(默认)，**back**(反向)，**both**(双向)，**none**(无)。`arrowhead`和`arrowtail`属性控制箭头的样式，有六个选项，分别是**normal**(默认)，**inv**(倒三角)，**dot**(实心点)，**invdot**(实心倒三角)，**odot**(空心点)，**invodot**(空心倒三角)。`arrowsize`属性控制箭头的大小。
- `bgcolor` 该属性控制图的背景颜色
##### **绘图方向，大小和间隔(Drawing Orientation, Size and Spacing)**
这部分的属性用来调整图的大小和各对象间的间隔，可以使用这些属性对图的布局作微调。

|属性|取值|说明|
|-|-|-|
|`nodesep`|0.5，1，...|指定同一rank上的结点之间的最小距离，单位为英寸|
|`ranksep`|equally，0.5，1.0，...|指定rank之间的间隔，单位为英寸。可以设置`ranksep=equally`控制所有rank之间间隔相同，也可以设置`ranksep=1.0 equally`控制所有rank之间间隔都为1英寸|
|`size`|"4,5"|指定最终图的布局的大小， 格式为`size="x,y"`。比如，无论初始布局大小，`size="7.5,10"`的图都会占用8.5*11页|
|`ratio`|2.0，fill，compress，auto|指定缩放或者填充方式，和`size`一起配合使用。当值为浮点数时，对图进行缩放；当值为fill时，对图进行填充到设置的`size`大小；当值为compress时，对图进行压缩到设置的`size`大小；当值为auto时，如果单张页面无法容纳下图，那么dot会忽略掉`size`，并自行确定一个合适的大小；|
|`rotate`|90,120，...|设置图的旋转度数，度度量。比如，当`rotate=90`或者`orientation=landscape`，图会旋转90?变成横屏模式 |
|`page`|**"8.5,11"**|If page=x, y is set, then the layout is printed as a sequence of pages which can be tiled or assembled into a mosaic. Common settings are page="8.5,11" or page="11,17". These values refer to the full size of the physical device; the actual area used will be reduced by the margin settings. For tiled layouts, it may be helpful to set smaller margins. This can take a single number, used to set both margins, or two numbers separated by a comma to set the x and y margins separately|
|`pagedir`|**BL**，TL，BR，BL|控制页面的打印方向，默认是BL方式，即自下而上，自左往右|
|`center`|true，false|如果单个页面能容纳下改图，`center=true`会将图置于页面中央|
##### **结点和边的放置(Node and Edge Placement)**
这部分的属性用来调整结点和边的放置位置，可以使用这些属性对图的布局作微调。

|属性|取值|说明|
|-|-|-|
|`rankdir`|TB,BT,LR,RL|控制边的走向|
|`rank`|same,min,source,max,sink|用来约束图的级别(rank)分配。一个子图的rank为same时，该子图的所有结点位于同样的rank里；为min时，该子图的所有结点的rank不会大于图中其他结点；为source时，该子图的所有结点的rank小于图中其他结点；为source时，该子图的所有结点的rank小于图中其他结点；max和sink则类似min和source|
|`ordering`|out|If a subgraph has ordering=out, then out-edges within the subgraph that have the same tail node will fan-out from left to right in their order of creation.|
|`group`||当边连接的两个结点都有group属性时，dot会使该边是直边，并避免与其他边交叉|
|`weight`||控制边的重要性。`weight`越高，该边的连接的结点的距离就越短，边也就越直。有时，可以使用不可见的边(`style="invis"`)结合`weight`来控制结点和边的位置。|
|`samehead`，`sametail`||控制一个结点的多个出边或入边交汇在一点|
|`constraint`|**true**,flase|一个边的头结点的rank被限制为比尾结点要高，当此属性为false时，则没有此限制|
|`minlen`|**1**,2,...|控制一条边的头结点和尾结点最小相隔的rank，一般用来阻止边的两个结点距离太近|
##### **结点端口(Node Ports)**
结点的端口(port)是边与结点的边界相交的点，当一个边没连接到结点的端口时，边会朝着结点的中心并链接到结点的边界上。
简单的端口可以使用属性`headport`与`tailport`，它们可以被赋予八个方向值，即"n"，"ne"，"e"，"se"，"s"，"sw"，"w"，"nw"。比如，当``时，边会连接到尾部节点的东南角上。

使用`record`形状的结点使用record结构来定义端口。在使用`record`形状时，使用`node_name:port_name`语法来声明边的连接。
```
digraph structs {
    node [shape=record];
    struct1 [shape=record,label="<f0> left|<f1> middle|<f2> right"];
    struct2 [shape=record,label="<f0> one|<f1> two"];
    struct3 [shape=record,label="hello\nworld |{ b |{c|<here> d|e}| f}| g | h"];
    struct1:f1 -> struct2:f0;
    struct1:f2 -> struct3:here;
}
```
当`record`形状的高度被设置成较小的值时，`record`形状会显得更合适。
```
digraph G {
    nodesep=.05;
    rankdir=LR;
    node [shape=record,width=.1,height=.1];
    
    node0 [label = "<f0> |<f1> |<f2> |<f3> |<f4> |<f5> |<f6> | ",height=2.5];
    node [width = 1.5];
    node1 [label = "{<n> n14 | 719 |<p> }"];
    node2 [label = "{<n> a1 | 805 |<p> }"];
    node3 [label = "{<n> i9 | 718 |<p> }"];
    node4 [label = "{<n> e5 | 989 |<p> }"];
    node5 [label = "{<n> t20 | 959 |<p> }"] ;
    node6 [label = "{<n> o15 | 794 |<p> }"] ;
    node7 [label = "{<n> s19 | 659 |<p> }"] ;
    
    node0:f0 -> node1:n;
    node0:f1 -> node2:n;
    node0:f2 -> node3:n;
    node0:f5 -> node4:n;
    node0:f6 -> node5:n;
    node2:p -> node6:n;
    node4:p -> node7:n;
}
```
##### **集群(Clusters)**
一个集群(cluster)是一个放置在矩形区域里的子图。当一个子图的名字的前缀是`cluster`时，一个子图便会被当做一个集群。集群的标签默认是左对齐的，可以设置`labeljust="r"`让其右对齐。
```
digraph G {
    subgraph cluster0 {
        node [style=filled,color=white];
        style=filled;
        color=lightgrey;
        a0 -> a1 -> a2 -> a3;
        label = "process #1";
    }
    subgraph cluster1 {
        node [style=filled];
        b0 -> b1 -> b2 -> b3;
        label = "process #2";
        color=blue
    }
    start -> a0;
    start -> b0;
    a1 -> b3;
    b2 -> a3;
    a3 -> a0;
    a3 -> end;
    b3 -> end;
    start [shape=Mdiamond];
    end [shape=Msquare];
}
```
### 参考
所有最新的属性请参看：http://www.graphviz.org/content/attrs
所有最新的形状请参看：http://www.graphviz.org/content/node-shapes：
所有最新的箭头请参看：http://www.graphviz.org/content/arrow-shapes
所有最新的颜色请参看：http://www.graphviz.org/content/color-names
## 相关理论
参看[这里](http://www.graphviz.org/Theory.php)获取更多绘图理论
