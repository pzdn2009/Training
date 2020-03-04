# WordUtils

* capitalize 首字母大寫
* capitalizeFully 首字母大寫，其他全小寫
* uncapitalize 首字母小寫
* initials 獲取首字母
* swapCase 大小寫相反
* wrap 解析\n和\t

Sample:

```java
@Test
public void wordUtilsDemo() {
    String str1 = "wOrD";
    String str3 = "DSFS";
    String str2 = "ghj\nui\tpo";
    System.out.println("首字母大写:" + WordUtils.capitalize(str1));
    System.out.println("首字母大写:" + WordUtils.uncapitalize(str3));
    System.out.println("首字母大写其它字母小写:" + WordUtils.capitalizeFully(str1));
    char[] ctrg = {'.'};
    System.out.println("在规则地方转换:" + WordUtils.capitalizeFully("i aM.fine", ctrg));
    System.out.println("获取首字母:" + WordUtils.initials(str1));
    System.out.println("取每个单词的首字母:" + WordUtils.initials("Ben John Lee", null));
    char[] ctr = {' ', '.'};
    System.out.println("按指定规则获取首字母:" + WordUtils.initials("Ben J.Lee", ctr));
    System.out.println("大小写逆转:" + WordUtils.swapCase(str1));
    System.out.println("解析\\n和\\t等字符:" + WordUtils.wrap(str2, 1));
}
```

Output：

```text
首字母大写:WOrD
首字母大写:dSFS
首字母大写其它字母小写:Word
在规则地方转换:I am.Fine
获取首字母:w
取每个单词的首字母:BJL
按指定规则获取首字母:BJL
大小写逆转:WoRd
解析\n和\t等字符:ghj
ui    po
```

