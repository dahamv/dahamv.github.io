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

###  Embeddable Objects
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

### Mapping Relations
#### One-to-One
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
  private Laptop sLaptop;

}
```

#### One-to-Many and Many-to-One
One Student can have many Laptops. there are two approaches.  
1. Create another table **student_laptop** and have the mappings there.
2. Create a column **laptopOwner** in the Laptop table having sId s. Since **one Laptop is owned by one Student** and **one Student has many Laptops.**
* When you say ```@...ToMany``` a table will be made.  
* When you say ```@...ToOne``` a column will be made.

##### Approach 1  

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
##### Approach 2 (Prefered)
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
#### Many-to-Many
One Laptop can be used by many Students. One Student can have Many Laptops. E.g. College computer lab.  
**We MUST need a another seperate table (Laptop_Student) to store the mapping.
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
### Eager and Lazy Fetch
- **FetchType.EAGER** - Hibernate runs the sql before hand. If a Student is read, all properties 
sId, sName and sLaptops (the List of Laptops) will be read right away at ```session.get(Student.class, studentId);```
- **FetchType.LAZY** - runs the sql only when its needed. When a Student is read only sId and sName will be read.
The List of Laptops will be read only when its asked for. ```student.getLaptops(); ``` 
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
