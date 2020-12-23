---
title: "处理用户输入"
date: 2020-04-30T10:09:51+08:00
weight: 3
---

在`Landmark`应用中，标记喜爱的地方，过滤地标列表，只显示喜欢的地标。要增加这些特性，首先要在列表上添加一个开关，用来过滤用户喜欢的地标。在地标上添加一个星标按钮，用户可以点击它来标记这个地标为自己喜欢的。

下载工程文件并且跟着下面的教程实践

{{%attachments title="项目文件" style="blue" pattern=".*zip" /%}}

---

### 第一节 标记用户最喜欢的地标

![mark-favorite](/tutorials/swiftui_essentials/images/swiftui-handle-user-input-mark-favorite.png?width=20pc)

给地标列表的每一行添加一个星标用来表示用户是否标记该地标为自己喜欢的

**步骤1** 打开工程项目，在项目导航下选择`LandmarkRow.swift`文件

**步骤2** 在空白占位后面添加一个`if`表达式，`if`表达式判断是否当前地标是用户喜欢的，如果用户标记当前地标为喜欢就显示星标。可以在SwitUI的代码块中使用`if`语句来条件包含视图

**步骤3** 由于系统图片是矢量类型的，可以使用`foregroundColor(_:)`来改变它的颜色。当地标landmark的`isFavorite`属性为真时，星标显示，稍后会讲怎么修改属性值。

![star](/tutorials/swiftui_essentials/images/swiftui-handle-user-input-star.png?width=50pc)

### 第二节 过滤列表

可以定制地标列表，让它只显示用户喜欢的地标，或者显示所有的地标。要实现这个功能，需要给`LandmarkList`视图类型添加一些状态变量。

`状态(State)`是一个值或者一个值的集合，会随着时间而改变，同时会影响视图的内容、行为或布局。在属性前面加上`@State`修饰词就是给视图添加了一个状态值

![state](/tutorials/swiftui_essentials/images/swiftui-handle-user-input-state.png?width=20pc)

**步骤1** 选择`LandmarkList.swift`文件，并给`LandmarkList`添加一个名为`showFavoritesOnly`的状态，初始值设置为`false`

**步骤2** 点击`Resume`按钮或快捷键`Command+Option+P`刷新画布。当对视图进行添加或修改属性等结构性改变时，需要手动刷新画布

**步骤3** 代码中通过检查`showFavoritesOnly`属性和每一个地标的`isFavorite`属性值来过滤地标列表所展示的内容

![state favorite](/tutorials/swiftui_essentials/images/swiftui-handle-user-input-state-favorite.png?width=50pc)

### 第三节 添加控件来切换状态

为了让用户控制地标列表的过滤器，需要添加一个可以修改`showFavoritesOnly`值的控件，传递一个绑定关系给`toggle`控件可以实现

一个绑定关系(`binding`)是对可变状态的引用。当用户点击`toggle`控件，从开到关或从关到开，`toggle`控件会通过绑定关系对应的更新视图的状态

![toggle state](/tutorials/swiftui_essentials/images/swiftui-handle-user-input-toggle-state.png?width=20pc)

**步骤1** 创建一个嵌套的`ForEach`组来把地标数据转换成地标行视图。在一个列表中组合静态和动态视图，或者组合两个甚至多个不同的动态视图组，使用`ForEach`类型动态生成而不是给列表传入数据集合生成列表视图

**步骤2** 添加一个`Toggle`视图作为列表的每一个子视图，传入一个`showFavoritesOnly`的绑定关系。使用`$`前缀来获得一个状态变量或属性的绑定关系

**步骤3** 实时预览模式下，点击`Toggle`控件来验证过滤器的功能

![toggle binding](/tutorials/swiftui_essentials/images/swiftui-handle-user-input-toggle-binding.png?width=50pc)

![live preview](/tutorials/swiftui_essentials/handling_user_input.files/toggle-state-live-preview.gif?width=20pc)

### 第四节 使用可观察对象来存储数据

要实现用户标记哪个地标为自己喜爱的地标这个功能，需要使用可观察对象(`observalble object`)存放地标数据

可观察对象是一种可以绑定到具体SwifUI视图环境中的数据对象。`SwiftUI`可以察觉它影响视图展示的任何变化，并在这种变化发生后及时更新对应视图的展示内容

![observable](/tutorials/swiftui_essentials/images/swiftui-handle-user-input-observable.png?width=10pc)

**步骤1** 创建一个名为`UserData.swift`的文件

**步骤2** 声明一个遵循`ObservableObject`协议的新数据模型，`ObservableObject`协议来自响应式框架`Combine`。SwiftUI可以订阅可观察对象，并在数据发生改变时更新视图的显示内容

**步骤3** 添加存储属性`showFavoritesOnly`和`landmarks`，并赋予初始值。可观察对象需要对外公布内部数据的任何改动，因此订阅此可观察对象的订阅者就可以获得对应的数据改动信息

**步骤4** 给新建的数据模型的每一个属性添加`@Published`属性修饰词

![combine](/tutorials/swiftui_essentials/images/swiftui-handle-user-input-combine.png?width=40pc)

### 第五节 视图中适配数据模型对象

已经创建了`UserData`可观察对象，现在要改造视图，让它使用这个新的数据模型来存储视图内容数据

![model](/tutorials/swiftui_essentials/images/swiftui-handle-user-input-model.png?width=30pc)

**步骤1** 在`LandmarkList.swift`文件中，使用`@EnvironmentObject`修饰的`userData`属性来替换原来的`showFavoritesOnly`状态属性，并对预览视图调用`environmentObject(_:)`修改器。只要`environmentObject(_:)`修改器应用在视图的父视图上，`userData`就能够自动获取它的值。

**步骤2** 替换原来使用`showFavoritesOnly`状态属性的地方，改为使用`userData`中的对应属性。与`@State`修饰的属性一样，也可以使用`$`前缀访问`userData`对象的成员绑定引用

**步骤3** 创建`ForEach`实例时使用`userData.landmarks`做为数据源

![envrionment object](/tutorials/swiftui_essentials/images/swiftui-handle-user-input-list-environment.png?width=50pc)

**步骤4** 在`SceneDelegate.swift`中，对`LandmarkList`视图调用`environmentObject`修改器，这样可以把`UserData`的数据对象绑定到`LandmarkList`视图的环境变量中，子视图可以获得父视图环境中的变量。此时如果在模拟器或者真机上运行应用，也可以正常展示视图内容

![scene delegate](/tutorials/swiftui_essentials/images/swiftui-handle-user-input-scene-delegate-environment.png?width=40pc)

**步骤5** 更新`LandmarkDetail`视图，让它从父视图的环境变量中取要展示的数据。之后在更新地标的用户喜爱状态时，会用到`landmarkIndex`这个变量

![landmark detail environment](/tutorials/swiftui_essentials/images/swiftui-handle-user-input-landmark-detail-environment.png?width=40pc)

**步骤6** 切换到`LandmarkList.swift`文件，并打开实时预览视图去验证所添加的功能是否正常工作

![landmark list environment](/tutorials/swiftui_essentials/handling_user_input.files/landmarklist-environment.gif?width=20pc)

### 第六节 为每一个地标创建一个喜爱按钮

`Landmark`这个应用可以在喜欢和不喜欢的地标列表间进行切换了，但喜欢的地标列表还是硬编码形成的，为了让用户可以自己标记哪个地标是自己喜欢的，需要在地标详情页添加一个标记喜欢的按钮

![favorite button](/tutorials/swiftui_essentials/images/swiftui-handle-user-input-favorite-button.png?width=20pc)

**步骤1** 在`LandmarkDetail.swift`的`HStack`中添加地标名称的`Text`

**步骤2** 在地标名称的`Text`控件旁边添加一个新的按钮控件。使用`if-else`条件语句设置不同的图片显示状态表示这个地标是否被用户标记为喜欢。在`Button`的动作闭包中，使用了`landmarkIndex`去修改`userData`中对应地标的数据。

![favorite star button](/tutorials/swiftui_essentials/images/swiftui-handle-user-input-favorite-star-button.png?width=50pc)

**步骤3** 切换到`landmarkList.swift`，并开启实时预览模式。当从列表页导航进入详情页后，点击喜欢按钮，喜欢的状态会在返回列表页后与列表中对应的地标喜欢状态保持一致，因为列表页和详情页的地标数据使用的是同一份，所以可以在不同页面间保持状态同步。

![star button completed](/tutorials/swiftui_essentials/handling_user_input.files/swiftui-handle-user-input-start-completed.gif?width=20pc)

### 检查是否理解

**问题1** 下列选项哪个可以把数据按视图层级关系传递下去？

- [ ] `@EnvironmentObject`属性
- [X] `environmentObject(_:)`修改器

**问题2** 绑定(binding)的作用是什么？

- [X] 绑定是值和改变值的方法
- [ ] 是一个视图连接在一起的方法，确保连续起来的视图接收同一份数据
- [ ] 是一个临时固化值的方式，目的是在其它视图状态变化时，保持值不改变


