# 異常處理

1. 新建一个类GlobalDefaultExceptionHandler，
2. 在class注解上@ControllerAdvice
3. 在方法上注解上@ExceptionHandler\(value = Exception.class\)
4. 示例代碼：

```java
@ControllerAdvice
public class GlobalDefaultExceptionHandler {

    private Logger logger = LoggerFactory.getLogger(this.getClass());

//    @ExceptionHandler(value = Exception.class)
//    public void defaultErrorHandler(HttpServletRequest req, Exception e) {
//        e.printStackTrace();
//        logger.debug("GlobalDefaultExceptionHandler.defaultErrorHandler()");
//    }

    @ExceptionHandler(Exception.class)
    @ResponseBody
    public ResponseEntity<Exception> handleException(HttpServletRequest request, Exception e) {
        logger.error("Request FAILD","detail:"+String.format("URL = %s method = %s", request.getRequestURI(), request.getMethod()));
        return new ResponseEntity<Exception>(e, HttpStatus.INTERNAL_SERVER_ERROR);
    }

    /**
     * 处理所有接口数据验证异常
     *
     * @param e
     * @return
     */
    @ExceptionHandler(MethodArgumentNotValidException.class)
    @ResponseBody
    public Response<String> handleMethodArgumentNotValidException(MethodArgumentNotValidException e) {

        logger.error("驗證錯誤", "detail:" + e.getMessage());

        Response<String> response = Response.create(500, e.getBindingResult().getAllErrors().get(0).getDefaultMessage(), null);
        return response;
    }
}
```

* **@ResponseEntity**，可以定义返回的HttpHeaders和HttpStatus。
* **@ExceptionHandler**，定义拦截的异常

```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface ExceptionHandler {

    /**
     * Exceptions handled by the annotated method. If empty, will default to any
     * exceptions listed in the method argument list.
     */
    Class<? extends Throwable>[] value() default {};

}
```

