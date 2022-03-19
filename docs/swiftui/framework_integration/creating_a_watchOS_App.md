---
title: "创建watchOS应用"
date: 2020-07-19T21:44:35+08:00
weight: 2
---

这篇教程让我们可以应用之前所学到的`SwiftUI`知识，把`Landmarks`应用从`iOS`平台迁移到`watchOS`平台上。在拷贝可以共用的数据和视图文件之前，需要先给项目中添加一个对应`watchOS`的`Target`编译目标，`assets`资源保持原状，只需要调整`SwiftUI`视图以适应在`watchOS`平台上展示就可以了。

按照下面的步骤构建工程，或者下载完成后的项目文件学习。

{{%attachments title="项目文件" style="blue" pattern=".*zip" /%}}

---

### 第一节 添加一个`watchOS`编译目标

要创建一个`watchOS`应用，第一步是给项目添加一个对应`watchOS`平台的编译目标。`Xcode`在新增`Target`的同时，添加一个文件组和相应的文件到工程中，同时还会新增编译运行方案，指定应用要运行的平台或模拟器。

![section 1](/tutorials/framework_integration/images/create_a_watchOS_app_section1.png?width=30pc)

**步骤1** 选择菜单`File`->`New`->`Target`。当模板列表出现后，在`watchOS`选项卡下选择`Watch App for iOS App`模板，并点击下一步(Next)。

![section 1 step 1 1](/tutorials/framework_integration/images/create_a_watchOS_app_section1_step1_1.png?width=30pc)

![section 1 step 1 2](/tutorials/framework_integration/images/create_a_watchOS_app_section1_step1_2.png?width=30pc)

选择这个模板会添加一个新的`watchOS`应用到工程中，嵌入到`iOS`应用平级。

**步骤2** 在模板创建表中，输入`WatchLandmarks`作为产品名称，设置语言为`Swift`，用户界面为`SwiftUI`实现方式，并勾选通知应用场景，最后点击完成(Finish)。

![section 1 step 2 1](/tutorials/framework_integration/images/create_a_watchOS_app_section1_step2_1.png?width=30pc)

![section 2 step 2 2](/tutorials/framework_integration/images/create_a_watchOS_app_section1_step2_2.png?width=30pc)

**步骤3** 如果`Xcode`提示激活`watchOS`平台编译运行方案，点击激活(Activate)。这会把编译运行方案从iOS平台切换到`watchOS`平台上来。

**步骤4** 在扩展`Target`**WatchLandmarks Extensions**的通用(`General`)选项卡下勾选`Supports Running Without iOS App Installation`，尽量创建一个可以独立于`iOS宿主`运行的`watchOS`应用。

![section 2 step 4](/tutorials/framework_integration/images/create_a_watchOS_app_section1_step4.png?width=40pc)

### 第二节 在`Target`间共享文件

现在`watchOS`平台的编译目标(Target)已经创建好，为了避免重复工作，可以复用一些之前在`iOS`项目中的资源。地标的数据模型文件可以复用，一些资源文件以及一些两个平台下不需要修改就可以展示的视图文件也可复用。

![section 2](/tutorials/framework_integration/images/create_a_watchOS_app_section2.png?width=20pc)

**步骤1** 在项目导航器中，按下`Command+鼠标左键`的同时，选中文件：`LandmarkRow.swift`、`Landmark.swift`、`UserData.swift`、`Data.swift`、`Profile.swift`、`Hike.swift`和`CircleImage.swift`。


![section 2 step 1](/tutorials/framework_integration/images/create_a_watchOS_app_section2_step1.png?width=20pc)

其中`Landmark.swift`、`UserData.swift`、`Data.swift`、`Profile.swift`、`Hike.swift`这几个文件定义了应用的数据模型。我们不会使用这些文件中的所有内容，但为了编译通过需要这些文件。`LandmarkRow.swift`、
`CircleImage.swift`这两个文件是不需要任何修改就可以在`watchOS`平台上展示的视图。要注意`Data.swift`文件中是否导入`ImageIO`模块，如果没有`import ImageIO`这一句，编译时会有报错。

**步骤2** 上一步选中需要文件的情况下，在文件检查器中，勾选`WatchLandmarks Extension`，让之前选中的文件也变成`WatchLandmarks Extension`编译时用到的文件。

![section 2 step 2](/tutorials/framework_integration/images/create_a_watchOS_app_section2_step2.png?width=50pc)

**步骤3** 在项目导航器中，选中`Landmarks`文件组下的`Assets.xcassets`文件，并在文件检查器中把它添加到`WatchLandmarks`编译运行目标中。这里的编译运行目标和上一步选中的编译运行目标不相同。`WatchLandmarks Extension`是用来放置应用的代码，`WatchLandmarks`是用来管理`storyboard`、图标以及相关资源的。

![section 2 step 3](/tutorials/framework_integration/images/create_a_watchOS_app_section2_step3.png?width=50pc)

**步骤4** 在项目导航器中，选择`Resources`文件夹下的所有文件，并在文件检查器中把它们添加到`WatchLandmarks Extension`编译目标下。

![section 2 step 4](/tutorials/framework_integration/images/create_a_watchOS_app_section2_step4.png?width=50pc)

### 第三节 创建详情视图

iOS编译目标下的资源可以在手表应用下使用，但我们需要创建一个专门适配手表尺寸的地标详情页来展示地标的具体信息。为了测试视图是否能适配手表展示，需要分别为最大尺寸和最小尺寸手表创建预览视图，并根据情况适当的调整圆形视图的布局来适应手表的界面大小。

![section 3](/tutorials/framework_integration/images/create_a_watchOS_app_section3.png?width=20pc)

**步骤1** 在项目导航器中，点击`WatchLandmarks Extension`文件夹左边的三角形展开箭头，查看文件夹下的具体内容，并添加一个名为`WatchLandmarkDetail`的`SwiftUI`视图。

![section 3 step 1](/tutorials/framework_integration/images/create_a_watchOS_app_section3_step1.png?width=50pc)

**步骤2** 在结构体`WatchLandmarkDetail`结构体中添加`userData`、`landmark`和`landmarkIndex`属性。这些新增和属性与在处理用户输入时在`LandmarkDetail`中添加的属性是对等的。

![section 3 step 2](/tutorials/framework_integration/images/create_a_watchOS_app_section3_step2.png?width=50pc)

添加完属性后发现，Xcode会报缺少参数的错误。要修复这个错误有两种方式，一种是为属性提供一个默认值，一种是给视图的属性传入对应的值。

**步骤3** 在预览视图中，创建一个用户数据的实例，并给`WatchLandmarkDetail`结构体的初始化器中传入一个地标对象作为参数。这里需要把用户数据设置为视图的环境对象。

![section 3 step 3](/tutorials/framework_integration/images/create_a_watchOS_app_section3_step3.png?width=50pc)

**步骤4** 在`WatchLandmarkDetail.swift`文件中，body()方法中返回一个`CircleImage`视图。这里的`CircleImage`是复用iOS项目中的视图，因为创建的图片是可缩放的，可以调用`.scaleToFill()`属性修改器让圆的大小添满整个手表显示屏。

![section 3 step 4](/tutorials/framework_integration/images/create_a_watchOS_app_section3_step4.png?width=50pc)

**步骤5** 创建最大尺寸(44mm)和最小尺寸(38mm)的手表预览视图。通过测试在最大最小尺寸上的展示情况，查看应用UI是否有问题，通常情况下，需要测试所有不同尺寸显示屏上UI展示情况是否符合预期。

![section 3 step 5](/tutorials/framework_integration/images/create_a_watchOS_app_section3_step5.png?width=50pc)

`CircleImage`缩放到高度完全填充显示器高度，但这种情况下适配了高度，宽度却被截断了。为了修复这个截断问题，需要把`CircleImage`视图嵌入到一个`VStack`视图容器中，并作一些调整，让`CircleImage`可以在所有尺寸的手表显示屏上正常展示。

**步骤6** 把`CircleImage`嵌入到`VStack`中，并在图片下方显示地标名称信息。

![section 3 step 6](/tutorials/framework_integration/images/create_a_watchOS_app_section3_step6.png?width=50pc)

很明显，此时的信息一屏展示不下，所以需要把视图内容放入到`ScrollView`中，以获取滚动查看的功能。

**步骤7** 把`VStack`整体嵌入到一个`ScrollView`中，这就让视图获取了滚动查看的能力，但同时也引入了另一个问题：`CircleImage`现在扩展到完全尺寸，把其它元素挤到没有地方显示。所以需要缩放`CircleImage`，让圆形图片和地标名称可以在一屏内同时显示出来。

![section 3 step 7](/tutorials/framework_integration/images/create_a_watchOS_app_section3_step7.png?width=50pc)

**步骤8** 改变`scaleToFill()`为`scaleToFit()`，这就让图片缩放按照显示器的宽度进行适配。

![section 3 step 8](/tutorials/framework_integration/images/create_a_watchOS_app_section3_step8.png?width=40pc)

**步骤9**

为了让地标名称在地标图片的下面展示了来，添加`padding`量来解决。

![section 3 step 9](/tutorials/framework_integration/images/create_a_watchOS_app_section3_step9.png?width=40pc)

**步骤10** 给返回按钮添加一个标题文本。这个返回按钮是看不见的，只有把`LandmarksList`视图加上后，才会出现返回按钮。

![section 3 step 10](/tutorials/framework_integration/images/create_a_watchOS_app_section3_step10.png?width=40pc)

### 第四节 添加一个`watchOS`地图视图

既然已经创建了详情页，现在就可以添加一个地图视图用来显示地标的地理位置了。不像`CircleImage`，这里就不能复用iOS应用的`MapView`了。需要创建一个`WKInterfaceObjectRepresentable`结构体来包装一个`WatchKit`地图。

![section 4](/tutorials/framework_integration/images/create_a_watchOS_app_section4.png?width=20pc)

**步骤1** 在`WatchKit Extension`中添加一个名为`WatchMapView`的视图。

![section 4 step 1](/tutorials/framework_integration/images/create_a_watchOS_app_section4_step1.png?width=50pc)

**步骤2** 让`WatchMapView`遵循`WKInterfaceObjectRepresentable`协议而不是`View`协议。

![section 4 step 2](/tutorials/framework_integration/images/create_a_watchOS_app_section4_step2.png?width=50pc)

目前，因为还没有实现协议`WKInterfaceObjectRepresentable`协议的方法，所以Xcode会报错。

**步骤3** 删除`body()`方法，并用属性`landmark`替换它。当创建一个地图视图时，需要给这个`landmark`属性传入一个值。像在预览视图中传入一个地标实例数据一样。

![section 4 step 3](/tutorials/framework_integration/images/create_a_watchOS_app_section4_step3.png?width=50pc)

**步骤4** 实例协议`WKInterfaceObjectRepresentable`方法`makeWKInterfaceObject(context:)`。这个协议方法创建`WatchKit`地图视图`WatchMapView`。

![section 4 step 4](/tutorials/framework_integration/images/create_a_watchOS_app_section4_step4.png?width=50pc)

**步骤5** 实现协议方法`updateWKInterfaceObject:(_:, context:)`，根据地标的坐标设置地图展示区域，现在项目可以编译通过了。

![section 4 step 5](/tutorials/framework_integration/images/create_a_watchOS_app_section4_step5.png?width=50pc)

**步骤6** 在`WatchLandmarkDetail.swift`文件中把`WatchMapView`添加在`VStack`的底部。在地图上面添加一上分割器。使用`.scaledToFit()`和`.paddding()`来修改地图尺寸，让它更适合屏幕。

![section 4 step 6](/tutorials/framework_integration/images/create_a_watchOS_app_section4_step6.png?width=50pc)

### 第五节 创建跨平台列表视图

对于地标列表，可以复用iOS应用中列表的行元素视图，但不同的平台下需要展示适合平台的详情页。这就需要把`LandmarkList`视图转换成一个通用的列表视图。

![section 5](/tutorials/framework_integration/images/create_a_watchOS_app_section5.png?width=20pc)

**步骤1** 在工具条中，选择`Landmarks`方案，让`Xcode`现在的编译运行方案指向iOS平台。这样做是为了确保对`LandmarkList`视图的重构不会影响原来在`iOS`平台时的表现，这样就可以在不影响`iOS`平台的条件下，把`LandmarkList`重构后应用到`watchOS`应用中。

![section 5 step 1](/tutorials/framework_integration/images/create_a_watchOS_app_section5_step1.png?width=30pc)

**步骤2** 切换到文件`LandmarkList.swift`，把类型声明为范型。改造为范型后，当要创建一个`LandmarkList`结构的实例时，会报范型参数类型无法推断的错误。这将在下面的步骤中解决。

![section 5 step 2](/tutorials/framework_integration/images/create_a_watchOS_app_section5_step2.png?width=50pc)

**步骤3** 添加一个闭包属性，用来创建详情视图。

![section 5 step 3](/tutorials/framework_integration/images/create_a_watchOS_app_section5_step3.png?width=50pc)

**步骤4** 使用`detailViewProducer`属性为地标创建详情视图。当需要创建一个`LandmarkList`时，需要提供一个闭包来创建对应地标的详情视图

![section 5 step 4](/tutorials/framework_integration/images/create_a_watchOS_app_section5_step4.png?width=50pc)

**步骤5** 切换到`Home.swift`文件，在`CategoryHome`结构体的`body`属性中添加一个闭包用来创建详情视图。`Xcode`会根据闭包的返回值类型推断出`LandmarkList`结构体的范型参数类型。

![section 5 step 5](/tutorials/framework_integration/images/create_a_watchOS_app_section5_step5.png?width=50pc)

**步骤6** 在`LandmarkList.swift`文件中，给预览视图添加相似的代码。为了适配不同的设备平台，这里需要使用条件编译，为不同平台编译不同的代码。

![section 5 step 6](/tutorials/framework_integration/images/create_a_watchOS_app_section5_step6.png?width=50pc)

### 第六节 添加地标列表

目前已经将`LandmarkList`视图改造为可以同时兼容`watchOS`和`iOS`两个平台，现在就可以把它应用在`watchOS`平台下的应用中了。

![section 6](/tutorials/framework_integration/images/create_a_watchOS_app_section6.png?width=20pc)

**步骤1** 在文件检查器中，将`LandmarkList.swift`添加为`WatchLandmark Extension`编译目标的成员。现在就可以在`watchOS`的应用中使用`LandmarkList.swift`这份代码文件了。

![section 6 step 1](/tutorials/framework_integration/images/create_a_watchOS_app_section6_step1.png?width=20pc)

这一步其实在**`第五节步骤六`**已经做过了。

**步骤2** 在工具条上，切换编译方案为`Watch Landmarks`

![section 6 step 2](/tutorials/framework_integration/images/create_a_watchOS_app_section6_step2.png?width=30pc)

**步骤3**  打开`LandmarkList.swift`文件，并对该视图进行预览. `Command + Option + Enter`打开预览画布，`Command + Option + P`启动预览。因为现在编译方案已经切换为`watchOS`平台，所以现在预览视图里显示的是手表预览。`watchOS`平台的应用根视图是`ContentView`，目前显示的是`Hello, World`文字。

![section 6 step 3](/tutorials/framework_integration/images/create_a_watchOS_app_section6_step3.png?width=10pc)

**步骤4** 修改`ContentView`让它显示地标列表

![section 6 step 4](/tutorials/framework_integration/images/create_a_watchOS_app_section6_step4.png?width=50pc)

**步骤5** 在模拟器上构建并运行`watchOS`应用。滚动地标列表，点击查看地标详情，标记地标为收藏状态，点击返回按钮从地标详情页返回到地标列表页，打开收藏开关，只查看补收藏的地标。测试一下`watchOS`应用的功能是否正常。

![section 6 step 5](/tutorials/framework_integration/creating_a_watchOS_App.files/watch_landmark_app_test.gif?width=20pc)

### 第七节 创建自定义通知界面

`watchOS`平台的`Landmarks`应用已经接近完成了。在最后一节中，会创建一个通知界面，当用户的地理位置靠近自己收藏过的地标位置时会收到通知提示用户，通知界面展示当前正在接近的地标相关信息。本节只讲当用户收到通知时怎样显示通知界面，不涉及怎样设置和发送通知给用户的内容。

![section 7](/tutorials/framework_integration/images/create_a_watchOS_app_section7.png?width=30pc)

**步骤1** 打开`NotificationView.swift`文件并创建一个显示地标信息、标题及消息的视图。由于任何通知都可能为`nil`，预览视图会展示两种不同的通知视图。第一个展示在没有数据时按默认值显示的视图，第二个展示有标题、消息及位置数据时的视图。

![section 7 step 1](/tutorials/framework_integration/images/create_a_watchOS_app_section7_step1.png?width=50pc)

**步骤2** 打开`NotificationController`添加`landmark`、`title`和`message`属性。这些属性值存储用户收到的通知值。

![section 7 step 2](/tutorials/framework_integration/images/create_a_watchOS_app_section7_step2.png?width=50pc)

**步骤3** 更新`body()`方法，在其内部使用这些属性。在`body`方法内初始化之前创建的`NotificationView`。

![section 7 step 3](/tutorials/framework_integration/images/create_a_watchOS_app_section7_step3.png?width=50pc)

**步骤4** 在`NatificationController`中定义`LandmarkIndex`键，使用这个键从通知中提取地标的下标值。

![section 7 step 4](/tutorials/framework_integration/images/create_a_watchOS_app_section7_step4.png?width=50pc)

**步骤5** 使用`didReceive(_:)`方法从收到的通知中解析相关数据值。这个方法会更新控件器的属性值。调用这个方法后，系统会让控制器的`body`属性失效，引起导航视图更新，之后系统会把通知界面显示在`Apple Watch`上。

![section 7 step 5](/tutorials/framework_integration/images/create_a_watchOS_app_section7_step5.png?width=50pc)

当`Apple Watch`接收到通知时，它会创建`NotificationController`并把它与通知的类型关联起来。为了给`NotificationController`设置类型，必须打开并编辑应用的`storyboard`。

**步骤6** 在项目导航器中，选择`WatchLandmarks`文件夹，并打开`Interface.storyboard`文件，然后选中指向静态通知界面控制器(`static notification interface controller`)的箭头。

![section 7 step 6](/tutorials/framework_integration/images/create_a_watchOS_app_section7_step6.png?width=50pc)

**步骤7** 在属性检查器中，设置通知类别的名称为`LandmarkNear`

![section 7 step 7](/tutorials/framework_integration/images/create_a_watchOS_app_section7_step7.png?width=50pc)

使用`LandmarkNear`类别配置测试数据(`payload`)，并把它传给`NotificationController`

**步骤8** 项目导航器中选中`WatchLandmarks Extensions`文件夹，打开`Push NotificationPayload.apns`文件，更新`title`、`body`、`category`和`landmarkIndex`属性值。确保设置`category`为`LandmarkNear`。除此之外还需要删除所有在本教程中不需要的其它无关字段，例如`subtitle`、`WatchKit Simulator Actions`和`customKey`。Payload文件是用来模拟从服务端发送的远程推送的数据。

![section 7 step 8](/tutorials/framework_integration/images/create_a_watchOS_app_section7_step8.png?width=50pc)

**步骤9** 选择编译方案`WatchLandmarks(Notificaton)`，编译并运行应用。第一次运行通知方案时，系统会请求发送通知的权限，选择允许(`Allow`)获取发送通知的权限。模拟器会展示一个可滚动的通知：一个标识应用身份的横幅、通知视图以及一个点击按钮用来响应通知被点击后的动作。

![section 7 step 9 1](/tutorials/framework_integration/images/create_a_watchOS_app_section7_step9_1.png?width=20pc)

![section 7 step 9 2](/tutorials/framework_integration/images/create_a_watchOS_app_section7_step9_2.png?width=20pc)

### 检查是否理解

**问题1** 当要在`iOS`工程中添加一个`watchOS`编译目标时，应该选用哪个应用模板?

![problem 1](/tutorials/framework_integration/images/create_a_watchOS_app_problem1.png?width=40pc&classes=border)

- [ ] App
- [X] Watch App for iOS App
- [ ] Game App

**问题2** 下面哪一个`watchOS`应用元素可以通过使用`SwiftUI`来实现？

- [ ] 只用`watchOS`应用的视图可以使用`SwiftUI`实现
- [X] `watchOS`应用的视图以及自定义通知界面
- [ ] `watchOS`应用的视图、自定义通知界面以及`complications`
- [ ] `watchOS`应用的视图、自定义通知界面、`complicatons`以及`Siri卡片`

**问题3** 为什么不能在`watchOS`平台上复用`LandmarkDetail`视图？

- [ ] 因为`LandmarkDetail`视图是为大屏设备设计的
- [ ] 因为`watchOS`的用户界面应该只展示最重要的信息，并提供访问额外详情的快速访问方式
- [ ] `MapView`不能在`watchOS`上使用，因为它遵循`UIViewRepresentable`协议，这个协议在`watchOS`平台上不可用
- [X] 以上所有

**问题4** 为什么需要针对`watchOS`平台来修改`LandmarkList`视图？

- [X] 当用户点击列表中的某一项时，需要改变列表展示的详情视图
- [ ] `LandmarkList`不包含任何平台相关的代码，所以要针对不同平台进行不同的布局
- [ ] 要支持多平台，必须使用范型
- [ ] 必须为每个平台重新设计视图

**问题5** 哪一个通知界面可以使用`SwiftUI`开发？

![problem 5](/tutorials/framework_integration/images/create_a_watchOS_app_problem5.png?width=40pc&classes=border)

- [X] 只有动态交互界面可以
- [ ] 静态和动态交互界面都可以