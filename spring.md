Dependency injection is for Loose Coupling. If you use **new** keyword its tight coupling. Therefor use configuration using xml.
## Dependency Injection using XML configuration 
```java
public interface Vehicle {
  public void drive();
}

public Car implements Vehicle {...}
public Bike implements Vehicle {...}

public static void main(..) {
  ApplicationContext context = new ClassPathXmlApplicationContext("spring.xml");
  Vehicle obj = (Vehicle)context.getBean("vehicle")
  obj.drive();
}
```
spring.xml file
```xml
<beans xmlns="http://....">
  <!-- can change the Bean class to do dependency injection -->
  <bean id="vehicle" class="package.name.of.bean.class"></bean>
</beans>
```

## Dependency Injection Using Annotations
use ```@Component``` annotations in bean classes. Class name will be the default ID of the bean instentiated in the Spring Container.

```java
@Component
public Car implements Vehicle {...}

@Component
public Bike implements Vehicle {...}

public static void main(..) {
  ApplicationContext context = new ClassPathXmlApplicationContext("spring.xml");
  Vehicle obj = (Vehicle)context.getBean("car")
  obj.drive();
}
```
```xml
<beans xmlns="http://....">
  <!-- Have to tell Spring to search in the package for Components -->
  <context:componenet-scan base-package="package.name.of.defined.components"></context:component-scan>
</beans>
```

## Bean Property (variable) Dependency Injection
```java
@Component
public Tyre{
  private String brand;
   
  getter() {}
  //This is used to do Property Depency Injection - called Setter Injection
  setter() {}
}
```
```xml
<beans xmlns="http://....">
  
  <bean id="tyre" class="package.name.of.bean.class">
    <!-- can change the Bean property value - dependency injection -->
    <property name="brand" value="MRF"></property>
  </bean>
</beans>
```

## Constructor Injection
```java
@Component
public Tyre{
  private String brand;

  public Tyre(String brand) {
    this.brand = brand;
  }
}
```
```xml
<beans xmlns="http://....">
  
  <bean id="tyre" class="package.name.of.bean.class">
    <!-- can inject value throug the constructor - constructor injection-->
    <constructor-arg value="MRF"></constructor-arg>
  </bean>
</beans>
```

## Autowired Configuaration

```java
@Component
public Car implements Vehicle {
  @Autowired
  private Tyre tyre;
}
```

# Configuration | Bean

```java
// we need a configuration class since we dont use XML based configuration. Configuration is done in the class.
@Configuration
public class AppConfig {
  @Bean // this is returning a Bean
  public Samsung getPhone() {
    return new Samsung();
  }  
  
  @Bean // this is returning a Bean
  public MobileProcessor getPrcessor() {
    return new SnapDraggon();
  } 
}

public interface MobileProcessor{ public void process(); } ;

public class SnapDraggon implements MobileProcessor { public void process() { ....} }

public class Samsung {
  @Autowired
  private MobileProcessor processor;
  
  getter();
  
  public void config() {
    processor.process();
  }
}

//main method
main() {
  //Specify the configuration class
  ApplicationContext factory = new AnnotationConfigApplicationContext(AppConfig.class);
  Samsung s7 = factory.getBean(Samsung.class);
  s7.config();
}
```

use ```@AutoWired``` annotation just like in previous examples to inject dependent Beans. Spring feamework searches in the Configuration class for Beans with required types.
