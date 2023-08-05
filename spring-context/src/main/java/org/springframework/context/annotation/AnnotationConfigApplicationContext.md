## reader 和 scanner
在 AnnotationConfigApplicationContext 中，reader 和 scanner 两个对象都扮演着重要的角色。

### reader：
reader 的类型是 AnnotatedBeanDefinitionReader，它的主要作用是读取并解析通过 Java 注解配置的 Bean。例如，当你使用 @Component、@Service、@Repository、@Controller、@Configuration 等注解标记的类时，AnnotatedBeanDefinitionReader 就会解析这些注解，并将解析结果封装为 BeanDefinition 对象，然后注册到 Spring 容器（BeanFactory）中。

AnnotatedBeanDefinitionReader 在 AnnotationConfigApplicationContext 的构造函数中被创建，并接收当前的 ApplicationContext 作为构造参数。

### scanner：
scanner 的类型是 ClassPathBeanDefinitionScanner。它的主要作用是扫描指定的包路径下的所有类，找出有 Spring 注解的类，并将这些类转换为 BeanDefinition 对象，然后注册到 Spring 容器中。

## BeanNameGenerator

BeanNameGenerator 在 Spring 中的主要作用是为 Spring 容器中的 bean 生成名称。在 Spring 中，每个 bean 都需要一个唯一的名称，这个名称可以用于从容器中获取 bean 实例。当你在配置文件或注解中没有显式地指定一个 bean 的名称时，Spring 会使用 BeanNameGenerator 来自动生成一个名称。

AnnotationConfigApplicationContext 中默认使用的 BeanNameGenerator 是 AnnotationBeanNameGenerator。AnnotationBeanNameGenerator 首先会检查目标 bean 类上是否有 @Component、@Repository、@Service 或 @Controller 等注解，并使用这些注解的 value 作为 bean 的名称。如果没有这些注解，或这些注解的 value 为空，那么 AnnotationBeanNameGenerator 将会使用目标 bean 类的简单类名（首字母小写）作为 bean 的名称。