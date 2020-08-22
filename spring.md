## Dependency Injection usind xml configuration 
```java
public interface Vehicle {
  public void drive();
}

public Car implements Vehicle {...}
public Bike implements Vehicle {...}

public static void main(..) {
  ApplicationContext context = new ClassPathXmlApplicationContext();
  Vehicle obj = context.getBean("vehicle")
  obj.drive();
}
```
spring.xml file
```xml
<beans xmlns="http://....">
  <!-- can change the Bean class do do dependency injection -->
  <bean id="vehicle" class="package.name.of.bean.class"></bean>
</beans>
```
