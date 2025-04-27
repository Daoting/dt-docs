---
weight: 3
title: "Android应用"

tags: []
---

生成apk
- 真机apk：选择 Release -> AndroidManifest.xml设置版本 -> 调整csproj文件的RuntimeIdentifier配置 -> 右键选择“发布...” -> apk文件在 bin\Release\net8.0-android\android-arm64\ 目录下

- 虚拟机：选择 Release -> 调整csproj文件的RuntimeIdentifier为android-x64 -> Ctrl + F5 自动打包部署到虚拟机并运行
也可用命令发布：
dotnet publish -f:net7.0-android -c:Release