---
title: "创建和组合视图"
date: 2023-12-10T10:08:56+08:00
weight: 1
---

本节将指导你构建 **Landmarks(地标)** 应用，用于发现和分享你喜欢的地标位置。
首先从构建显示地标详细信息的视图开始。为了布局视图，Landmarks 使用栈来组合图像、​​文本等视图组件。
要将地图添加到视图，需要依赖一个标准 MapKit 组件。当你修改视图设计时，Xcode 会实时作出反应，
以便你可以看到改动如何转化为代码。

{{%attachments title="Xcode 15 项目文件" style="orange" pattern=".*zip" /%}}

跟着下面的教程内容，自己从头开始创建一个工程。过程中需要下载上面的项目文件，并在自己的工程中引用其中的图片资源文件。

---

### 第一节 创建新项目并探索画布

> 创建一个使用 SwiftUI 的 Xcode 新项目。探索画布、预览功能和 SwiftUI 模板代码。
在 Xcode 中预览画布内的视图并进行交互，使用教程中描述的所有功能需要确保 Mac 运行的系统是 **macOS Sonoma** 或更高版本。
> ![os](/swiftui/swiftui_essentials/images/swiftui_dev_os_requirement.png?width=30pc)

**步骤1** 打开Xcode，在启动页面点击 **创建新项目**，或者在菜单中选择 **文件 -> 新建 -> 项目**

![create new project xcode](/swiftui/swiftui_essentials/images/create-new-project-xcode.png)

![create new project xcode menu](/swiftui/swiftui_essentials/images/create-new-project-xcode-menu.png)

**步骤2** 选择 **iOS** 作为项目平台，选项 **应用(App)** 作为项目模板，并点击 **下一步(Next)**

![create new project app template](/swiftui/swiftui_essentials/images/create-new-project-app-template.png)

**步骤3** 输入 **Landmarks** 作为项目名称，使用 **SwiftUI** 框架创建用户界面，并选择 **Swift** 作为开发语言，然后点击 **下一步(Next)** ，在磁盘目录下选择一个位置来存放新创建的项目

![create new project info](/swiftui/swiftui_essentials/images/create-new-project-info.png)

**步骤4** 项目创建并打开后，在左边的文件导航器中，选中并查看 **LandmarksApp.swift** 文件内容

使用 SwiftUI 模架开发的App类型应当遵循 **App** 协议，其中 **body** 属性中会返回一个或多个 **Scene(场景)**，各个场景类型会提供用于应用程序界面显示的内容。

![app arch](/swiftui/swiftui_essentials/images/create-new-project-app-arch.png)

使用 **@main** 标记的类型是应用程序运行的入口。如果进一步查看 **App** 协议的具体内容，就会发现，**App** 协议内部有一个 **main** 函数的扩展方法，其具体实现由 **SwiftUI** 框架提供，当一个类型被 **@main** 标记时，操作系统负责在应用启动时调用这个类型中的 **main** 方法，从而启动应用

![app protocol](/swiftui/swiftui_essentials/images/create-new-project-app-protocol.png)

**步骤5** 在文件导航器中选择 **ContentView.swift** 文件，查看 SwiftUI 视图的组成结构。

默认情况下，SwiftUI 视图的代码文件包含两部分：

- 遵循 **View** 协议的视图类型，用来描述视图的内容和布局
- 用于实时预览的 Swift 宏定义 **#Preview**，用来创建视图的预览界面

![app content](/swiftui/swiftui_essentials/images/create-new-app-content-view.png)

**步骤6** 可以使用快捷键 **Command + Option + P** 手动触发右边预览界面刷新，默认情况，预览界面会根据左边代码的变动，实时自动刷新

{{% notice tip %}}
如果工程中没有出现预览界面，可以通过快捷键：**Command + Option + Enter** 打开/关闭预览界面。也可以通过菜单选项进行相关操作
{{% /notice %}}

![preview canvas](/swiftui/swiftui_essentials/images/create-new-app-canvas.png)

**步骤7** 在 ContentView 类型的 **body** 属性内部，修改文字内容： **Hello, world!**，文本内容改变的同时，预览视图会实时更新

![canvas refresh](/swiftui/swiftui_essentials/images/create-new-app-canvas-refresh.gif)

### 第二节 自定义文本视图

> 可以通过修改代码改变一个视图的显示样式，也可以通过视图检查器查看视图哪些属性可以修改。
>
> 应用开发过程中，可以随意使用 **代码编辑器** 、**画布** 或者 **视图检查器**
>
> 无论使用哪一个工具编辑视图，代码都会预览界面的展示样式保持一致
> ![customize text view](/swiftui/swiftui_essentials/images/customize-text-view.gif?width=30pc)

下面我们使用 **视图检查器** 来自定义视图显示样式。

**步骤1** 将预览界面切换到 **选择模式**

{{% notice tip %}}
预览界面默认开启 **实时预览(Live)** 模式，以方便进行界面交互。
![canvas live mode](/swiftui/swiftui_essentials/images/create-new-app-canvas-preview-live.png?width=10pc)
如果要通过视图检查器在预览界面对视图进行编辑，需要切换到 **选择模式(Selectable)**
![canvas selectable mode](/swiftui/swiftui_essentials/images/create-new-app-canvas-preview-selectable.png?width=10pc)
{{% /notice %}}

**步骤2** 预览界面选择模式下，按下 **Command + Control** 同时用鼠标点击控件，会弹出一个浮层，选择 **检查器(Show SwiftUI Inspector...)**, 编辑弹层显示所有可以自定义的视图属性，选择的控件不同，可以自定义的属性集合也不同

![swift preview inspectror](/swiftui/swiftui_essentials/images/swiftui-preview-inspector.gif?width=20pc)

**步骤3** 使用视图检查器把文字更改为 **Turtle Rock**，这是应用中显示的第一个地标的名称

![swiftui preivew inspector change text](/swiftui/swiftui_essentials/images/swiftui-preview-inspector-change-text.gif)

**步骤4** 改变字体为 **Title** 样式，使用系统字体修饰文字，可以自动按照用户在设备设置中的字体偏好大小进行调整。

![swiftui preview inspector change font](/swiftui/swiftui_essentials/images/swiftui-preview-inspector-change-font.gif)

这个步骤删除了不使用的 **Image** 视图。**使用视图检查器进行视图调整后，左边的代码编辑器内容也会同步更新**

{{% notice tip %}}
自定义 SwiftUI 视图所调用的方法被称为视图 **修改器(Modifiers)**，修改器在原视图的基础上修改部分显示样式和属性，返回一个新的视图，这样就可以让多个修改器串连起来，形成链式调用。
{{% /notice %}}

**步骤5** 手动在代码编辑器中添加 **.foregroundStyle(.green)** 属性修改器，可以把文字颜色调整为绿色。

![swiftui code change foreground color](/swiftui/swiftui_essentials/images/swiftui-code-chage-foreground-color.png)

**步骤6** 在代码编辑器中，按下 **Control** 的同时点击 **Text** 组件可以打开右键弹出菜单，从中选择视图检查器，然后改变 **Foreground Style**，来切换不同的文字颜色

![swiftui code inspector change foreground style](/swiftui/swiftui_essentials/images/swiftui-code-inspector-change-foreground-style.gif)

### 第三节 使用栈(Stack)组合视图

{{% notice tip %}}
创建 SwiftUI 视图就是在 **body** 属性中描述视图的内容、布局及行为，但 **body** 属性要求只返回单个视图，通过组合多个视图，把它们放入一个栈(Stack)中，通过水平、垂直、前后三种类型的栈(**VStack**/**HStack**/**ZStack**)，嵌套多个视图完成视图组合，做为一个整体在 **body** 属性中返回
{{% /notice %}}

这一节中，使用一个垂直栈(**VStack**)，把主标题放在地标详情内容的上方，地标详情内容则使用水平栈(**HStack**)布局相关的内容，可以使用Xcode提供的结构化布局来把视图嵌套在容器视图中

![swiftui layout stack](/swiftui/swiftui_essentials/images/swiftui-layout-stack.png?width=20pc)

**步骤1** 清理模板代码中不需要的部分后，用鼠标选中 **Text** 视图，并右键单击打开弹出菜单，选择 **嵌入进垂直栈中(Embed in VStack)**

{{% notice tip %}}
使用快捷键 **Command + Option + [** 和 **Command + Option + ]** 可以快速把当前选中代码整体上移或者下移
{{% /notice %}}

![swiftui view embed in vertical stack](/swiftui/swiftui_essentials/images/swiftui-view-embed-in-vertical-stack.gif)

**步骤2** 点击 Xcode 右上角的 **+** 号打开库，拖动一个 **Text** 控件到指定位置，代码立即就会在编辑器中补全

**步骤3** 把 **Text** 视图的占位文本修改为 **Joshua Tree Nation Park**，视图会自动调整位置布局

**步骤4** 设置位置控件的字体为子标题样式

![swiftui inspector add text from library](/swiftui/swiftui_essentials/images/swiftui-inspector-add-text-from-library.gif)

**步骤5** 设置 **VStack** 初始化参数为左对齐内部的子视图。默认情况下，栈会把内部视图在自己的主轴上居中对齐，并自动计算各子视图的间距。

![swiftui vstack leadng alignment](/swiftui/swiftui_essentials/images/swiftui-vstack-leading-alignment.gif)

下一步要添加一个 **Text** 控件用来描述地标位置所在州县信息，它水平排列在位置信息的右边。

**步骤6** 在画布内，**Control** 按下的同时点击位置 **Text** 视图，在弹出的菜单中选择 **嵌入到水平栈中(Embed in HStack)**

**步骤7** 在位置控件的后面加一个显示所属州县的 **Text** 视图，并把占位文字改为 **California**，字体设置为 **Subtitle** 样式

**步骤8** 为了水平布局使用整个屏幕宽度，在位置控件和所属州县控件中间添加一个 **Spacer** 控件，用来填充两个控件中间的空白部分，并把两个控件分别顶向屏幕的两侧。**Spacer**是一个可以伸缩的空白控件，他负责占用其它控件布局完成后剩下的所有空间。

**步骤9** 使用 **padding()** 修改器给地标信息内容视图整体加内边距

![swiftui embed in hstack](/swiftui/swiftui_essentials/images/swiftui_hstack_spacer_padding.gif)

### 第四节 创建自定义图像视图

> 有了地标名称、地标位置及地标所属州县信息，下一步再添加一个地标图片信息展示视图。
> 可以创建一个单独的自定义视图，将遮罩(mask)、边框(border)和阴影(shadow)应用到图片信息上
> ![custom image](/swiftui/swiftui_essentials/images/swiftui-custom-image-demo.gif?width=30pc)

{{%attachments title="示例工程" style="orange" pattern=".*zip" /%}}

**步骤1** 下载上面附件中的示例工程，从其中的 **Resources** 文件夹找到 **turtlerock\@2x.jpg** 图片，把它拖入自建的项目资源编辑器(asset catalog editor)中，Xcode会创建一个新的图片集来存放这个图片

![swiftui assets catalog editor](/swiftui/swiftui_essentials/images/swiftui-assets-catalog-editor.gif)

**步骤2** 选择 **文件 -> 新建 -> 文件**，打开模板选择器。在用户界面(User Interface)板块下，选择`SwiftUI View`并点击下一步，命名为`CircleImage.swift`，并点击创建(Create)。现在你已经准备好插入图片并修改布局来满足设计目标

![swiftui create circle image file](/swiftui/swiftui_essentials/images/swiftui-create-swiftui-file.gif)

**步骤3** 用 **Image** 替换 **Text** ，并使用 **turtlerock** 图片初始化 **Image** 视图

**步骤4** 添加 **clipShape(Circle())** 修改器，给图片添加圆形剪切效果。**Circle** 是一种形状，它可以被用来当作蒙版、圆形边框或者填充视图。

**步骤5** 创建另一个灰色的圆圈并把它作为一个浮层添加到图片上，相当于给图片加了一个灰色边框

**步骤6** 给视图添加半径为 **7** 的阴影

**步骤7** 把圆形边框的颜色改成白色，就完成了自定义图片视图的创建。

![swiftui circle image completed](/swiftui/swiftui_essentials/images/swiftui-circle-image-completed.gif)

### 第五节 使用其他框架中的 SwiftUI 视图

> 接下来要创建一个地图视图，可以使用 MapKit 框架中提供的 **Map** 视图类来渲染地图。**Map 是一个 SwiftUI 视图**
> ![swiftui mapkit swiftui mapview](/swiftui/swiftui_essentials/images/swiftui-mapkit-mapview-demo.png)

首先需要创建一个自定义视图用来展示地图信息

**步骤1** 选择 **文件 -> 新建 -> 文件**，选择 **iOS** 平台，选择 **SwiftUI View** 模板，并点击 **下一步(Next)**，命名文件为 **MapView.swift**，并点击 **创建(Create)**

{{% notice tip %}}
也可以使用 **Command + N** 快捷键，进行文件新建
{{% /notice %}}

**步骤2** 代码中导入 MapKit 框架引用，就可以使用 MapKit 框架中提供的 SwiftUI 相关功能。

![swiftui mapkit import](/swiftui/swiftui_essentials/images/swiftui-mapkit-import.png?width=30pc)

**步骤3** 创建一个私有计算变量，用来保存地图区域信息

![swiftui private region info](/swiftui/swiftui_essentials/images/swiftui-mapkit-mapview-private-region.png?width=30pc)

 **步骤4** 将默认 **Text** 视图替换为 **Map**，使用刚才创建的私有计算变量设置 **Map**的位置区域

![swiftui mapkit map init with region](/swiftui/swiftui_essentials/images/swiftui-mapkit-mapview-map-init.png)

**步骤5** 现在可以在实时预览中看到地图展示出来了，把预览界面切换到 **实时预览(Live)** 模式，使用 **Option + 鼠标单击**  在预览界面可以缩放地图查看位置信息

### 第六节 组合详情视图
 
> 前面我们创建了个详情视图所需要的各子视图：**名称及地点信息视图**、**圆形图像视图** 以及 **地图视图**，现在可以把这些子视图组合在一起形成一整个详情视图
> ![swiftui combine view begin](/swiftui/swiftui_essentials/images/swiftui-combine-view-begin.png?width=20pc)

**步骤1** 在文件导航器中选择 **ContentView.swift** 文件

**步骤2** 将现有的 **VStack** 嵌入另一个 **VStack**中

**步骤3** 将自定义视图 **MapView** 添加到外层 **VStack** 顶部，并设置高度为 **300**。仅指定高度时，视图会自动调整宽度以填充可用空间

**步骤4** 将 **CircleImage** 添加到外层 **VStack** 中

**步骤5** 将 **CircleImage** 垂直偏移 **-130**，并设置底部内边距 **-130**

**步骤6** 在外层 **VStack** 底部插入一个 **Spacer**，把显示内容挤到页面顶部展示

**步骤7** 添加分割线和一些附加描述性文本内容

**步骤8** 把 **Text** 的字体修改器 **.font(.subtitle)** 外移到包裹它们的 **HStack** 上，并添加 **.foregroundStyle(.secondary)**

![swiftui combine view completed](/swiftui/swiftui_essentials/images/swiftui-combine-view-completed.gif)

---

### 检查是否理解

**问题1** 创建自定义 SwiftUI 视图时，视图布局要声明的在哪里？

- ❌ 视图初始化器中

- ✅ **body** 属性中

- ❌ **layoutSubviews** 方法中

{{% notice note %}}
**View** 协议中要求实现 **body** 属性，每一个 SwiftUI 视图都遵循 **View** 协议
{{% /notice %}}


**问题2** 选出以下代码布局对应的视图是哪个？

```swift
var body: some View {
    HStack {
        CircleImage()
        VStack(alignment: .leading) {
            Text("Turtle Rock")
                .font(.title)
            Text("Joshua Tree National Park")
        }
    }
}

```
|||
|:---:|---|
|❌|![swiftui combine view problem2-1](/swiftui/swiftui_essentials/images/swiftui-combine-view-problem2-1.png?width=30pc)|
|❌|![swiftui combine view problem2-2](/swiftui/swiftui_essentials/images/swiftui-combine-view-problem2-2.png?width=30pc)|
|✅|![swiftui combine view problem2-3](/swiftui/swiftui_essentials/images/swiftui-combine-view-problem2-3.png?width=30pc)|


**问题3** 下面哪种方法是从 **body** 属性中返回三个视图的正确方法？

|||
|--|---|
|✅|![swiftui combine view problem3-1](/swiftui/swiftui_essentials/images/swiftui-combine-view-problem3-1.png?width=30pc)|
|❌|![swiftui combine view problem3-2](/swiftui/swiftui_essentials/images/swiftui-combine-view-problem3-2.png?width=29pc)|
|❌|![swiftui combine view problem3-3](/swiftui/swiftui_essentials/images/swiftui-combine-view-problem3-3.png?width=30pc)|

{{% notice info %}}
**body** 属性中如果有多个子视图要返回，可以先把它们包成一个整体后，再统一返回
{{% /notice %}}


**问题4** 布局视图时，下面哪种视图修改器的使用方法是正确的？

|||
|---|---|
|❌|![swiftui combine view problem4-1](/swiftui/swiftui_essentials/images/swiftui-combine-view-problem4-1.png?width=32pc)|
|❌|![swiftui combine view problem4-2](/swiftui/swiftui_essentials/images/swiftui-combine-view-problem4-2.png?width=29pc)|
|✅|![swiftui combine view problem4-3](/swiftui/swiftui_essentials/images/swiftui-combine-view-problem4-3.png?width=29pc)|

{{% notice note %}}
视图修改器的返回结果是一个修改完成的新视图，所以多个视图修改器可以链式调用
{{% /notice %}}