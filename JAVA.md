# JAVA

参考：

[https://doocs.github.io/advanced-java/#/?id=%e4%ba%92%e8%81%94%e7%bd%91-java-%e5%b7%a5%e7%a8%8b%e5%b8%88%e8%bf%9b%e9%98%b6%e7%9f%a5%e8%af%86%e5%ae%8c%e5%85%a8%e6%89%ab%e7%9b%b2%c2%a9](https://doocs.github.io/advanced-java/#/?id=互联网-java-工程师进阶知识完全扫盲©)

## 常用工具库

### lombok

*lombok*是一个可以通过简单的注解的形式来帮助我们简化消除一些必须有但显得很臃肿的 Java 代码的工具。**只在编译阶段生效**，不需要打入包中。Lombok在编译期将带Lombok注解的Java文件正确编译为完整的Class文件。

#### mvn引入

```xml
<dependency>
  <groupId>org.projectlombok</groupId>
  <artifactId>lombok</artifactId>
  <version>1.18.6</version>
</dependency>
```

#### 实现原理

自从Java 6起，javac就支持“JSR 269 Pluggable Annotation Processing API”规范，只要程序实现了该API，就能在javac运行的时候得到调用。

Lombok就是一个实现了"JSR 269 API"的程序。

1. javac对源代码进行分析，生成一棵**抽象语法树**(AST)
2.  **javac编译**过程中**调用**实现了JSR 269的**Lombok**程序
3. Lombok对第一步得到的AST进行处理，找到Lombok注解所在类对应的语法树    (AST)，然后**修改**该语法树(AST)，**增加Lombok注解定义的相应树节点**
4. javac使用修改后的抽象语法树(AST)**生成字节码文件**

#### 常用注解

- **@Getter/@Setter** 

  - 作用于类：生成所有成员变量的getter/setter方法；

  - 作用于成员变量：生成该成员变量的getter/setter方法。可以设定访问权限及是否懒加载等。

    ```java
    @Getter(value=AccessLevel.PUBLIC)
    ```

- **@ToString**

  - 作用于类：覆盖默认的toString()方法，可以通过of属性限定显示某些字段，通过exclude属性排除某些字段。

    ```java
    @ToString(of={"param1", "param2"}, exclude={"param3"})
    ```

- **@EqualsAndHashCode**

  - 作用于类：覆盖默认的equals和hashCode

- **@NonNull**

  - 主要作用于成员变量和参数中：标识不能为空，否则抛出空指针异常。

- **@NoArgsConstructor, @RequiredArgsConstructor, @AllArgsConstructor**

  - 作用于类：用于生成构造函数。有staticName、access等属性。**staticName属性一旦设定，将采用静态方法的方式生成实例，access属性可以限定访问权限。**

  - **@NoArgsConstructor：生成无参构造器；**

  - **@RequiredArgsConstructor：生成包含final和@NonNull注解的成员变量的构造器；**

  - **@AllArgsConstructor：生成全参构造器**

    ```java
    @NoArgsConstructor(staticName="of", access=AccessLevel.PRIVATE)
    @RequiredArgsConstructor(access=AccessLevel.PACKAGE)
    @AllArgsConstructor(access=AccessLevel.PUBLIC)
    ```

- **@Data**

  - 作用于类：是以下注解的集合：@ToString @EqualsAndHashCode @Getter @Setter @RequiredArgsConstructor

- **@Builder**

  - 作用于类：将类转变为建造者模式

- **@Log**

  - 作用于类：生成日志变量。针对不同的日志实现产品，有不同的注解

- **@Cleanup**

  - 自动关闭资源，针对实现了java.io.Closeable接口的对象有效，如：典型的IO流对象

- **@SneakyThrows**

  - 可以对受检异常进行捕捉并抛出

- **@Synchronized**

  - 作用于方法：可以替换synchronize关键字或lock锁

  



## 语法

### ThreadLocal

线程独享参数

### 范型

- T：确定类型
- E：元素
- K、V：key、value
- ？：通配符，不确定类型

- ? extends XXX：限定XXX的子类
- ? super XXX：限定XXX的父类

#### T 和 ? 的区别

```java
// 1. 通过T确保范型参数一致性
public <T extends Number> void test(List<T> dst, List<T> src)
// ?无法保证参数类型一致
public <? extends Number> void test(List<? extends Number> dst, List<? extends Number> src)
  

// 2. T可以多重限定，通配符？不行
T extends A & B
  
// 3. T只有一种类型限定方式
T extends A
// ？有两种
? extends A
? super A
  
// 4. class<T> 和 class<?> 区别
// class<?>是个通配范型，?可以代表任何类
public class<?> clazz;
public class<T> clazzT; //会报错
// class<T>实例化的时候，T要替换成具体类
public class Test<T> {
  public class<?> clazz;
  public class<T> clazzT; //不会报错
}

```

#### 