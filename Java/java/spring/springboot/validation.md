# validation 

Bean Validation 1.0（JSR-303）是一个校验规范。

在spring Boot项目由于自带了hibernate validator 5(http://hibernate.org/validator/)实现。

JSR-303原生支持的限制有如下几种：

限制 | 说明
--- | ---
@Null | 限制只能为null
@NotNull | 限制必须不为null
@AssertFalse | 限制必须为false
@AssertTrue | 限制必须为true
@DecimalMax(value) | 限制必须为一个不大于指定值的数字
@DecimalMin(value) | 限制必须为一个不小于指定值的数字
@Digits(integer,fraction) | 限制必须为一个小数，且整数部分的位数不能超过integer，小数部分的位数不能超过fraction
@Future | 限制必须是一个将来的日期
@Max(value) | 限制必须为一个不大于指定值的数字
@Min(value) | 限制必须为一个不小于指定值的数字
@Past | 限制必须是一个过去的日期
@Pattern(value) | 定的正则表达式
@Size(max,min) | 限制字符长度必须在min到max之间

除此之外，hibernate也还提供了其它的限制校验,在org.hibernate.validator.constraints包下

@NotBlank(message =) 验证字符串非null，且长度必须大于0
@Email 被注释的元素必须是电子邮箱地址
@Length(min=,max=) 被注释的字符串的大小必须在指定的范围内
@NotEmpty 被注释的字符串的必须非空
@Range(min=,max=,message=) 被注释的元素必须在合适的范围内

@Valid 对po实体类进行校验

BindingResult 
```java
 if(result.hasErrors()){  
            List<ObjectError> ls=result.getAllErrors();  
            for (int i = 0; i < ls.size(); i++) {  
                System.out.println("error:"+ls.get(i));  
            }  
        }  
```