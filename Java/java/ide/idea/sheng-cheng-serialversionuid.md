# 生成serialVersionUID

Steps：

1. 点击File->Setting->Plugins->Browse Repositories，然后搜索GenerateSerialVersionUID的插件，下载、安装后关闭IDEA，然后再打开项目。

2. 默认情况下IntellijIDEA是关闭了继承了java.io.Serializable的类生成serialVersionUID的警告。如果需要idea提示生成serialVersionUID，那么需要做以下设置：
 * File->setting->Inspections->Serialization issues，将其展开后将serialzable class without "serialVersionUID"打上勾；
 * 将光标放到类名上，按alt＋enter键，就会提示生成serialVersionUID了。
