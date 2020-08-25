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

the bean class should be a JPA entity

```java
//to change entity name @Entity(name="entityName"). If @Table isn't used entityName is assigned to tableName
@Entity
@Table(name="tableName")
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


