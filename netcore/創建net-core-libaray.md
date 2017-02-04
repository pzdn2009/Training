# 創建.net core libaray 

test項目的project.json：
```
{
  "version": "1.0.0-*",

  "dependencies": {
    "Library": {
      "target": "project",
      "version": "1.0.0.*"
    },
    "NETStandard.Library": "1.6.0",
    "xunit": "2.2.0-beta5-build3474",
    "dotnet-test-xunit": "2.2.0-preview2-build1029"
  },

  "frameworks": {
    "netcoreapp1.0": {
      "imports": [ "dnxcore50", "portable-net45+win8" ]
    }
  },
  "testRunner": "xunit",
  "runtimes": {
    "win7-x64": {}
  }
}
```

- Library：引用的類庫項目。

  这是为了避免将 Library项目解析到具有相同名称的 NuGet 包。 将目标显式设置为“项目”可确保工具首先搜索具有此名称的项目，而不是包。
- "xunit": "2.2.0-beta5-build3474", "dotnet-test-xunit": "2.2.0-preview2-build1029"，"testRunner": "xunit",為配置單元測試。
- 编辑 project.json 并将 "imports": "dnxcore50" 替换为 "imports": [ "dnxcore50", "portable-net45+win8" ]。

  这使项目可正确恢复和使用 xunit 库：已将这些库编译为与可移植配置文件一起使用，这些配置文件中包括“portable-net45+win8”。
- Can not find runtime target for framework '.NETCoreApp,Version=v1.0' compatible with one of the target runtimes: 'win7-x64'. 

 添加runtimes即可

最後，運行所有測試
