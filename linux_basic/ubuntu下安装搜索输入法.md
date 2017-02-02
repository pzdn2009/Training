# ubuntu下安装搜狗输入法

参考文献：http://jingyan.baidu.com/article/ad310e80ae6d971849f49ed3.html 

Steps：

- 系统设置-->语言支持。
  
  如果没有的安装语言支持的话，就会安装；在这里也可以安装中文的输入法；有了语言支持就可以在右上角选择语言键盘了。
- 打开http://pinyin.sogou.com/linux/?r=pinyin  下载64位的deb
- 然后点击安装，中间会输入密码；
- 输入命令：im-config配置，选择fcitx；
- 输入命令：fcitx-config-gtk3。点击对话框左下角的（+）按钮，弹出另一个对话框如上图所示。然后，取消Only Show Current Language ，然后输入Sogou pinyin，选中点击OK即可；如果（+）了之后看不到，注销，重现登录

![](/assets/sougou1.png)

![](/assets/sougou2.png)

![](/assets/sougou3.png)

