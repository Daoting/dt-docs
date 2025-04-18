---
weight: 5
title: "通用视图"

tags: []
---

通用视图是对通用框架的再次简化，对于`单表、一对多、多对多`三种模式利用通用框架生成的代码，当只是在UI上做一些调整，比如对Lv调整显示模式、列序、修改列宽列标题，对Fv表单重新定义xaml等，通用视图就是很好的选择，它不需要额外的代码，只需要为`通用视图`定义好实体类型、Lv Fv的xaml、菜单项等，这些配置保存在json串提供给`通用视图`即可，大大减少冗余代码量。

通用视图有两种用法：
1. 菜单配置方式，视图参数为json格式的配置，零代码实现所有功能，业务逻辑在实体类和领域服务中实现
1. 代码调用方式，相当于将json格式的视图参数用代码构造

以下针对三种模式演示通用视图的用法

## 通用单表视图

### 菜单配置
在菜单管理中添加新菜单项，参见[添加菜单](/dt-docs/5rbac/1菜单/#添加)，视图名称选择`通用单表视图`，然后`编辑`视图参数
{{< bilibili id=BV1oG5CzPETh >}}

表单的校验在`实体类`的InitHook中实现
{{< highlight cs >}}
protected override void InitHook()
{
    OnSaving(async () =>
    {
        if (IsAdded && 发布插入事件)
        {
            AddEvent(new 插入Event { ID = ID });
        }
                
        if (c不重复.IsChanged)
        {
            int cnt = await AtSvc.GetScalar<int>($"select count(1) from crud_基础 where 不重复='{不重复}' and ID!={ID}");
            if (cnt > 0)
            {
                Throw.Msg("[不重复]列存在重复值！", c不重复);
            }
        }

        if (禁止保存)
        {
            Throw.Msg("已选中[禁止保存]，保存前校验不通过！", c禁止保存);
        }

        if (c值变事件.IsChanged)
        {
            AddEvent(new 值变Event
            {
                OriginalVal = _cells["值变事件"].GetOriginalVal<string>(),
                NewVal = 值变事件,
            });
        }
    });

    OnDeleting(() =>
    {
        if (禁止删除)
        {
            Throw.Msg("已选中[禁止删除]，删除前校验不通过！");
        }

        if (发布删除事件)
        {
            AddEvent(new 删除Event { Tgt = this });
        }
        return Task.CompletedTask;
    });

    OnChanging(c限长4, e =>
    {
        e.NewVal = e.Str.ToUpper();

        // 内部已有默认的超长校验，按照数据库字段长度
        Throw.If(e.GbkLength > 8, "超出最大长度", e.Cell);
        Throw.If(e.Str.Length > 8, "超出最大长度", e.Cell);
        Throw.If(e.Utf8Length > 8, "超出最大长度", e.Cell);
        Throw.If(e.UnicodeLength > 8, "超出最大长度", e.Cell);
    });

    OnChanging(c禁止选中, e =>
    {
        Throw.If(e.Bool, "[禁止选中]列无法选中");
    });

}
{{< /highlight >}}

### 代码调用
实体视图的配置
{{< highlight cs >}}
public class EntityCfg
{
    /// <summary>
    /// 实体类全名，包括程序集名称
    /// </summary>
    public string Cls { get; set; }

    /// <summary>
    /// 查询面板的xaml
    /// </summary>
    public string QueryFvXaml { get; set; }

    /// <summary>
    /// 列表配置
    /// </summary>
    public EntityListCfg ListCfg { get; set; }

    /// <summary>
    /// 表单配置
    /// </summary>
    public EntityFormCfg FormCfg { get; set; }

    /// <summary>
    /// 一对多时子实体的父表主键字段名
    /// </summary>
    public string ParentID { get; set; }

    /// <summary>
    /// 是否一对多的子表
    /// </summary>
    public bool IsChild { get; set; }
}
{{< /highlight >}}

内含Lv Menu的Tab，名称为List，其配置
{{< highlight cs >}}
public class EntityListCfg
{
    /// <summary>
    /// Lv的XAML
    /// </summary>
    public string Xaml { get; set; }

    /// <summary>
    /// Tab标题
    /// </summary>
    public string Title { get; set; }

    /// <summary>
    /// 是否显示添加菜单
    /// </summary>
    public bool ShowAddMi { get; set; } = true;

    /// <summary>
    /// 是否显示删除菜单
    /// </summary>
    public bool ShowDelMi { get; set; } = true;

    /// <summary>
    /// 是否显示多选菜单
    /// </summary>
    public bool ShowMultiSelMi { get; set; } = true;
}
{{< /highlight >}}

内含Fv Menu的Tab，名称为Form，其配置
{{< highlight cs >}}
public class EntityFormCfg
{
    /// <summary>
    /// 表单的XAML
    /// </summary>
    public string Xaml { get; set; }

    /// <summary>
    /// Tab标题
    /// </summary>
    public string Title { get; set; }

    /// <summary>
    /// 是否显示添加菜单
    /// </summary>
    public bool ShowAddMi { get; set; } = true;

    /// <summary>
    /// 是否显示保存菜单
    /// </summary>
    public bool ShowSaveMi { get; set; } = true;

    /// <summary>
    /// 是否显示删除菜单
    /// </summary>
    public bool ShowDelMi { get; set; } = true;
}
{{< /highlight >}}

打开通用单表视图窗口
{{< highlight cs >}}
GenericView.OpenSingleTbl(new EntityCfg { Cls = "Demo.Base.基础X,Demo.Base" });
{{< /highlight >}}





## 通用一对多视图

### 菜单配置
在菜单管理中添加新菜单项，视图名称选择`通用一对多视图`，然后`编辑`视图参数
{{< bilibili id=BV1wq5rzgEry >}}

### 代码调用
打开通用一对多视图窗口
{{< highlight cs >}}
OneToManyCfg cfg = new OneToManyCfg();
cfg.IsUnionForm = true;
cfg.ParentCfg = new EntityCfg { Cls = "Demo.Base.父表X,Demo.Base" };
cfg.ChildCfgs.Add(new EntityCfg { Cls = "Demo.Base.大儿X,Demo.Base", ParentID = "parent_id", IsChild = true });
cfg.ChildCfgs.Add(new EntityCfg { Cls = "Demo.Base.小儿X,Demo.Base", ParentID = "group_id", IsChild = true });

GenericView.OpenOneToMany(cfg);
{{< /highlight >}}




## 通用多对多视图

### 菜单配置
在菜单管理中添加新菜单项，视图名称选择`通用多对多视图`，然后`编辑`视图参数
{{< bilibili id=BV1YR5rzoEux >}}

### 代码调用
打开通用多对多视图窗口
{{< highlight cs >}}
var cfg = new ManyToManyCfg();
cfg.MainCfg = new EntityCfg { Cls = "Demo.Base.角色X,Demo.Base" };
cfg.RelatedCfgs.Add(
    new RelatedEntityCfg
    {
        RelatedCls = "Demo.Base.用户X,Demo.Base",
        MiddleCls = "Demo.Base.用户角色X,Demo.Base",
        MainFk = "role_id",
        RelatedFk = "user_id"
    });
cfg.RelatedCfgs.Add(
    new RelatedEntityCfg
    {
        RelatedCls = "Demo.Base.权限X,Demo.Base",
        MiddleCls = "Demo.Base.角色权限X,Demo.Base",
        MainFk = "role_id",
        RelatedFk = "prv_id"
    });

GenericView.OpenManyToMany(cfg)
{{< /highlight >}}