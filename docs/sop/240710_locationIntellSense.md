
## 官方的汉化教程

目前仅提供了.NET5 的本地化 IntelliSense 文件

<https://mp.weixin.qq.com/s?__biz=MzI3ODc3NzIxMw==&mid=2247484031&idx=2&sn=e6f80202df670941b96c9f1517820c9b&chksm=eb509e6ddc27177b4cc98129817728e6c71ddded91b7ae3fca1c1ca7db4eb1911f8638bd7f0c&token=1223988246&lang=zh_CN#rd>

## IntelliSenseLocalizer

用于生成和安装本地化IntelliSense文件的工具。

### 简介

在`.net6`之前，我们可以在这个页面 - [Download localized .NET IntelliSense files](https://dotnet.microsoft.com/en-us/download/intellisense)下载本地化的智能感知文件。但`.net6`发布很长一段时间后，这个页面也没有添加`.net6`的本地化的智能感知文件。根据`dotnet/docs`中的这个[issue](https://github.com/dotnet/docs/issues/27283)，里面说不再提供本地化智能感知文件了 - "`Yes, unfortunately, we will no longer be localizing IntelliSense.`"。但是[在线文档](https://docs.microsoft.com)里面还有本地化描述。所以有了这个工具。

`IntelliSenseLocalizer`使用[在线文档](https://docs.microsoft.com)生成本地化智能感知文件。工具会下载所有的api页面并分析页面以匹配原始的智能感知文件，然后生成目标`xml`。

得益于[在线文档](https://docs.microsoft.com)良好的本地化和统一的页面布局。这个工具理论上可以生成所有区域的智能感知文件。但是`如果页面布局变动了，这个工具无法自动的适配新的布局`。

### 如何使用

#### 1. 安装本工具

```shell
dotnet tool install -g islocalizer
```

#### 运行 `islocalizer -h` 可以看到更多的命令和帮助信息

在命令最后加上参数 -h 即可查看命令的帮助，例如：

```shell
islocalizer install auto -h
islocalizer cache -h
```

### 2. 尝试从nuget.org安装已生成好的智能感知文件

#### 查看可用的包 [Nuget](https://www.nuget.org/packages/IntelliSenseLocalizer.LanguagePack)

这个命令将尝试从nuget.org找到并安装`zh-cn`的`net6.0`智能感知包:

```shell
islocalizer install auto -m net6.0 -l zh-cn
```

你也可以使用`-cc`来指定内容双语对照类型

```shell
islocalizer install auto -m net6.0 -l zh-cn -cc LocaleFirst
```

### 3. 自己构建本地化智能感知文件

构建`net6.0`相关的文件:

```shell
islocalizer build -m net6.0
```

这个命令可能会运行很久。。。不过缓存完文件后，第二次生成会快很多。
生成的压缩包将会存放到默认输出目录，可以在控制台输出中找到路径。

### 4. 安装生成的智能感知文件

```shell
islocalizer install {ArchivePackagePath}
```

`ArchivePackagePath` 是build命令输出的路径.

![](https://img2024.cnblogs.com/blog/1920368/202407/1920368-20240708111509529-1421251667.png)

Net9目前还是预览版，暂时不支持，敬请更新

#### 推荐阅读

> [看看这样的Dotnet后台管理，那真是叫一个清新优雅高颜值！！！](https://mp.weixin.qq.com/s/XBwMUMWnakmLrTjQzz7xWg)
>
> <https://malus.dotnetshare.com>
>
> [超级漂亮干净的Dotnet学习网站](https://mp.weixin.qq.com/s/SjDB0522JGer4zI4WVJ1sw)
