# Something

## TODO

## WEB Worker

REF： [http://blog.csdn.net/aitangyong/article/details/49445307](http://blog.csdn.net/aitangyong/article/details/49445307)

### 1. 什么是Web Worker?

### 2. 检测浏览器对Web Worker支持

### 3. 创建web worker文件

```text
worker新线程使用通过postMessage和onmessage方法：
1.通过postMessage( data ) 方法来向主线程发送数据。
2.绑定onmessage方法来接收主线程发送过来的数据。
```

### 4. 创建Web Worker对象

```text
我们的main.htm(WEB主线程)主要做以下事情：
1.通过 worker = new Worker( url ) 加载一个JS文件来创建一个worker，同时返回一个worker实例。
2.通过worker.postMessage( data ) 方法来向worker发送数据。
3.绑定worker.onmessage方法来接收worker发送过来的数据。
4.使用 worker.terminate() 来终止一个worker,释放占用的资源。
```

### 5. 运行含Web Worker的页面

### 6. Web Worker的跨域访问限制

### 7. Web Worker的异常处理和终止

### 10.使用Blob URL实现Web Worker的inline

[http://www.cnblogs.com/RachelChen/p/5439717.html](http://www.cnblogs.com/RachelChen/p/5439717.html)

## Blob之web圖片裁剪

Ref： [http://www.cnblogs.com/tarol/p/5263050.html](http://www.cnblogs.com/tarol/p/5263050.html)

Blob、File、FileReader、ArrayBuffer、ArrayBufferView、DataURL

DataUrl：

data:\[mimeType\];base64,\[base64\(binaryString\)\]

A blob has its size and MIME type just like a file has.

Ref：[http://www.cnblogs.com/RachelChen/p/5440000.html](http://www.cnblogs.com/RachelChen/p/5440000.html)

## FormData

Ref: [http://www.cnblogs.com/lhb25/p/html5-formdata-tutorials.html](http://www.cnblogs.com/lhb25/p/html5-formdata-tutorials.html) [https://developer.mozilla.org/zh-CN/docs/Web/API/FormData/Using\_FormData\_Objects](https://developer.mozilla.org/zh-CN/docs/Web/API/FormData/Using_FormData_Objects)

## URL.createObjectURL

URL.createObjectURL\(\)方法会根据传入的参数创建一个指向该参数对象的URL. 这个URL的生命仅存在于它被创建的这个文档里. 新的对象URL指向执行的File对象或者是Blob对象.

objectURL = URL.createObjectURL\(blob \|\| file\);

参数:

File对象或者Blob对象

这里大概说下File对象和Blob对象:

File对象,就是一个文件,比如我用input type="file"标签来上传文件,那么里面的每个文件都是一个File对象.

Blob对象,就是二进制数据,比如通过new Blob\(\)创建的对象就是Blob对象.又比如,在XMLHttpRequest里,如果指定responseType为blob,那么得到的返回值也是一个blob对象.

Ref：[http://www.mamicode.com/info-detail-456059.html](http://www.mamicode.com/info-detail-456059.html)

## Stomp协议

Simple \(or Streaming\) Text Orientated Messaging Protocol.

Ref：[http://blog.csdn.net/chszs/article/details/46592777](http://blog.csdn.net/chszs/article/details/46592777)

