# MySQL表中字段有非空限制，Hibernate添加默认值


## 问题背景

> Java开发中，使用Hibernate作为映射数据库的表，所以创建了一个modle类

``` java
@javax.persistence.Entity
@javax.persistence.Table(name = "HibernateTest")
public class HibernateTest extends BaseModel {
    private static final long serialVersionUID = 9124938173965082237L;
	
    private Long id;
    private String name;
    private Integer age;
	
    @javax.persistence.Id
    @javax.persistence.GeneratedValue(strategy = javax.persistence.GenerationType.IDENTITY)
    @javax.persistence.Column(name = "Id")
    public Long getId() {
        return id;
    }
	
    public void setId(Long id) {
        this.id = id;
    }
	
    @javax.persistence.Column(name = "Name")
    public String getName() {
        return name;
    }
	
    public void setName(String name) {
        this.name = name;
    }
	
    @javax.persistence.Column(name = "Age")
    public Integer getAge() {
        return age;
    }
	
    public void setAge(Integer age) {
        this.age = age;
    }
}
```

> 对应的数据库，由于DBA要求非字符串字段必须有默认值，则

```sql
CREATE TABLE `HibernateTest` (
  `Id` bigint(16) NOT NULL AUTO_INCREMENT COMMENT '主键',
  `Name` varchar(32) DEFAULT NULL COMMENT '名称',
  `Age` int(8) NOT NULL DEFAULT '-1' COMMENT '年龄',
  PRIMARY KEY (`Id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
```

> 这种情况下，使用Hibernate默认的save或update方法来添加或修改数据，如果有非空约束的字段(比如Age)没有赋值，会抛出异常。
> 因为Hibernate执行save或update时，会将映射类中的所有映射字段加入到生成的SQL语句中，所以导致数据库报错。

## 解决方法

> 在映射类的类名上方，添加注解

```
@org.hibernate.annotations.Entity(dynamicInsert = true, dynamicUpdate = true)
```
	
> 这样Hibernate在执行save或update方法时，映射类中映射字段为NULL的，就不会加入到最后生成的SQL语句中了。

