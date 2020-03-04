# Form 表单

分类：

* Form属性
* Form方法
* Form事件
* Form-Item属性
* Form-Item插槽
* Form-Item方法

## 示例1

```markup
<el-form ref="form" :model="form" label-width="80px">
 <el-form-item label="活动名称">
 <el-input v-model="form.name"></el-input>
 </el-form-item>

 <el-form-item label="活动区域">
 <el-select v-model="form.region" placeholder="请选择活动区域">
 <el-option label="区域一" value="shanghai"></el-option>
 <el-option label="区域二" value="beijing"></el-option>
 </el-select>
 </el-form-item>

 <el-form-item
 <el-button type="primary" @click="onSubmit">立即创建</el-button>
 <el-button>取消</el-button>
 </el-form-item>

</el-form>
```

技术点：

* 组成结构： 在 Form 组件中，每一个表单域由一个 Form-Item 组件构成，表单域中可以放置各种类型的表单控件，包括 Input、Select、Checkbox、Radio、Switch、DatePicker、TimePicker。
* el-form, el-form-item, el-input\|el-select\|el-data-picker\|el-switch\| el-checkbox-group\| el-radio-group\|
* 使用**model属性**来绑定表单数据
* 当前表单的引用ref="form"，公布对外引用
* label-width设置表单的宽度
* :inline=true设置为行内表单
* :label-position 属性可以改变表单域标签的位置，left、right、top

## 示例2

* Form 组件提供了表单验证的功能，只需要通过 **rules** 属性传入约定的验证规则，并 **Form-Item 的 prop** 属性设置为需校验的字段名即可。

```markup
<el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="100px" class="demo-ruleForm"> 

<el-form-item label="活动时间" required> 
<el-form-item> 
  <el-button type="primary" @click="submitForm('ruleForm')">立即创建</el-button> 
  <el-button @click="resetForm('ruleForm')">重置</el-button> 
</el-form-item>
```

校验规则：

```javascript
rules: {
 name: [
 { required: true, message: '请输入活动名称', trigger: 'blur' },
 { min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur' }],

 region: [
 { required: true, message: '请选择活动区域', trigger: 'change' }],

 date1: [
 { type: 'date', required: true, message: '请选择日期', trigger: 'change' }],

 date2: [
 { type: 'date', required: true, message: '请选择时间', trigger: 'change' }],

 type: [
 { type: 'array', required: true, message: '请至少选择一个活动性质', trigger: 'change' }],

 resource: [
 { required: true, message: '请选择活动资源', trigger: 'change' }
 ],

 desc: [
 { required: true, message: '请填写活动形式', trigger: 'blur' }
 ]
}

methods: {

 submitForm(formName) {
 this.$refs[formName].validate((valid) => {
 if (valid) {
 alert('submit!');
 } else {
 console.log('error submit!!');
 return false;
 }
 });
 },

 resetForm(formName) {
 this.$refs[formName].resetFields();
 }
 }
```

技术点：

* 定义rules，ref
* 验证：min,max,message,trigger,required,type
* trigger：blur,change
* type: date,array
* submitForm，调用validate方法，内置一个回调。
* resetForm

