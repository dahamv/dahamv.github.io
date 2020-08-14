Container = Spring container - where objects are stored for Dependency injection

class annotatinos
-----------------
>>component>>
@Componenet - creates a bean object in Spring container (a sigleton is made when without @Scope i.e. use same obj all the time)
@Componenet("name") - to change the name of the bean obj made in container. default name is class name
@Scope("prototype")  - creates an obj everytime asked contex.getBean(className)

>>spring MVC>>
@Controler  - In spring MVC take this class as a Spring Contriler. 

method annotations
------------------
@RequestMapping - map a request to a method in a Controller. 
@ResponseBody - when this is NOT used, spring MVC will think (since return type of method is String) that you are returning a View name.
                so it will search for the View (jsp file). This annotation is used when you want to return a data and SpringMVC 
		DispatcherServlet will simply pass that data to the client(browser) in the response.
		return repo.findAll().toString();
@PathVariable - When wildcards are used as @RequestMapping("/alian/{aid}")

variable annotations
--------------------
@Autowired - spring searchs for an object of the variable type in container.
@Qualifire("name to search in the container")  - every bean object in spring container has a name
@RequestParam - to get URL parameters assigned to method arguments.


ModleAndView class send data(model) from Controler to View. get that data from View(jsp file) with ${varName} i.e. JSTL format
in the controler request dispatch method >> mv.addObject(key,value) ; mv.setViewName(nameOfJSPFile)



JPA accotations
---------------

@Entity    - For Spring Modelclasses when need to be persisted by JPA
@Id - primary key
@Query("the query in JPQL") - above a method name in the repo interface do run a custom querry. 

in JPA you need a Repository interface as a DAO to deal with a database
   >> public interface MyRepo extends CrudRepository<MyPOJO, Integer(i.e. primaryKey)>
