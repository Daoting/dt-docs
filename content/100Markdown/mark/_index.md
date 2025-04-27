---
weight: 2
title: "扩展语法"
tags: []
---

内容摘要

<!--more-->

## 代码

语法高亮
{{< highlight cs >}}
    protected override void ConfigureServices(IServiceCollection p_svcs)
    {
        base.ConfigureServices(p_svcs);
        p_svcs.AddSingleton<IRpcConfig, RpcConfig>();
        p_svcs.AddTransient<IBackgroundJob, BackgroundJob>();
        p_svcs.AddTransient<IReceiveShare, ReceiveShare>();
        //p_svcs.AddSingleton<ILogSetting, LogSetting>();
        //p_svcs.AddTransient<ITheme, CustomTheme>();
    }
{{< /highlight >}}

行高亮
{{< highlight cs "hl_lines=2 5-6" >}}
    protected override void ConfigureServices(IServiceCollection p_svcs)
    {
        base.ConfigureServices(p_svcs);
        p_svcs.AddSingleton<IRpcConfig, RpcConfig>();
        p_svcs.AddTransient<IBackgroundJob, BackgroundJob>();
        p_svcs.AddTransient<IReceiveShare, ReceiveShare>();
        //p_svcs.AddSingleton<ILogSetting, LogSetting>();
        //p_svcs.AddTransient<ITheme, CustomTheme>();
    }
{{< /highlight >}}

普通代码
```代码
hugo new site my_website
cd my_website
```

`行内代码段`


## 强调

**粗体显示**

*渲染为斜体*

~~两个~删除线.~~

选中内容按住shift + *

水平线：三个连续的星号、下划线或破折号 
***


## 横幅

{{< admonition >}}
默认标题为注意
{{< /admonition >}}

{{< admonition abstract >}}
摘要
{{< /admonition >}}

{{< admonition info >}}
信息
{{< /admonition >}}

{{< admonition tip >}}
技巧
{{< /admonition >}}

{{< admonition success >}}
成功
{{< /admonition >}}

{{< admonition question >}}
问题
{{< /admonition >}}

{{< admonition warning >}}
警告
{{< /admonition >}}

{{< admonition failure >}}
失败
{{< /admonition >}}

{{< admonition danger >}}
危险
{{< /admonition >}}

{{< admonition bug >}}
Bug
{{< /admonition >}}

{{< admonition example >}}
示例
{{< /admonition >}}

{{< admonition quote >}}
引用
{{< /admonition >}}

{{< admonition warning "自定义标题" false >}}
自定义标题 和 是否默认展开
{{< /admonition >}}


## 列表

* 无序列表项1
* 无序列表项2

1. 有序项
1. 有序项
1. 有序项

- [ ] 任务项
- [x] 任务项


## 链接

[搬运工](https://github.com/daoting/dt)

[搬运工](https://github.com/daoting/dt "鼠标悬停提示")

[同页跳转](#代码)


## 图片

{{< image src="fw.png" caption="框架图 (`可交互`)" >}}


## 表格

| 列名1 | 列名2 | 列名3 |
|:------:| -----:| ----- |
|  两边加冒号  | 右侧加冒号右对齐 | 默认左对齐 |
| 居中对齐 | 右侧加冒号右对齐 | 默认左对齐 |


## 版本

{{< version 2.1 >}}

{{< version 2.5 changed >}}


{{< version 1.6 deleted >}}


## 视频

{{< bilibili id=BV1Sx411T7QQ >}}

分段
{{< bilibili id=BV1TJ411C7An p=3 >}}

自动播放
{{< bilibili id=BV1Sx411T7QQ autoplay=true >}}

## 双排

[WYSIWYG]^(所见即所得)