---
title: "创建和组合视图"
date: 2020-04-30T10:08:56+08:00
weight: 1
---

这个教程指导你构建一个名为`Landmarks(地标)`的应用。这个应用的功能是可以发现并分享你喜欢的地标。首先从创建地标详情页开始。

Landmarks使用栈来按层组合图片、文本等视图元素，从而布局页面。在视图中添加地图，需要引入`MapKit`组件，在你布局页面的过程中，
Xcode可以提供实时的反馈，让你所做的改动立即转化成对应的代码实现。

{{%attachments title="项目文件" style="blue" pattern=".*zip" /%}}

---

### 第一节 创建新项目并体验画布

创建SwiftUI项目工程，体验画布、预览模式和SwiftUI模板代码

要想在Xcode中预览画布中的视图或者与画布中的视图进行交互，要求你的Mac系统版本号不低于`macOS Catalina 10.15`

![create new project](/tutorials/swiftui_essentials/creating_and_combining_views.files/create-new-project.gif?height=30pc)

**步骤1** 打开Xcode，在启动页面点击**创建新工程**或者在菜单中选择**文件->新建->项目**

![create new project xcode](/tutorials/swiftui_essentials/images/create-new-project-xcode.png?width=20pc)

![create new project xcode menu](/tutorials/swiftui_essentials/images/create-new-project-xcode-menu.png?width=20pc)

**步骤2** 在项目模板选择器中，选择`iOS`作为项目平台，选项**单视图应用(Single View App)**作为项目模板，并点击**下一步(Next)**

![create new project app template](/tutorials/swiftui_essentials/images/create-new-project-app-template.png?width=30pc)

**步骤3** 输入`Landmarks`作为项目名称，选择`SwiftUI`作为用户界面的创建方式，并点击**下一步(Next)**，在磁盘目录下选择一个位置用来存放新创建的工程项目

![create new project info](/tutorials/swiftui_essentials/images/create-new-project-info.png?width=30pc)

**步骤4** 工程创建好并打开后，在文件导航器中，选择`ContentView.swift`文件，可以浏览一下`SwiftUI`视图的组成结构。默认情况下，`SwiftUI`的视图文件包含两个结构体(**Struct**)
第一个结构体遵循`View`协议，描述视图的内容和布局。第二个结构体声明为第一个视图的预览视图。

**步骤5** 在**画布(Canvas)**上，点击**恢复(Resume)**按钮可以显示预览视图，也可以使用快捷键`Command+Option+P`

{{% notice tip %}}
如果工程中没有出现**画布(Canvas)**，可以选择菜单:**编辑器(Editor) -> 编辑器和画布(Editor and Canvas)** 打开画布进行预览
{{% /notice %}}

![create new project completed](/tutorials/swiftui_essentials/images/create-new-project-completed.png?width=40pc)

**步骤6** 在**body**属性内部，修改文字`Hello World`为其它的不同的文字，当你在改变代码的同时，预览视图也会实时的更新对应的内容变化

![creating and combining views](/tutorials/swiftui_essentials/creating_and_combining_views.files/xcode-live-preview.gif?width=40pc)

### 第二节 定制文本视图(Text View)

可能通过修改代码来改变一个视图的显示样式，也可以通过检查器获取视图可修改属性，然后再写对应的代码改变样式。在创建应用的过程中，可以同时使用**源码编辑器、画布或者检查器**，无论当前使用的是哪一个工具编辑视图，代码会保持和这些编辑器展示的样式一致

![customize text view](/tutorials/swiftui_essentials/creating_and_combining_views.files/customize-text-view.gif?width=30pc)

下面我们使用**检查器**来定制视图的显示样式

**步骤1** 在预览视图中，按下`Command`键的同时点击控件，会弹出一个编辑弹层，然后选择**检查器(Inspect)**, 编辑弹层显示所有可以定制的视图属性，选中的控件不同，可以定制的属性集合也不相同

![swift preview inspectror](/tutorials/swiftui_essentials/images/swiftui-preview-inspector.png?width=20pc)

**步骤2** 使用检查器把文字更改为**Turtle Rock**，也就是在应用中显示的第一个地标的名称

![swiftui preivew inspector change text](/tutorials/swiftui_essentials/images/swiftui-preview-inspector-change-text.png?width=20pc)

**步骤3** 改变字体修改器为**Title**，使用系统字体修饰文字，可以自动按照用户在设备中设置的字体偏好大小进行调整。定制SwiftUI视图所调用的方法被称为视图**修改器(Modifiers)**，修改器在原视图的基础上修改部分显示样式和属性，返回一个新的视图，这样就可以让多个修改器串连进行，形成水平方向的链式调用，或者垂直方向的堆叠调用

![swiftui preview inspector change font](/tutorials/swiftui_essentials/images/swiftui-preview-inspector-change-font.png?width=20pc)

**步骤4** 手动在代码中添加**foregroundColor(.green)** 属性修改器，就会把文字的颜色调整为绿色。代码是决定视图样式的根本，当我们使用检查器来改变或移除一个属性修改器时，Xcode也会在代码编辑器中同步改变或移除对应的修改器代码

![swiftui code change foreground color](/tutorials/swiftui_essentials/images/swiftui-code-chage-foreground-color.png?width=40pc)

**步骤5** 在代码编辑器中，按下**Command**的同时点击**Text**单词也可以属性弹窗，从中选择检查器后，再点击**Color**弹出菜单，选择**继承(Inherited)**，让文字的颜色恢复成原来的黑色

![swiftui code inspector resume font](/tutorials/swiftui_essentials/images/swiftui-code-inspector-resume-font.png?width=20pc)

**步骤6** 当我们移除 **foregroundColor(.green)** 时，Xcode会自动更新你的代码来反映视图的实际显示状况

![swiftui xcode resume](/tutorials/swiftui_essentials/images/swfitui-xcode-resume.png?width=40pc)

### 第三节 使用栈来组合视图

上一节创建了标题视图，接下来要添加一些文本视图来描述地标所在州及所在公园的名称等其它详细信息

![swiftui layout stack](/tutorials/swiftui_essentials/images/swiftui-layout-stack.png?width=20pc)

创建SwiftUI视图就是在`body`属性中描述视图的内容、布局及行为，但`body`属性只返回单个视图，这时组合多个视图时可以把它们放入一个栈中，通过水平、垂直、前后嵌套多个视图完成视图组合，做为一个整体在`body`属性中返回

这一节中，使用一个垂直栈，把标题放在包含公园详情的水平栈的上方，在水平栈中，布局公园详情相关的内容

可以使用Xcode提供的结构化布局来把视图嵌套在容器视图中

**步骤1** 按下`Command`键的同时，点击`Text`视图的初始化代码打开结构化编辑弹窗，然后选择把控件嵌套在**垂直栈中(Embed in VStack)**，在栈中添加`Text View`控件可以从组件中直接拖进栈中完成

![swiftui view embed in vertical stack](/tutorials/swiftui_essentials/images/swiftui-view-embed-in-vertical-stack.png?width=20pc)

**步骤2** 点击Xcode右上角的**+**号，托动一个`Text`控件到指定位置，代码立即就会在编辑器中补全

**步骤3** 把`Text`视图的占位文本修改为`Joshua Tree Nation Park`，视图会自动调整位置布局

**步骤4** 设置位置控件的字体为子标题样式

![swiftui inspector add text view](/tutorials/swiftui_essentials/creating_and_combining_views.files/swiftui-inspector-add-text-view.gif?width=50pc)

**步骤5** 设置`VStack`初始化参数为左对齐内部的子视图。默认情况下，栈会把内部视图在自己的主轴上居中对齐，并自动计算各子视图的间距。下一步要添加一个`Text`控制用来描述公园的状态，它水平排列在位置信息的右边。

![swiftui vstack leadng alignment](/tutorials/swiftui_essentials/images/swiftui-vstack-leading-alignment.png?width=50pc)

**步骤6** 在画布内，`command`按下的同时点击位置视图，在弹出的菜单中选择**嵌入到水平栈中(Embed in HStack)**

**步骤7** 在位置控件的后面加一个公园状态的`Text`视图，并把占位文字改为`California`，字体设置为子标题样式

**步骤8** 为了水平布局使用整个屏幕宽度，在位置控件和公园状态控件中间添加一个`Spacer`控件，用来填充两个控件中间的空白部分，并把两个控件分别顶向屏幕的两侧。`Spacer`是一个可以伸缩的空白控件，他负责占用其它控件布局完成后剩下的所有空间。

**步骤9** 使用`padding()`修改器给地标信息内容视图整体加内边距

![swiftui embed in hstack](/tutorials/swiftui_essentials/creating_and_combining_views.files/swiftui-embed-in-hstack.gif?width=50pc)

### 第四节 创建自定义图像视图(Image)

有了地标名称、地标位置及状态视图，下一步再添加一个地标图片视图。这个图片视图将自定义遮罩(mask)、边框(border)和阴影(shadow)

从控件加中拖一个`Image`到画布，或直接写代码到代码编辑器中

**步骤1** 在项目资源文件中找到`turtlerock.png`图片，把它拖入资源编辑器(asset catalog editor)中，Xcode会创建一个新的图片集来存放这个图片，然后创建一个`SwiftUI`视图

![swiftui assets catalog editor](/tutorials/swiftui_essentials/images/swiftui-assets-catalog-editor.png?width=50pc)

**步骤2** 选择**文件->新建->文件**，打开模板选择器。在用户界面(User Interface)板块下，选择`SwiftUI View`并点击下一步，命名为`CircleImage.swift`，并点击创建(Create)。现在你已经准备好插入图片并修改布局来满足设计目标

![swiftui create swiftui file](/tutorials/swiftui_essentials/images/swiftui-create-swiftui-file.png?width=30pc)

![swiftui create circle image](/tutorials/swiftui_essentials/images/swifui-create-circle-image.png?width=20pc)

**步骤3** 用`Image`替换`Text`，并使用`turtlerock`图片初始化`Image`视图

**步骤4** 添加`clipShape(Circle())`修改器到`Image`，给图片添加圆形剪切效果。`Circle`是一个形状，它可以被用作遮罩、也可以是圆圈，还可以是圆形填充视图。

**步骤5** 创建另一个灰色的圆圈并把它作为一个浮层添加到图片上，相当于给图片加了一个灰色边框

**步骤6** 给视图添加半径为`10`的阴影

![swiftui turtlerock overlay](/tutorials/swiftui_essentials/images/swifui-turtlerock-overlay.png?width=50pc)

**步骤7** 把圆形边框的颜色改成白色，就完成了自定义图片视图的创建。

![swiftui circle image completed](/tutorials/swiftui_essentials/images/swiftui-circle-image-completed.png?width=50pc)

### 第五节 UIKit视图与SwiftUI视图混合使用

现在要创建一个地图视图，可以使用MapKit中的MKMapView视图类来渲染地图。要在SwiftUI中使用UIView及其子类，需要把这些UIView包裹在一个遵循`UIViewRepresentable`协议的SwiftUI视图中，SwiftUI中也包含适配`WatchKit`和`AppKit`的类似的协议。

![swiftui uikit swiftui combine](/tutorials/swiftui_essentials/images/swiftui-uikit-swiftui-combine.png?width=20pc)

首先需要创建一个自定义视图用来容纳和显示`MKMapView`

**步骤1** 选择**文件->新建->文件**，选择`iOS`平台，选择`SwiftUI View`模板，并点击下一步(Next)，命名文件为`MapView.swift`，并点击创建(Create)

**步骤2** 代码中导入MapKit引用，声明`MapView`遵循`UIViewRepresentable`协议。`UIViewRepresentable`协议要求实现两个方法`UIView(context:)`和`updateUIView(_:context:)`，第一个方法用来创建MKMapView，第二个方法用来配置视图响应状态变化

**步骤3** 替换`body`，用`makeUIView(context:)`方法来代替，创建并返回一个空的`MKMapView`

**步骤4** 创建方法`updateUIView(_:context:)`，在方法内部设置地图视图的坐标为`Turle Rock`的中心。在静态模式下预览时，只会渲染`swiftUI`视图的部分，因为`MKMapView`是`UIView`的子类，所以需要切换到实时预览模式下才能看到地图被完全渲染出来

![swiftui mapview mkmapview wrapper](/tutorials/swiftui_essentials/images/swiftui-mapview-mkmapview-wrapper.png?width=40pc)

**步骤5** 点击`Live Preview(实时预览)`按钮，可能需要点击`Try Again`和`Resume`按钮来激活预览模式的切换。切换到实时预览模式下不久就可以看到指定地标所在的地图位置了

![swiftui mkmapview live preview](/tutorials/swiftui_essentials/images/swiftui-mkmapview-live-preview.png?width=20pc)

### 第六节 组合地标详情页

前面我们创建了个地标详情页所需要的各种子视图元素：名称、地点、圆形图片以及位置地图，现在可以把这些视图元素组合在一起形成地标详情页的整个视图

![swiftui combine view begin](/tutorials/swiftui_essentials/images/swiftui-combine-view-begin.png?width=20pc)

1. 在项目工程浏览器中选择`ContentView.swift`文件

2. `body`属性中嵌入一个`VStack`视图，它内部包含另一个`VStack`视图，内部的`VStack`视图又包含三个`Text`视图

3. 在外层`VStack`的顶部添加自定义的地图视图`MapView`，并使用`frame(width:height:)`设置视图大小。当只指定高度时，宽度会自动计算为父视图的宽度，在这里就是屏幕宽度

4. 点击`Live Preview`按钮进入实时预览模式，查看地图渲染情况。在实时预览模式下可以编辑视图，最新的改动也可以实时的刷新出来。

5. 在`MapView`后面再添加一个`CircleImage`视图

6. 为了让图片视图叠放在地图视图的上面，可以设置图片视图的垂直偏移量为`-130`，图片视图的底部内边距也为`-130`，这个效果就是把图片垂直上移了130，同时和下面的文字区域留出了130的空白分隔区

7. 在外层`VStack`内部的最下面加上`Spacer`，可以让上面的视图内容顶到屏幕的上边

8. 为了让地图的视图内容显示在状态栏的下方，可以给`MapView`添加`edgesIgnoringSafeArea(.top)`修改器，这可以让它在布局时忽略顶部的安全区域边距

![swiftui combine view completed](/tutorials/swiftui_essentials/images/swiftui-combine-view-completed.png?width=50pc)

### 检查是否理解

**问题1** 在声明自定义SwiftUI视图时，视图布局要声明的在哪里？

- [ ] 在视图初始化器中
- [X] `body`属性中
- [ ] `layoutSubviews`方法中

`View`协议中要求实现`body`属性，每一个SwiftUI视图都遵循`View`协议

**问题2** 代码布局的视图是以下哪个？

![swiftui combine view problem2](/tutorials/swiftui_essentials/images/swiftui-combine-view-problem2.png?width=30pc&classes=border)

- [ ] ![swiftui combine view problem2-1](/tutorials/swiftui_essentials/images/swiftui-combine-view-problem2-1.png?width=30pc&classes=border)
- [ ] ![swiftui combine view problem2-2](/tutorials/swiftui_essentials/images/swiftui-combine-view-problem2-2.png?width=30pc&classes=border)
- [X] ![swiftui combine view problem2-3](/tutorials/swiftui_essentials/images/swiftui-combine-view-problem2-3.png?width=30pc&classes=border)

**问题3** 下面哪种方法是从body属性中返回三个视图的正确方法？

- [X] ![swiftui combine view problem3-1](/tutorials/swiftui_essentials/images/swiftui-combine-view-problem3-1.png?width=30pc&classes=border)
- [ ] ![swiftui combine view problem3-2](/tutorials/swiftui_essentials/images/swiftui-combine-view-problem3-2.png?width=30pc&classes=border)
- [ ] ![swiftui combine view problem3-3](/tutorials/swiftui_essentials/images/swiftui-combine-view-problem3-3.png?width=30pc&classes=border)

**问题4** 配置视图时，下面哪种是正确使用修改器的方式？

- [ ] ![swiftui combine view problem4-1](/tutorials/swiftui_essentials/images/swiftui-combine-view-problem4-1.png?width=30pc&classes=border)
- [ ] ![swiftui combine view problem4-2](/tutorials/swiftui_essentials/images/swiftui-combine-view-problem4-2.png?width=30pc&classes=border)
- [X] ![swiftui combine view problem4-3](/tutorials/swiftui_essentials/images/swiftui-combine-view-problem4-3.png?width=30pc&classes=border)

修改器每次都是返回一个新的对象，所以多个修改器可以通过链式调用
