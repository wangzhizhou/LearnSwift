---
title: "创建macOS应用"
date: 2020-07-19T21:44:35+08:00
weight: 3
---

创建了`watchOS`平台的`Landmarks`应用后，下一步就是把`Landmarks`带到`MacOS`平台上。运用之前学到的所有知识，完成在`iOS`、`watchOS`及`macOS`的全平台应用。

在项目工程中添加`macOS`编译目标，复用在`iOS`应用中的代码和资源，使用`SwiftUI`创建`macOS`平台上的列表和详情视图。

按照步骤来编译工程，或者下载工程查看完成后的代码。

{{%attachments title="项目文件" style="blue" pattern=".*zip" /%}}

---

### 第一节 项目中添加`macOS`编译目标

项目中添加`macOS`编译目标，`Xcode`会自动添加一个文件组与一些初始文件，还会生成一个编译运行方案。

![section 1](/tutorials/framework_integration/images/creating_a_macOS_App_seciont1.png?width=30pc)

**步骤1** 选择`File`->`New`->`Target`，模板选择页面出现后，选择`macOS`选项卡，选中`App`模板并点击`Next`。这个模板会添加一个新的`macOS`编译目标到项目里。

![section 1 step1](/tutorials/framework_integration/images/creating_a_macOS_App_seciont1_step1.png?width=40pc)

**步骤2** 在信息表中，输入`MacLandmarks`作为项目的名称，设置编程语言为`Swift`，界面构建方法为`SwiftUI`，然后点击`Finish`。

![section 1 step2](/tutorials/framework_integration/images/creating_a_macOS_App_seciont1_step2.png?width=40pc)

**步骤3** 设置运行方案为`MacLandmarks` -> `My Mac`。这样就可以编译并运行`macOS`应用。

![section 1 step3](/tutorials/framework_integration/images/creating_a_macOS_App_seciont1_step3.png?width=30pc)

这个应用的运行依赖一些特性，这些特性在早期的macOS上是不支持的，所以可能需要改变部署目标。

**步骤4** 在项目导航器中，选择顶部的`Xcode`项目，在可用编译运行目标栏中，选择部署目标为`10.15`。

![section 1 step4](/tutorials/framework_integration/images/creating_a_macOS_App_seciont1_step4.png?width=40pc)

**步骤5** 在`MacLandmarks`文件夹中，选择`ContentView.swift`文件，打开预览画布，点击`恢复(Resume)`，查看预览。`SwiftUI`会提供`main`视图和它的预览视图提供者，就像`iOS`应用，可以预览应用的主窗口。

![section 1 step5](/tutorials/framework_integration/images/creating_a_macOS_App_seciont1_step5.png?width=50pc)

### 第二节 共享数据和资源

下一步，复用来自`iOS`应用的模型和资源文件到`macOS`应用中。

![section 2](/tutorials/framework_integration/images/creating_a_macOS_App_seciont2.png?width=30pc)

**步骤1** 在项目导航器中，打开`Landmarks`文件夹并选中所有`Models`和`Resources`文件夹。`landmarkData.json`文件包含在教程的启动项目，里面包含了一个新的`description`字段，这是之前的教程中所没有的内容。

![section 2 step1](/tutorials/framework_integration/images/creating_a_macOS_App_seciont2_step1.png?width=20pc)

**步骤2** 在文件检查器中，为选中的文件设置目标成员关系为`MacLandmarks`项目。应用编译时需要访问这些共享资源。要使用新的`description`字段，需要在`Landmark`结构体中添加一个对应的字段。

![section 2 step2](/tutorials/framework_integration/images/creating_a_macOS_App_seciont2_step2.png?width=40pc)

**步骤3** 打开`Landmark.swift`文件，添加一个`description`属性。因为载入的数据遵循`Codable`协议，只需要确保属性名称和`json`文件中对应的字段名称一致就可以导入新增的字段数据了。

![section 2 step3](/tutorials/framework_integration/images/creating_a_macOS_App_seciont2_step3.png?width=40pc)

### 第三节 创建行视图

对于使用`SwiftUI`来构建视图，一般是自底向上的方式，先创建小视图，然后用小视图组合成更大的视图。下面将创建一个列表的行视图。这个行视图包含地标的名称、地理位置、图片以及一个可选的标记，表标这个地标是否被收藏。

![section 3](/tutorials/framework_integration/images/creating_a_macOS_App_seciont3.png?width=20pc)

**步骤1** 在`MacLandmarks`文件夹下添加一个新的`SwiftUI`视图，命名为`LandmarkRow.swift`。`iOS`应用下也有一个与之同名的文件，重名文件可以通过设置文件的目标成员为适合的App来解决重名的问题。

![section 3 step1](/tutorials/framework_integration/images/creating_a_macOS_App_seciont3_step1.png?width=40pc)

**步骤2** 添加一个`landmark`属性到`LandmarkRow`结构体中，并更新预览视图，让新创建的视图可以在预览视图中展示出来。

![section 3 step2](/tutorials/framework_integration/images/creating_a_macOS_App_seciont3_step2.png?width=30pc)

**步骤3** 用`VStack`包裹的地标图片视图替换占位文本`Text`视图。

![section 3 step3](/tutorials/framework_integration/images/creating_a_macOS_App_seciont3_step3.png?width=50pc)

**步骤4** 添加一个包裹在`VStack`中的描述地标的文本视图。

![section 3 step4](/tutorials/framework_integration/images/creating_a_macOS_App_seciont3_step4.png?width=50pc)

**步骤5** 添加一个收藏指示视图，把它和其它现有的内容用一个`Spacer`分割开。`Spacer`会把已有的视图推向左边，但是收藏指示视图要放在右边，目前是不可见状态，因为此时还没有图片资源与之对应。

![section 3 step5](/tutorials/framework_integration/images/creating_a_macOS_App_seciont3_step5.png?width=50pc)

**步骤6** 从`Resources`文件夹下拖动`star-filled.pdf`和`star-empty.pdf`文件到`macOS`应用的`Assets.xcassets`文件内。

![section 3 step6](/tutorials/framework_integration/images/creating_a_macOS_App_seciont3_step6.png?width=50pc)

**步骤7** 给行视图添加内边距，现在就能够把黄色的收藏标记显示出来了。行视图的内边距可以提高可读性，当把多个行视图集合到列表视图内时，这一点就能很明显的看出来了。

![section 3 step7](/tutorials/framework_integration/images/creating_a_macOS_App_seciont3_step7.png?width=50pc)

### 第四节 把行视图组合进列表视图中

使用上一节创建的行视图，创建一个列表视图，用来展示用户了解的所有地标。当`showFavoritesOnly`属性为真时，列表中只展示那些被用户收藏的地标。

![section 4](/tutorials/framework_integration/images/creating_a_macOS_App_seciont4.png?width=30pc)

**步骤1** 添加一个名为`LandmarkList.swift`的新的`SwiftUI`视图

![section 4 step1](/tutorials/framework_integration/images/creating_a_macOS_App_seciont4_step1.png?width=50pc)

**步骤2** 添加`userData`属性作为环境注入对象，并更新预览视图。这样就可以让视图访问全局用户地标数据。

![section 4 step2](/tutorials/framework_integration/images/creating_a_macOS_App_seciont4_step2.png?width=50pc)

**步骤3** 创建一个列表，行使用使用`landmarkRow`定义的类型。

![section 4 step3](/tutorials/framework_integration/images/creating_a_macOS_App_seciont4_step3.png?width=50pc)

**步骤4** 让列表的行可以被用户选中，需要给列表提供一个绑定可选地标成员的关系，并用地标数据自己来标识行。之后会使用这个被选中的地标来展示地标详情页。

![section 4 step4](/tutorials/framework_integration/images/creating_a_macOS_App_seciont4_step4.png?width=50pc)

**步骤5** 根据`showFavoritesOnly`的状态值以及地标数据是否被用户标记为收藏来决定列表中展示的行的内容。

![section 4 step5](/tutorials/framework_integration/images/creating_a_macOS_App_seciont4_step5.png?width=50pc)

### 第五节 创建过滤器来管理列表的展示内容

因为用户可以标记地标为收藏状态，所以需要提供方式让用户只看到自己收藏过的地标。现在要创建一个过滤器视图，使用`Toggle`控件给用户提供一个勾选设置，让用户选择是否过滤列表中的非收藏地标，只展示收藏过的地标。

为了让用户可以快速筛选出自己喜欢的地标，这里会添加一下选择器弹出按钮，让用户可以根据地标的不同类别，选择过滤展示自己收藏的地标数据。

![section 5](/tutorials/framework_integration/images/creating_a_macOS_App_seciont5.png?width=20pc)

**步骤1** 添加一个名为`Filter.swift`的`SwiftUI`视图。

**步骤2** 添加`userData`属性作为环境注入对象，并更新预览视图。

**步骤3** 用`Toggle`控件来展示布尔值`showFavoritesOnly`属性，并给它一个恰当的标签文本。

![section 5 step3](/tutorials/framework_integration/images/creating_a_macOS_App_seciont5_step3.png?width=50pc)

当用户选择勾选框时，列表视图也会跟着一起刷新展示，因为它们都绑定了同一上环境注入对象中的值`showFavoritesOnly`。除此之外，还可以使用地标的类别来定义额外的过滤条件。

**步骤4** 创建`FilterType`类型，用来存放地标的类别以及类别对应的名称。确保`FilterType`遵循`Hashable`协议，这样`FilterType`就可以被用在选择器。`FilterType`中的名称属性可以展示在选择器中，让用户选择过滤哪一种类别的地标。

![section 5 step4](/tutorials/framework_integration/images/creating_a_macOS_App_seciont5_step4.png?width=40pc)

**步骤5** 定义一个`all`类型用来表示不使用任何地标类别过滤。这个额外的过滤类别要求`FilterType`有一个特殊的初始构建器，用来处理类别为空的初始化场景。

![section 5 step5](/tutorials/framework_integration/images/creating_a_macOS_App_seciont5_step5.png?width=30pc)

遵循`CaseIterable`和`Identifiable`协议，让`FilterType`可以做为`ForEach`的初始化入参，之后就可以使用这个`FilterType`类型了。

**步骤6** 遵循`CaseIterable`协议，给列表提供所有可能的类别。

![section 5 step6](/tutorials/framework_integration/images/creating_a_macOS_App_seciont5_step6.png?width=40pc)

**步骤7** 遵循`Identifiable`协议并定义一个`id`属性。

![section 5 step7](/tutorials/framework_integration/images/creating_a_macOS_App_seciont5_step7.png?width=40pc)

**步骤8** 在`Filter.swift`中，给`Filter`视图添加一个选择器，选择器使用一个`FilterType`的绑定用来记录用户选择，`FilterType`的名称用来表示用户在选择器菜单中的选项。使用`FilterType`的绑定关系可以让父视图观察到用户的选择。

![section 5 step8](/tutorials/framework_integration/images/creating_a_macOS_App_seciont5_step8.png?width=50pc)

**步骤9** 返回到列表视图，添加`FilterType`绑定关系。对于过滤器视图来说，这允许它和父视图共享变量`filter`。

**步骤10** 更新列表行的创建逻辑，让它包含类别过滤功能。查找那些与用户选中的过滤类别相匹配的地标类别，或者任何用户选择的特色类别地标。

![section 5 step10](/tutorials/framework_integration/images/creating_a_macOS_App_seciont5_step10.png?width=50pc)

### 第六节 组合列表视图与过滤器视图

创建一个组列过滤器和列表的视图。为过滤器提供新的状态信息，同时绑定地标选择到主视图的父视图上。

![section 6](/tutorials/framework_integration/images/creating_a_macOS_App_seciont6.png?width=20pc)

**步骤1** 项目中添加一个新的`SwiftUI`视图，命名为`NavigationPrimary.swift`。

**步骤2** 声明一个`FilterType`状态。这个状态会被绑定到过滤器和列表视图中。

![section 6 step2](/tutorials/framework_integration/images/creating_a_macOS_App_seciont6_step2.png?width=50pc)

**步骤3** 添加过滤器视图并绑定`FilterType`状态。现在预览是失败的，因为过滤器依赖环境中的用户数据，下一步会处理这块儿。

![section 6 step3](/tutorials/framework_integration/images/creating_a_macOS_App_seciont6_step3.png?width=50pc)

**步骤4** 注入用户数据对角到环境中。导航主视图是不直接需要用户数据的，但它的子视图需要。为了可以进行预览，把用户数据作为环境对象注入到导航主视图中。

![section 6 step4](/tutorials/framework_integration/images/creating_a_macOS_App_seciont6_step4.png?width=50pc)

**步骤5** 添加一个绑定到当前选中地标的关系。

**步骤6** 添加地标列表视图，并把它绑定到选中的地标和过滤器状态上。预览视图中选中第二个选项，因为输入数据是`landmarkData[1]`作为用户选中的地标输入数据。

![section 6 step6](/tutorials/framework_integration/images/creating_a_macOS_App_seciont6_step6.png?width=50pc)

**步骤7** 限制导航视图的宽度，防止用户让它变的太宽或太窄。

![section 6 step7](/tutorials/framework_integration/images/creating_a_macOS_App_seciont6_step7.png?width=50pc)

### 第七节 复用`CircleImage`

有时只需要经过稍微修改，就可以跨平台复用一些视图。当构建`macOS`平台的地标详情页视图时，会复用`iOS`版地标应用中的`CircleImage`视图。为了适配`macOS`平台下的不同布局要求，会添加一个参数来控件阴影半径。

![section 7](/tutorials/framework_integration/images/creating_a_macOS_App_seciont7.png?width=20pc)

**步骤1** 在项目导航栏中选中`Landmarks` -> `Supporting Views`并选择`CircleImage.swift`文件。

![section 7 step1](/tutorials/framework_integration/images/creating_a_macOS_App_seciont7_step1.png?width=50pc)

**步骤2** 把`CircleImage.swift`文件添加到时`MacLandmarks`编译目标。

![section 7 step2](/tutorials/framework_integration/images/creating_a_macOS_App_seciont7_step2.png?width=50pc)

**步骤3** 在`CircleImage.swift`文件中，修改结构体，使用新的阴影半径参数。通过给新参数提供默认值，可以确保`iOS`和`watchOS`平台的应用都能与原来保持一致，同时还能在`macOS`平台上使用。

![section 7 step3](/tutorials/framework_integration/images/creating_a_macOS_App_seciont7_step3.png?width=40pc)

### 第八节 为`macOS`扩展`MapView`

类似于`CircleImage`，这里要在`macOS`上复用`MapView`。然而，`MapView`要做更大的改动，因为`MapView`使用的是`MapKit`依赖于`UIKit`框架。在`macOS`平台上使用`MapKit`需要依赖于`AppKit`框架，所以需要添加编译器指令，让编译过程在`macOS`目标上进行正确的依赖。

![section 8](/tutorials/framework_integration/images/creating_a_macOS_App_seciont8.png?width=20pc)

**步骤1** 在项目导航器中，选择`Landmarks` -> `Supporting Views`，选中`MapView.swift`文件。

**步骤2** 把`MapView.swift`文件添加到`MacLandmarks`编译目标上。此时`Xcode`会报错，因为`MapView`使用了`UIViewRepresentable`协议，这个协议在`macOS SDK`里是没有的。下面的步骤中，会使用`NSViewRepresentable`协议来扩展`MapView`，让它能在`macOS`平台上使用。

![section 8 step2](/tutorials/framework_integration/images/creating_a_macOS_App_seciont8_step2.png?width=50pc)

**步骤3** 插入条件编译指令，用来指定特定平台行为。用条件编译的两个条件分支把协议`UIViewRepresentable`和`NSViewRepresentable`协议的遵循分开。

**步骤4** 使用条件编译，把在`iOS`平台上要实现的协议`UIViewRepresentable`及协议方法`makeUIView`、`updateUIView`放在`MapView`的扩展实现中，这样就把`MapKit`的平台依赖性解耦了。

**步骤5** 添加在`macOS`平台上的`NSViewRepresentable`协议遵循。与`UIViewRepresentable`协议一样，`NSViewRepresentable`协议的实现也可以使用主类中的方法。

![section 8 step5](/tutorials/framework_integration/images/creating_a_macOS_App_seciont8_step5.png?width=50pc)

### 第九节 构建详情视图

详情视图展示用户选中的地标信息。创建一个类似`iOS`平台地标应用的地标详情视图，不同之处在于，`macOS`平台有不同的数据表示方法，这就需要针对`macOS`平台对详情视图作一些裁剪，复用一些之前调整过的视图。

![section 9](/tutorials/framework_integration/images/creating_a_macOS_App_seciont9.png?width=30pc)

**步骤1** 项目中添加一个新的视图，命名为`NavigationDetail.swift`，并添加一个`landmark`属性。初始化详情视图时会使用`landmark`属性来指定详情页展示的地标信息。

**步骤2** 在`NavigationDetail.swift`内部创建一个滚动视图，滚动视图中包含一个`VStack`，`VStack`中又包含一个`HStack`,`HStack`中展示关于地标的图片`CircleImage`及`Text`地标文本信息。通过设置`VStack`的最大最小宽度，确保展示的内容保持一定的宽度，以适合用户阅读。跨平台复用视图是非常方便的，定制一下`CircleImage`视图，以满足当前的布局要求。

![section 9 step2](/tutorials/framework_integration/images/creating_a_macOS_App_seciont9_step2.png?width=50pc)

**步骤3** 把输入的图片变为可缩放，并设置图片按视图大小展示，这样可以让`CircleImage`视图的大小与`Text`块文本的大小看上去比较匹配。这种修改方法不需要调整`CircleImage`的内部实现。

![section 9 step3](/tutorials/framework_integration/images/creating_a_macOS_App_seciont9_step3.png?width=50pc)

**步骤4** 调整阴影半径，以匹配更小的图片。这个修改依赖之前对`CircleImage`视图所作的参数化改造。

![section 9 step4](/tutorials/framework_integration/images/creating_a_macOS_App_seciont9_step4.png?width=50pc)

用户使用按钮标记一个地标是否被收藏。为了让这个动作生效，需要访问用户数据中的对应变量。

**步骤5** 添加用户数据对应的环境对象，并创建一个基于当前选中地标的存储属性`landmarkIndex`

![section 9 step5](/tutorials/framework_integration/images/creating_a_macOS_App_seciont9_step5.png?width=50pc)

**步骤6** 添加一个按钮，水平方式对齐地标名称，使用星星图标，并在点击时可以切换用户对这个地标的收藏状态。当用户修改地标数据时，在用户数据中查找被修改的地标数据，并用最新的数据更新原来的数据，让数据保持最新状态。

![section 9 step6](/tutorials/framework_integration/images/creating_a_macOS_App_seciont9_step6.png?width=50pc)

**步骤7** 在分割区载下再添加一个地标的信息，对应数据中新增的字段`description`。

![section 9 step7](/tutorials/framework_integration/images/creating_a_macOS_App_seciont9_step7.png?width=50pc)

预览视图中标题块会被挤到左边，因为描述内容比较多，把水平方向的宽度撑满了。

**步骤8** 在详情视图顶部插入地图，调整地图的偏移，让地图和其它内容有一定区域的重叠。地图占满视图全宽，因此会把详情文本挤到预览视图的底部看不到的位置，但它实际上是存在的。

![section 9 step8](/tutorials/framework_integration/images/creating_a_macOS_App_seciont9_step8.png?width=50pc)

**步骤9** 导入`MapKit`并添加一个`Open in Maps`的按钮，当按钮被点击时，打开地图应用并定位到地标位置。

![section 9 step9](/tutorials/framework_integration/images/creating_a_macOS_App_seciont9_step9.png?width=50pc)

**步骤10** 把`Open in Maps`按钮叠放在地图的右下角。

![section 9 step10](/tutorials/framework_integration/images/creating_a_macOS_App_seciont9_step10.png?width=50pc)

### 第十节 把主视图和详情视图组合起来

已经构建了所有的视图元素，把主视图和详情视图组合起来，共同构成`ContentView`。

![section 10](/tutorials/framework_integration/images/creating_a_macOS_App_seciont10.png?width=30pc)

**步骤1** 在`MacLandmarks`文件夹中，选择`ContentView.swift`文件。

**步骤2** 为选中的地标设置对应的属性`selectedLandmark`，并用`@State`属性标识为状态属性。使用可选类型定义`selectedLandmark`，可以不用为它设置默认值。因此，无论是预览视图还是应用初始化时，都可以不需要用户选中地标进行渲染。

**步骤3** 把用户数据作为环境对象注入。`ContentView`本身不会直接依赖用户数据，但它的子视图需要访问用户数据。对于预览视图来说，为了正常预览和编译成功，`ContentView`需要获取用户数据。

![section 10 step3](/tutorials/framework_integration/images/creating_a_macOS_App_seciont10_step3.png?width=40pc)

**步骤4** 在`AppDelegate.swift`中，为`ContentView`注入环境对象，这样可以让它的子视图访问到用户数据，应用也可以编译成功。

![section 10 step4](/tutorials/framework_integration/images/creating_a_macOS_App_seciont10_step4.png?width=40pc)

**步骤5** 在`ContentView`中添加`NavigationView`作为顶级视图，并设置一个最小尺寸。

![section 10 step5](/tutorials/framework_integration/images/creating_a_macOS_App_seciont10_step5.png?width=50pc)

**步骤6** 添加主视图，展示选中的地标。当用户选中地标列表中的某个地标时，被选中的地标数据就会被赋值到`selectedLandmark`属性上。

![section 10 step6](/tutorials/framework_integration/images/creating_a_macOS_App_seciont10_step6.png?width=50pc)

**步骤7** 添加详情视图，详情视图不接收可选地标数据， 因些传入详情视图的地标数据需要确保不为空。用户选中地标前，地标详情视图不会渲染，这就是为会预览视图没有任何改变，还是和之前一样。

![section 10 step7](/tutorials/framework_integration/images/creating_a_macOS_App_seciont10_step7.png?width=50pc)

**步骤8** 构建并运行应用。尝试改变过滤器的设置，或者点击详情页中的收藏按钮，观察视图内容的变化。

![section 10 step8](/tutorials/framework_integration/creating_a_macOS_App.files/macos-landmark.gif?width=50pc)

### 检查是否理解

**问题1** 在`macOS`平台上，怎么构建一个由可移动分割条分割的主副视图界面？

- [ ] ![problem 1 answer 1](/tutorials/framework_integration/images/creating_a_macOS_App_problem1_answer1.png?width=40pc&classes=border)
- [ ] ![problem 1 answer 2](/tutorials/framework_integration/images/creating_a_macOS_App_problem1_answer2.png?width=40pc&classes=border)
- [X] ![problem 1 answer 3](/tutorials/framework_integration/images/creating_a_macOS_App_problem1_answer3.png?width=40pc&classes=border)

**问题2** 下面哪一个协议是`Picker`控件所使用数据类型必须遵循的？

- [ ] Identifiable
- [X] Hashable
- [ ] Equatable

**问题3** 下面哪一个示例正确的展示了将`selectedLandmark`属性绑定到列表中被选中的地标数据？

- [X] ![problem 3 answer 1](/tutorials/framework_integration/images/creating_a_macOS_App_problem3_answer1.png?width=40pc&classes=border)
- [ ] ![problem 3 answer 2](/tutorials/framework_integration/images/creating_a_macOS_App_problem3_answer2.png?width=40pc&classes=border)
- [ ] ![problem 3 answer 3](/tutorials/framework_integration/images/creating_a_macOS_App_problem3_answer3.png?width=40pc&classes=border)

**问题4** 在`MacLandmarks`应用的完成版中，为什么主视图要维持一个对`selectedLandmark`的绑定关系，但详情视图只是取`selectedLandmark`属性的值？

![problem 4](/tutorials/framework_integration/images/creating_a_macOS_App_problem4.png?width=40pc&classes=border)

- [ ] 因为详情页的入参不能为空，所以不能传入一个绑定关系
- [X] 主视图可以根据用户的输入，改变`selectedLandmark`属性的值，但详情视图只是读取`selectedLandmark`属性的值来进行展示
- [ ] 只可以在一个子视图中绑定一个值，为了避免多个视图更新时发生冲突