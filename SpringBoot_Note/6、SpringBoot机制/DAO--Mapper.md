## 职责划分

### Mapper

> - **定义**: Mapper 是 MyBatis 等 ORM 框架提供的功能，用于将 SQL 语句与 Java 方法关联起来。它直接映射数据库表和 Java 对象之间的关系。
> - **主要功能**: 执行具体的 SQL 查询、插入、更新和删除操作。通常是一个接口，其中的方法通过 XML 或注解定义对应的 SQL。

### DAO

> - **定义**: DAO（数据访问对象）是一个设计模式，提供了一组操作数据的方法，并可以包含一些业务逻辑。
> - **主要功能**: 封装所有的数据访问逻辑，处理更复杂的场景，比如事务管理、数据转换等。DAO 可以依赖 Mapper 来执行具体的数据库操作。



## 使用场景

### Mapper

> - 适用于对单个表进行简单的 CRUD 操作。
> - 它是面向数据库的，主要集中在 SQL 语句和数据库的交互上。

### DAO

> - 更适合于需要复杂业务逻辑的场景
>   - 在插入新记录时，需要**==先进行某些检查或数据转换==**。
>   - 需要处理事务，将**==多个数据库操作封装在一个方法==**中。
>   - 需要**==整合多个 Mapper 的结果==**，或者实现一些复杂的查询逻辑。
> - 将业务与查询语句物理隔离



## 实例

### Mapper

```java
public interface EmployeeMapper {
    void insert(Employee employee);
    Employee selectById(Long id);
    void update(Employee employee);
    void delete(Long id);
    List<Employee> selectAll();
}

```



### DAO

```java
public interface EmployeeDAO {
    void addEmployee(Employee employee);
    Employee getEmployeeById(Long id);
    void updateEmployee(Employee employee);
    void deleteEmployee(Long id);
    List<Employee> getAllEmployees();
}

public class EmployeeDAOImpl implements EmployeeDAO {
    
    //注入mapper
    private final EmployeeMapper employeeMapper;

    public EmployeeDAOImpl(EmployeeMapper employeeMapper) {
        this.employeeMapper = employeeMapper;
    }

    @Override
    public void addEmployee(Employee employee) {
        // 在这里添加额外的业务逻辑
        employeeMapper.insert(employee);
    }

    @Override
    public Employee getEmployeeById(Long id) {
        return employeeMapper.selectById(id);
    }
    
    // 其他方法实现...
}

```

> - 



## 总结

> - Mapper 负责具体的 SQL 执行，而 DAO 则负责更高层次的逻辑和多表操作的组合。
>   - 对于简单操作，**==避免过度设计==**，可以只用 Mapper；
>   - 对于复杂操作
>     - 当你希望将 **Mapper** 的 SQL 操作与业务逻辑  **==完全隔离==**  时
>     - 
>     - DAO 可以提供更好的组织和灵活性。
> - 使用 `mapper` 层来进行单表操作的组合时，`service` 层直接调用 `mapper` 可以简化开发流程，减少中间层（如 DAO 层）的引入，从而保持代码简洁。





## 外键实例

### 数据库表结构

```mysql
CREATE TABLE departments (
    dept_id INT PRIMARY KEY,
    dept_name VARCHAR(100)
);

CREATE TABLE employees (
    emp_id INT PRIMARY KEY AUTO_INCREMENT,
    emp_name VARCHAR(100),
    dept_id INT
);
```

#### Mapper

```java
//DepartmentMapper.java

@Mapper
public interface DepartmentMapper {

    @Select("SELECT COUNT(*) FROM departments WHERE dept_id = #{deptId}")
    int countById(int deptId);
}
```



```java
// EmployeeMapper.java

@Mapper
public interface EmployeeMapper {

    @Insert("INSERT INTO employees (emp_name, dept_id) VALUES (#{empName}, #{deptId})")
    void insertEmployee(String empName, int deptId);
}
```



#### XML

```xml
<mapper namespace="com.example.mapper.DepartmentMapper">

    <select id="countById" resultType="int" parameterType="int">
        SELECT COUNT(*) FROM departments WHERE dept_id = #{deptId}
    </select>

</mapper>
```



```xml
<mapper namespace="com.example.mapper.EmployeeMapper">

    <insert id="insertEmployee" parameterType="map">
        INSERT INTO employees (emp_name, dept_id) VALUES (#{empName}, #{deptId})
    </insert>

</mapper>
```

#### Service

```java
@Service
public class EmployeeService {

    @Autowired
    private DepartmentMapper departmentMapper;

    @Autowired
    private EmployeeMapper employeeMapper;

    public void addEmployee(String empName, int deptId) {
        // 检查部门是否存在
        if (departmentMapper.countById(deptId) > 0) {
            // 插入员工
            employeeMapper.insertEmployee(empName, deptId);
        } else {
            throw new RuntimeException("部门不存在，无法插入员工");
        }
    }
}
```

#### Controller

```java
@RestController
@RequestMapping("/employees")
public class EmployeeController {

    @Autowired
    private EmployeeService employeeService;

    @PostMapping
    public String addEmployee(@RequestParam String empName, @RequestParam int deptId) {
        try {
            employeeService.addEmployee(empName, deptId);
            return "员工添加成功";
        } catch (Exception e) {
            return e.getMessage();
        }
    }
}
```



> - **在这个示例中，由于业务逻辑简单且 MyBatis Mapper 已经提供了 DAO 的功能，所以直接在 Service 层 使用 Mapper 进行数据库操作是这种方式更加符合 MyBatis的最佳实践**
> - service调用mapper而不采用DAO时是因为当时的sql操作全都是 **mapper中定义的单表操作的组合**
> - 结构清晰，便于维护，没有额外定义 DAO 层，是为了减少冗余代码，简化架构。
> - 在需要对多个表进行复杂查询或者逻辑分离时，才会对DAO有需求