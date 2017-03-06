# Migration

基本命令
```
Enable-Migration
Add-Migration
Update-Database
```
**1）	Update-Database : 无法将“Update-Database”项识别为 cmdlet、函数、脚本文件或可运行程序的名称
**
EF所在的項目**必須有package.config**，且有EF的包配置，才可以更新成功。
```xml
<packages>
  <package id="EntityFramework" version="6.1.3" targetFramework="net46" />
</packages>
```

**2）	Update-Database -Verbose 可以查看詳細的SQL語句**

**3）	Update-Database –Force 可以強制執行**

**4）	禁用遷移檢查：**

```csharp
Database.SetInitializer<CouponContext>(null);
```

```xml
<contexts>
  <context type="YourNamespace.YourDbContext, YourAssemblyName" disableDatabaseInitialization="true"/>
</contexts>
```