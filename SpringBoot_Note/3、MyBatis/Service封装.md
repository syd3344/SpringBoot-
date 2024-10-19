## 业务逻辑

> - 假设要实现一个员工管理系统，其中包括两个功能：
>   - **基础功能**：通过 ID 查询员工信息（由 `ServiceImpl` 提供的 `getById` 完成）。
>   - **自定义功能**：员工登录验证（由 `empMapper` 实现的自定义查询完成）。



## 基本配置

### 数据表

```sql
CREATE TABLE `tb_user` (
  `id` bigint NOT NULL AUTO_INCREMENT COMMENT '主键ID',
  `user_name` varchar(20) NOT NULL COMMENT '用户名',
  `password` varchar(20) NOT NULL COMMENT '密码',
  `name` varchar(30) DEFAULT NULL COMMENT '姓名',
  `age` int DEFAULT NULL COMMENT '年龄',
  `email` varchar(50) DEFAULT NULL COMMENT '邮箱',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=8 DEFAULT CHARSET=utf8mb3


```



## 表的映射对象实现

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



## Mapper层接口实现

```java
@Mapper
public interface Usermapper extends BaseMapper<User>{
    
    //继承MybatisPlus的接口-BaseMapper<User>
    //此时已经具备了基础的的CRUD功能,可直接使用
    
}
```



## Service层接口实现

```java
@Service
public interface UserService extends IService<User>{

	//继承MybatisPlus的接口-IService<User>
	//此时已经具备了基础的的CRUD功能,可直接使用
	
}

```



## Service层实现类实现

```java
public class UserServiceImpl extends ServiceImpl<Usermapper,User> implements UserService{
    @Autowired
    private Usermapper usermapper;

    public User getUser(Integer id){

        User byId = getById(id);
        User user = usermapper.selectById(id);

        return user;
    }

    public User login(String name,String userName){
        Map map =new HashMap();
        map.put("name",name);
        map.put("userName",userName);
        User user =(User) usermapper.selectByMap(map);
        return user;
    }

}
```













































