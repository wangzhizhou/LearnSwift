---
title: "命令行下的SPM"
date: 2020-11-24T02:22:36+08:00
weight: 1
---

## 查看命令行帮助信息

Swift工具链安装完成后，可以在控制台下使用下面命令查看Swift Package Manager的使用方法:

```bash
swift package --help 
```

可以看到下面的帮助信息：

```bash
$ swift package --help 

OVERVIEW: Perform operations on Swift packages

SEE ALSO: swift build, swift run, swift test

USAGE: swift package <options> <subcommand>

OPTIONS:
  --package-path <package-path>
                          Specify the package path to operate on (default current directory). This changes the working directory before any other operation
  --cache-path <cache-path>
                          Specify the shared cache directory path
  --config-path <config-path>
                          Specify the shared configuration directory path
  --security-path <security-path>
                          Specify the shared security directory path
  --scratch-path <scratch-path>
                          Specify a custom scratch directory path (default .build)
  --enable-dependency-cache/--disable-dependency-cache
                          Use a shared cache when fetching dependencies (default: true)
  --enable-build-manifest-caching/--disable-build-manifest-caching
                          (default: true)
  --manifest-cache <manifest-cache>
                          Caching mode of Package.swift manifests (shared: shared cache, local: package's build directory, none: disabled (default: shared)
  -v, --verbose           Increase verbosity to include informational output
  --very-verbose, --vv    Increase verbosity to include debug output
  --disable-sandbox       Disable using the sandbox when executing subprocesses
  --enable-netrc/--disable-netrc
                          Load credentials from a .netrc file (default: true)
  --netrc-file <netrc-file>
                          Specify the .netrc file path.
  --enable-keychain/--disable-keychain
                          Search credentials in macOS keychain (default: true)
  --resolver-fingerprint-checking <resolver-fingerprint-checking>
                          (default: strict)
  --enable-prefetching/--disable-prefetching
                          (default: true)
  --force-resolved-versions, --disable-automatic-resolution, --only-use-versions-from-resolved-file
                          Only use versions from the Package.resolved file and fail resolution if it is out-of-date
  --skip-update           Skip updating dependencies from their remote during a resolution
  --disable-scm-to-registry-transformation
                          disable source control to registry transformation (default: disabled)
  --use-registry-identity-for-scm
                          look up source control dependencies in the registry and use their registry identity when possible to help deduplicate across the two origins (default: disabled)
  --replace-scm-with-registry
                          look up source control dependencies in the registry and use the registry to retrieve them instead of source control when possible (default: disabled)
  -c, --configuration <configuration>
                          Build with configuration (default: debug)
  -Xcc <Xcc>              Pass flag through to all C compiler invocations
  -Xswiftc <Xswiftc>      Pass flag through to all Swift compiler invocations
  -Xlinker <Xlinker>      Pass flag through to all linker invocations
  -Xcxx <Xcxx>            Pass flag through to all C++ compiler invocations
  --triple <triple>
  --sdk <sdk>
  --toolchain <toolchain>
  --sanitize <sanitize>   Turn on runtime checks for erroneous behavior, possible values: address, thread, undefined, scudo
  --auto-index-store/--enable-index-store/--disable-index-store
                          Enable or disable indexing-while-building feature (default: autoIndexStore)
  --enable-parseable-module-interfaces
  -j, --jobs <jobs>       The number of jobs to spawn in parallel during the build process
  --emit-swift-module-separately
  --use-integrated-swift-driver
  --experimental-explicit-module-build
  --print-manifest-job-graph
                          Write the command graph for the build manifest as a graphviz file
  --build-system <build-system>
                          (default: native)
  --enable-dead-strip/--disable-dead-strip
                          Disable/enable dead code stripping by the linker (default: true)
  --static-swift-stdlib/--no-static-swift-stdlib
                          Link Swift stdlib statically (default: false)
  --version               Show the version.
  -h, -help, --help       Show help information.

SUBCOMMANDS:
  clean                   Delete build artifacts
  purge-cache             Purge the global repository cache.
  reset                   Reset the complete cache/build directory
  update                  Update package dependencies
  describe                Describe the current package
  init                    Initialize a new package
  _format
  diagnose-api-breaking-changes
                          Diagnose API-breaking changes to Swift modules in a package
  dump-symbol-graph       Dump Symbol Graph
  dump-pif
  dump-package            Print parsed Package.swift as JSON
  edit                    Put a package in editable mode
  unedit                  Remove a package from editable mode
  config                  Manipulate configuration of the package
  resolve                 Resolve package dependencies
  show-dependencies       Print the resolved dependency graph
  tools-version           Manipulate tools version of the current package
  generate-xcodeproj      Generates an Xcode project. This command will be deprecated soon.
  compute-checksum        Compute the checksum for a binary artifact.
  archive-source          Create a source archive for the package
  completion-tool         Completion tool (for shell completions)
  plugin                  Invoke a command plugin or perform other actions on command plugins

  See 'swift help package <subcommand>' for detailed help.
```

## 具体使用方法

就像帮助信息中所列的，命令行下使用SPM的格式如下：

```
swift package <options> <subcommand>
```

因为选项太多，我们在具体使用过程中可以在需要的时候从命令行帮助信息中查询，这里我们主要学习下这些子命令的使用方法，关于子命令的具体使用帮助信息，可以使用命令：`swift help package <subcommand>`在控制台随时进行查询学习




