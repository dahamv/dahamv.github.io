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
  Vehicle obj = context.getBean("vehicle")
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
  
  //These are used for Property Depency Injection
  getters() and setters();
}
```
```xml
<beans xmlns="http://....">
  
  <bean id="vehicle" class="package.name.of.bean.class">
    <!-- can change the Bean property value - dependency injection -->
    <property name="brand" value="MRF"></property>
  </bean>
</beans>
```

## Constructor Injection

