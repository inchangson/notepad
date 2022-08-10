# MyBatis Test
> http://mybatis.org/spring-boot-starter/mybatis-spring-boot-test-autoconfigure/index.html

## dependancy 추가

### Maven
~~~
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter-test</artifactId>
    <version>2.2.2</version>
    <scope>test</scope>
</dependency>
~~~

### Gradle
~~~
dependencies {
    testCompile("org.mybatis.spring.boot:mybatis-spring-boot-starter-test:2.2.2")
}
~~~

### Issue(Compile vs Implementation)
> https://stackoverflow.com/questions/44493378/whats-the-difference-between-implementation-api-and-compile-in-gradle

- gradle 버젼관련 이슈로, 
- testCompile("...") 대신
- testImplementation '...'로 
- 바꿔야 한다.

### Issue 관련 이론 정리
> https://bluayer.com/13

- 추후 추가

## 어노테이션 추가

### 이슈
- class path resource [org/mybatis/spring/boot/autoconfigure/MybatisLanguageDriverAutoConfiguration.class] cannot be opened because it does not exist

