---
title: "视图动画和转场"
date: 2020-05-07T23:20:35+08:00
weight: 2
---

使用SwiftUI可以把视图状态的改变转成动画过程，`SwiftUI`会处理所有复杂的动画细节

在这篇中，会给跟踪用户徒步的图表视图添加动画。使用`animation(_:)`修改器给一个视图添加动画效果非常容易

下载起步项目并跟着本篇教程一步步实践，或者查看本篇完成状态时的工程代码去学习

{{%attachments title="项目文件" style="blue" pattern=".*zip" /%}}

---

### 第一节 给每个视图单独添加动画

在视图上使用`animation(_:)`修改器时，`SwiftUI`会在视图的任何可进行动画的属性发生改变时产生对应的动画效果。视图的颜色、不透明度、旋转角度、大小及一些其它属性都是可进行动画的

![animate button](/tutorials/drawing_and_animation/images/swiftui-drawing-path-and-shape-animate-button.png?width=20pc)

**步骤1** 在`HikeView.swift`中，打开实时预览，体验一下图表的打开和隐藏，此时的状态改变时是没有添加动画效果的。在本篇的实践中，保持实时预览一直打开，每一步修改的效果就可以实时的看到

![live preview animation](/tutorials/drawing_and_animation/animating_views_and_transitions.files/live-preview-animations.gif?width=20pc)

**步骤2** 给显示/隐藏切换的箭头按钮添加旋转动画，会发现现在按钮点击时的旋转有一个动画过渡的效果了

![rotate button animation](/tutorials/drawing_and_animation/images/swiftui-animation-transition-rotate-button-animation.png?width=30pc)

![rotate button animation video](/tutorials/drawing_and_animation/animating_views_and_transitions.files/rotate-button-aniamtion.gif?width=20pc)

**步骤3** 当视图从隐藏到展示时，让切换按钮变大1.5倍

![rotate button scale](/tutorials/drawing_and_animation/images/swiftui-animation-transition-rotate-button-scale.png?width=30pc)

![rotate button scale video](/tutorials/drawing_and_animation/animating_views_and_transitions.files/rotate-button-animation-scale.gif?width=20pc)

**步骤4** 把动画的类型从`easeInOut`改为`spring()`。SwiftUI包含一些预设或可自定义的动画类型，像`弹簧(spring)`动画和类型`液体(fluid)`动画类型。可以调整动画开始前的等待时长、动画的速度也可以指定让动画循环重复的进行

![rotate button spring](/tutorials/drawing_and_animation/images/swiftui-animation-transtion-rotate-button-spring.png?width=30pc)

**步骤5** 如果只想让按钮具有缩放动画而不进行旋转动画，可以在`scaleEffect`添加`animation(nil)`来实现。可以在这里做一些实验，如果把其它的一些动画效果结合在一起，会怎么样

![rotate button no rotate](/tutorials/drawing_and_animation/images/swiftui-animation-transition-rotate-button-no-rotate.png?width=30pc)

**步骤6** 学下一节之前，把本节中添加的`animation(_:)`修改器都去掉

![rotate button resume](/tutorials/drawing_and_animation/images/swiftui-animation-transition-rotate-button-resume.png?width=30pc)

### 第二节 把视图的状态改态转化成动画效果

已经学会了给单个视图添加动画的方法，现在可以学习怎么在视图的状态发生改变时添加动画效果。当用户点击按钮时会切换`showDetail`状态的值，在视图变化过程中添加动画效果。

![state change](/tutorials/drawing_and_animation/images/swiftui-animation-transtion-state-change.png?width=10pc)

**步骤1** 把`showDetail.toggle()`包裹在`withAnimation`函数调用块中。`showDetail`的改变影响了视图`HikeDetail`和详情切换按钮，在显示/隐藏详情的过程中都有了过滤动画效果。

![with_animation block](/tutorials/drawing_and_animation/animating_views_and_transitions.files/with-animation-block.gif?width=50pc)

放慢动画速度，可以观察`SwiftUI`动画在被中断下是怎么运作的

**步骤2** 给`withAnimation`传入一个`时长4秒`的基本动画参数`.easeInOut(duration:4)`，可以指定动画过程时长，给`withAnimation`传入的动画参数与`.animation(_:)`修改器可用参数一致。

![with animation duration block](/tutorials/drawing_and_animation/animating_views_and_transitions.files/with-animation-duration.gif?width=50pc)

**步骤3** 在动画过程进行中点击按钮切换视图状态，查看对应的动画被中断时的效果

![with animation interrupt](/tutorials/drawing_and_animation/animating_views_and_transitions.files/with-animation-interrupt.gif?width=20pc)

**步骤4** 读下一节之前，把动画时长参数(`.easeInOut(duration: 4)`)去掉，让动画不再缓慢进行。

### 第三节 定制视图转场动画

默值情况下，视图离屏和入屏时的动画效果是`渐隐/渐现`， 这个默认的转场效果可以使用`transition(_:)`修改器进行定制。

![transitions](/tutorials/drawing_and_animation/animating_views_and_transitions.files/customize-view-transitions.gif?width=20pc)

**步骤1** 给`HikeView`视图添加`transition(_:)`修改器，并定制转场参数为`.slide`，转场动画为`滑入/滑出`

![transition slide](/tutorials/drawing_and_animation/animating_views_and_transitions.files/transition-slide.gif?width=40pc)

**步骤2** 可以把`滑入/滑出`这种转场动画封装起来，方便其它视图复用同样的转场效果

![custom transition effect](/tutorials/drawing_and_animation/images/swiftui-animation-transition-custom-transition.png?width=30pc)

**步骤3** 在`moveAndFade`转场效果的定义中使用`move(edge:)`，让滑入/滑出从屏幕的同一边进行

![move and fade custom](/tutorials/drawing_and_animation/animating_views_and_transitions.files/custom-move-and-fade.gif?width=50pc)

**步骤4** 使用`asymmetric(insertion:removal:)`修改器来定制视图显示/消失时的转场动画效果

![custom move and fade slide scale](/tutorials/drawing_and_animation/animating_views_and_transitions.files/custom-move-and-fade-slide-scale.gif?width=50pc)

### 第四节 组合复杂的动画效果

点击图表下面的三个按钮，会在三个不同的数据集间进行切换并展示。本节中会使用组合动画，让图表在不同数据集间切换时的转换动画流畅自然。

![combine animation](/tutorials/drawing_and_animation/images/swiftui-animation-transition-combine-animation.png?width=20pc)

**步骤1** 把`showDetail`的默认值改为`true`，并把`HikeView`的预览模式视图固定在画布上。这样可以在编辑其它文件时，依然看到动画效果的变化。

![pin canvas](/tutorials/drawing_and_animation/images/swiftui-animation-transition-pin-canvas.png?width=50pc)

**步骤2** 在`HikeGraph.swift`中定义了一个新的`波动`动画，并把它与`滑入/滑出`动画一起应用到图表视图上。

![hike graph ripple](/tutorials/drawing_and_animation/images/swiftui-animation-transition-hike-graph-ripple.png?width=50pc)

**步骤3** 把动画切换为弹簧动画(`spring`)，并设置弹簧阻尼系数为`0.5`，动画过程中产生了逐渐回弹效果

![spring animation](/tutorials/drawing_and_animation/animating_views_and_transitions.files/hike-graph-transition-spring-animation.gif?width=50pc)

**步骤4** 加速弹簧动画的执行速度，缩短切换图表的时间

![spring animation speed](/tutorials/drawing_and_animation/animating_views_and_transitions.files/spring-animation-speed.gif?width=50pc)

**步骤5** 以当条形在图表中的位置为参数，添加延迟效果，图表中的每个条形会顺序动起来

![spring animation index based delay](/tutorials/drawing_and_animation/animating_views_and_transitions.files/spring-animation-delay-index-based.gif?width=50pc)

**步骤6** 观察一下自定义`波动(rippling)`效果是怎么作用在视图转场中的

### 检查是否理解

**问题1** 怎样从一串动画效果调用中，去掉其中的一种动画效果。以下面的代码为例，怎样去掉旋转动画

![problem 1](/tutorials/drawing_and_animation/images/swiftui-animation-transition-problem1.png?width=20pc)

- [X] ![a1](/tutorials/drawing_and_animation/images/swiftui-animation-transition-problem1-1.png?width=30pc&classes=border)
- [ ] ![a2](/tutorials/drawing_and_animation/images/swiftui-animation-transition-problem1-2.png?width=30pc&classes=border)
- [ ] ![a3](/tutorials/drawing_and_animation/images/swiftui-animation-transition-problem1-3.png?width=30pc&classes=border)

**问题2** 当你开发动画的过程上，为什么要把预览视图固定在画布上？

- [ ] 为了固定动画过程中的当前帧
- [ ] 为了在多个设备配置开发中预览动画效果
- [X] 为了在切换到其它不同文件时，固定显示当前视图的预览

**问题3** 在视图状态改变时，如何快速测试一个动画在被中断时的表现

- [ ] 在包含`animation(_:)`修改器的代码行上打一个断点，然后单步按动画帧进行测试
- [X] 调整动画的持续时长，让动画在足够长的时间内完成，这样就可以调整动画的细节
- [ ] 重复的调用`sleep(100)`来减慢动画的执行
