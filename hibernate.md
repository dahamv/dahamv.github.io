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
in **hibernate.cfg.xml** specify driver.class, connection-url, username, password, hibernate-dialect  
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
  @OneToOne // Student tablt will have a forignKey colomn to Laptop.
  private Laptop sLaptop;

}
```

#### One-to-Many and Many-to-One
One Student can have many Laptops. there are two approaches.  
1. Create another table **student_laptop** and have the mappings there.
2. Create a column **laptopOwner** in the Laptop table having sId s. Since **one Laptop is owned by one Student** and **one Student has many Laptops.**

##### Approach 1
```java
@Entity
public class Student {
  @Id
  private int sId;
  private String sName;
  @OneToMany // Another table student_laptop (with columns lId and sId) will be created.
  private List<Laptop> sLaptops;

}
```
##### Approach 2
```java
@Entity
public class Laptop {
  @Id
  private int lId;
  private String lName;
  @ManyToOne // a column will be made in Laptop table to store the sId of the Studends who are the laptop owners
  private Student laptopOwner;
}

@Entity
public class Student {
  @Id
  private int sId;
  private String sName;
  @OneToMany(mappedBy="laptopOwner") // If mappedBy is not used, the student_laptop table will be made by hibernate. So do this to prevent that.
  private List<Laptop> sLaptops;

}
```


