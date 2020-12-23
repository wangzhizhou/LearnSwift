---
title: "组合复杂用户界面"
date: 2020-05-19T23:15:25+08:00
weight: 1
---

`Landmarks`应用的首页是一个纵向滚动的地标类别列表，每一个类别内部是一个横向滑动列表。随后将构建应用的页面导航，这个过程中可以学习到如果组合各种视图，并让它们适配不同的设备尺寸和设备方向。

{{%attachments title="项目文件" style="blue" pattern=".*zip" /%}}

---

### 第一节 添加一个首页视图

已经创建了所有在`Landmarks`应用中需要的视图，现在给应用创建一个首页视图，把之前创建的视图整合起来。首页不仅仅包含之前创建的视图，它还提供页面间导航的方式，同时也可以展示各种地标信息。

![section 1](/tutorials/app_design_and_layout/images/swiftui-app-design-layout-section1.png?width=20pc)

**步骤1** 创建一个名为`CategoryHome.swift`的自定义视图文件

![section 1 step 1](/tutorials/app_design_and_layout/images/swiftui-app-design-layout-section1-step1.png?width=20pc)

**步骤2** 把应用的场景代理(scene delegate)的根视图从之前的地标列表视图更改为新创建的首页视图。现在应用启动后的每一个页面就是首页了，所以还需要添加从首页导航跳转到其它页面的方法。

![section 1 step 2](/tutorials/app_design_and_layout/images/swiftui-app-design-layout-section1-step2.png?width=40pc)

**步骤3** 添加`NavigationView`，这个`NavigationView`将会容纳`Landmarks`应用中其它不同的视图。配合使用`NavigationView`、`NavigationLink`及相关的修改器，就可以构建出应用的页面间导航结构

![section 1 step 3](/tutorials/app_design_and_layout/images/swiftui-app-design-layout-section1-step3.png?width=50pc)

**步骤4** 设置导航栏标题为`Featured`

![section 1 step 4](/tutorials/app_design_and_layout/images/swiftui-app-design-layout-section1-step4.png?width=50pc)

### 第二节 创建地标类别列表

`Landmarks`应用为了便于用户浏览各种类别的地标，将地标按类别竖向排列形成列表视图，对于每一个类别内的具体地标，又把它们按照水平方向排列，形成横向列表。组合使用垂直栈(vertical statck)和水平栈(horizontal stack)并给列表添加滚动

![section 2](/tutorials/app_design_and_layout/images/swiftui-app-design-layout-section2.png?width=20pc)

**步骤1** 使用`Dictionary`结构体的初始化方法`init(grouping:by:)`，把地标数据的类别属性`category`传入作为分组依据，可以把地标数据按类别分组。工程文件中已经为每一个地标样本数据预定义了类别。

![section 2 step 1](/tutorials/app_design_and_layout/images/swiftui-app-design-layout-section2-step1.png?width=30pc)

**步骤2** 使用`List`显示地标数据的类别。`Landmark.Category`是枚举类型，它的值标识列表中每一种类别，可以保证类别不会有重复定义

![section 2 step 1](/tutorials/app_design_and_layout/images/swiftui-app-design-layout-section2-step2.png?width=50pc)

### 第三节 添加针对单个类别的地标行列表

`Landmarks`应用对每个类别下的地标采用横向滑动的行进行展示。添加一个新的视图类型用来表示这样一个地标行，然后使用这个新创建的行类型具体展示某一具体类型上的所有地标。

![section 3](/tutorials/app_design_and_layout/composing_complex_interfaces.files/add-rows-landmarks.gif?width=20pc)

**步骤1** 定义一个新的视图类型，用来展示地标类别行的内容。新建行视图需要存放地标具体类别的展示数据

![section 3 step 1](/tutorials/app_design_and_layout/images/swiftui-app-design-layout-section3-step1.png?width=50pc)

**步骤2** 更新`CategoryHome.swift`的代码，把地标类别信息传给新建的行视图类型

![section 3 step 2](/tutorials/app_design_and_layout/images/swiftui-app-design-layout-section3-step2.png?width=50pc)

**步骤3** 在`CategoryRow.swift`中使用一个`HStack`展示类别下的地标内容

![section 3 step 3](/tutorials/app_design_and_layout/images/swiftui-app-design-layout-section3-step3.png?width=50pc)

**步骤4** 为行内容指定一个高度，并把行内容嵌入到`ScrollView`中，以支持横向滑动。预览视图时，可以多增加几个地标数据，用来查看列表的滑动是否正常。

![section 3 step 4](/tutorials/app_design_and_layout/images/swiftui-app-design-layout-section3-step4.png?width=50pc)

### 第四节 组合首页

`Landmarks`应用的首页在用户点击查看地标详情前需要先把地标的一些简单信息展示出来。复用之前创建的视图构建具体某一类别地标的行视图

![section 4](/tutorials/app_design_and_layout/images/swiftui-app-design-layout-section4.png?width=20pc)

**步骤1** 在`CategoryRow.swift`文件中，与`CategoryRow`类型并列，创建一个新的自定义视图类型`CategoryItem`，用这个新的视图类型替换`CategoryRow`的地标名称`Text`控件

![section 4 step 1](/tutorials/app_design_and_layout/images/swiftui-app-design-layout-section4-step1.png?width=50pc)

**步骤2** 在`CategoryHome.swift`中，添加一个名为`FeaturedLandmarks`的简单视图，这个视图用来显示地标数据中`isFeatured`属性为真的那些地标。在之后的教程中，会把`FeaturedLandmarks`这个视图修改成一个交互式轮播图。目前，这个视图仅仅展示一张缩放和剪裁后的地标图片。

![section 4 step 2](/tutorials/app_design_and_layout/images/swiftui-app-design-layout-section4-step2.png?width=50pc)

**步骤3** 把视图的边距设置为0，让展示内容可以尽量贴着屏幕边沿

![section 4 step 3](/tutorials/app_design_and_layout/images/swiftui-app-design-layout-section4-step3.png?width=50pc)

### 第五节

现在所有类别的地标都可以在首页视图中展示出来，用户还需要能够进入应用其它页面的方法。使用页面导航和相关API来实现用户从应用首页到地标详情页、收藏列表页及用户个人中心页的跳转。

![section 5](/tutorials/app_design_and_layout/images/swiftui-app-design-layout-section5.png?width=20pc)

**步骤1** 在`CategoryRow.swift`中，把`CategoryItem`视图包裹在`NavigationLink`视图中。`CategoryItem`这时做为跳转按钮的内容，`destination`指定点击`NavigationLink`按钮时要跳转的目标视图。

![section 5 step 1](/tutorials/app_design_and_layout/images/swiftui-app-design-layout-section5-step1.png?width=50pc)

![section 5 step 1 gif](/tutorials/app_design_and_layout/composing_complex_interfaces.files/swiftui-app-design-layout-section5-step1.gif?width=20pc)

**步骤2** 使用`renderingMode(_:)`和`foregroundColor(_:)`这两个属性修改器来改变地标类别项的导航样式。做为`NavigationLink`标签的`CategoryItem`中的文本会使用`Environment`中的强调颜色，图片可能以模板图片的方式渲染，这些都可以使用属性修改器来调整，达到最佳效果。

![section 5 step 2](/tutorials/app_design_and_layout/images/swiftui-app-design-layout-section5-step2.png?width=50pc)

**步骤3** 在`CategoryHome.swift`中，添加一个模态展示的用户信息展示页，点击了用户图标时弹出展示。当状态`showProfile`被置为`true`时，展示用户信息页，当`showProfile`状态置为`false`时，用户信息页消失。

![section 5 step 3](/tutorials/app_design_and_layout/images/swiftui-app-design-layout-section5-step3.png?width=50pc)

**步骤4** 在导航条上添加一个按钮，用来切换`showProfile`状态的值：`true`或者`false`

![section 5 step 4](/tutorials/app_design_and_layout/images/swiftui-app-design-layout-section5-step4.png?width=50pc)

![section 5 step 4 gif](/tutorials/app_design_and_layout/composing_complex_interfaces.files/swiftui-app-design-layout-section5-step4.gif?width=10pc)

**步骤5** 在`CategoryHome.swift`中添加一个跳转链接，点击时跳转到全部地标的筛选页面。

![section 5 step 5](/tutorials/app_design_and_layout/images/swiftui-app-design-layout-section5-step5.png?width=50pc)

![section 5 step 5 gif](/tutorials/app_design_and_layout/composing_complex_interfaces.files/swiftui-app-design-layout-section5-step5.gif?width=20pc)

**步骤6** 把`LandmarkList.swift`中的把包裹地标列表视图的`NavigationView`移动到对应的预览视图中。因为在应用中，`LandmarkList`总是会被展示在`CategoryHome.swift`定义的导航视图中。

![section 5 step 6](/tutorials/app_design_and_layout/images/swiftui-app-design-layout-section5-step6.png?width=50pc)

### 检查是否理解

**问题1** 对于`Landmarks`这个应用来说，哪一个视图是它的根视图？

- [ ] SceneDelegate
- [ ] Landmarks
- [X] CategoryHome

**问题2** `CategoryHome`这个视图是如何与应用的其它视图联动起来的

- [ ] 在不同地标之间复用图片资源
- [ ] 与其它视图使用一致的命名规范和属性修改器语法
- [X] 使用导航结构把地标应用中所有视图连接在一起

**问题3** 下面哪段代码可以将一个普通视图转化为一个具体点击导航功能的视图

- [ ] ![problem 3 answer 1](/tutorials/app_design_and_layout/images/swiftui-drawing-animation-problem3-answer1.png?width=30pc&classes=border)
- [X] ![problem 3 answer 2](/tutorials/app_design_and_layout/images/swiftui-drawing-animation-problem3-answer2.png?width=30pc&classes=border)
- [ ] ![problem 3 answer 3](/tutorials/app_design_and_layout/images/swiftui-drawing-animation-problem3-answer3.png?width=30pc&classes=border)