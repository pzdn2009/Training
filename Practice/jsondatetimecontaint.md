# 時間T處理

Web API 默認的序列化時間：2015-02-10T15:18:21.7046433+08:00

# 1. 方案1

Formatters：
```csharp
GlobalConfiguration.Configuration.Formatters.JsonFormatter.SerializerSettings.Converters.Add(
                new Newtonsoft.Json.Converters.IsoDateTimeConverter()
                {
                    DateTimeFormat = "yyyy-MM-dd HH:mm:ss"
                }
            );

```

# 2. 方案2

Formatters添加自定義Converter：
```csharp
GlobalConfiguration.Configuration.Formatters.JsonFormatter.SerializerSettings.Converters.Insert(  
0, new JsonDateTimeConverter());  
```

實現自定義Convertor：
```
/// <summary>  
/// Json日期带T格式转换  
/// </summary>  
public class JsonDateTimeConverter : IsoDateTimeConverter  
{  
    public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer)  
    {  
        DateTime dataTime;  
        if (DateTime.TryParse(reader.Value.ToString(), out dataTime))  
        {  
            return dataTime;  
        }  
        else  
        {  
            return existingValue;  
        }  
    }  
  
    public JsonDateTimeConverter()  
    {  
        DateTimeFormat = "yyyy-MM-dd HH:mm:ss";  
    }  
}  

```