In the context of **Java Persistence API (JPA)** and **Hibernate**, **TPQL (Transaction Persistence Query Language)**, **HQL (Hibernate Query Language)**, and **Native Query** are different ways to query the database, each with its own use case and syntax.

### 1. **HQL (Hibernate Query Language)**

#### **Concept:**
HQL is the **Hibernate-specific query language**, similar to SQL but it operates on **Hibernate entities** (objects) instead of database tables. It is **object-oriented** and abstracts away the database-specific details.

- **Entity-based**: HQL operates on **persistent objects** (Java classes) rather than directly on database tables.
- **Database-independent**: HQL is **database-agnostic**, meaning the same HQL query can be used across different databases, provided the entity mappings are correctly configured.

#### **Key Features of HQL:**
- It allows querying data based on Java classes (entities), not database tables.
- HQL supports **joins**, **grouping**, and other SQL operations but using Java objects and fields.
- It can be used for both **select** and **update** queries.
- It also supports **projection**, **aliasing**, and **subqueries**.

#### **Example:**
```java
// HQL query to select a list of users
String hql = "FROM User u WHERE u.name = :name";
Query query = session.createQuery(hql);
query.setParameter("name", "John");
List<User> users = query.list();
```

Here, `User` is a Java entity, and `name` is a property of the `User` class, not a database column.

---

### 2. **JPQL (Java Persistence Query Language)**

#### **Concept:**
JPQL is a **JPA specification** for querying relational databases through **Java objects (entities)**. It is similar to HQL but follows the **JPA specification**, meaning it works in any **JPA-compliant persistence framework** like **Hibernate** and **EclipseLink**.

- **Object-Oriented**: JPQL works with Java objects and their properties (fields), not the database tables.
- **Standardized**: Since it's a **JPA specification**, it's portable across JPA implementations (e.g., Hibernate, EclipseLink, OpenJPA).
  
#### **Key Features of JPQL:**
- It is **database-agnostic**, meaning you can use it with any JPA provider.
- Supports **select**, **insert**, **update**, and **delete** operations.
- Supports **relationships** between entities using object references instead of foreign keys.

#### **Example:**
```java
// JPQL query to select users by their status
String jpql = "SELECT u FROM User u WHERE u.status = :status";
TypedQuery<User> query = entityManager.createQuery(jpql, User.class);
query.setParameter("status", "active");
List<User> activeUsers = query.getResultList();
```

In this example, `User` is an entity, and the query retrieves all `User` objects with a specific status.

---

### 3. **Native SQL Query**

#### **Concept:**
A **Native SQL query** is a standard **SQL query** (written in native database SQL) executed directly against the **relational database**, bypassing any abstraction provided by **JPA** or **Hibernate**.

- **Database-Specific**: Native SQL queries are **not database-agnostic** since they are tied to a specific database dialect.
- **Direct SQL Access**: Unlike HQL and JPQL, which abstract the underlying database, native SQL allows for the full flexibility of SQL syntax and can be used for complex queries, database-specific functions, and optimization.

#### **Key Features of Native Queries:**
- **Supports database-specific SQL**: Allows the use of **vendor-specific SQL features** (e.g., `LIMIT` in MySQL, `ROWNUM` in Oracle).
- It is **not object-oriented**—native SQL works directly on database tables, which means you query the tables and columns directly.
- You can retrieve results as **objects**, **primitives**, or **arrays**.

#### **Example:**
```java
// Native SQL query to retrieve users by status
String sql = "SELECT * FROM users WHERE status = :status";
Query query = session.createNativeQuery(sql, User.class);
query.setParameter("status", "active");
List<User> activeUsers = query.getResultList();
```

In this case, `users` is a database table, and the query is a standard SQL query.

---

### **Key Differences:**

| Feature                 | **HQL**                               | **JPQL**                               | **Native Query**                            |
|-------------------------|---------------------------------------|----------------------------------------|---------------------------------------------|
| **Query Language**       | Hibernate-specific                    | JPA-compliant (standardized)           | Database-specific SQL                       |
| **Use Case**             | For Hibernate-based queries           | For JPA-based queries                  | For database-specific complex queries       |
| **Entities or Tables**   | Queries on Java entities (objects)    | Queries on Java entities (objects)     | Queries on database tables (no abstraction) |
| **Portability**          | Hibernate-specific, not portable      | Portable across JPA implementations    | Not portable, tied to a specific database   |
| **Complex Queries**      | Supports complex object-based queries | Supports complex object-based queries  | Supports complex SQL queries with database functions |
| **Database Independence**| Yes (HQL is database-agnostic)        | Yes (JPQL is database-agnostic)        | No (depends on SQL dialect of database)     |
| **Performance**          | Can be slower than native SQL         | Can be slower than native SQL          | Faster for complex queries as it is directly SQL |
  
---

### **When to Use Each?**
1. **HQL/JPQL**:
   - When you want to work with **Java entities** and need a **database-agnostic** approach.
   - When you need **portable code** across different databases.
   - When you need to leverage JPA/Hibernate's object-relational mapping (ORM) features.

2. **Native SQL**:
   - When you need to execute **complex SQL** that cannot be represented in HQL/JPQL.
   - When you need to use **database-specific functions** (e.g., `JOIN`, `GROUP BY`, `HAVING`).
   - For **performance optimization** in cases where HQL/JPQL may not perform well.

---

### **Conclusion:**
- **HQL** and **JPQL** are **object-oriented query languages** designed for querying Java entities, with **JPQL** being the JPA standard.
- **Native SQL** provides **direct access to SQL queries**, giving you full control over the database but losing the database-agnostic flexibility.
- **Choosing between them** depends on the complexity of your query, performance needs, and whether you need database-specific functionality.

