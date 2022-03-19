---
title: "绘制路径和形状"
date: 2020-05-07T23:20:15+08:00
weight: 1
---

用户在浏览完一个地标后会得到一个徽章。但用户要得到徽章首先要先要创建一个徽章。本篇教程就是使用路径和形状创建徽章的过程，创建的徽章可以和其它图形组合形成位置标志。

如果想要针对不同种类的地标创建不同的徽章，可以尝试改变徽章基本组成符号的重复次数、角度或大小。

跟着教程一步步走，可以下载工程文件进行实践。

{{%attachments title="项目文件" style="blue" pattern=".*zip" /%}}

---

### 第一节 创建徽章视图

创建徽章前需要使用SwiftUI的矢量绘画API创建一个徽章视图

![badge](/tutorials/drawing_and_animation/images/swiftui-drawing-animation-badge.png?width=20pc)

**步骤1** 选择**文件->新建->文件**，然后从iOS文件模板列表中选择**SwiftUI View**。点击**下一步(Next)**，输入文件名**Badge**后点击**创建(Create)**

![create file](/tutorials/drawing_and_animation/images/swiftui-drawing-animation-create-file.png?width=50pc)

![name file](/tutorials/drawing_and_animation/images/swiftui-drawing-animation-name-file.png?width=20pc)

**步骤2** 调整**Badge**视图，暂时先让它显示"Badge"文本，一会儿再绘制徽章的形状

![badge text](/tutorials/drawing_and_animation/images/swiftui-drawing-animation-badge-text.png?width=40pc)

### 第二节 绘制徽章背景

使用SwiftUI的图形API绘制一个徽章形状

![badge background](/tutorials/drawing_and_animation/images/swiftui-drawing-animation-badge-background.png?width=20pc)

**步骤1** 查看在文件`HexagonParameters.swift`中的代码。`HexagonParameters`结构体定义了绘制徽章六边形形状的控制点参数。不需要修改这些绘制相关的数据，仅仅使用这些数据指定绘制徽章形状时，线段和曲线的控制点位置。

![hexagonal data](/tutorials/drawing_and_animation/images/swiftui-drawing-animation-hexagonal-data.png?width=40pc)

**步骤2** 在`Badge.swift`文件中，绘制徽章的形状并使用`fill`修改器给六边形填充颜色，形成一个视图。使用路径可以把多条直线、曲线或其它绘制形状的基本笔划连成一个复杂的图形，就像形成徽章六边形背景这样.

![Path](/tutorials/drawing_and_animation/images/swiftui-drawing-animation-path.png?width=20pc)

**步骤3** 给路径添加起点，`move(to:)`方法可以把绘图光标移动到绘图中的一点，准备绘制的起点

![path start point](/tutorials/drawing_and_animation/images/swiftui-drawing-animation-path-start-point.png?width=20pc)

**步骤4** 使用六边形的绘制参数数据`HexagonParameters`，依次绘制六边形的边，形成大致轮廓.`addLine(to:)`方法会使用当前绘图光标所在点为起点，方法参数中指定的点为终点绘制直线。目前六边形看起来有点问题，不过不要担心，这是意料中的事，下面的步骤做完，六边形的形状就会和开头显示的徽章的六边形形状一致了

![path fill](/tutorials/drawing_and_animation/images/swiftui-drawing-animation-badge-path-fill.png?width=40pc)

**步骤5** 使用`addQuadCurve(to:control:)`方法绘制贝塞尔曲线，让六边形的角变的更圆润些。

![badge hexagonal](/tutorials/drawing_and_animation/images/swiftui-drawing-animation-badge-hexagonal.png?width=40pc)

**步骤6** 把徽章路径包裹在一个`Geometry Reader`中，这样徽章可以使用容器的大小，定义自己绘制的尺寸，这样就不需要硬编码绘制尺寸了(**100**)。当绘制区域不是正方形时，使用绘制区域的最小边长(长宽中哪个最小使用哪个)作为绘制徽章背景的边长，并保持徽章背景的长宽比为`1:1`

![geometry reader](/tutorials/drawing_and_animation/images/swiftui-drawing-animation-badge-geometry-reader.png?width=40pc)

**步骤7** 使用`xScale`和`xOffset`参数调整变量，把徽章几何绘图区域居中绘制出来

![badge square](/tutorials/drawing_and_animation/images/swiftui-drawing-animation-badge-square.png?width=40pc)

**步骤8** 把黑色实心填充色改为渐变色，使徽章看上去和开始设计的样式一致

![badge gradient](/tutorials/drawing_and_animation/images/swiftui-drawing-animation-badge-gradient.png?width=50pc)

**步骤9** 渐变色上再使用`aspectRatio(_:contentMode:)`修改器，让渐变色按内容宽高比进行成比例渐变填充。保持`1:1`的长宽比，徽章背景可以保持居中在徽章视图中，不管徽章视图本身是不是正方形

![badge center](/tutorials/drawing_and_animation/images/swiftui-drawing-animation-badge-center.png?width=50pc)

### 第三节 绘制徽章符号

地标徽章中心有一个以地标App图标中的山峰图形改造形成的标志。山峰这个符号由两个形状组成，一个是表示山顶被雪覆盖的部分，另一个是山体。这里会使用有一定间距的两个局部三角形形状绘制这个徽章符号

![badge symbol](/tutorials/drawing_and_animation/images/swiftui-drawing-animation-badge-symbol.png?width=20pc)

**步骤1** 把之前的徽章视图形状抽出来单独形成一个`BadgeBackground`视图，并生成一个新的视图文件`BadgeBackground.swift`

![badge background](/tutorials/drawing_and_animation/images/swiftui-drawing-animation-badgebackground.png?width=50pc)

**步骤2** 把`BadgeBackground`放在`Badge`的`body`属性中。

![refactor badge](/tutorials/drawing_and_animation/images/swiftui-drawing-animation-badge-refactor-background.png?width=50pc)

**步骤3** 创建名为`BadgeSymbol`的自定义视图，这个视图是一个山峰的形状，把这个形状复制多次并按一定角度旋转多次拼成一个徽章的图案

![badge symbol](/tutorials/drawing_and_animation/images/swiftui-drawing-animation-badge-symbol-create.png?width=50pc)

**步骤4** 使用`path`API来绘制徽章符号的上半部分，试着调节`spacing`、`topWidth`、`topHeight`的系数，观察这些系数是怎么影响图形绘制的结果的

![badge symbol top](/tutorials/drawing_and_animation/images/swiftui-drawing-animation-badge-symbol-top.png?width=50pc)

**步骤5** 绘制徽章图案的下半部分，使用`move(to:)`把绘图光标移到另一个图形绘制的起点，绘制新的形状

![badge symbol bottom](/tutorials/drawing_and_animation/images/swiftui-drawing-animation-badge-symbol-bottom.png?width=50pc)

**步骤6** 用紫色填充徽章符号

![badge symbol fill](/tutorials/drawing_and_animation/images/swiftui-drawing-animation-badge-symbol-fill.png?width=50pc)

### 第四节 组合徽章的前景符号和背景形状

徽章设计思路是在背景形状上面再绘制多个有固定旋转角度的山峰符号。定义一个新的类型用于展示旋转一定角度的徽章符号，使用`ForEach`生成不同旋转角度的山峰符号，绘制在徽章背景上，从而形成最终的徽章。

![badge combine](/tutorials/drawing_and_animation/images/swiftui-drawing-animation-badge-combine.png?width=20pc)

**步骤1** 创建`RotatedBadgeSymbol`视图封装旋转徽章符号，调整旋转的角度，并在预览视图中查看效果

![badge symbol rotate 4](/tutorials/drawing_and_animation/images/swiftui-drawing-animation-rotate-badge-5.png?width=50pc)

**步骤2**  在`Badge.swift`中，使用`ZStack`把徽章图标放在徽章背景层上面。此时会发现，徽章符号的尺寸相比徽章背景大了许多，这不符合最初设计的预期

![badge symbols](/tutorials/drawing_and_animation/images/swiftui-drawing-animation-badge-symbols.png?width=50pc)

**步骤3** 缩放符号尺寸到合适的大小

![badge geometry scale](/tutorials/drawing_and_animation/images/swiftui-drawing-animation-badge-geometry-scale.png?width=50pc)

**步骤4** 使用`ForEach`复制多个徽章图标，按360度周解均分，每一个徽章符号都比前一个多旋转45度，这种就会形成一个类似太阳和徽章图标

![badge symbol completed](/tutorials/drawing_and_animation/images/swiftui-drawing-animation-badge-symbol-completed.png?width=50pc)

### 检查是否理解

**问题1** `GeometryReader`的作用是什么?

- [ ] `GeometryReader`可以把父视图分割成网格，便于在屏幕上布局视图
- [X] `GeometryReader`可以动态的绘制、定位、缩放视图，不需要写死它们的尺寸。这样可以在不同尺寸的屏幕上复用已经写好的视图
- [ ] 使用`GeometryReader`可以自动识别应用视图层级上形状的类型和位置，例如: `(圆)Circle`

**问题2** 下面代码段布局后是哪一个图？

![problem 2](/tutorials/drawing_and_animation/images/swiftui-drawing-animation-problem2.png?width=30pc)

- [X] ![answer 1](/tutorials/drawing_and_animation/images/swiftui-drawing-animation-problem2-answer1.png?width=30pc&classes=border)
- [ ] ![answer 2](/tutorials/drawing_and_animation/images/swiftui-drawing-animation-problem2-answer2.png?width=30pc&classes=border)
- [ ] ![answer 3](/tutorials/drawing_and_animation/images/swiftui-drawing-animation-problem2-answer3.png?width=30pc&classes=border)

**问题3** 下面代码绘制出哪个图？

![problem 3](/tutorials/drawing_and_animation/images/swiftui-drawing-animation-problem3.png?width=30pc)

- [ ] ![answer 1](/tutorials/drawing_and_animation/images/swiftui-drawing-animation-problem3-answer1.png?width=30pc&classes=border)
- [ ] ![answer 2](/tutorials/drawing_and_animation/images/swiftui-drawing-animation-problem3-answer2.png?width=30pc&classes=border)
- [X] ![answer 3](/tutorials/drawing_and_animation/images/swiftui-drawing-animation-problem3-answer3.png?width=30pc&classes=border)
