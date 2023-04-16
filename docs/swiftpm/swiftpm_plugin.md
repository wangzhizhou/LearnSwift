---
title: "Swift Package Plugin"
date: 2023-04-16T16:56:08+08:00
---

Xcode 11 引入了Swift Package, 它提供了一种将库作为源代码发布的好方法，Xcode 14将这种方法进行了扩展，让它可以使用插件来执行操作。例如构建过程中生成源码、自动完成发布任务。

### 什么是包插件(Package Plugin)

包插件是一种可以对软件包或者Xcode项目执行操作的Swift脚本，包插件使用Xcode专门提供的API，以软件包(Swift Package)的形式实现。一个软件包可以只包含一个插件，也可以包含多个插件。一个插件可以由多个Swift源文件构成。

一个软件包(Swift Package)中不仅可以包含库(.library)、产品(.product)、可执行文件(.executable)，也可以包含插件(package plugins)

软件包中定义的插件可以只限制在当前软件包中使用，也可以作为产品，提供给其它依赖方使用。与正常软件包被依赖的方式不同的时，插件的运行时内容不会带入App，但它可以访问项目所在构建机器上的相关工具或者目录。

Xcode 14中支持了两种插件类型：`Command Plugin`和`Build Tool Plugin`
    
- 命令插件可以执行类似代码格式化、更新git仓库贡献者列表或者一些预发布操作，有一些插件需要获取文件访问权限(例如代码格式化操作)，其它的插件可能不需要操作文件内容

- 构建工具插件可以作用于每一个需要它们的编译Target，可以在项目构建过程中生成源码或者处理相关资源

如果一个包插件在一个软件包中已经实现并允许被依赖，那么依赖包插件的方式和正常使用软件包的方式相关，只需要将插件所在的软件包添加到项目依赖中即可。

要执行包插件，可以在Xcode中的包名上右键选择弹出的包插件菜单项，也可以用命令行的方式运行包插件。如果包插件定义了用户可输入的参数，在执行时也可以接受用户输入参数。

包插件在Xcode中运行时，会弹窗提示用户需要针对哪些Target运行，如果插件运行涉及到获取文件访问权限，Xcode也会提示用户，并可以跳转到插件中实际申请文件访问权限的代码位置。

### 包插件是如何工作的?

包插件是根据需要进行编译运行的Swift脚本，每一个包插件都在一个单独的进程中运行。包插件可以获取到运行所在软件包的相关信息、机器的命令行工具、操作文件系统、调用基础库能力去完成工作。

包插件是在阻止访问网络的沙盒环境中运行的，默认可以直接读写指定的几个文件系统位置。命令插件需要访问其它文件系统位置，需要额外请求对应的权限。

包插件也可以将执行结果发给Xcode，让Xcode展示一些提示信息、警告或者错误。

包插件的实现依赖Xcode中`PackagePlugin`模块提供的API。下面是一个插件定义的简化示例：

```swift 
import PackagePlugin

@main
struct MyPlugin: CommandPlugin {

    // 入口函数的定义，依赖于插件的具体类型
}
```

### 命令插件(Command Plugin)

命令插件扩展了开发工作流程，它直接作用于软件包，不在项目构建期间运行。并非所有的命令插件都需要文件系统权限，但如果命令插件确实需要申请文件系统权限，需要在包清单文件中指明相关权限申请项信息。有申请权限的命令插件，在运行之前，Xcode会提示用户是否授权命令插件需要的对应权限，只有用户允许对应的权限，插件才能运行。

插件通常很小，依赖其它工具完成任务，依赖项可以是源码，也可以二进制文件，在插件运行之前，Xcode会对相关依赖工具的源码进行编译。

一个命令插件的定义框架示例：

```swift 
import PackagePlugin

@main
struct MyPlugin: CommandPlugin {

    // 入口函数的定义，依赖于插件的具体类型
    func performCommand(context: PluginContext, arguments: [String]) throws {
        // 执行插件逻辑
    }
}
```

除了在Xcode中使用GUI的方式调用插件运行外，还可以使用命令行工具运行插件。Swift Package Manager 5.6时提供了和插件相关的子命令，`swift package plugin --list`可以用来查看所有插件。涉及到需要获取额外文件系统权限申请的命令插件时，使用命令行方式运行，也会有对应的权限提示，询问用户是否授与对应插件相应的权限。如果在命令行运行插件时想到默认直接允许权限申请时，可以使用`--allow-writing-to-package-directory`选项。命令行运行时，也可以给插件传入对应的参数。

### 构建工具插件(Build Tool Plugin)

构建工具插件在调用时不会立即完成任务，它只是为构建系统提供对应时机的回调，以便在项目构建的对应时机执行预先定义好的行为。

构建工具插件在阻止网络访问的沙盒中运行，它不能对包做任何修改。一个构建工具插件的定义框架发示例如下:

```swift
import PackagePlugin

@main
struct MyPlugin: BuildToolPlugin {

    // 入口函数的定义，依赖于插件的具体类型
    func createBuildCommands(context: PluginContext, target: Target) throws -> [Command] {
        // 配置并返回构建命令
    }
}
```

构建工具插件可以定义两种类型的构建命令：
1. 预构建命令：构建开始之前执行的命令
2. 普通构建命令：指定输入/输出路径，只用在输出路径缺失或者输入路径发生变化时执行


### 创建包插件(Swift Packag Plugins)

软件包插件是一种使用 `PackagePlugin API` 的类似于软件包清单的Swift代码，插件可以通过定义明确的扩展点扩展 Xcode或Swift软件包管理器的功能。

Swift工具链版本需要在5.6版本及以上才支持包插件功能。Swift清单文件中需要指定工具链版本至少是5.6，清单文件中定义插件的示例：

```swift
.plugin(
    name: "GenerateContributors",
    capability:.command(
        intent: .custom(
            verb: "regenerate-contributors-list",
            description: "Generates the CONSTRIBUTORS.txt file based on Git logs"
        ),
        permissions: [
            .writeToPackageDirectory(reason: "Writes CONTRIBUTORS.txt to the source root.")
        ]
    )
)
```

如果在Xcode中右键包名时，菜单里没有出现插件项时，重启Xcode可以解决。

在软件包根目录中创建`Plugins/GenerateContributors/`目录，并新建`plugin.swift`文件，

```swift
import Foundation
import PackagePlugin

@main
struct GenerateContributors: CommandPlugin {
    
    func performCommand(context: PackagePlugin.PluginContext, arguments: [String]) async throws {
        let process = Process()
        process.executableURL = URL(fileURLWithPath: "/usr/bin/git")
        process.arguments = ["log", "--pretty=format:- %an <%ae>%n"]
        
        let outputPipe = Pipe()
        process.standardOutput = outputPipe
        try process.run()
        process.waitUntilExit()
        
        let outputData = outputPipe.fileHandleForReading.readDataToEndOfFile()
        let output = String(decoding: outputData, as: UTF8.self)
        
        let contributors = Set(output.components(separatedBy: CharacterSet.newlines)).sorted().filter { !$0.isEmpty }
        try contributors.joined(separator: "\n").write(toFile: "CONTRIBUTORS.txt", atomically: true, encoding: .utf8)
    }
}
```

#### 插件运行细节

插件运行在沙盒中，网络连接以及除插件本身运行目录之外的非临时位置都会被禁止。自定义命令可以选择性声明是否写入软件包根目录，如果要包装已有的第三方工具，必须考虑如何将其限制在沙盒模式中。例如：可以通过配置生成文件的写入位置来实现

#### 构建工具插件

构建工具插件的两种不同类型区分的要点是工具是否定义了输出集。如果有输出集，可以创建构建时命令，输出与输入对比如果过时，构建系统会自动重新运行。如果没有输出集，可以创建构建前命令，构建前命令会在每次构建开始时运行，应该避免在构建前命令中执行耗时操作。

##### 构建时命令

构建时命令与自定义命令的不同之处在于，除了描述可执行文件，还需要指定输入输出

```swift
    ...
    .executableTarget(name: "AssetConstantsExec"),
    .plugin(name: "AssetConstants", capability: .buildTool(), dependencies: ["AssetConstantsExec"])
```

Plugins/AssetConstants/plugin.swift文件示例如下：

```swift
import PackagePlugin

struct AssetConstants: BuildToolPlugin {
    func createBuildCommands(context: PluginContext, target: Target) async throws -> [Command] {
        guard let target = target as? SourceModuleTarget else {
            return []
        }
        return try target.sourceFiles(withSuffix: "xcassets").map { asset in
            let base = asset.path.stem
            let input = asset.path
            let output = context.pluginWorkDirectory.appending(["\(base).swift"])
            
            return .buildCommand(displayName: "Generating constrants for \(base)", executable: try context.tool(named: "AssetConstantsExec").path, arguments: [input.string, output.string],
                                 inputFiles: [input],
                                 outputFiles: [output]
            )
        }
    }
}
```

##### 预编译命令

与构建时命令的区别之处在于，预编译命令返回的值是`.prebuildCommand`

```swift
import PackagePlugin

struct AssetConstants: BuildToolPlugin {
    func createBuildCommands(context: PluginContext, target: Target) async throws -> [Command] {
        guard let target = target as? SourceModuleTarget else {
                    return []
                }

        let resourcesDirectoryPath = context.pluginWorkDirectory
                    .appending(subpath: target.name)
                    .appending(subpath: "Resources")
                let localizationDirectoryPath = resourcesDirectoryPath
                    .appending(subpath: "Base.lproj")

                try FileManager.default.createDirectory(atPath: localizationDirectoryPath.string, withIntermediateDirectories: true)

        let swiftSourceFiles = target.sourceFiles(withSuffix: ".swift")
                let inputFiles = swiftSourceFiles.map(\.path)

        return [
                    .prebuildCommand(
                        displayName: "Generating localized strings from source files",
                        executable: .init("/usr/bin/xcrun"),
                        arguments: [
                            "genstrings",
                            "-SwiftUI",
                            "-o", localizationDirectoryPath
                        ] + inputFiles,
                        outputFilesDirectory: localizationDirectoryPath
                    )
                ]
    }
}
```

### 相关参考资料

- [Swift 软件包插件简介](https://developer.apple.com/videos/play/wwdc2022/110359/)
- [构建Swift软件包插件](https://developer.apple.com/videos/play/wwdc2022/110401/)
