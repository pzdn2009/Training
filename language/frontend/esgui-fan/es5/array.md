# Array

* forEach
* map

## 1. forEach

```javascript
//原型：回调(value,index,array)
[].forEach(function(value, index, array) {
    // ...
});
//原型：
array.forEach(callback,[ thisObject])

[1, 2 ,3, 4].forEach(alert);
[1, 2 ,3, 4].forEach(console.log);

var database = {
  users: ["张含韵", "江一燕", "李小璐"],
  sendEmail: function (user) {
    if (this.isValidUser(user)) {
      console.log("你好，" + user);
    } else {
      console.log("抱歉，"+ user +"，你不是本家人");    
    }
  },
  isValidUser: function (user) {
    return /^张/.test(user);
  }
};

// 给每个人法邮件
database.users.forEach(  // database.users中人遍历
  database.sendEmail,    // 发送邮件
  database               // 使用database代替上面标红的this
);

// 结果：
// 你好，张含韵
// 抱歉，江一燕，你不是本家人
// 抱歉，李小璐，你不是本家
```

## 2.map

```javascript
array.map(callback,[ thisObject]); //原型
[].map(function(value, index, array) {
    // ...
});

var data = [1, 2, 3, 4];
var arrayOfSquares = data.map(function (item) {
  return item * item;
});

alert(arrayOfSquares); // 1, 4, 9, 16
```

