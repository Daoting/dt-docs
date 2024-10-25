---
weight: 1
title: "概述"

tags: []
---

搬运工全名“数据的搬运工(Data Transfer)”，缩写dt，是一个利用 c# + xaml 进行快速业务开发的跨平台框架。

## 定位

搬运工采用.net技术栈，看到 c# + xaml 和 跨平台字眼容易联想到maui技术，但搬运工客户端并非基于maui技术，而是采用 [uno platform](https://github.com/unoplatform/uno) 跨平台方案。因为maui的xaml延续Xamarin.Forms的标准，xaml命名写法和基础的测量、布局与传统的xaml大不相同，对于原来使用wpf silverlight uwp winui的人员需要重新学习，并且采用这些技术的旧项目很难升级到maui，贫道曾尝试将旧控件升级到maui，发现基本重写，遂放弃。前几年微软有个统一xaml规范的项目，但也不疾而终。两种xaml标准成了技术选型的鸿沟，搬运工选择传统xaml标准，因此打算采用maui技术的朋友请慢走。

{{< admonition tip 适用人群>}}
* 熟悉 asp.net core wpf silverlight uwp 或 winui
* 打算升级旧项目以支持在Windows、浏览器(WebAssembly)、手机上跨平台运行
* 继续采用c# + xaml开发支持跨平台的新项目
{{< /admonition >}}

## 前生今世

能阅读到此处的朋友应该都是同道中人:smile_cat:，技术日新月异，尤其前端技术迭代更快，先给道友们讲述一下搬运工的前生。

2009年贫道接手一个新项目，是医院的信息系统，对技术选型没有要求，但必需是B/S架构，因为B/S架构的产品在市场叫得响，是香饽饽。平心而论，这个项目更适合C/S架构，因为对前端的交互要求非常高，比如挂号、收费等操作都需要行如流水的键盘操作，快捷键用的多、界面刷新快，这些都是web的弱项，但迫于市场驱动、迎合用户心里，必需采用B/S架构。那时前端正流行ajax，jquery用的还不怎么多，js开发是痛苦的，因为弱类型、无法编译导致js代码非常随意，多人协作更是让人摸不着边际，经过近两个月的开发贫道决定放弃js，采用当时很火的siverlight，只试用一周siverlight3.0，就彻底征服了贫道，对于这个项目简直完美！到siverlight4.0发布时项目接近完成。

但很快siverlight被微软放弃，一项技术几年时间就从火热走入消亡，我们的产品宣传也删除siverlight字眼:innocent:，避免被对手攻击。其实直到今天也想不出能替代siverlight的技术，包括现在的blazor、uno版的WebAssembly，在性能、用户体验、调试上差很多，但不管怎样也要准备技术升级，况且产品还需要支持在手机上跨平台运行。

2018年开始尝试将部分功能移植到Xamarin.Forms，发现此xaml并非传统xaml，移植基本等于重写，放弃。尝试移植到uwp倒是很顺畅，但无法实现手机跨平台并且还有很多安全限制。经过一番折腾后发现无路可走，贫道仰天长叹，**青丝白发一瞬间，年华老去向谁言**，人生能有几个十年，除了挣那碎银几两，一切如梦如烟。

2018年底在github上搜到[uno platform](https://github.com/unoplatform/uno)，迫不及待地尝鲜，把已经移植到uwp的部分引用uno包编译竟然直接成功，并能在android模拟器上运行，虽然只是简单功能，但贫道眼里泛起希望、感激的泪光，找到组织了，那时uno刚刚开源，才200多star。uno想法非常简单，就是一个桥接器，把uwp/winui应用完整的迁移到其它平台，目前已支持 iOS Android WebAssembly macOS Linux。

{{< admonition quote >}}
* 在iOS，Android和macOS上，uno依靠.net在canvas上绘制UI。
* 在WebAssembly上，uno平台直接依赖于WASM运行时，这是 of.net 的一部分。
* 在Linux上，uno依靠Skia在canvas上绘制UI。
* 总之，uno 平台允许你在所有这些平台上运行单个代码库、C# 和 XAML 应用。

   [uno 如何工作](https://platform.uno/how-it-works/)
{{< /admonition >}}

2019年7月贫道在github上创建[搬运工](https://github.com/daoting/dt)，从0行代码开始，投入到c# + xaml 的跨平台阵营。跨平台本身就是一个妥协过程，不像开发单纯的windows应用无需顾忌太多，跨平台的开发过程总是跌跌撞撞，经常出现在一个平台上运行异常而其它正常的情况，有时是uno的bug、有时是原生的应用不规范、有时是平台特殊的原因、有时是运气不佳:joy:，一个问题能卡上好几天，回想起这几年的日子，怎能以一个**爽**字了得。

由于叛徒的出卖，微软放弃uwp转投winui，为了跟进uno，2022年7月停止uwp版的开发，将uwp版的搬运工迁移到[新库](https://github.com/Daoting/dt-uwp)不再维护，全力主攻winui版，2022年10月升级基本完成。接下来两年从.net7.0到 .net8.0，直到最近算是完成了搬运工的主体功能，[最新Release版本](https://github.com/daoting/dt/releases/latest)。


## 为什么

以下是道友们比较关心的问题，开始使用搬运工之前需要明确：

{{< admonition question 1>}}
既然 uno 已经提供跨平台，为何还需要搬运工？

* 搬运工是一个整体，除了提供前端跨平台外，还包括多个通用的微服务，如消息服务、文件服务等
* 搬运工的核心在**快速业务开发**上，让道友们少踩技术坑
* 理想很丰满，现实很骨感，uno从2018年一路走来，经历多次大的变动，从最初支持uwp到支持winui，bug不断，虽然现在比较稳定，但出现问题也会劝退很多道友
* 使用搬运工开发，uno完全透明像不存在一样，前端控件都已做跨平台适配
{{< /admonition >}}

{{< admonition question 2>}}
uno 比 maui 有何优点？

* uno采用wpf silverlight uwp winui的传统xaml，maui采用自定义xaml
* 使用winui开发的应用和uno一毛钱关系没有，纯原生，但使用maui开发的windows应用在winui外包裹一层
* uno支持linux和WebAssembly，**满足信创对国产操作系统的要求**，maui不支持
{{< /admonition >}}

{{< admonition question 3>}}
搬运工主要提供哪些功能？

* 搬运工由客户端和服务端两部分组成，是一套用于快速构建业务系统的基础技术实现和规范集，本身与具体业务无关
* 服务端全面采用.net云原生开发，提供4个通用的微服务，利用这些预定义的内容可直接快速的开发出符合常规要求的业务系统；
* 客户端采用 .net + winappsdk(winui) + uno，通过[uno platform](https://github.com/unoplatform/uno)支持在Windows、浏览器(wasm)、手机上跨平台运行，包括一系列可复用的前端控件和各种基础功能模块
* 同时，客户端和服务端两部分都支持独立使用，也可根据规范在不同层面、不同粒度下扩展框架，从而满足特定的业务需求
{{< /admonition >}}

{{< admonition question 4>}}
搬运工是旧项目升级，还是从0行开始的新项目？

* 从0行开始的新项目，前端控件都已做跨平台适配
* 搬运工客户端和服务端都采用当前主流的.net版本
{{< /admonition >}}

{{< admonition question 5>}}
搬运工主要有哪些特色？

* 自适应式界面布局，内置两种界面模式：windows模式和Phone模式，能随界面大小的变化在两种模式之间切换，使大小屏幕无缝融合，详见[界面框架](/dt-docs/3客户端/2界面框架/)
* 实现客户端领域层和服务端领域层代码相同，既可以将业务放在客户端处理也可以稍加修改放在服务端处理，完美实现技术与业务解耦、业务逻辑解耦、业务封装复用等关键功能，为道友们提供一种易懂易用的框架风格，详见[总体架构](/dt-docs/2基础/1总体架构/#总体架构)
* 丰富的日志输出，客户端和服务端日志输出方法相同，两端的通信内容、业务处理过程等一览无余，详见[日志](/dt-docs/2基础/2基础功能/#日志)
* 提供Visual Studio扩展工具，提高开发效率，详见[扩展工具](/dt-docs/1开始/1开发环境/#安装搬运工扩展)
* ...
{{< /admonition >}}

{{< admonition question 6>}}
搬运工始终开源吗？

* 始终开源、任意复制、任意修改。
{{< /admonition >}}
