## xml配置

> - 确保在 mybatis-config.xml 文件中已经正确配置了映射文件（Mapper 文件）。

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

//上边的复制过去就行

//namespace后边跟的是绑定持久层的包路径+类名
<mapper namespace="com.example.tlias_11.dao.EmpDao" >
```

> - id绑定方法名，resultType后跟返回参数类型

```xml
 <select id="selectAll" resultType="com.example.tlias_11.entity.Emp">
        select *
        from emp
        <where>
            <if test=" name!=null and name!='' ">
                name like concat('%',#{name},'%')
            </if>

            <if test=" gender!=null ">
                and gender = #{gender}
            </if>

            <if test=" begin!=null and end!=null ">
                and entrydate between #{begin} and #{end}
            </if>

        </where>
    </select>
```

> - Dao层被xml捆绑的sql方法

```
List<Emp> selectAll();
```