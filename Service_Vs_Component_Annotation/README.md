### **Short Difference: @Service vs @Component**  

| Feature        | `@Component` | `@Service` |
|--------------|-------------|------------|
| **Purpose**  | Generic Spring bean | Business logic layer |
| **Use Case** | Utility/helper classes | Service classes (business logic) |
| **Spring Behavior** | Bean scanning | Bean scanning + AOP support |
| **Example** | `EmailValidator` | `LoanService` |

🔹 **Use `@Component`** for generic utilities.  
🔹 **Use `@Service`** for business logic and transactions.
