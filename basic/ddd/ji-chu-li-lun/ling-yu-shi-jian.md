# 領域事件

## 領域事件

## 1. 概念

DE表示領域中所發生的事情。 “當。。。。。。” “如果發生。。。。。。” “當。。。。。。的時候，請通知我” “發生。。。。。。時”

聚合創建發佈事件。

## 2. 建模

命令方法：commitTo

事件輸出：Committed

一般使用構造函數傳遞參數，因為代表一類經常不變的數據。

Entity裡面也可以使用領域事件。

## 3. 事件存儲

1. 將事件存儲作為一個消息隊列來使用，該消息隊列的作用是將所有的領域事件通過消息設施發佈出去。它允許在不同的界限上下文之間進行集成。
2. 檢查由模型的命令方法所產生的所有結果的歷史記錄。可用於跟蹤BUG，不僅僅是一個審計日誌，且包含了聚合命令方法所產生的完整結果。
3. 使用時間存儲中的數據來進行業務預測和分析。
4. 使用事件來重建該聚合實例。
5. 撤銷對聚合的操作。

EventStore.append執行事件的存儲。

```csharp
public class StoredEvent{
  public long EventId { get; set;}
  public object EventBody { get; set;}
  public DateTime OccurredOn { get; set;}
  public string TypeName { get; set;}
}
```

