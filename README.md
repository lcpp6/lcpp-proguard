<p align="center">
  <br />
  <br />
  <a href="https://www.guardsquare.com/proguard">
    <img
      src="https://www.guardsquare.com/hubfs/Logos/ProGuard-Logo-Email.png"
      alt="ProGuard" width="400">
  </a>
</p>

<!-- Badges -->
<p align="center">
  <!-- CI -->
  <a href="https://github.com/Guardsquare/proguard/actions?query=workflow%3A%22Continuous+Integration%22">
    <img src="https://github.com/Guardsquare/proguard/workflows/Continuous%20Integration/badge.svg">
  </a>
  
  <!-- Github version -->
  <!--
  <a href="releases">
    <img src="https://img.shields.io/github/v/release/guardsquare/proguard">
  </a>
  -->
    
  <!-- Maven -->
  <a href="https://search.maven.org/search?q=g:com.guardsquare">
    <img src="https://img.shields.io/maven-central/v/com.guardsquare/proguard-base">
  </a>
  
  <!-- License -->
  <a href="LICENSE">
    <img src="https://img.shields.io/github/license/guardsquare/proguard">
  </a>

  <!-- Twitter -->
  <a href="https://twitter.com/Guardsquare">
    <img src="https://img.shields.io/twitter/follow/guardsquare?style=social">
  </a>
</p>

<br />
<p align="center">
  <a href="#-quick-start"><b>Quick Start</b></a> •
  <a href="#-features"><b>Features</b></a> •
  <a href="#-contributing"><b>Contributing</b></a> •
  <a href="#-license"><b>License</b></a>
</p>
<br />

ProGuard 是一个免费的 Java 字节码压缩器、优化器、混淆器和预验证器：

- 它检测并删除未使用的类、字段、方法和属性。

- 它优化了字节码并删除了未使用的指令。

- 它使用简短而无意义的名称重命名剩余的类、字段和方法。

- 最终的应用程序和库更小、更快。


## 🚀 🚀 快速入门

### 1）命令行方式

从这下载最新版： [GitHub releases](https://github.com/Guardsquare/proguard/releases).

Linux/MacOS：

```bash
bin/proguard.sh <options...>
```

Windows:

```
bin\proguard.bat <options...>
```

通常，你会把大多数选项放在配置文件中（例如 myconfig.pro），然后只需调用

```bash
bin/proguard.sh @myconfig.pro
```
或者在 Windows 上：

```
bin\proguard.bat @myconfig.pro
```

All available options are described in the [configuration section of the manual](https://www.guardsquare.com/manual/configuration/usage).

### 2）Gradle 任务方式

ProGuard 可以作为 Gradle 中的任务运行。在使用 proguard 任务之前，您必须确保 Gradle 可以在构建时在其类路径中找到它。

一种方法是将以下行添加到您的build.gradle文件中，该文件将从 Maven Central 下载 ProGuard：

```Groovy
buildscript {
    repositories {
        mavenCentral()
    }
    dependencies {
        classpath 'com.guardsquare:proguard-gradle:7.6.0'
    }
}
```

然后您可以使用配置定义一个任务：

```Groovy
tasks.register('proguard', ProGuardTask) {
    configuration file('proguard.pro')

    injars(tasks.named('jar', Jar).flatMap { it.archiveFile })

    // Automatically handle the Java version of this build.
    if (System.getProperty('java.version').startsWith('1.')) {
        // Before Java 9, the runtime classes were packaged in a single jar file.
        libraryjars "${System.getProperty('java.home')}/lib/rt.jar"
    } else {
        // As of Java 9, the runtime classes are packaged in modular jmod files.
        libraryjars "${System.getProperty('java.home')}/jmods/java.base.jmod", jarfilter: '!**.jar', filter: '!module-info.class'
        //libraryjars "${System.getProperty('java.home')}/jmods/....."
    }

    verbose

    outjars(layout.buildDirectory.file("libs/${baseCoordinates}-minified.jar"))
}
```

嵌入的配置与标准 ProGuard 配置非常相似。您可以在 [Gradle setup page](https://www.guardsquare.com/manual/setup/gradle).设置页面上找到更多详细信息。

## ✨ 特点介绍

ProGuard 的工作原理类似于高级优化编译器，删除未使用的类、字段、方法和属性，缩短标识符，合并类，内联方法，传播常量，删除未使用的参数等。

优化通常可将应用程序的大小减少 20% 到 90%。减少量主要取决于 ProGuard 可以全部或部分删除的外部库的大小。

优化还可提高应用程序的性能，最高可达 20%。对于服务器和台式机上的 Java 虚拟机，这种差异通常并不明显。

ProGuard 还可以从应用程序及其库中删除日志代码，而无需更改源代码 - 事实上，根本不需要源代码！

手册页（markdown、 html）详细介绍了 ProGuard 的功能和用法。

The manual pages ([markdown](docs/md),
[html](https://www.guardsquare.com/proguard/manual)) cover the features and usage of
ProGuard in detail.

## 💻 构建 ProGuard

构建 ProGuard 非常简单 - 您只需要安装 Java 8 JDK。要从源代码构建，请克隆 ProGuard 存储库的副本并运行以下命令：
```bash
./gradlew assemble
```

工件将在lib目录中生成。然后，您可以使用 中的脚本执行 ProGuard bin，例如：

```bash
bin/proguard.sh
```

您可以使用以下方式将工件发布到本地 Maven 存储库：

```bash
./gradlew publishToMavenLocal
```

## 🤝 贡献 Contributing

Contributions, issues and feature requests are welcome in both projects.
Feel free to check the [issues](https://github.com/Guardsquare/proguard/issues) page and the [contributing
guide](CONTRIBUTING.md) if you would like to contribute.

## 📝 License

Copyright (c) 2002-2023 [Guardsquare NV](https://www.guardsquare.com/).
ProGuard is released under the [GNU General Public License, version
2](LICENSE), with [exceptions granted to a number of
projects](docs/md/manual/license/gplexception.md).
