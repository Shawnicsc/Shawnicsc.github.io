#     JavaWeb

## Maven

基于项目对象的模型，管理项目的构建

### 1、环境配置

1、下载 官网，注意maven 版本与 jdk 版本是否适配 

<http://maven.apache.org/download.cgi> 

2、配置系统环境变量

右键此电脑 -> 属性 -> 高级系统设置 -> 环境变量 -> 系统变量 -> 新建 MAVEN_HOME （变量值只想maven安装目录）-> path 变量中新增 %MAVEN_HOME%\bin 

3、验证配置成功 

cmd 窗口中 ,显示出maven 版本 以及jdk 版本等信息

```
mvn -v
```

4、修改本地仓库（也可以不修改）

conf 文件夹下的 settings.xml ，在maven安装目录下新建个文件夹作为仓库

![184c5a299877ec74b71dc74cb6699aa](C:\Users\13627\AppData\Local\Temp\WeChat Files\184c5a299877ec74b71dc74cb6699aa.png)

5、maven 镜像配置

![5d29c3ec93a1c267a99f184b97bb1c8](C:\Users\13627\AppData\Local\Temp\WeChat Files\5d29c3ec93a1c267a99f184b97bb1c8.png)

```xml
<mirror>
      <id>alimaven</id>
      <name>aliyun maven</name>
      <url>https://maven.aliyun.com/repository/public/</url>
      <mirrorOf>central</mirrorOf>
    </mirror>
```

6、idea 配置maven

settings -> Build -> Maven 修改 user setting file 为conf 下的settings.xml（旁边打勾方可修改），修改local repository  为刚才设置的 本地仓库 -> apply  ok 

7、之后就可以使用maven创建项目了

### 2、maven基本操作

```
mvn clean compile // 清理并编译java文件 生产target目录
mvn package //打包生成 jar 或者war 文件
mvn clean install // 打包放到本地仓库
```



## MyBatis

### 1、作用

简化jdbc 持久层框架

### 2、代理模式

1、mapper接口要和xml配置文件在同一个作用域下关联

2、接口名字，命名空间需相同

3、id 与接口方法名字需相同

### 3、mybatis-config.xml 文件配置

#### 1、environment : 运行环境配置 

**POOLED** – 这种数据源的实现利用“池”的概念将 JDBC 连接对象组织起来，避免了创建新的连接实例时所必需的初始化和认证时间。 这种处理方式很流行，能使并发 Web 应用快速响应请求。 

每一个数据库对应一个sqlSessionFactory 实例

```xml
<environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql:///db1?useSSL=false"/>
                <property name="username" value="root"/>
                <property name="password" value="1234"/>
            </dataSource>
        </environment>
    </environments>
```


#### 2、typeAliases ：类型别名 

~~~xml
```
<!-- 1、第一种 -->
<package name="com.pojo"/>
<!-- 2 -->
<typeAlias type="com.pojo" alias="Brand"></typeAlias>
<!-- 3.注解别名，在类名前加 @Alias -->
@Alias("Brand")

```
~~~
3、mapper 映射器  定义sql映射语句

### 4、mapper.xml 配置

#### 1、 参数占位符
1、#{} : 将其替换为 ？ 防止sql注入
2、${} ： 将其替换成 传入的值 表名或者列名不固定的时候，可以使用，否则会有sql注入的风险
#### 2、参数类型
parameterType: 可以省略
#### 3、特殊字符处理
1、转义字符：查询转义字符表
2、CDATA区 

### 5、初始化过程 及使用

获取 sqlSessionFactory 对象  (需抛出io异常)
```java
String resource = "org/mybatis/example/mybatis-config.xml";
InputStream inputStream = Resources.getResourceAsStream(resource);
SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
```

获取 sqlSession  代理模式通过class对象获取mapper 映射器
``` java
		SqlSession sqlSession = sqlSessionFactory.openSession();
        // 代理模式，通过class对象获取mapper
		BrandMapper mapper = sqlSession.getMapper(BrandMapper.class);

```

执行sql语句 返回结果集
``` java
Brand brands = mapper.selectId(id);
```

释放 sqlSession 

```java
sqlSession.close();
```





