# Key

**1） 標誌：GUID也是標誌列，自增的一種；**

**2） GUID，自增，分佈式自增；** select newid\(\)；以及順序主鍵newsequentialid\(\)

**3） Usage：**

模型：

```csharp
modelBuilder.Entity<Foo>().Property(o => o.Id).HasDatabaseGeneratedOption(DatabaseGeneratedOption.Identity);
```

標籤：

```csharp
[Key, DatabaseGenerated(DatabaseGeneratedOption.Identity)]
```

