# validation 

Bean Validation 1.0（JSR-303）是一个校验规范。

在spring Boot项目由于自带了hibernate validator 5(http://hibernate.org/validator/)实现。

## 规范

JSR-303原生支持的限制有如下几种：

限制 | 说明
--- | ---
@Null | 只能为null
@NotNull | 必须不为null
@AssertFalse | 必须为false
@AssertTrue | 必须为true
@DecimalMax(value) | 必须为一个不大于指定值的数字
@DecimalMin(value) | 必须为一个不小于指定值的数字
@Digits(integer,fraction) | 必须为一个小数，且整数部分的位数不能超过integer，小数部分的位数不能超过fraction
@Future | 必须是一个将来的日期
@Max(value) | 必须为一个不大于指定值的数字
@Min(value) | 必须为一个不小于指定值的数字
@Past | 必须是一个过去的日期
@Pattern(value) | 正则表达式
@Size(max,min) | 字符长度必须在min到max之间

除此之外，hibernate也还提供了其它的限制校验,在org.hibernate.validator.constraints包下

@NotBlank(message =) 验证字符串非null，且长度必须大于0
@Email 被注释的元素必须是电子邮箱地址
@Length(min=,max=) 被注释的字符串的大小必须在指定的范围内
@NotEmpty 被注释的字符串的必须非空
@Range(min=,max=,message=) 被注释的元素必须在合适的范围内

在Controller中加上对RequestBody的校验：——@Valid 对po实体类进行校验

## BindingResult 

### 一般用法，从Controller传入
```java
    @ApiOperation(value = "創建應用程序")
    @RequestMapping(value = "createResourceApplication", method = RequestMethod.POST)
    public Response<Boolean> createResourceApplication(@Valid @RequestBody CreateResourceApplicationRequestDTO requestDTO, BindingResult bindingResult) {

        if(bindingResult.hasErrors()){
            List<ObjectError> ls=bindingResult.getAllErrors();
            for (int i = 0; i < ls.size(); i++) {
                System.out.println("error:"+ls.get(i));
            }
        }
    }
```

### 全局拦截
```java
@ExceptionHandler(MethodArgumentNotValidException.class)
    @ResponseBody
    public Response<String> handleMethodArgumentNotValidException(MethodArgumentNotValidException e) {

        logger.error("驗證錯誤", "detail:" + e.getMessage());

        Response<String> response = Response.create(500, e.getBindingResult().getAllErrors().get(0).getDefaultMessage(), null);
        return response;
    }
```