SessionFactory - has information to connect to DB. i.e. the db-url, username, password, db-driver etc.  
```java
Alien alien = new Alien();
alien.setValues(....);

//hibernate configuration. hibernate.cfg.xml is the default name so no need to specify it. 
Configuration con = new Configuration().configure("hibernate.cfg.xml").addAnnotatedClass(Alien.class);
//deprecated. but for the time being
SessionFactory sf = con.buildSessionFactory();
Transaction...
session session = sf.openSession();

session.save(alien)
```
in **hibernate.cfg.xml** specify driver.class, connection-url, username, password, hibernate-dialect

the bean class should be a JPA entity

```java
@Entity
public class Alien {
  //JPA annotation for primary key 
  @Id
  private int aid;
}
```


