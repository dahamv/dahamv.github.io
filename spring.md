## Spring MVC

**Container** = Spring container - where objects are stored for **Dependency injection**

### Modle annotatinos

```@Componenet``` - creates a bean object in Spring container (a sigleton is made when without @Scope i.e. use same obj all the time)  
```@Componenet("name")``` - to change the name of the bean obj made in container. default name is class name  
```@Scope("prototype")```  - creates an obj everytime asked contex.getBean(className)

```@Autowired``` - spring searchs for an object of the variable type in container. Used by **dependency injection**  
```@Qualifire("name to search in the container")```  - every bean object in spring container has a name. 

```java
// a Model
@Componenet
@Scope(value="prototype")
public class Alien {
	private int aId;
	private String aName;
	private String tech;
	@Autowired  // the componenet Laptop will be Autowired with Dependency injection
	@Qualifire("laptop")
	private Laptop laptop; // There is another Componenet (A model - bean class) called Laptop
	// getters and setters
}
```
### Controler annotations

```@Controler```  - In spring MVC take this class as a Spring Controler to get HTTP requests. There is a servlet running underlying.  
```@RequestParam``` - to get URL parameters assigned to method arguments.  
```@RequestMapping``` - map a request to a method in a Controller.   
```@ResponseBody``` - when this is NOT used, spring MVC will think (since return type of method is String) that you are returning a View name.   
                so it will search for the View (jsp file). This annotation is used when you want to return a data and SpringMVC   
		DispatcherServlet will simply pass that data to the client(browser) in the response.   
		```return repo.findAll().toString();```    
```@RestController``` - when the controller is annotated with this, ```@ResponseBody``` isn't required.  


**ModleAndView** class sends data(model) from **Controler to View**. get that data from **view(jsp file)** with **${varName}** i.e. JSTL format

```java
@Controler
public class HomeControler {
	@RequestMapping("home")
	public ModelAndView home(@RequestParam("name") String myName) {
		ModelAndView mv = new ModelAndView();
		mv.addObject("name",myName);
		mv.setViewName("home"); // now myName can be read from home.jsp file ${name}		
	}
}
```

```@PathVariable``` - When wildcards are used as ```@RequestMapping("/alian/{aid}")```   

```java
@RequestMapping("/alian/{aid}")
@ResponseBody
public String getAlien(@PathVariable("aid") int aid) {
	return repo.findById(aid).toString();
}
```

to get a JSON response. return a Model object (a bean/POJO)

### JPA annotations

```@Entity```    - For Spring Modelclasses when need to be persisted by JPA   
```@Id``` - primary key   
```@Query("the query in JPQL")``` - above a method name in the repo interface do run a custom querry.       
     
in JPA you need a Repository interface as a DAO to deal with a database   
    
```java
public interface MyRepo extends CrudRepository<MyPOJO, Integer(i.e. primaryKey)> {
	//The magic is no Implementation is needed.
	List<Alien> findByTech(String tech) // note that there is a var called tech in Alien class (model)
	List<Alien> findByAidGreaterThan(int aid) // there is a var called aid in the model
}


```
see [Spring data jpa - querry creation](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#jpa.query-methods.query-creation)

**JpaRepository** interface has more features than CrudRepository. eg. findAll() returns a List. the other one returns an Iterable
