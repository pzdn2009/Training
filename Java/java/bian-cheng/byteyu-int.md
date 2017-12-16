# byte 與 int

byte的範圍：-128~127

## 1、byte与int转换 

```java
public static byte intToByte(int x) {   
    return (byte) x;   
}   
public static int byteToInt(byte b) {   
//Java 总是把 byte 当做有符处理；我们可以通过将其和 0xFF 进行二进制与得到它的无符值 
    return b & 0xFF;   
} 
```
 
## 2、byte[]与int转换 
```java
BytesHelper.toInt();
BytesHelper.fromInt();
```