# OptimisticConcurrency

```csharp
[Timestamp]
public byte[] RowVersion { get; set; }

this.Property(p => p.RowVersion).IsRowVersion();  //时间戳
```

生成的SQL語句加上了版本號了。

如果執行失敗，則會報異常：

```csharp
catch (DbUpdateConcurrencyException ex)
{           
    Console.WriteLine(ex.Entries.First().Entity.GetType().Name + " 保存失败");
}
```

對於非\[TimeStamp\]類型的，使用ConcurrencyCheck來標記。

