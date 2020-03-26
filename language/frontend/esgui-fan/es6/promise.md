# ES6-Promise

# 1. 基本概念

Ref：https://www.jianshu.com/p/d2f61e8795d2
https://www.jianshu.com/p/68ff093756a2


* 名称： 译为“承诺”，这也就表达了将来会执行的操作，代表异步操作；
* 状态： 一共有三种状态，分别为`pending`（进行中）、`fulfilled`（已成功）和`rejected`（已失败）。
* 特点：
 * 只有异步操作可以决定当前处于的状态，并且任何其他操作无法改变这个状态；
 * 一旦状态改变，就不会在变。状态改变的过程只可能是：从pending变为fulfilled和从pending变为rejected。如果状态发生上述变化后，此时状态就不会在改变了，这时就称为resolved（已定型）
 
# 2. Usage

创建一个Promise：

```js
var promise = new Promise((resolve,reject)=>{
  // ... 
  if (/* 异步操作成功 */){
    resolve(res);
  } else {
    reject(error);
  }
})

//Promise 实例生成以后，可以用then方法分别指定Resolved状态和Reject状态的回调函数。
var promise = new Promise((resolve,reject)=>{
  // ... 
}).then(res=>{
     //成功
     resolve(res);
  }, err=>{
    //失败
     reject(err);
  })
```

# 3. axios示例

```js
import axios from 'axios';
import qs from 'qs';
let instance = axios.create({
  baseURL: '',  // 基础url前缀
  timeout: 10000 
});
function fetchApi(url){
  return new Promise((resolve,reject)=>{
    //指定参数
    let param = {
      //...
    }
    
    //指定配置
    let config = {
       headers: {'Content-Type': "multipart/form-data"}
    };
    
    //发送post
    instance.post(url, qs.stringify(param), config)
      .then(response => {
        resolve(response.data);
      }, err => {
        reject(err);
      })
      .catch((error) => {
        reject(error)
      })
  })
}
function login() {
    return fetchApi('/login')
}
login();
```