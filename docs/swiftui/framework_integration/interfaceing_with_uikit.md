---
title: "与UIKit交互"
date: 2020-07-19T21:44:35+08:00
weight: 1
---

`SwiftUI`可以在苹果全平台上无缝兼容现有的UI框架。例如，可以在`SwiftUI视图`中嵌入`UIKit视图`或`UIKit视图控制器`，反过来在`UIKit视图`或`UIKit视图控制器`中也可以嵌入`SwiftUI`视图。

本篇教程展示如何把`landmark`应用的主页混合使用`UIPageViewController`和`UIPageControl`。使用`UIPageViewController`来展示由`SwiftUI`视图构成的轮播图，使用状态变量和绑定来操作用户界面数据的更新。

跟着教程一步步走，可以下载工程文件进行实践。

{{%attachments title="项目文件" style="blue" pattern=".*zip" /%}}

---

### 第一节 创建一个用来展示UIPageViewController的SwiftUI视图

为了在`SwiftUI`视图中展示`UIKit`视图和`UIKit`视图控制器，需要创建遵循`UIViewRepresentable`和`UIViewControllerRepresentable`协议的类型。创建的自定义视图类型，用来创建和配置所要展示的`UIKit`类型，`SwiftUI`框架来管理`UIKIt`类型的生命周期并在适当的时机更新它们。

![section 1](/tutorials/framework_integration/images/interfacing_with_uikit_section1.png?width=20pc)

**步骤1** 创建一个新的`SwiftUI`视图文件，命名为`PageViewController.swift`，并且声明`PageViewController`类型遵循`UIViewControllerRepresentable`。这个页面视图控制器存放一个`UIViewController`实例数组，数组中的每一个元素代表在地标滚动过程中的一页视图。

![section 1 step 1](/tutorials/framework_integration/images/interfacing_with_uikit_section1_step1.png?width=40pc)

下一步添加`UIViewControllerRepresentable`协议的两个实现, 目前因为协议方法没有完成实现，会有报错提示。

**步骤2** 添加一个`makeUIViewController(context:)`方法，方法内部以指定的配置创建一个`UIPageViewController`。`SwiftUI`会在准备显示视图时调用一次`makeUIViewController(context:)`方法创建`UIViewController`实例，并管理它的生命周期。

![section 1 step 2](/tutorials/framework_integration/images/interfacing_with_uikit_section1_step2.png?width=40pc)

由于还缺少一个协议方法没有实现，所以目前还是会报错。

**步骤3** 添加`updateUIViewController(_:context:)`方法，这个方法里调用`setViewControllers(_:direction:animated:)`方法展示数组中的第一个视图控制器

![section 1 step 3](/tutorials/framework_integration/images/interfacing_with_uikit_section1_step3.png?width=40pc)

创建另一个`SwiftUI`视图展示遵循`UIViewControllerRepresentable`协议的视图

**步骤4** 创建一个名为`PageView.swift`的视图，声明一个`PageViewController`作为子视图。初始化时使用一个视图数组来初始化，并把每一个视图都嵌入在一个`UIHostingController`中。`UIHostingController`是一个`UIViewController`的子类，用来在`UIKit`环境中表示一个`SwiftUI`视图。

![section 1 step 4](/tutorials/framework_integration/images/interfacing_with_uikit_section1_step4.png?width=40pc)

**步骤5** 更新预览视图，并传入视图数组，预览视图就会开始工作了

![section 1 step 5](/tutorials/framework_integration/images/interfacing_with_uikit_section1_step5.png?width=40pc)

**步骤6** 在继续下面的步骤前，先把`PageView`的预览视图固定住，以避免在文件切换时不能实现预览到`PageView`的改变。

![section 1 step 6](/tutorials/framework_integration/images/interfacing_with_uikit_section1_step6.png?width=20pc)

### 第二节 创建视图控制器的数据源

短短几个步骤就做了很多事，`PageViewController`使用`UIPageViewController`去展示来自`SwiftUI`内容。现在是时候添加挥动手势进行页面之间的翻动了。

![section 2](/tutorials/framework_integration/images/interfacing_with_uikit_section2.png?width=20pc)

一个展示`UIKit`视图控制器的`SwiftUI`视图可以定义一个`Coordinator`类型，这个`Coordinator`类型由SwitUI管理，用来作为视图展示的环境

**步骤1** 在`PageViewControlelr`中定义一个嵌套类型`Coordiantor`。`SwiftUI`管理`UIViewController Representable`类型的`coordinator`，并在调用方法时把它作为环境的一部分。

![section 2 step 1](/tutorials/framework_integration/images/interfacing_with_uikit_section2_step1.png?width=30pc)

**步骤2** 在`PageView Controller`中添加另一个方法，创建`coordinator`。`SwiftUI`在调用`makeUIViewController(context:)`前会先调用`makeCoordinator()`方法，因此在配置视图控制器时是可以访问到`coordiantor`对象的。可以使用`coordinator`为实现通用的`Cocoa模式`,例如：`代理模式`、`数据源`以及`目标-动作`。

![section 2 step 2](/tutorials/framework_integration/images/interfacing_with_uikit_section2_step2.png?width=50pc)

**步骤3** 让`Coordinator`类型添加`UIPageViewControllerDataSource`协议遵循，并且实现两个必要方法。这两个必要方法会建立起视图控制器之间的联系，因此可以实现页面之前的前后切换。

![section 2 step 3](/tutorials/framework_integration/images/interfacing_with_uikit_section2_step3.png?width=50pc)

**步骤4** 把`coordiantor`作为`UIPageViewController`的数据源

![section 2 step 4](/tutorials/framework_integration/images/interfacing_with_uikit_section2_step4.png?width=50pc)

**步骤5** 打开实时预览，并测试一下前后页面切换的功能是否正常

![swipe landmarks](/tutorials/framework_integration/interfaceing_with_uikit.files/swipe-landmarks.gif?width=20pc)

### 第三节 在`SwiftUI`视图的状态下跟踪页面

如果要添加一个自定义的`UIPageControl`控件，就需要一种方式能够在`PageView`中跟踪当前展示的页面。这就需要在`PageView`中声明一个`@State`属性，并传递一个针对该属性的绑定关系给`PageViewController`视图，在`PageViewController`中通过绑定关系更新状态属性，来反映当前展示的页面。

![section 3](/tutorials/framework_integration/images/interfacing_with_uikit_section3.png?width=20pc)

**步骤1** 在`PageViewController`中添加一个绑定属性`currentPage`。除了使用关键字`@Binding`声明属性为绑定属性外，还需要更新一下函数`setViewControllers(_:direction:animated:)`，给它传入`currentPage`绑定属性

![section 3 step 1](/tutorials/framework_integration/images/interfacing_with_uikit_section3_step1.png?width=50pc)

做到这一步还不能正常运行，继续进行下一步。

**步骤2** 在`PageView`中声明`@State`变量，并在创建`PageViewController`时把绑定属性传入。注意使用`$`语法创建一个针对状态变量的绑定关系。

![section 3 step 2](/tutorials/framework_integration/images/interfacing_with_uikit_section3_step2.png?width=50pc)

**步骤3** 通过改变`PageView`视图中的`currentPage`初始值来测试绑定关系是否正常生效。也可以做一个测试按钮，点击按钮时让第二个页面展示出来

![section 3 step 3](/tutorials/framework_integration/images/interfacing_with_uikit_section3_step3.png?width=50pc)

**步骤4** 添加一个`TextView`控件来展示状态变量`currentPage`的值，拖动页面切换时观察`TextView`上的值，目前不会发生变化。因为`PageViewController`内部没有在切换页面的过程中更新`currentPage`的值。

![section 3 step 4](/tutorials/framework_integration/images/interfacing_with_uikit_section3_step4.png?width=50pc)

**步骤5** 在`PageViewController.swift`中让`coordinator`作为`UIPageViewController`的代理，并添加`pageViewController(_:didFinishAnimating:previousViewControllers:transitionCompleted completed: Bool)` 方法。因为`SwiftUI`在页面切换动画完成时会调用这个方法，这样就可以这个方法内部获取当前正在展示的页面的下标，并同时更新绑定属性`currentPage`的值。

![section 3 step 5](/tutorials/framework_integration/images/interfacing_with_uikit_section3_step5.png?width=50pc)

**步骤6** `coordinator`除了是`UIPageViewController`数据源外，再把它赋值为`UIPageViewController`的代理。由于绑定关系是双向的，所以当页面切换时，`PageView`视图上的`Text`就会实时展示当前的页码。

![section 3 step 6](/tutorials/framework_integration/images/interfacing_with_uikit_section3_step6.png?width=50pc)


![section 3 step 6 gif](/tutorials/framework_integration/interfaceing_with_uikit.files/swipe-binding-text.gif?width=20pc)

### 第四节 添加一个自定义`PageControl`

我们已经为包裹在`UIViewRepresentable`视图中的子视图上添加了一个自定义`UIPageControl`

![section 4](/tutorials/framework_integration/images/interfacing_with_uikit_section4.png?width=20pc)

**步骤1** 创建一个新的`SwiftUI`视图，命名为`PageControl.swift`，并使用`PageControl`类型遵循`UIViewRepresentable`协议。`UIViewRepresentable`和`UIViewControllerRepresentable`类型有相同的生命周期，在UIKit类型中都有对应的生命周期方法。

![section 4 step 1](/tutorials/framework_integration/images/interfacing_with_uikit_section4_step1.png?width=50pc)

**步骤2** 在`PageView`中用`PageControl`替换`Text`,并把`VStack`换成`ZStack`。因为总页数和当前页面都已经传入`PageControl`，所以`PageControl`已经可以正确的显示。

![section 4 step 2](/tutorials/framework_integration/images/interfacing_with_uikit_section4_step2.png?width=50pc)

下一步要处理`PageControl`与用户的交互，让它可以被用户点击任意一边进行页面间的切换。

**步骤3**  在`PageControl`中创建一个嵌套类型`Coordiantor`，添加一个`makeCoordinator()`方法创建并返回一个`coordinator`实例。因为`UIControl`子类(包括`UIPageControl`)使用`Target-Action`模式，`Coordinator`实现一个`@objc`方法来更新`currentPage`绑定属性的值。

![section 4 step 3](/tutorials/framework_integration/images/interfacing_with_uikit_section4_step3.png?width=50pc)

**步骤4** 把`coordinator`作为`PageControl`值改变事件的目标处理器，并指定`updateCurrentPage(sender:)`方法为处理函数

![section 4 step 4](/tutorials/framework_integration/images/interfacing_with_uikit_section4_step4.png?width=50pc)

**步骤5** 现在就可以尝试`PageControl`的各种交互来切换页面，`PageView`展示了`SwiftUI`和`UIKit`视图如何混合使用。

![section 4 step 5 gif](/tutorials/framework_integration/interfaceing_with_uikit.files/page-control.gif?width=20pc)

### 检查是否理解

**问题1** 下面哪个协议可以用来把`UIKit`中的视图控件器桥接进`SwiftUI`？

- [ ] UIViewRepresentable
- [ ] UIHostingController
- [X] UIViewControllerRepresentable

**问题2** 对于`UIViewControllerRepresentable`类型，下面哪个方法可以为它创建一个代理或数据源？

- [ ] 在`makeUIViewController(context:)`方法中创建`UIViewController`实例的地方
- [ ] 在`UIViewControllerRepresentable`类型的初始化器中
- [X] 在`makeCoordinator()`方法中