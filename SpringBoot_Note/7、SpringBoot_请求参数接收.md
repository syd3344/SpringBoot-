## 键值对

### key名直接接

```
@RequestMapping("/complex")
    public Result test(User user) {
        return Result.success(user);
    }
```



## 一个key多个值

###　数组

```java
//数组 一个key多个值
    @RequestMapping("/arrayParam")
    public Result arrayParam(String[] hobby) {
        return Result.success(hobby);
    }
```



## Json

### @RequestBody

```java
@PostMapping("/create")
public String createUser(@RequestBody User user) {
    // 使用user对象进行处理
    return "User Created: " + user.getName();
}
```



## 可变参数

### @PathVariable

```java
@RequestMapping("/variablePath/{id}/{name}")
    public String jsonTest(@PathVariable int id,@PathVariable String name) {
        log.info(String.valueOf(id)+","+name);
        return "success:"+id+","+name;
    }
```



## 日期

### @DateTimeFormat

```java
@RequestMapping("/dateParam")
    public String timeParam(@DateTimeFormat(pattern = "yyyy年MM月dd日 HH:mm:ss")LocalDateTime updateTime) {
        log.info(String.valueOf(updateTime));
        return "success";
    }
```



## 参数绑定

### @RequestParam

将值绑定和参数绑定

```java
@GetMapping("/users")
public List<User> getUsers(@RequestParam("age") int age, @RequestParam("name") String name) {
    // 使用age和name进行过滤
    return userService.findUsers(age, name);
}
```



