# AppEngineBookshelfPostgres

Modifications to the Bookshelf App for Java. Switched from MySQL to PostgreSQL. 

Changes made:

`pom.xml`
Add PostgreSQL dependencies and removed MySQL dependencies and updated Google api-client and appengine dependencies.

```
<dependency>
  <groupId>com.google.cloud.sql</groupId>
  <artifactId>postgres-socket-factory</artifactId>
  <version>1.0.11</version>
</dependency>

<dependency>
  <groupId>org.postgresql</groupId>
  <artifactId>postgresql</artifactId>
  <version>42.1.1</version>
</dependency>

<dependency>
  <groupId>com.google.api-client</groupId>
  <artifactId>google-api-client-appengine</artifactId>
  <version>1.23.0</version>
</dependency>

<dependency>
  <groupId>com.google.appengine</groupId>
  <artifactId>appengine-api-1.0-sdk</artifactId>
  <version>1.9.71</version>
</dependency>
```

Updated `sql.urlRemote`
```
<context-param>
  <param-name>sql.urlRemote</param-name>
  <param-value>jdbc:postgresql://google/${sql.dbName}?cloudSqlInstance=${sql.instanceName}&amp;socketFactory=com.google.cloud.sql.postgres.SocketFactory&amp;user=${sql.userName}&amp;password=${sql.password}</param-value>
</context-param>
``` 

Updated `listBooks` query to work with Postgres
```java

final String listBooksString = "SELECT author, createdBy, createdById, "
				+ "description, id, publishedDate, title, imageUrl, count(*) OVER() AS total_count FROM books2 ORDER BY title ASC "
				+ "LIMIT 10 OFFSET ?";
```
