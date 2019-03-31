## 1、关于Controller层的一些注解。

- @ResponseBody 注解
- @RequestBody注解
- @RequestMapping注解
- @RequestParam注解
- @Controller注解

- @ResponseBody 注解：将内容或对象作为 HTTP 响应正文（即响应体）返回，并调用适合HttpMessageConverter的Adapter转换对象，写入输出流。一般注释在方法上，意思就是将方法的返回值通过一定的转换发送给前端页面。
- @RequestBody 注解：将HTTP请求正文（即请求体，post请求的内容）转换为适合的HttpMessageConverter对象。
- @RequestMapping 注解：用来处理请求地址映射的注解，可用于类或方法上。用于类上，表示类中的所有响应请求的方法都是以该地址作为父路径。
例子如下：

```java
@RequestMapping("/create")
@ResponseBody
public Result createProject(@RequestBody DevopsGitlabProject project) {   
   try {        
        Result<GitlabProject> result = new Result<>();          
        result.setResult(gitProjectService.createProject(project));        
        return result;   
   } catch (Exception e) {        
        logger.info(ErrorInfo.errorInfo(e));        
        return ErrorInfo.handError(e);    
  }
}

```
@RequestParam注解：匹配前台传来的参数给注解参数。比如下面前台传来的是project_id，匹配到projectId参数上。
例子如下：

```java

@RequestMapping(“/project")
@ResponseBody
public Result deleteProject(@RequestParam(name = "project_id") Integer projectId) {    
    try {        
        Result<String> result = new Result<>();        
        gitProjectService.deleteProject(projectId);        
        result.setResult("分支已删除!");        
        return result;    
    } catch (IOException e) {        
        logger.info(ErrorInfo.errorInfo(e));        
        return ErrorInfo.handError(e);    
    }
}

```

## 2. 关于Controller 类以及  @Controller 注解
- 1:spring mvc 中将  controller  认为是 MVC中的C --控制层
- 2：规范命名 类名  xxxController
- 3：如果不基于注解：该类需要继承  CommandController
如果基于注解：    在类名前加上   @controller
- 4：补充：将类名前加上该注解，当spring启动  或者web服务启动  spring会自动扫描所有包（当然，这个可以设置）
作用:  就是告诉服务器  这个类是MVC中的C    这个类可以接收用户请求    处理用户请求
备注：
@RequestMapping 注解的详解
理解：用来处理请求地址映射的注解，可用于类或方法上。用于类上，表示类中的所有响应请求的方法都是以该地址作为父路径。
RequestMapping注解有六个属性，下面我们把她分成三类进行说明。
附注：属性就是指指写在注解后面括号里面的内容，即：@RequestMapping（属性1＝属性值，属性2=属性值....）
1、 value， method；
value：     指定请求的实际地址，指定的地址可以是URI Template 模式（后面将会说明）；默认RequestMapping("....str...")即为value的值；
method：  指定请求的method类型， GET、POST、PUT、DELETE等；
2、 consumes，produces；
consumes： 指定处理请求的提交内容类型（Content-Type），例如application/json, text/html;
produces:    指定返回的内容类型，仅当request请求头中的(Accept)类型中包含该指定类型才返回；
3、 params，headers；
params： 指定request中必须包含某些参数值是，才让该方法处理。
headers： 指定request中必须包含某些指定的header值，才能让该方法处理请求。
2、@Service层常用的注解

@Service注解
@Resource注解
@Autowired注解
@Resource注解
@Value注解

@Service注解：将service层对象注入到spring容器中，和xml配置文件中的<bean></bean>标签作用一样。
@Resource注解：注入bean对象

如果同时指定了name和type，则从Spring上下文中找到唯一匹配的bean进行装配，找不到则抛出异常
如果指定了name，则从上下文中查找名称（id）匹配的bean进行装配，找不到则抛出异常
如果指定了type，则从上下文中找到类型匹配的唯一bean进行装配，找不到或者找到多个，都会抛出异常
如果既没有指定name，又没有指定type，则自动按照byName方式进行装配；如果没有匹配，则回退为一个原始类型进行匹配，如果匹配则自动装配；

@Autowired：按byType自动注入对象
@Autowired 与@Resource的区别：
1、 @Autowired与@Resource都可以用来装配bean. 都可以写在字段上,或写在setter方法上。
2、 @Autowired默认按类型装配（这个注解是属业spring的），默认情况下必须要求依赖对象必须存在，如果要允许null值，可以设置它的required属性为false，如：@Autowired(required=false) ，如果我们想使用名称装配可以结合@Qualifier注解进行使用，如下：
@Autowired()
@Qualifier("baseDao”)
private  BaseDao baseDao;
附注：关于ByName和Bytype的使用，ByName的意思是在容器中找到与注释下的变量名一致的对象注入给变量。ByType就是找到上下文与注释下的变量类型一致的类对象的类型，注入给注释下的变量。
推荐使用：@Resource注解在字段上，这样就不用写setter方法了，并且这个注解是属于J2EE的，减少了与spring的耦合。这样代码看起就比较优雅。
@Value注解意思：在connection中有的，指定资源文件中的某个变量值。
例子如下：
@Value("${git_host}")private String HOSTNAME;
@Transactional注解的作用：事务处理，当删除出现异常，回滚之前删除的。
注意：删除的操作必须要有@ Transactional注解，添加和修改也要有这个注解
例子如下：
@Override
@Transactional
public void deleteUser(Integer userId) throws IOException {    
      aclUserRepository.deleteUser(userId);            
      gitlabAPI.deleteUser(userId);
}

## 3、持久化层的相关注解

``` java
@Repository注解
@Query注解

@Param注解
@Repository注解意思：操作表的接口。@Query注解表示：下面的方法执行的就是该注解中的查询的sql语句，方法的返回值就是对应的查询结果，@Param注解表示
@Query：方法执行的查询语句，查询的结果就是方法的返回值（删除修改等无返回值不返回结果）
@Param：将方法中的参数值赋给注解中的参数，再将注解的参数放入查询语句的与注解同名的参数中。
例子如下：
@Repository
public interface AclExaminationRepository extends JpaRepository<AclExamination, Integer>, JpaSpecificationExecutor<AclExamination> {    
    @Query(value = "select * from acl_examination where leader_id=:leaderId",nativeQuery = true)    
    public List<AclExamination> getExaminationMessageList(@Param("leaderId")Integer leaderId);    
    
    @Query(value = "select * from acl_examination where id=:examinationId",nativeQuery = true)    
    public AclExamination getExaminationMessage(@Param("examinationId")Integer examinationId);
}

```
## 4、表的映射类的相关注解

* @Entity注解
* @Table注解
* @GeneratedValue
* @Transient
* @Id
* @Temporal
* @Entity注解：表示的是关于表映射的类的注解
* @Table注解：类映射的表的表名
* @GeneratedValue 注解：在表的映射类的主键上加入这个注解，那么在向表中创建这个对象的时候，就会自动创建主键id。
* @Transient注解：在映射类中有的字段，但表中没有，忽略注解下字段的映射。
* @Id注解：这个属性是表中的主键
* @Temporal(TemporalType.TIMESTAMP)注解：数据库中的Date类型，取到页面上是yyyy-MM-dd hh-mm-ss格式
@Temporal则可以获取自己想要的格式类型
TIMESTAMP　　yyyy-MM-dd hh:mm:ss 2016-12-07 11:47:58.697这个是会显示到毫秒的
DATE　　　　　yyyy-MM-dd
TIME　　　　　hh:mm:ss
例子如下：

```java

@Entity
@Table(name="acl_role")
public class AclRole {    
  @Id    
  @Column(name = "id")    
  @GeneratedValue    
  private Integer id;    
  @Column(name = "role_name")    
  private String roleName;    
  @Column(name = "role_permission")    
  private String rolePermission;
    
  @Transient    private String sshKey;//该属性不映射表中字段
    .. .. .. .
  @Temporal(TemporalType.TIMESTAMP)
  @Column(name = "created_at")
  private Date createdAt;

}

```
## 5、其它注解
@JsonProperty注解：通过前台得到的字段根据这个注解匹配到相应字段（如果没有注解，会直接根据变量名匹配）。
写这个注解的目的就是为了避免前台书写变量格式和后台变量命名格式不统一的冲突！

``` java

public class DevopsExamineBacklogVO {    
  private String initiator;    
  private String type;    
  @JsonProperty("task_id")    
  private String taskId;    
  @JsonProperty("created_at")    
  private String createdAt;
}

@Component注解意思： 把普通pojo实例化到spring容器中，相当于配置文件中的<bean id="" class=""/>
@PostConstruct注解：在当前类构造器初始化执行执行注解下的方法
@PostConstruct
private void init() {    
  this.gitlabAPI = GitlabAPI.connect(HOSTNAME,APITOKEN);    
  this.gitlabAPI.setRequestTimeout(REQUESTTIMEOUT);
}

@Bean注解：将方法返回值的bean对象注入spring容器中
@Bean
public RestTemplate restTemplate(){    
  return new BasicAuthRestTemplate(username, password);
}

```
