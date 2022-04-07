## Entity
Entities represent persistent data stored in a relational database automatically using container-managed persistence
An entity maps to a table in a database. Every instance of an entity maps to a row in the table.
All the data members of a JPA entity are considered persistent fields unless annotated with `@Transient`.

## Entity Lifecycle
<p align="center">
<img src="https://user-images.githubusercontent.com/44853589/162128711-7ed5fa25-4732-48b0-98ef-cf4482ab914d.png" width=70% height=70%>
</p>

### Transient
As soon as a new object is instantiated it is in the *transient* state. It doesn't represent any database record.
Persistence context is not aware of newly instantiated object. Therefore, it doesn't track any changes. 
It's just a plain object without JPA functionalities.

```java
User user = new User();
user.setName("Elyorbek");
```

### Managed
When the objects are attached to the current persistence context they are in the *managed* lifecycle state.
Any changes on the objects triggers underlying persistence provider the generation of required SQL INSERT or UPDATE statements
when it flushes the persistence context. There are different ways to change the objects into *maanaged* state.

1. Persist a new entity objects through calling `persist()` method of `EntityManager`.
```java
User user = new User();
user.setName("Elyorbek");
entityManager.
```
2. Load the entity object using `find()` method of `EntityManager`, `JPQL`, `Criteria`, or native `SQL` query.
```java
User user = entityManager.find(User.class, 1L);
```
3. Merge the state of the entity object into the current persistence context using `merge()` method of `EntityManager`
```java
entityManager.merge(user);
```

### Detached
An entity that was previously managed but is no longer attached to the current persistence context is in the *detached* state. 

When the persistence context is closed the entity gets detached. Persisting a new entity object to the database can be a good example.
After the request got processed the database transaction gets commited and the persistence is closed. Then the entity object gets returned to
the caller. Retrieved object is in the *detached* state.

Detaching an entity can be done at the application level as well by calling the `detach()` method of `EntityManager`.
```java
entityManager.detach(user);
````

### Removed
Calling `remove()` method of `EntityManager` doesn't remove the mapped record immediately. Instead, the entity object lifecycle state changes
to *removed*. Next `flush()` operation triggers the generation of SQL DELETE statement, thus synchronizing the persistence context to the underlying database.
```java
entityManager.remove(user);
```
