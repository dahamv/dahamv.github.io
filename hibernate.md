SessionFactory - has information to connect to DB. i.e. the db-url, username, password, db-driver etc.  
```java
//hibernate configuration
Configuration con = new Configuration();
//deprecated. but for the time being
SessionFactory sf = con.buildSessionFactory();
session session = sf.openSession();
session.save(obj)

```

