## Entity
Entities represent persistent data stored in a relational database automatically using container-managed persistence
An entity maps to a table in a database. Every instance of an entity maps to a row in the table.
All the data members of a JPA entity are considered persistent fields unless annotated with `@Transient`.
