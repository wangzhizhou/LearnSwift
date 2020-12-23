---
title: "玩转UI控件"
date: 2020-05-19T23:15:53+08:00
weight: 2
---

在`Landmarks`应用中，用户可以创建一个简介来描述他们自已的个人情况。为了让用户可以编辑自己的简介，我们需要添加一个编辑模式并设计一个偏好设置界面。

这里使用多种通用控件来展示用户的各种数据，并在用户保存他们所做的数据修改时更新地标数据模型。

按照步骤在下面的项目工程中一步步进行实践。

{{%attachments title="项目文件" style="blue" pattern=".*zip" /%}}

---

### 第一节 展示用户简介

`Landmarks`应用在本地存储了一些配置和用户偏好设置。在用户编辑这些数据前，会被展示在一个没有编辑按钮的概要视图上。

![secion 1](/tutorials/app_design_and_layout/images/working-with-ui-controls-section1.png?width=30pc)

**步骤1** 

在项目文件导航栏的`Landmarks`文件组下面新建一个名为`Profile`的文件组，并在这个新建的文件组下面添加一个新视图`ProfileHost`, 这个新视图包含一个`TextView`，用来展示用户名称。`ProfileHost`将会展示静态概要信息，同时支持编辑模式

![secion 1 step 2](/tutorials/app_design_and_layout/images/working-with-ui-controls-section1-step1.png?width=50pc)

**步骤2** 用步骤1创建的`ProfileHost`替换`Home.swift`中的静态文本`Text`视图。现在主页中的`profile`按钮点击时可以调起一个用户简介页面了

![secion 1 step 2](/tutorials/app_design_and_layout/images/working-with-ui-controls-section1-step2.png?width=50pc)

**步骤3** 创建一个新的视图命名为`ProfileSummary`，它会持有一个`Profile`实例，并显示一些用户的基本信息。`Profile`概要视图持有一个`Profile`对像的原因是，因为它的父视图`ProfileHost`管理着视图的状态，它不能与`Profile`进行绑定。

![secion 1 step 3](/tutorials/app_design_and_layout/images/working-with-ui-controls-section1-step3.png?width=50pc)

**步骤4** 更新`ProfileHost`文件，显示新的概要视图

![secion 1 step 4](/tutorials/app_design_and_layout/images/working-with-ui-controls-section1-step4.png?width=50pc)

**步骤5** 创建一个名为`HikeBadge`的新视图，这个新视图由`Badge`视图和一些描述性文字构成。`Badge`仅仅是一个图形，在`HikeBadge`视图中的文本与`accessibility(label:)`属性修改器一起，可以让这个徽章对用户更加清晰。注意`frame(width:height:)`的两种不同的用法用来配置徽章以不同的缩放尺寸显示。

![secion 1 step 5](/tutorials/app_design_and_layout/images/working-with-ui-controls-section1-step5.png?width=50pc)

**步骤6** 更新`ProfileSummary`文件，添加几个不同的徽章代表用户得到的不同徽章

![secion 1 step 6](/tutorials/app_design_and_layout/images/working-with-ui-controls-section1-step6.png?width=50pc)

**步骤7** 把`HikeView`包含在`ProfileSummary`页面中后，就完成了第一节的实践内容了。

![secion 1 step 7](/tutorials/app_design_and_layout/images/working-with-ui-controls-section1-step7.png?width=50pc)

### 第二节 添加编辑模式

用户需要能够在浏览模式和编辑模式之间进行切换来查看或者修改用户简介的信息。通过在`ProfileHost`上添加一个`Edit Button`，然后创建一个用来编辑简介信息的页面。

![secion 2](/tutorials/app_design_and_layout/images/working-with-ui-controls-section2.png?width=20pc)

**步骤1** 添加一个`Enviornment`视图属性，用来使用`\.edit`模式。可以使用这个属性来读写当前编辑模式。

![secion 2 step 1](/tutorials/app_design_and_layout/images/working-with-ui-controls-section2-step1.png?width=30pc)

**步骤2** 创建一个编辑按钮，可以切换编辑模式

![secion 2 step 2](/tutorials/app_design_and_layout/images/working-with-ui-controls-section2-step2.png?width=50pc)

**步骤3** 更新`UserData`类，包含一个Profile实例，即使用户简介页面消失后也可以存储编辑后的信息

![secion 2 step 3](/tutorials/app_design_and_layout/images/working-with-ui-controls-section2-step3.png?width=30pc)

**步骤4** 从环境变量中读取用户简介信息，并把数据传递给`ProfileHost`视图的控件上进行展示。为了在编辑状态下修改简介信息后确认修改前避免更新全局状态(例如在编辑用户名的过程中)，编辑视图在一个备份属性中进行相应的修改操作，确认修改后，才把备份属性同步到全局应用状态中。

![secion 2 step 4](/tutorials/app_design_and_layout/images/working-with-ui-controls-section2-step4.png?width=30pc)

**步骤5** 添加一个条件视图，可以用来显示静态用户简介视图或者是用户简介视图的编辑模式。当前的编辑模式只支持静态文本框的编辑。

![secion 2 step 5](/tutorials/app_design_and_layout/images/working-with-ui-controls-section2-step5.png?width=50pc)

### 第三节 定义简介编辑器

用户简介编辑器包含几个单独的控件用来修改对应简介信息。在简介中，一些项例如徽章是不可以编辑修改的，所以它们不会出现在简介编辑器中。为了保持简介在编辑模式和浏览模式的一致性，需要按照简介页面各项相同的顺序进行添加。

**步骤1** 创建一个名为`ProfileEditor`的新视图，并绑定用户简介中的草稿。视图中的第一个控件是`TextField`，用来更新用户名字段值。创建`TextField`时要提供一个标签和一个绑定字符串。

![secion 3 step 1](/tutorials/app_design_and_layout/images/working-with-ui-controls-section3-step1.png?width=50pc)

**步骤2** 更新`ProfileHost`中的条件内容，让它包含条件编辑器并把简单的绑定关系传递给简介编辑器。现在当你点击`Edit`按钮，简介视图就会变成编辑模式了。

![secion 3 step 2](/tutorials/app_design_and_layout/images/working-with-ui-controls-section3-step2.png?width=50pc)

**步骤3** 添加一个切换开关，用来设置用户是否接收相关地标事件的推送通知。这个`Toggle`控件打开和关闭正好对应着布尔值的`true`或`false`。

![secion 3 step 3](/tutorials/app_design_and_layout/images/working-with-ui-controls-section3-step3.png?width=50pc)

**步骤4** 把一个`Picker`和一个`Text`放在`VStack`结构里，让这个地标可以选择不同季节。

![secion 3 step 4](/tutorials/app_design_and_layout/images/working-with-ui-controls-section3-step4.png?width=50pc)

**步骤5** 最后，在季节图片选择器下方添加一个`DatePicker`，用来修改地标的目标浏览日期

![secion 3 step 5](/tutorials/app_design_and_layout/images/working-with-ui-controls-section3-step5.png?width=50pc)

### 第四节 延迟编辑传播

在编辑模式时，使用用户简介信息的备份进行修改，当用户确认进行修改后，再用修改的备份信息覆盖真正的用户信息。直到用户退出编辑模式前都不让编辑的备份生效。

![secion 4](/tutorials/app_design_and_layout/images/working-with-ui-controls-section4.png?width=20pc)

**步骤1** 在`ProfileHost`视图上添加一个取消按钮。不像编辑模式按钮提供的完成按钮，取消按钮不会应用修改后的简介备份信息到实际的简介数据上。

![secion 4 step 1](/tutorials/app_design_and_layout/images/working-with-ui-controls-section4-step1.png?width=40pc)

**步骤2** 当用户点击完成按钮后，使用`onAppear(perform:)`和`onDisappear(perform:)`来更新或保存用户简介数据。下一次进入编辑模式时，使用上一次的用户简介数据来展示。

![secion 4 step 2](/tutorials/app_design_and_layout/images/working-with-ui-controls-section4-step2.png?width=40pc)

### 检查是否理解

**问题1** 编辑状态改变时，怎样更新一个视图，例如，当用户编辑了用户简介信息后点击完成按钮的情况下，是怎么更新一个视图的

- [ ] ![problem 1 answer 1](/tutorials/app_design_and_layout/images/working-with-ui-controls-problem1-answer1.png?width=30pc&classes=border)
- [X] ![problem 1 answer 2](/tutorials/app_design_and_layout/images/working-with-ui-controls-problem1-answer2.png?width=30pc&classes=border)
- [ ] ![problem 1 answer 3](/tutorials/app_design_and_layout/images/working-with-ui-controls-problem1-answer3.png?width=30pc&classes=border)

**问题2** 什么情况下需要添加一个`accessiblity`标签，使用`accessibility(label:)`修改器?

- [ ] 在应用的每一个视图都添加一个`accessibility`标签
- [X] 当可以让用户界面元素对用户变的更清晰时，添加一个`accessibility`标签
- [ ] 只有当你没有给视图清加tag时才可以使用`accessibility(label:)`

**问题3** 模态和非模态视图展示有什么差别？

- [ ] 当模态展示一个视图时，源视图设置目标视图的编辑模式
- [ ] 当非模态展示一个视图时，目标视图会盖住源视图并且替代当前的导航栈
- [X] 当模态展示一个视图时，目标视图盖住源视图并替换当前导航栈