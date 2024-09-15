## 异常规范

> - 逐层往上抛，在最外层统一处理
> - 自定义父类接收所有业务异常
> - Exception接收所有系统异常

![1726381290550](SpringBoot_全局异常处理.assets/1726381290550.png)



## SpringBoot 中的异常处理

![](SpringBoot_全局异常处理.assets/1726378793681.png)



### 方案一：

> - 在Controller层 用try{ }catch( ){ } 统一处理
>   - 返回信息臃肿
>   - 挨个方法进行包裹，代码冗余



### 方案二：全局异常处理

![1726382068085](SpringBoot_全局异常处理.assets/1726382068085.png)



> - 流程概览 --- 在Controller层监听所有异常
>   - 创建异常处理器类
>     - @RestControllerAdvice
>       - @ControllerAdvice + @ResponseBody
>       - 可以封装返回结果
>   - 定义处理异常的方法
>     - @ExceptionHandler( )



> - 异常处理类

```java
/**
 * 全局异常处理器类
 */

@RestControllerAdvice
//@RestControllerAdvice 包含了 @ControllerAdvice(aop增强) + @ResponseBody(可以直接封装返回结果)
//@ControllerAdvice 包含了 @Component(IOC创建此处理器对象处理异常)

public class GlobalExceptionHandler {

    //系统异常
    @ExceptionHandler(Exception.class)
    //标明当前方法是一个可以处理系统异常的方法
    public Result handleSysException(Exception e){

        //打印堆栈信息-用于定位问题
        e.printStackTrace();

        //返回结果给前端
        return Result.error("对不起,操作失败");
    }

    //业务异常
    @ExceptionHandler(BaseException.class)
    //标明当前方法是一个可以处理系统异常的方法
    public Result handleSysException(UsernameNotFoundException e){

        //打印堆栈信息-用于定位问题
        e.printStackTrace();

        //返回结果给前端
        return Result.error(e.getMessage());
    }

}
```



> - 定义业务异常父类

```java
public class BaseException extends RuntimeException{
    public BaseException() {
    }

    public BaseException(String message) {
        super(message);
    }
}

```



> - 定义具体异常的子类

> username

```java
/**
 * 自定义用户名异常
 */

public class UsernameNotFoundException extends BaseException{
    public UsernameNotFoundException(String message) {
        super(message);
    }

    public UsernameNotFoundException() {
    }
}

```



> passward

```java
/**
 * 自定义用户名异常
 */

public class PasswardErrorException extends BaseException{
    public PasswardErrorException(String message) {
        super(message);
    }
    public PasswardErrorException() {
    }
}
```





