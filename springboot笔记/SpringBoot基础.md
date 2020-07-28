---
typora-root-url: ..\笔记\springboot笔记
---

# 一、Spring Boot

## 1、Spring Boot简介

```插入引用：> + 空格```

> 简化Spring应用开发的一个框架
>
> 整个Spring技术栈的一个大整合
>
> J2EE开发的一站式解决方案

## 2、微服务

2014，Martin Fowler

微服务：架构风格（服务微化）

一个应用应该是一组小型服务；可以通过HTTP的方式进行互通；

单体运用：ALL IN ONE

微服务：每一个功能元素最终都是可独立替换和独立升级的软件单元

```插入链接：ctr + k （[示例]```

[详细参照微服务文档](https://martinfowler.com/microservices/)

## 3、Springboot HelloWord

[springboot guides]((https://spring.io/guides/gs/spring-boot/))

### 1、创建一个maven工程；（jar）

### 2、导入依赖spring boot相关的依赖

```xml
<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.2.2.RELEASE</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>

<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
			<exclusions>
				<exclusion>
					<groupId>org.junit.vintage</groupId>
					<artifactId>junit-vintage-engine</artifactId>
				</exclusion>
			</exclusions>
		</dependency>
	</dependencies>

```

### 3、编写一个主程序；启动Spring Boot应用

```java
@SpringBootApplication
public class DemoApp {
    public static void main(String[] args) {
        SpringApplication.run(DemoApp.class,args);
    }
}
```



### 4、编写相关的Controller、Service、Entity

```java
@RestController
public class GreetingController {

    private static final String template = "Hello, %s!";
    private final AtomicLong counter = new AtomicLong();

    @GetMapping("/greeting")
    public Greeting greeting(@RequestParam(value = "name", defaultValue = "World") String name) {
        return new Greeting(counter.incrementAndGet(), String.format(template, name));
    }
}
```

```java
public class Greeting {

    private final long id;
    private final String content;

    public Greeting(long id, String content) {
        this.id = id;
        this.content = content;
    }

    public long getId() {
        return id;
    }

    public String getContent() {
        return content;
    }
}

```

### 5、运行主程序测试

### 6、简化部署

```xml
  <!--这个插件可以将运用打包成一个可执行的jar包,没有这个无法生成文件清单-->
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
```

将这个运用打成jar包，直接用java -jar 执行

## 4、Hello World探究

https://spring.io/projects/spring-boot

### 1、POM文件

#### 1、父项目

```xml
 <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.2.2.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>

它的父项目是
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-dependencies</artifactId>
    <version>2.2.2.RELEASE</version>
    <relativePath>../../spring-boot-dependencies</relativePath>
  </parent>
他来真正管理Spring Boot应用里面的所有依赖版本
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-dependencies</artifactId>
    <version>2.2.2.RELEASE</version>
    <relativePath>../../spring-boot-dependencies</relativePath>
  </parent>

里面定义了大量的版本号属性
 <properties>
    <activemq.version>5.15.11</activemq.version>
    <antlr2.version>2.7.7</antlr2.version>
    <appengine-sdk.version>1.9.77</appengine-sdk.version>
    <flatten-maven-plugin.version>1.1.0</flatten-maven-plugin.version>
    <wsdl4j.version>1.6.3</wsdl4j.version>
    <xml-maven-plugin.version>1.0.2</xml-maven-plugin.version>
    <xmlunit2.version>2.6.3</xmlunit2.version>
  </properties>
```

Spring Boot的版本仲裁中心：

以后我们导入依赖默认是不需要写版本；（没有在dependencies里面管理的依赖自然需要声明版本号）

#### 2、导入依赖

```xml
<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
```

spring-boot-starter-web:

​		spring-boot-starter：spring-boot场景启动器；帮助我们导入了web模块正常运行所依赖的组件；

Spring Boot将所有的功能场景都抽取出来，做成一个个的starters(启动器),只需要在项目里面引入这些starter相关场景的所有依赖都会导入进来，要用什么功能就导入什么场景的启动器。

https://docs.spring.io/spring-boot/docs/2.3.2.RELEASE/reference/html/using-spring-boot.html#using-boot-starter

### 2、主程序类、主入口类

```java
@SpringBootApplication
public class RestServiceApplication {
        public static void main(String[] args) {
            SpringApplication.run(RestServiceApplication.class, args);
        }
    }
```

@**SpringBootApplication**：Spring Boot运用标注在某个类上说明这个类是SpringBoot的主配置类，springboot就应该运行这个类的main方法来启动springboot应用；

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(
    excludeFilters = {@Filter(
    type = FilterType.CUSTOM,
    classes = {TypeExcludeFilter.class}
), @Filter(
    type = FilterType.CUSTOM,
    classes = {AutoConfigurationExcludeFilter.class}
)}
)
```

@**SpringBootConfiguration**:Spring Boot的配置类：

​			标注在某个类上，表示这是一个Spring Boot的配置类；

​			@**Configuration**：配置类上标注这个注解；

​				配置类------配置文件；配置类也是容器中的一个组件；@**Component**

@**EnableAutoConfiguration**：开启自动配置功能

​			以前我们需要配置的东西，springboot帮我们自动配置；@EnableAutoConfiguration告诉Springboot开启自动配置功能；这样自动配置才能生效；

```java
@AutoConfigurationPackage
@Import({AutoConfigurationImportSelector.class})
public @interface EnableAutoConfiguration {
```

@**AutoConfigurationPackage**：自动配置包

```java
@Import({Registrar.class})
public @interface AutoConfigurationPackage {
} 
//Spring的底层注解@Import，给容器中导入一个组件；导入的组件由Registrar.class指定
```

===将主配置类（@SpringBootApplication标注的类）所在的包以及下面所有子包里面的所有的组件扫描到Spring容器; ==

​		@**Import**({AutoConfigurationImportSelector.class})；

​			给容器中导入组件？

​			AutoConfigurationImportSelector：导入哪些组件的选择器；

​			将所有需要导入的组件以全类名的方式返回；这些组件就会被添加到容器中；

​			会给容器中导入非常多的自动配置类（XXXAutoConfiguration）;就是给容器导入这个场景需要的所有组件，并配置好这些组件；

![自动配置类](/images/微信图片_20200725102228.png)

有了自动配置类，免去了我们手动编写配置注入功能组件等工作；

```java
SpringFactoriesLoader.loadFactoryNames(EnableAutoConfiguration.class,)
```

springboot在启动的时候从类路径下的`META-INF/spring.factories`中获取EnableAutoConfiguration指定的值，将这些值作为自动配置类导入到容器中，自动配置类就生效，帮我们进行自动配置工作；以前我们需要自己配置的东西，自动配置类都帮我们：

J2EE的整体整合方案和自动配置都在`D:\Users\scott\.m2\repository\org\springframework\boot\spring-boot-autoconfigure\2.2.2.RELEASE\spring-boot-autoconfigure-2.2.2.RELEASE.jar`

## 5、使用Spring Initializer快速创建项目

IDE都支持使用spring的项目向导快速创建一个spring boot 项目；

选择我们需要的模块；向导会联网创建spring boot 项目；

默认生成的springboot 项目；

- 主程序已经生成好了，我们只需要我们自己的逻辑

- resource文件夹中目录机构；

  - static:保存所有的静态资源：js css images
  - templates:保存所有的模板页面；（spring boot默认jar包使用嵌入式的Tomcat,默认不支持JSP页面）；可以使用模板引擎（freemarker、thymeleaf）;
  - application.properties

# 二、配置文件

## 1.配置文件

Springboot使用一个全局的配置文件，配置文件名是固定的；

- application.properties
- application.yml

配置文件的作用：修改springboot自动配置的默认值；springboot在底层都给我们自动配置好；

YAML (YAML Ain't Markup Language)

YAML A Markup Lunguage：是一个标记语言

YAML Isn't Markup Lunguage：不是一个标记语言

标记语言：

​		以前的配置文件；大多都是XXX.xml

​        YAML：以数据为中 心，比json、xml等更适合做配置文件；

​		YAML:

```yaml
server:
  port: 8081
```

​		XML:

```xml
<server>
	<port>8081</port>
</server>
```

## 2.YAML语法

### 1.基本语法

`k:[空格]v`表示一对键值对（空格必须有）；

以**空格**的缩进来控制层级关系；只要是左对齐的一列数据，都是同一个层级的

```yaml
server:
	port:8081
	path:/hello
```

属性和值也是大小写敏感；

### 2、值的写法

**1、字面量：普通的值（数字，字符串，布尔）**

​		**k: v**：字面量直接来写；

​					字符串默认不用加上单引号或者双引号；

​				    ""：双引号；不会转义字符串里面的特殊字符；特殊字符会作为本身想表示的意思

​							e.g  `name: "zhangsan \n lisi"`;输出；zhangsan 换行 lisi

​				    ‘’：单引号；会转义特殊字符，特殊字符最终只是一个普通的字符串数据

​                  		  e.g   name: 'zhangsan \n lisi'  ; 输出；zhangsan \n lisi

**2、对象、Map(属性和值)（键值对）**

​		k: v  在下一行来写对象的属性和值的关系；注意缩进

```yaml
friend:
	lastName: zhangsan
	age: 20
```

​		行内写法

```yaml
friend: {lastName: zhangsan,age: 18}
```

### 3、数组（List、Set）

用- 值表示数组中的一个元素

```yaml
pets:
  - dog
  - cat
  - pig
```

行内写法

```yaml
pets: [cat,dog,pig]
```

### 3、配置文件值注入

配置文件

```yaml
server:
  port: 8081
person:
  lastName: hello
  boss: false
  birth: 2017/12/12
  maps: {k1: v1,k2: v2}
  lists:
    - lisi
    - zhaoliu
  dog:
    name: 小狗
    age: 12
  age: 16
```

java bean

```java
/**
 * 将配置文件中配置的每一个属性的值，映射到这个组件中
 * @ConfigurationProperties告诉springboot将本类的所有属性和配置文件相关的配置进行绑定，默认从全局配置  文件获取值
 *   prefix = "person":告诉springboot配置文件中哪个下面的所有属性进行一一映射
 */
@Component
@ConfigurationProperties(prefix = "person")
public class Person {
    private String lastName;
    private Integer age;
    private Boolean boss;
    private Date birth;
    private Map<String,Object> maps;
    private List<Object> lists;
    private Dog dog;

    @Override
    public String toString() {
        return "Person{" +
                "lastName='" + lastName + '\'' +
                ", age=" + age +
                ", boss=" + boss +
                ", birth=" + birth +
                ", maps=" + maps +
                ", lists=" + lists +
                ", dog=" + dog +
                '}';
    }

    public String getLastName() {
        return lastName;
    }

    public void setLastName(String lastName) {
        this.lastName = lastName;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    public Boolean getBoss() {
        return boss;
    }

    public void setBoss(Boolean boss) {
        this.boss = boss;
    }

    public Date getBirth() {
        return birth;
    }

    public void setBirth(Date birth) {
        this.birth = birth;
    }

    public Map<String, Object> getMaps() {
        return maps;
    }

    public void setMaps(Map<String, Object> maps) {
        this.maps = maps;
    }

    public List<Object> getLists() {
        return lists;
    }

    public void setLists(List<Object> lists) {
        this.lists = lists;
    }

    public Dog getDog() {
        return dog;
    }

    public void setDog(Dog dog) {
        this.dog = dog;
    }
}
```

我们可以导入配置文件处理器，通过使用spring-boot-configuration-processor jar，可以轻松地从带有@ConfigurationProperties注释的项生成您自己的配置元数据文件。jar包含一个Java注释处理器，在编译项目时调用它。这样我们在编写配置文件时就有提示。

https://docs.spring.io/spring-boot/docs/2.1.9.RELEASE/reference/html/configuration-metadata.html#configuration-metadata-annotation-processor

```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-configuration-processor</artifactId>
	<optional>true</optional>
</dependency>
```

#### 1、properties配置文件在idea中默认utf-8可能会乱码

![微信图片_20200725180041](/images/微信图片_20200725180041.png)

#### 2、@Value获取值和@ConfigurationProperties获取值比较

|                | @ConfigurationProperties | @Value     |
| -------------- | ------------------------ | ---------- |
| 功能           | 批量注入配置文件中的属性 | 一个个指定 |
| 松散绑定       | 支持                     | 不支持     |
| SpEL           | 不支持                   | 支持       |
| JSR303数据校验 | 支持                     | 不支持     |
| 复杂类型封装   | 支持                     | 不支持     |

配置文件yml还是properties他们都能获取到值

如果说，我们只是在某个业务逻辑中需要获取一下配置文件中的某项值，使用@Value

如果说，我们专门编写了一个javaBean来和配置文件进行映射，我们就直接使用@ConfigurationProperties

### 4、**@PropertySource** & **@ImportResource**

**@PropertySource**：加载指定的配置文件













# 五、Docker

## 1、简介

Docker是一个开源的应用容器引擎；

Docker支持将软件编译成一个镜像，然后在镜像中各种软件做好配置，将镜像发布出去，其它使用者可以直接使用这个镜像；

运行中的这个镜像称为容器，容器启动的速度是非常块的。

## 2、Docker核心概念

docker主机（Host）：安装了Docker程序的机器（Docker直接安装在操作系统之上）；

docker客户端（Client）:连接docker主机进行操作；

docker仓库（Registry）：用来保存各种打包好的软件镜像；

docker镜像（Images）:软件打包好的镜像，放在docker仓库中；

docker容器（Container）：镜像启动后的实例称为一个容器，容器是独立运行的一个或者一组应用

使用docker的步骤：

1. 安装Docker
2. 去Docker仓库找到这个软件对应的镜像；
3. 使用Docker运行这个镜像，这个镜像就会生成一个Docker容器；
4. 对容器的启动停止就是对这个软件的启动停止

## 3、安装docker

### 1、安装步骤（CenOS

```shell
1.检查内核版本，必须是3.10以上
uname -r        打印机器和操作系统的信息。
2.安装docker
sudo yum install docker
3.启动docker
systemctl start docker
4.查看版本号
docker -v
5.设置开机启动
systemctl enable docker
6.关闭docker
systemctl stop docker
```

[docker官方仓库](https://hub.docker.com/)

### **2、镜像操作**

| 操作 | 命令                    | 说明                                                    |
| ---- | ----------------------- | ------------------------------------------------------- |
| 检索 | docker search 关键字    | 我们经常去docker hub上检索镜像的详细信息，如镜像的TAG   |
| 拉去 | docker pull 镜像名：tag | :tag是可选的，tag表示标签，多为软件的版本，默认是latest |
| 列表 | docker images           | 查看所有本地镜像                                        |
| 删除 | docker rmi 镜像Id       | 删除指定的本地镜像                                      |

### 3、容器操作

| 操作     | 命令                                                | 说明                              |
| -------- | --------------------------------------------------- | --------------------------------- |
| 运行     | docker run --name container-name -d image-name：TAG | --name：自定义容器名  -d 后台运行 |
| 列表     | docker ps                                           | 加上-a;可以查看所有容器           |
| 停止     | docker stop container-name/container-id             | 停止当前你运行的容器              |
| 启动     | docker start container-name/container-id            | 启动容器                          |
| 删除     | docker rm container-id                              | 删除指定容器                      |
| 端口映射 | docker run -d -p 8888:8080 tomcat                   | -p:主机端口映射到容器内部端口     |
| 容器日志 | docker logs container-name/container-id             |                                   |

更多命令：https://docs.docker.com/engine/reference/commandline/docker/

### 4、安装mysql示例

```shell
docker pull mysql
```

错误的启动

```shell
docker run --name mysql01 -d mysql

mysql退出了

//用docker logs mysql 查看日志

ec2-user:~/environment $ docker logs 01ab4c1498b4
2020-07-25 16:13:13+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 8.0.21-1debian10 started.
2020-07-25 16:13:14+00:00 [Note] [Entrypoint]: Switching to dedicated user 'mysql'
2020-07-25 16:13:14+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 8.0.21-1debian10 started.
2020-07-25 16:13:14+00:00 [ERROR] [Entrypoint]: Database is uninitialized and password option is not specified
        You need to specify one of MYSQL_ROOT_PASSWORD, MYSQL_ALLOW_EMPTY_PASSWORD and MYSQL_RANDOM_ROOT_PASSWORD

docker rm image-id 删除容器，然后设置密码启动
```

更多详细启动查看官方文档https://hub.docker.com/_/mysql?tab=description

