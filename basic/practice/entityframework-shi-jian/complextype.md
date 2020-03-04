# ComplexType

**1） ComplexType標記，類似Value Object。**

**2） 配置位置**

申明：

```csharp
/// <summary>
/// 模板內容
/// </summary>
[ComplexType]
public class CouponTemplateContent
{
    /// <summary>
    /// 模板代碼
    /// </summary>
    public virtual string TemplateCode { get; set; }
}
```

DbContext：

```csharp
protected override void OnModelCreating(DbModelBuilder modelBuilder)
{   
    //CouponTemplateContent是一個複雜類型
    modelBuilder.ComplexType<CouponTemplateContent>().Property(t => t.TemplateCode).IsRequired().HasMaxLength(8).HasColumnName("TemplateCode");
    //。。。。。。。
}
```

