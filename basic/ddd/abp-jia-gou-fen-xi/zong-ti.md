# 總體

## 總體

* N層架構
* 模塊系統
* 啟動配置
* 多租戶
* OWIN集成

ABP集成了：

* 公共結構：Authorization, Validation, Exception Handling, Logging, Localization, Database Connection Management, Setting Management, Audit Logging 
* DDD架構： 分層，模塊，DI，DDD，約定等。

## 1. 分層架構

* 呈現層

  SPA，MPA，ASP.NET Core，SignalR，

* 應用層

  DTO、輸入驗證、AutoMapper、Session

* 領域層

  實體、倉儲、領域事件、工作單元

* 基礎設施層

  提供領域層的接口、EF、NH實現、SMS、Email、DI、Log4net

## 2. 模塊系統

ABP提供構建模塊的基礎設施。

多個模塊構成應用程序。

模塊之間具有依賴關係。

### 2.1 模塊定義

繼承：AbpModule

```csharp
public class MyBlogApplicationModule : AbpModule
{
    public override void Initialize()
    {
        IocManager.RegisterAssemblyByConvention(Assembly.GetExecutingAssembly());
    }
}
```

### 2.2 生命週期方法

* 預初始化,PreInitialize
* 初始化,Initialize
* 后初始化,PostInitialize

> 如果A 依賴 B，啟動順序：PreInitialize-B, PreInitialize-A, Initialize-B, Initialize-A, PostInitialize-B and PostInitialize-A.

### 2.3 模塊依賴

ABP從 startup module 遞歸地解析依賴的模塊并初始化他們，Startup module 最後初始化。

```csharp
[DependsOn(typeof(MyBlogCoreModule))]
public class MyBlogApplicationModule : AbpModule
{
    public override void Initialize()
    {
        IocManager.RegisterAssemblyByConvention(Assembly.GetExecutingAssembly());
    }
}
```

### 2.4 插件模塊

動態地加載插件。IPlugInSource。

```csharp
//asp.net core 
services.AddAbp<MyStartupModule>(options =>
{
    options.PlugInSources.Add(new FolderPlugInSource(@"C:\MyPlugIns"));
});

services.AddAbp<MyStartupModule>(options =>
{
    options.PlugInSources.AddFolder(@"C:\MyPlugIns");
});

//web api 
public class MvcApplication : AbpWebApplication<MyStartupModule>
{
    protected override void Application_Start(object sender, EventArgs e)
    {
        AbpBootstrapper.PlugInSources.AddFolder(@"C:\MyPlugIns");
        //...
        base.Application_Start(sender, e);
    }
}
```

### 2.5 自定義模塊

```csharp
public class MyModule1 : AbpModule
{
    public override void Initialize()
    {
        IocManager.RegisterAssemblyByConvention(Assembly.GetExecutingAssembly());
    }

    public void MyModuleMethod1()
    {
        //this is a custom method of this module
    }
}

[DependsOn(typeof(MyModule1))]
public class MyModule2 : AbpModule
{
    private readonly MyModule1 _myModule1;

    public MyModule2(MyModule1 myModule1)
    {
        _myModule1 = myModule1;
    }

    public override void PreInitialize()
    {
        _myModule1.MyModuleMethod1(); //Call MyModule1's method
    }

    public override void Initialize()
    {
        IocManager.RegisterAssemblyByConvention(Assembly.GetExecutingAssembly());
    }
}
```

### 2.6 Module生命週期

Module classes are automatically registered as **singleton**.

