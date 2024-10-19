## 概述

> - `Builder` 是一种设计模式，用于简化对象的创建，尤其是当一个对象有多个可选参数时。它通过分步构造对象并允许链式调用

```java
//链式封装对象
member = Member.builder().openId(openId).phone(phoneNumber).gender(0).build();
memberMapper.save(member);
```



## 静态内部类Builder

```java
public class Member {
    private String openId;
    private String phone;
    private int gender;

    // 私有构造函数，使用Builder来创建对象
    private Member(Builder builder) {
        this.openId = builder.openId;
        this.phone = builder.phone;
        this.gender = builder.gender;
    }

    // 静态内部类 Builder
    public static class Builder {
        private String openId;
        private String phone;
        private int gender;

        public Builder openId(String openId) {
            this.openId = openId;
            return this;
        }

        public Builder phone(String phone) {
            this.phone = phone;
            return this;
        }

        public Builder gender(int gender) {
            this.gender = gender;
            return this;
        }

        public Member build() {
            return new Member(this);
        }
    }

    // 提供一个便捷的方法，直接返回 Builder 对象
    public static Builder builder() {
        return new Builder();
    }
}
```



## @Builder

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
@Builder
public class Member extends BaseEntity {

    /**
     * 手机号
     */
    private String phone;

    /**
     * 名称
     */
    private String name;

    /**
     * 头像
     */
    private String avatar;

    /**
     * OpenID
     */
    private String openId;

    /**
     * 性别
     */
    private Integer gender;

}
```



