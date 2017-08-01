# 異常處理

1. 新建一个类GlobalDefaultExceptionHandler，
2. 在class注解上@ControllerAdvice
3. 在方法上注解上@ExceptionHandler(value = Exception.class)
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
}
```

@ResponseEntity

