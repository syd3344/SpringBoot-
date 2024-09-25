## 概述

> - 为简化而生，只做增强，不做改变，为简化开发，提高效率而生
> - 适用于单表查询
>   - 节省了大量单表的CRUD的时间
>   - 我只能说一句，太爽啦！！！！！
> - `BaseMapper` 接口提供了基本的 CRUD 方法，但你仍然可以在自定义复杂查询时，手动写 `Mapper` 方法，保持与数据库的清晰交互。
> - 官网：https://baomidou.com



## 特性

> - 无侵入
>   - 只做增强不做改变，引入他不会对工程有任何影响，丝滑
> - 强大的CRUD操作
>   - 内置通用mapper，通用service，仅通过少量配置即可完成单表的大部分CRUD操作
> - 支持Lambda形式调用
> - 内置分页插件
>   - 支持多种数据库
> - 内置全局拦截插件



## 基本使用

### 导入mybatisplus依赖

```xml
<dependency>
	<groupId>com.baomidou</groupId>
	<artifactId>mybatis-plus-boot-starter</artifactId>
   <version>3.5.7</version>
</dependency>
```

> - 尽量不要同时导入mybatis 和 mybatisPlus 
>   - mybatisPlus 内嵌了 mybatis 的依赖，无需再次导入依赖
>   - 版本不一致容易导致冲突
>

### 连接数据库

```yaml
spring:
  application:
    name: mp-quickstart
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/mp
    username: root
    password: 123456
```

> - driver-class-name
>   - mysql5：com.mysql.jdbc.Driver
>   - mysql8：com.mysql.cj.jdbc.Driver
>   - 高版本兼容低版本

### 定义Mapper

> - 自定义 Mapper 继承 MybatisPlus 提供的 BaseMapper<T> 接口
> - MybatisPlus 是无侵入的，继承接口后不影响原方法执行
> - 范型 - <T> -  传的是User
>   - 本质是User连接的@TableName("tb_user")连接的那张表tb_user
>   - 将该实体类和数据库tb_user映射关联

```
public interface Usermapper extends BaseMapper<User> {

}
```

> - 于是继承了父接口的一堆方法

![1726990664194](mybatisPlus.assets/1726990664194.png)



### 扫描器

> - 在启动类上标注 @MapperScan("com.itheima.mp.mapper")
> - 或者在接口上加 @Mapper



##  配置日志



![1727102945540](mybatisPlus.assets/1727102945540.png)

### yml配置

```
#控制台日志
mybatis-plus:
  configuration:
        log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
```

## CRUD

### insert

> - 自动生成ID
>   - 数据库插入id的默认值为全局唯一ID

#### 主键的生成策略

> - 雪花算法（snowflake）
>   - 分布式ID算法生成器
>   - 结果是一个Long型的ID
>   - 可以保证几乎全球唯一
> - UUID
>   - 没有排序，太长了
> - 自增ID
>   - 一主多从，读写分离时会出现故障
>   - 在实体类ID上增加@TableId（type = IdType.AUTO）
>     - 数据库的字段一定要是自增的，否则会报错

### update

####　**通过条件动态拼接SQL**

#### 自动填充

> - 概述
>
>   - 创建时间，修改时间，自动化完成
>
> - 实现
>
>   - 在实体类中，使用 `@TableField` 注解来标记哪些字段需要自动填充，并指定填充的策略
>
>     -  @TableField ( )
>
>     - fill属性 -- 自动填充策略
>
>       ```java
>       public enum FieldFill {
>           //
>           DEFAULT,
>           //插入时更新
>           INSERT,
>           //更新的时候更新
>           UPDATE,
>           //插入和更新的时候更新
>           INSERT_UPDATE;
>       
>           private FieldFill() {
>           }
>       }
>       ```
>
>       
>
>   - 实现 MetaObjectHandler，编写处理器来处理这个策略
>
>     - 创建一个类来实现 `MetaObjectHandler` 接口，并重写 `insertFill` 和 `updateFill` 方法
>
>       ```java
>       @Component
>       public class MyMetaObjectHandler implements MetaObjectHandler {
>       
>           @Override
>           public void insertFill(MetaObject metaObject) {
>               log.info("开始插入填充...");
>               this.strictInsertFill(metaObject, "createTime", LocalDateTime.class, LocalDateTime.now());
>           }
>       
>           @Override
>           public void updateFill(MetaObject metaObject) {
>               log.info("开始更新填充...");
>               this.strictUpdateFill(metaObject, "updateTime", LocalDateTime.class, LocalDateTime.now());
>           }
>       }
>       ```
>
>   
>
>   - 配置自动填充处理器
>     - 确保 `MyMetaObjectHandler` 类被 Spring 管理
>       - 通过 `@Component` 或 `@Bean` 注解来实现。

### select

> -   单个ID
> - usermapper.selectById(1);

> - 批量ID
> - usermapper.selectBatchIds(Arrays.asList(1,2,3));

> - 条件查询
> -  usermapper.selectByMap(map);

![1726820443156](mybatisPlus.assets/1726820443156.png)

```
 Map map = new HashMap();
        map.put("id", 7);
        map.put("password", "88888");
        List list = usermapper.selectByMap(map);
```



#### 分页查询及拦截器

> - 拦截器

```java
 /*mybatis-plus分页拦截器*/
    /*拦截一个,拼接一个*/
    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor(){
        MybatisPlusInterceptor mybatisPlusInterceptor = new MybatisPlusInterceptor();
        mybatisPlusInterceptor.addInnerInterceptor(new PaginationInnerInterceptor());
        return mybatisPlusInterceptor;
    }
```

> - 分页

> -  Page<User> page = new Page<>(pageNum, pageSize);
>   - 参数一：当前页
>   - 参数二：页面大小
> - sermapper.selectPage(page, wapper);
>   - wapper：条件构造器
> - 所有的数据又封装进page

![1726822879297](mybatisPlus.assets/1726822879297.png)

```java
 void selectPage() {
        int pageNum = 1;/*第一页*/
        int pageSize = 2;/*每页数量*/
        Page<User> page = new Page<>(pageNum, pageSize);
        usermapper.selectPage(page, null);
        long pages = page.getPages();
        long total = page.getTotal();
        List<User> records = page.getRecords();

        System.out.println(pages);/*页数*/
        System.out.println(total);/*总数*/
        System.out.println(records);/*返回数据*/

    }
```



### delete

> - 批量删除
> - ID删除
> - map删除

### 逻辑删除

> **概念**
>
> - 物理删除
>   - 从数据库中直接移除
> - 逻辑删除
>   - 在数据库中没有删除，通过一个变量让他失效
>   - deleted = 0 => deleted = 1;
>   - 管理员可以查看被删除的记录，防止数据丢失，类似于回收站！
>
> **实现**
>
> - 在数据表中增加一个deleted字段
>
> - 在字段上加上注解 @TableLogic
>
> - ![1727096242398](mybatisPlus.assets/1727096242398.png)
>
> -  配置逻辑删除
>
> - ```yml
> #所有的实体类
> mybatis-plus:
> global-config:
> db-config:
>   #:前缀
>   table-prefix: tb_
>   #:主键自增
>   id-type: auto
>   #控制台日志
>   #配置逻辑删除
>   logic-delete-value: 1
>   logic-not-delete-value: 0
>   ```
>
> 
>
> 
>
> - 本质走的是更新操作，并不是删除
> - ![1727096848359](mybatisPlus.assets/1727096848359.png)
> ```
> 
> ```







## 常用注解

### @TableName("tb_user")

![1726819876263](mybatisPlus.assets/1726819876263.png)

###　@TableId（type = IdType.NONE）

```java
public @interface TableId {
    String value() default "";

    IdType type() default IdType.NONE;
}

```

> - type参数为枚举类型

**IdType类型**

```Java
public enum IdType {
    //自增
    AUTO(0),
    //不使用
    NONE(1),
    //手动输入，
    //此时ID值为null，需要手动写ID
    INPUT(2),
    //默认全局ID
    ASSIGN_ID(3),
    //全局唯一ID
    ASSIGN_UUID(4);

    private final int key;

    private IdType(int key) {
        this.key = key;
    }

    public int getKey() {
        return this.key;
    }
}
```



###  @TableField ("user_name")

> - l驼峰
> -  /* 起别名--绑定 */

```
  @TableField ("user_name")
   	 private String userName;
```



### @Version

#### 乐观/悲观锁

> - 乐观锁是一种并发控制机制，用于确保在更新记录时，该记录未被其他事务修改。
> - 乐观：总认为不会出现问题，无论干什么都不去上锁，如果出现问题在测试加锁
> - 悲观：认为总会出现问题，无论干什么都会上锁

#### 实现

> - 给数据库中增加Version字段
> - 给实体类增加对应的字段
> - ![1727080103366](mybatisPlus.assets/1727080103366.png)
> -  注册组件
> - ![1727080077653](mybatisPlus.assets/1727080077653.png)
> - ![1727081632054](mybatisPlus.assets/1727081632054.png)



## 性能分析插件

> - 



## 条件构造器 -- Wrapper

![1726824718705](mybatisPlus.assets/1726824718705.png)

### 概述

> - Wrapper是个接口和map类似
>   - UpdateWrapper
>   - QueryWrapper
>     - QueryWrapper<User> wrapper = new QueryWrapper<>();
>     - select *from tb_user where id = 2;
>     - delete from tb_user where id = 2;
>     - 查询语句完全一致，故没必要新建一个DeleteWrapper()
>   - LambdaUpdateWrapper
> -  和map类似，最好加个范型（对哪张表操作）

### UpdateWrapper



### QueryWrapper

```java
QueryWrapper<User> wrapper = new QueryWrapper<>();
        wrapper.eq("name", "dengdeng")
                    /*.or()*/
                .ge("age", 18);
```



```java
QueryWrapper<User> wrapper = new QueryWrapper<>();
        wrapper
                .like("name", "李")/*全模糊*/
                .notLike("name", "李")/*不包含*/
                .likeLeft("name", "李")/*左模糊*/
                .likeRight("name", "李")/*右模糊*/

                /*.or()*/
                .ge("age", 18);
```



### LambdaUpdateWrapper

```java
LambdaUpdateWrapper<User> wrapper = new LambdaUpdateWrapper<>();
        wrapper.eq(User::getId,1)
                .set(User::getPassword,8888);
```



> - 好处
>   - 不再是和字段名强绑定，而是和实体类属性绑定
>   - 数据库若换名字，则没关系



### 子查询

> - 



## 封装service

![1727262848755](mybatisPlus.assets/1727262848755.png)

> - 实体类    ----    User
> - Mapper接口    ----    继承BaseMapper<User>接口    ----    直接获取MybatisPlus封装的CRUD的方法实现
> - UserService接口    ----    继承IService<User>接口    ----    定义了一系列 CRUD 操作的规范，但并没有提供具体的实现
> - UserServiceImpl实现类    
>   - ----    实现UserService接口    ----    只是规则上的约束，无实际意义
>   - ----    继承ServiceImpl<Usermapper,User>实现类    ----    获得具体的CRUD方法实现
>     - ServiceImpl 是 IService 的具体实现类



> - 实体类

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
//捆绑表
@TableName("tb_user")
public class User {
    //id自增
    @TableId(type = IdType.AUTO)
    Long id ;
    //绑定字段
    @TableField("user_name")
    String userName;
    String password;
    String name;
    Integer age;
    String email;
}
```



> - mapper接口

```java
@Mapper
public interface Usermapper extends BaseMapper<User>{

}
```



> - service接口

```java
@Service
public interface UserService extends IService<User>{
}

```



> - service实现类

```java
public class UserServiceImpl extends ServiceImpl<Usermapper,User> implements UserService{
    @Autowired
    private Usermapper usermapper;

    public User getUser(Integer id){

        //service层的CRUD
        User byId = getById(id);
        //Mapper层的CRUD
        User user = usermapper.selectById(id);

        return user;
    }

}
```









