---
weight: 14
title: "实体框架"

tags: []
---

若只是将搬运工作为控件库使用，请略过。此内容属于**快速业务开发**部分。

参照[总体架构](/dt-docs/2基础/1总体架构)，实体框架就是总体架构的领域层。

领域层是业务开发过程中的重要部分，包括实体、实体读写、业务逻辑、领域服务等，并且实现**客户端领域层和服务端领域层代码相同**，既可以将业务放在客户端处理也可以稍加修改放在服务端处理。但系统未采用DDD模式的聚合根、值对象、仓库等概念，不再区分聚合根、值对象等类型，实体之间的关系可自定义调整。