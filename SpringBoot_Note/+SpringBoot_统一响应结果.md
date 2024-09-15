## 概述

> - 统一返回结果是一种常见的设计模式，目的是让所有 API 或服务的响应结构一致，便于前端处理和错误追踪。
> - 统一返回结果通常包含一些固定的字段，如状态码、消息、数据和错误信息等。
> - 本质是个工具类

### 结构一般如下

```json
{
  "code": 200,            // 状态码，如 200 表示成功
  "message": "success",   // 消息，表明请求的处理结果
  "data": {},             // 返回的数据,可能是对象、列表或 null
}
```



### Java实现

```java
@Data
@NoArgsConstructor
@AllArgsConstructor
public class Result {
    private Integer code;//响应码，1 代表成功; 0 代表失败
    private String msg;  //响应信息 描述字符串
    private Object data; //返回的数据
	
    
    //静态方法
    //成功-无数据
    public static Result success(){
        return new Result(1,"success",null);
    }
    /成功-有数据
    public static Result success(Object data){
        return new Result(1,"success",data);
    }
    //失败响应
    public static Result error(String msg){
        return new Result(0,msg,null);
    }
}

```



## 使用场景

> - 叠加全局异常处理使用
>   - 为了确保 API 响应的统一性，可以使用 Spring 的 `@ControllerAdvice` 注解来捕获所有的异常，并返回统一格式的错误信息。