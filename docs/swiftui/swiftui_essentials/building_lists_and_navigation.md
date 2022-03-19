---
title: "创建列表和导航"
date: 2020-04-30T10:09:35+08:00
weight: 2
---

地标详情页视图已经创建完成，我们需要提供一种方式让用户可以查看完整的地标列表，并且可以查看每一个地标的详情

下面会创建一个可以展示任何地标信息的视图，并动态生成一个可滚动列表，用户可以点击列表项去查看地标的详细信息。优化视图显示时，可以使用Xcode画布来渲染多个不同设备大小下的预览视图。

下载下面的工程文件，并跟着教程一步步学习构建列表和视图间导航

{{%attachments title="项目文件" style="blue" pattern=".*zip" /%}}

---

### 第一节 了解样本数据

前面的教程中，自定义视图所展示的信息都直接被写死在代码中，这篇教程中会学习给自定义视图传入样本数据进行展示

![swiftui-building-list](/tutorials/swiftui_essentials/images/swiftui-building-list.png?width=20pc)

**步骤1** 打开项目导航器，选择**Models->Landmark.swift**文件，这个文件中声明了需要在应用中展示一个地标所需要信息的结构化名称，并通过导入`landmarkData.json`文件中的数据，生成一个地标信息数组。

![building list model](/tutorials/swiftui_essentials/images/swiftui-building-list-landmark-model.png?width=50pc)

**步骤2** 在项目导航器中选择**Resources->landmarkData.json**，在后面的教程中我们都会使用这个样本数据文件

![building list sample data](/tutorials/swiftui_essentials/images/swiftui-building-list-sampe-data.png?width=50pc)

**步骤3** 注意，之前的`ContentView`视图，已经被改名为`LandmarkDetail`了，在本教程和后面的教程中，还会创建一些其它的视图

![landmark detail](/tutorials/swiftui_essentials/images/swiftui-building-list-landmark-detail.png?width=20pc)

### 第二节 创建行视图

本教程中创建的第一个视图就是用来显示每个地标的行视图，行视图把地标的相关信息存储在一个属性中，一行就可以代表一个地标，稍后就会把这些行组合成为一个列表。

![swiftui-building-list-landmark-row](/tutorials/swiftui_essentials/images/swiftui-building-list-landmark-row.png?width=20pc)

**步骤1** 创建一个名为`LandmarkRow.swift`的SwiftUI视图

![landmark row create](/tutorials/swiftui_essentials/images/swiftui-building-list-landmark-row-create.png?width=20pc)

**步骤2** 如果预览视图没有出现，可以选择菜单**编辑器->画布**，打开画布，并点击**Resume**进行预览，或者使用**Command+Option+Enter**快捷键调出画面，再使用**Command+Option+P**快捷键开始预览模式

**步骤3** 添加`landmark`属性做为`LandmarkRow`视图的一个存储属性。当添加`landmark`属性后，预览视图可能会停止工作，因为`LandmarkRow`视图初始化时需要有一个`landmark`实例。要想修复预览视图，需要修改`Preview Provider`

**步骤4** 在`LandmarkRow_Previews`的静态属性`previews`中给`LandmarkRow`初始化器中传入`landmark`参数，这个参数使用`landmarkData`数组的第一个元素。预览视图当前显示`Hello, World`

![landmark row layout](/tutorials/swiftui_essentials/images/swiftui-building-list-landmark-row-layout.png?width=20pc)

**步骤5** 在一个`HStack`中嵌入一个`Text`

**步骤6** 修改这个`Text`，让它使用`landmark`属性的`name`字段

**步骤7** 在`Text`视图前面添加一个图片视图，在`Text`视图后面添加`Spacer`视图

![landmark layout 1](/tutorials/swiftui_essentials/images/swiftui-building-list-landmark-row-layout-1.png?width=50pc)

### 第三节 自定义行预览

Xcode的画布会自动识别当前代码编辑器中遵循`PreviewProvider`协议的类型，并将它们渲染并展示在画面上。一个视图预览提供者(preview provider)返回一个或多个视图，这些视图可以配置不同的大小和设备型号。

可以定制从`preview provider`中返回的视图被渲染在何种场景下。

![row preivew](/tutorials/swiftui_essentials/images/swiftui-building-list-row-preview.png?width=20pc)

**步骤1** 在`LandmarkRow_Previews`中，把`landmark`参数更新为`landmarkData`数组的第二个元素，预览视图会立即刷新反映第二个元素的渲染情况

![preivew row 2](/tutorials/swiftui_essentials/images/swiftui-building-list-preview-row-2.png?width=40pc)

**步骤2** 使用`previewLayout(_:)`修改器设置一个行视图在列表中显示的尺寸大小。可以使用`Group`的方式，返回多个不同场景下的预览视图

![preview layout size](/tutorials/swiftui_essentials/images/swiftui-building-list-preivew-layout-size.png?width=40pc)

**步骤3** 把预览的行视图包裹在`Group`中，把之前的第一个行视图也加进去。`Group`是一个容器，它可以把视图内容组织起来，Xcode会把`Group`内的每个子视图当作画布内一个单独的预览视图处理

![preview group size](/tutorials/swiftui_essentials/images/swiftui-building-list-preivew-group.png?width=40pc)

**步骤4** 为了简化代码，可以把`previewLayout(_:)`这个修改器应用到外层的`Group`上，`Group`的每一个子视图会继承自己所处环境的配置。对`preivew provider`的修改只会影响预览画布的表现，对实际的应用不会产生影响。

![preview group coniguration](/tutorials/swiftui_essentials/images/swiftui-building-list-preview-layout-group-configuration.png?width=40pc)

### 第四节 创建地标列表

使用SwiftUI列表类型可以展示平台相关的列表视图。列表的元素可以是静态的，类似于栈内部的子视图，也可以是动态生成的视图，也可以混合动态和静态的视图。

![landmark list](/tutorials/swiftui_essentials/images/swiftui-building-list-landmark-list.png?width=30pc)

**步骤1** 创建SwiftUI视图，命名为`LandmarkList.swift`

**步骤2** 用`List`替换默认创建的`Text`，并将前两个`LandmarkRow`实例做为列表的子元素，预览视图中会以列表的形式展示出两个地标

![landmark list file create](/tutorials/swiftui_essentials/images/swiftui-building-list-landmark-list-file.png?width=20pc)

![landmark list landmark list tow rows](/tutorials/swiftui_essentials/images/swiftui-building-list-landmark-list-two-rows.png?width=50pc)

### 第五节 创建动态列表

除了单独列出列表中的每个元素外，列表还可以从一个集合中动态的生成。

![landmark list dynamic](/tutorials/swiftui_essentials/images/swiftui-building-list-landmark-list-dynamic.png?width=20pc)

创建列表时可以传入一个集合数据和一个闭包，闭包会针对每一个数据元素返回一个视图，这个视图就是列表的行视图。

**步骤1** 从列表中移除两个静态指定的行视图，给列表初始化器传入`landmarkData`数据，列表要配合可辨别的数据类型使用。想让数据变成可辨别的数据类型有两种方法:

1. 传入一个`keypath`指定数据中哪一个字段用来唯一标识这个数据元素。

2. 让数据遵循`Identifiable`协议

**步骤2** 在闭包中返回一个`LandmarkRow`视图，`List`初始化器中指定数据集合`landmarkData`和唯一标识符**keypath:**`\.id`，这样列表就会动态生成，如下图所示

![keypath identifier list data](/tutorials/swiftui_essentials/images/swiftui-building-list-landmark-list-keypath-data.png?width=50pc)

**步骤3** 切换到文件`Landmark.swfit`，声明`Landmark`类型遵循`Identifiable`协议，因为`Landmark`类型已经定义了`id`属性，正好满足`Identifiable`协议，所以不需要添加其它代码

![identifiable data](/tutorials/swiftui_essentials/images/swiftui-building-list-landmark-data-identifiable.png?width=50pc)

**步骤4** 现在切换回文件`LandmarkList.swift`，移除keypath`\.id`，因为`landmarkData`数据集合的元素已经遵循了`Identifiable`协议，所以在列表初始化器中可以直接使用，不需要手动标明数据的唯一标识符了

![identifiable list](/tutorials/swiftui_essentials/images/swiftui-building-list-landmark-list-identifiable-data.png?width=50pc)

### 第六节 设置从列表页到详情页的页面导航

地标列表可以正常渲染展示，但是列表的元素点击后没有反应，跳转不到地标详情页。现在就要给列表添加导航能力，把列表视图嵌套到`NavigationView`视图中，然后把列表的每一个行视图嵌套进`NavigationLink`视图中，就可以建立起从地标列表视图到地标详情页的跳转。

![landmark list to detail](/tutorials/swiftui_essentials/images/swiftui-building-list-landmark-list-to-detail.png?width=20pc)

**步骤1** 把动态生成的列表视图嵌套进一个`NavigationView`视图中

![embed in navigation view](/tutorials/swiftui_essentials/images/swifui-building-list-embed-in-navigation-view.png?width=20pc)

**步骤2** 调用`navigationBarTitle(_:)`修改器设置地标列表显示时的导航条标题

![landmark list navigation view](/tutorials/swiftui_essentials/images/swiftui-building-list-navigation-view-title.png?width=50pc)

**步骤3** 在列表的闭包中，将每一个行元素包裹在`NavigationLink`中返回，并指定`LandmarkDetail`视图为目标视图

![navigation link](/tutorials/swiftui_essentials/images/swiftui-building-list-navigationlink.png?width=30pc)

**步骤4** 切换到实时预览模式下可以直接点击地标列表的任意一行，现在就可以跳转到地标详情页了。

![list navigation](/tutorials/swiftui_essentials/building_lists_and_navigation.files/swifui-building-list-navigation.gif?width=50pc)

### 第七节 子视图传入数据

`LandmarkDetail`视图目前还是使用写死的数据进行展示，与`LandmarkRow`视图一样，`LandmarkDetail`视图及它内部的子视图也需要传入`landmark`数据，并使用它来进行实际的展示

从`LandmarkDetail`的子视图(`CircleImage`、`MapView`)开始，需要把它们都改造成为使用传入的数据进行展示，而不是在布局代码中写死数据展示

![pass data](/tutorials/swiftui_essentials/images/swiftui-building-list-pass-data.png?width=15pc)

**步骤1** 在`CircleImage.swift`文件中，添加一个存储属性，命名为`image`。这是一种在构建SwiftUI视图中很常用的模式，常常会包裹或封装一些属性修改器。

![circle image data](/tutorials/swiftui_essentials/images/swiftui-building-list-circle-image-data.png?width=20pc)

**步骤2** 更新`CirleImage`的预览结构体，并传入`Turtle Rock`这个图片进行预览

![circle image preview](/tutorials/swiftui_essentials/images/swiftui-building-list-circle-image-preview.png?width=20pc)

**步骤3** 在`MapView.swift`中添加一个`coordinate`属性，并使用这个属性来替换写死的经纬度坐标

![map view data](/tutorials/swiftui_essentials/images/swiftui-building-list-map-view-data.png?width=20pc)

**步骤4** 更新`MapView`的预览结构体，并传入每一个地标的经纬度数据

![map view preview](/tutorials/swiftui_essentials/images/swiftui-building-list-map-view-preview.png?width=20pc)

**步骤5** 在`LandmarkDetail.swift`中添加`landmark`属性。

**步骤6** 更新`LandmarkDetail`预览结构体，并传入第一个地标的数据

**步骤7** 把对应子视图的数据传入

![landmark detail](/tutorials/swiftui_essentials/images/swiftui_building-list-landmark-detail-data.png?width=20pc)

**步骤8** 最后调用`navigationBarTitle(_:displayMode:)`修改器为地标详情页展示时在导航条上设置一个标题

![landmark detail preview](/tutorials/swiftui_essentials/images/swiftui-building-list-landmark-detail-preview.png?width=20pc)

**步骤9** 在`SceneDelegate.swift`中把应用的根视图替换为`LandmarkList`。应用在模拟器中独立启动时使用`SceneDelegate`的根视图做为第一个展示的视图

![scene delegate root view](/tutorials/swiftui_essentials/images/swiftui-buidling-list-sencedelegate-rootview.png?width=50pc)

**步骤10** 在`LandmarkList.swift`中，传入当前行的地标数据到地标详情页`LandmarkDetail`

![landmark list data](/tutorials/swiftui_essentials/images/swiftui-building-list-landmark-list-data.png?width=50pc)

**步骤11** 切换到实时预览模式下去查看从地标列表页对应的行跳转到对应地标详情页是否正常

![landmark list preview](/tutorials/swiftui_essentials/building_lists_and_navigation.files/swiftui-building-list-landmark-list-preview.gif?width=25pc)

### 第八节 动态生成预览视图

![dynamic preivew](/tutorials/swiftui_essentials/images/swiftui-building-list-preview-dynamic.png?width=20pc)

接下来要在不同尺寸设备上展示不同的预览视图，默认情况下，预览视图会选择当前`Scheme`选中的设备尺寸进行渲染，可以使用`previewDevice(_:)`修改器来改变预览视图的设备

**步骤1** 改变当前预览列表，让它渲染在`iPhone SE`设备上。可以使用`Xcode Scheme菜单`上的设备名称来指定渲染设备。

![iPhone SE Preview](/tutorials/swiftui_essentials/images/swifui-building-list-preview-on-iphonese.png?width=50pc)

**步骤2** 在列表的预览视图中，还可以把`LandmarkList`嵌套进入`ForEach`实例中，使用设备数组名作为数据。`ForEach`运算作用在集合类型的数据上，就和列表使用集合类型数据一样，可以在子视图使用的任何场景下使用`ForEach`，例如：`stack`、`list`、`group`等。当元素数据是简单值类型时(例如字符串类型)，可以使用`\.self`作为`keypath`去标识

![preiview multiple device](/tutorials/swiftui_essentials/images/swiftui-building-list-preivew-multiple-device.png?width=50pc)

**步骤3** 使用`previewDisplayName(_:)`修改器可以给预览视图添加设备标签

**步骤4** 可以在画布上多设置几个设备进行预览，比较不同设备下视图的展示情况

![preivew multiple devices](/tutorials/swiftui_essentials/images/swiftui-building-list-preview-muldevices.png?width=50pc)

### 检查是否理解

**问题1** 除了`List`外，下面哪种类型可以从集合数据中展示动态列表视图

- [ ] `Group`
- [X] `ForEach`
- [ ] `UITableView`

**问题2** 可以从遵循了`Identifiable`协议的集合数据创建列表视图。但如果集合数据不遵循`Identifiable`协议，还有什么办法可以创建列表视图？

- [ ] 在集合数据上调用`map(_:)`方法
- [ ] 在集合数据上调用`sorted(by:)`方法
- [X] 给`List(_:id:)`类型传入集合数据的同时，使用`keypath`指定一个唯一标识符字段

**问题3** 使用什么类型才能让列表的行实现点击跳转到其它视图页面？

- [X] `NavigationLink`
- [ ] `UITableViewDelegate`
- [ ] `NavigationView`

**问题4** 下面哪种方式不是用来设置预览设备的？

- [ ] 改变活动`scheme`中选中的模拟器
- [X] 在画面设置中设置一个不同的预览设备
- [ ] 使用`previewDevice(_:)`指定一个或多个预览设备
- [ ] 连接开发机并点击设备预览按钮
