### **🔹 `@Transactional` Annotation in Spring Boot**  

The `@Transactional` annotation in **Spring Boot** is used to manage database transactions automatically. It ensures that **a set of database operations (CRUD) is executed as a single unit of work**, maintaining **data integrity and consistency**.

---

### **1️⃣ Why Use `@Transactional`?**
🔹 **Ensures Atomicity** – If one operation fails, all previous operations in the transaction are rolled back.  
🔹 **Manages Transactions Automatically** – No need to write manual commit/rollback logic.  
🔹 **Optimizes Performance** – Reduces database connection overhead by batching multiple operations.  

---

### **2️⃣ How `@Transactional` Works?**
- When you apply `@Transactional` to a method, Spring **opens a transaction** before the method runs.
- If the method completes successfully, the transaction **commits**.
- If an exception occurs, the transaction **rolls back** (by default, for unchecked exceptions: `RuntimeException` and `Error`).

---

### **3️⃣ Example Usage in Spring Boot**
#### ✅ **Basic Usage**
```java
@Service
public class ProductService {
    @Autowired
    private ProductRepository productRepository;

    @Transactional
    public void saveProduct(Product product) {
        productRepository.save(product);
        // If an exception occurs here, the transaction will be rolled back
    }
}
```
🔹 **If `saveProduct()` runs successfully → transaction commits.**  
🔹 **If an exception occurs → transaction rolls back.**  

---

### **4️⃣ Rollback Behavior**
By default, `@Transactional` **only rolls back for unchecked exceptions (`RuntimeException` and `Error`)**.

#### ❌ **Will NOT Rollback (Checked Exception)**
```java
@Transactional
public void updateProduct(Product product) throws Exception {
    productRepository.save(product);
    throw new Exception("Checked exception - No rollback!"); // Transaction still commits
}
```
✅ **To force rollback for checked exceptions, specify `rollbackFor = Exception.class`:**
```java
@Transactional(rollbackFor = Exception.class)
public void updateProduct(Product product) throws Exception {
    productRepository.save(product);
    throw new Exception("Checked exception - Rollback will happen!");
}
```

---

### **5️⃣ Propagation Types**
You can control transaction behavior using **`propagation`** attribute:

| **Propagation Type** | **Description** |
|---------------------|---------------|
| `REQUIRED` (default) | Uses an existing transaction or creates a new one if none exists. |
| `REQUIRES_NEW` | Always starts a new transaction, suspending any existing one. |
| `MANDATORY` | Must run inside an existing transaction, else throws an exception. |
| `SUPPORTS` | Uses an existing transaction if available, else runs without one. |
| `NOT_SUPPORTED` | Runs outside any existing transaction. |
| `NEVER` | Throws an error if a transaction exists. |

**Example:**
```java
@Transactional(propagation = Propagation.REQUIRES_NEW)
public void updateStock(Long productId, int quantity) {
    // This runs in a separate transaction
}
```

---

### **6️⃣ Isolation Levels**
You can control how transactions **isolate changes** from other transactions.

| **Isolation Level** | **Description** |
|---------------------|---------------|
| `DEFAULT` | Uses database default isolation level. |
| `READ_COMMITTED` | Prevents dirty reads (default in most databases). |
| `READ_UNCOMMITTED` | Allows dirty reads (unsafe but faster). |
| `REPEATABLE_READ` | Prevents non-repeatable reads. |
| `SERIALIZABLE` | Strictest level, ensures full isolation (slowest). |

**Example:**
```java
@Transactional(isolation = Isolation.REPEATABLE_READ)
public void processOrder(Long orderId) {
    // Prevents other transactions from modifying data until this transaction completes
}
```

---

### **7️⃣ Summary**
✔ `@Transactional` ensures **atomicity** of database operations.  
✔ **Automatically rolls back transactions on `RuntimeException` or `Error`**.  
✔ **Supports transaction propagation and isolation levels** for fine control.  
✔ **Prevents data corruption** in concurrent applications.  

🚀 **In short, `@Transactional` makes database operations safer, more efficient, and easier to manage!**
