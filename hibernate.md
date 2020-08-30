# Hibernate
**[Home](index.md)**

## CRUD
_________
### Create
SessionFactory - has information to connect to DB. i.e. the db-url, username, password, db-driver etc.  
```java
Alien alien = new Alien();
alien.setValues(....);

//hibernate configuration. hibernate.cfg.xml is the default name so no need to specify it. 
Configuration con = new Configuration().configure("hibernate.cfg.xml").addAnnotatedClass(Alien.class);
/*
this way is deprecated.
SessionFactory sf = con.buildSessionFactory();
session session = sf.openSession();
*/
ServiceRegistry reg  = new ServiceRegistryBuilder().applySettings(con.getProperties()).buildServiceRegistry();
SessionFactory sf = con.buildSessionFactory(reg);
session session = sf.openSession();
Transaction tx = session.beginTransaction();
session.save(alien);
tx.commit();
```
in **hibernate.cfg.xml** specify **driver.class, connection-url, username, password, hibernate-dialect**   
Add ```<property name="hbm2ddl.auto">update</property>"``` so that hibernate will create tables if not exist and update the existing table.  
If ```create``` is used a new table will be created everytime.   
```show_sql - true``` will show the sql query executed underneeth.  

The bean class should be a JPA entity

```java
//to change entity name @Entity(name="entityName"). If @Table isn't used entityName is assigned to tableName
@Entity
@Table(name="changedTableName")
public class Alien {
  //JPA annotation for primary key 
  @Id
  private int aid;
  @Column(name="changedColumnName")
  private String name;
  @Transient // this attribute is not used in table as a column. Temperory data.
  private String color;
}
```

### Read

To fetch values
```java
Alien alien = null;
//.....transaction begin
alien = (Alien) session.get(Alien.class, 101); // give primary key value
//.....
```
### Update and Delete
- To update: ```session.saveOrUpdate(alien)``` 
- To delete: ```Session.delete(alien)```

##  Embeddable Objects
_________
```java
@Entity
public class Alien {
  @Id
  private int aid;
  private AlienName name;
}

@Embeddable
public class AlienName {
  private String fName;
  private String mName;
  private String lName;
}
```

## Mapping Relations
_________

Use ```@Cascade``` to tell hibernate when Relation Owner (Student) is saved, its related Entities(Laptops) will also be saved automatically. Can be used with One-to-One, One-to-Many, Many-to-One and Many-to-Many.

### One-to-One
```java
@Entity
public class Laptop {
  @Id
  private int lId;
  private String lName;
}

@Entity
public class Student {
  @Id
  private int sId;
  private String sName;
  @OneToOne // Student table will have a forignKey colomn laptop_lid.
  @Cascade({CascadeType.SAVE_UPDATE, CascadeType.DELETE}) 
  private Laptop sLaptop;
}
```

### One-to-Many and Many-to-One
One Student can have many Laptops. there are two approaches.  
1. Create another table **student_laptop** and have the mappings there.
2. Create a column **laptopOwner** in the Laptop table having sId s. Since **one Laptop is owned by one Student** and **one Student has many Laptops.**
* When you say ```@...ToMany``` a table will be made.  
* When you say ```@...ToOne``` a column will be made.

#### Approach 1  

```java
@Entity
public class Student {
  @Id
  private int sId;
  private String sName;
  //Another table Student_Laptop (with columns student_sId and laptop_lid) will be created.
  @OneToMany
  private List<Laptop> sLaptops = new ArrayList<Laptop>();

}
```
#### Approach 2 (Prefered)
```java
@Entity
public class Laptop {
  @Id
  private int lId;
  private String lName;
  //a column student_sId will be made in Laptop table to store the sId of the Studends who own that.
  @ManyToOne 
  private Student laptopOwner;
}

@Entity
public class Student {
  @Id
  private int sId;
  private String sName;
  //If mappedBy is not used, Hibernate will create Student_Laptop table. 
  //We don't need that since Laptop table has student_sId.
  @OneToMany(mappedBy="laptopOwner")
  private List<Laptop> sLaptops = new ArrayList<Laptop>();
}
```
You can create the Join table manually by using  ```@JoinTable and @JoinColumn``` annotations.

### Many-to-Many
One Laptop can be used by many Students. One Student can have Many Laptops. E.g. College computer lab.  
**We MUST** have another seperate table (Laptop_Student) to store the mapping.
```java
@Entity
public class Laptop {
  @Id
  private int lId;
  private String lName;
  //A table Laptop_Student will be made to have the mappings.
  @ManyToMany 
  private List<Student> laptopOwners = new ArrayList<Student>();
}

@Entity
public class Student {
  @Id
  private int sId;
  private String sName;
  //If mappedBy is not used, hibernate will create a Student_Laptop table as well. 
  //We don't need two tables. Only Laptop_Student table will be enough.
  @ManyToMany(mappedBy="laptopOwners")
  private List<Laptop> sLaptops = new ArrayList<Laptop>();
}
```
## Eager and Lazy Fetch
_________
- **FetchType.EAGER** - Hibernate runs the sql before hand. If a Student is read, all properties 
**sId, sName and sLaptops** (the List of Laptops) will be read right away at ```session.get(Student.class, studentId);```
- **FetchType.LAZY** - runs the sql only when its needed. When a Student is read only **sId and sName** will be read.
The List of **sLaptops** will be read only when its asked for. ```student.getLaptops(); ``` 
```java
@Entity
public class Student {
  @Id
  private int sId;
  private String sName;
  // default fetch type is LAZY. when EAGER hibernage sql query will 
  //left outer join selected student table and the selected laptop table.
  @OneToMany(mappedBy="laptopOwner", fetch=FetchType.EAGER)
  private List<Laptop> sLaptops = new ArrayList<Laptop>();
}
```

## Hibernate Inheritence
_________
* Use ```@MappedSuperclass``` when several ```@Entity``` classes have a parent class. All Parent attributes are shown in corresponding child tables.
* To get all child Entities in a **single table** use ```@Inheritance``` and ```@DiscriminatorColumn```. A Discriminator column called **DTYPE** will be made to store the names of the child entities as a value: so that we can differentiate between them
There are different Inheritence stratergies
```@Inheritance(strategy = InheritanceType.SINGLE_TABLE)```  
```@Inheritance(strategy = InheritanceType.TABLE_PER_CLASS)```  
```@Inheritance(strategy = InheritanceType.JOINED)```  

Read more at [https://www.baeldung.com/hibernate-inheritance]

## Hibernate Caching
_________
- **First Level caching** - cache for each hibernate session. This caching is not shared among hibernate sessions. Enabled by default.
- **Second Level caching** - can have a thirdparty (like Ehcache) cache to share among sessions.

For Ehcache second level caching add dependencies **ehcache** and **hibernate-ehcache.** in pom.xml and the following properties in **hibernate.cfg.xml** file.   
*Note - This is applicable only for ```session.get()``` method and not for Queries.*   
```<property name="hibernate.cache.use_second_level_cache">true</property>``` and    
```<property name="hibernate.cache.region.factory_class">org.hibernate.cache.ehcache.EhCacheRegionFactory</property>```
```java
@Entity
@Cacheble // To tell hibernate that this entity is cacheble in secondlevel cacheing. 
@Cache(usage=CacheConcurrencyStratergy.READ_ONLY) // define the caching stratergy. default is NONE
public class Alien {}
//Now the select query will be executed only once because of seconlevel caching.
Alien a = (Alien) session1.get(Alien.class, 101);
Alien b = (Alien) session2.get(Alien.class, 101);
```
### Hibernate Caching with queries
Add this property in the hibernate.cfg.xml file and setCacheble in Query object.
```<property name="hibernate.cache.use_query_cache">true</property>``` This is false by default.      
```java
Query q1 = session1.createQuery("from Alien where aid = 101");
q1.setCacheble(true);
Alien a = (Alien)q1.uniqueResult();
Query q2 = session2.createQuery("from Alien where aid = 101");
q2.setCacheble(true);
Alien b = (Alien)q2.uniqueResult(); 
```

## HQL - hibernate query language
_________
in SQL ````select column_name from table```       
in HQL ```select property_name from Entity```   
Can use builtin functions in HQL.   
``` "select sum(marks) from Student s where s.marks>60" ```

You can also run SQL queries(Native queries) in hibernate.
```java
SQLQuery q = session.createSQLQuery("select * from student where marks > 60");
//Only if you are getting the entire row (select *) and you have to tell that the result set should be mapped to Student entities.
q.addEntity(Student.class);
List<Student> students = q.list();
//If you are geting only a couple of columns (select name, marks), then you cant map that to Student entity. 
//So map the results to key value pairs
q.setResultTransformer(Criteria.ALIAS_TO_ENTITY_MAP);
//key = culumn_name, value = column_value
List<Map> students = q.list();
```

## Hibernate Object States | Persistence Life Cycle
_________
![](https://cdn.jstobigdata.com/wp-content/uploads/2019/08/Entity-instance-states-1024x536.png)
Four states of Hibernate Entity objects   
* **Transient** - by default all **new** entity objects.
* **Persistent** - when ```session.save(obj)``` or ```session.persist(obj)``` is called, Or when you get data from the DB with ```session.get(id)``` or ```session.find(id)```, the entity objects are in Persistent state. **Only Persistent objects will be stored in the DB when**  ```transaction.commit()``` **is called.** 
* **Detatched** - when ```session.detatch(obj)``` is called. Changes to detached objects are not reflected on the DB. Call ```session.merge(obj)``` to reattach the object to persistent state. Once all data is stored in DB by calling ```transaction.commit()```, **All entity objects in that session will be Detatched.**
* **Removed** - from Persistent state call ```session.remove(obj)``` and data is removed from the DB but its in the JVM.  
** *Note -Transient, Removed and Detatched objects are garbage collected.*   
** *Note - When the entity object is in **Persistent** state and when you change values ```alien.setTech("python")``` will **change the value in the DB**.*  

## Hibernate Get vs Load
_________

Both will give the same output but ```session.get(obj)``` will fire the SQL rightaway and ```session.load(obj)``` will first give a **Proxy** object and will actually fire the SQL and get the real object when the object is used e.g. ```obj.toString()```.

## Hibernate + JPA

Import hibernate-jpa, mysql-connector in th pom.xml file. hibernate-jpa jar has the javax package.    
JPA interface EntityManager === Hibernate Session.    

```java 
EntityManagerFactory emf = Persistence.createEntityManagerFactory("pu");
EntityManager em = emf.createEntityManager();
//fetch Alien with id = 4 from the DB. similler to hibernate session.get()
Alien a = em.find(Alien.class, 4);
Alien b = new Alien(...);
Transaction tx = em.getTransaction();
tx.begin();
//persist() is similer to hibernate session.save();
em.persist(b);
tx.commit();
```
in java/resources/META_INF/persistence.xml

```xml
<persistence .....>
  <persistence-unit name="pu">
    <properties>
      <property name="javax.persistence.jdbc.driver" value="mysql.DriverClass"/>
      //URL, Uname, Passwd
    </properties>
  </persistence-unit/>  
</persistence>
```
