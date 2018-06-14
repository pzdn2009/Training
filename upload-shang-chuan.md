# Upload 上传

不使用input type="file"，這樣獲取值獲取不到，直接用Upload組件，結合計算屬性即可實現上傳。

## 手动上传例子

```html
<el-form-item label="文件">
  <el-upload
    class="upload-demo"
    ref="upload"
    v-bind:action="uploadUrl"
    :on-preview="handlePreview"
    :on-remove="handleRemove"
    :file-list="fileList"
    :auto-upload="false">
    <el-button slot="trigger" size="small" type="primary">选取文件</el-button>
    <el-button style="margin-left: 10px;" size="small" type="success" @click="submitUpload">上传到服务器</el-button>
  </el-upload>
</el-form-item>
```

```js
computed: {
  uploadUrl: function () {
    return 'http://localhost:8082/api/ms/upload/' + 
    this.form.name + '?commitMessage=' + this.form.commitMessage;
  }
},

this.$refs.upload.submit();
```

* el-upload,其中action屬性是必須值的，如果需要动态的url，则使用computed来计算
* :auto-upload 设置false
* 调用submit即可上传
