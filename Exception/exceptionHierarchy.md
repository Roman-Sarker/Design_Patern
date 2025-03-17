# **Exception Handling in Java – From `Throwable` to Custom Exceptions**  

Exception handling is a crucial part of Java programming that ensures smooth execution of applications by handling runtime errors efficiently. In Java, all exceptions and errors are derived from the **`Throwable`** class.

---

## **1. Understanding the `Throwable` Class**
The **`Throwable`** class is the root of the exception hierarchy in Java. It has two main subclasses:  

1. **`Exception`** – Recoverable exceptions (e.g., `IOException`, `SQLException`)  
2. **`Error`** – Unrecoverable errors (e.g., `OutOfMemoryError`, `StackOverflowError`)  

```
java.lang.Throwable
│
├── java.lang.Exception  → Checked & Unchecked Exceptions
│   ├── java.io.IOException
│   ├── java.sql.SQLException
│   ├── java.lang.RuntimeException  → Unchecked Exceptions
│       ├── NullPointerException
│       ├── ArrayIndexOutOfBoundsException
│
└── java.lang.Error  → Unrecoverable Errors
    ├── OutOfMemoryError
    ├── StackOverflowError
```

---

## **2. Types of Exceptions in Java**
### **A. Checked Exceptions (Compile-Time)**
Checked exceptions are **mandatory to handle** using `try-catch` or `throws` keyword. If not handled, the code won’t compile.  
🔹 Examples:
- `IOException` – When file operations fail.  
- `SQLException` – When database access issues occur.  
- `ClassNotFoundException` – When a class is missing at runtime.

✅ **Example: Handling Checked Exception**
```java
import java.io.*;

public class CheckedExceptionExample {
    public static void main(String[] args) {
        try {
            FileReader file = new FileReader("file.txt");
            BufferedReader br = new BufferedReader(file);
            System.out.println(br.readLine());
        } catch (IOException e) {
            System.out.println("File not found: " + e.getMessage());
        }
    }
}
```

### **B. Unchecked Exceptions (Runtime)**
Unchecked exceptions occur **at runtime** and do not require explicit handling.  
🔹 Examples:
- `NullPointerException` – Accessing methods on `null`.
- `ArrayIndexOutOfBoundsException` – Indexing outside array bounds.
- `ArithmeticException` – Division by zero.

✅ **Example: Handling Unchecked Exception**
```java
public class UncheckedExceptionExample {
    public static void main(String[] args) {
        try {
            int result = 10 / 0;  // Causes ArithmeticException
        } catch (ArithmeticException e) {
            System.out.println("Cannot divide by zero!");
        }
    }
}
```

---

## **3. `try-catch-finally` Block**
The `try-catch-finally` mechanism is used to **handle exceptions gracefully**.

✅ **Example with `finally`**
```java
public class FinallyExample {
    public static void main(String[] args) {
        try {
            int arr[] = {1, 2, 3};
            System.out.println(arr[5]); // Causes ArrayIndexOutOfBoundsException
        } catch (ArrayIndexOutOfBoundsException e) {
            System.out.println("Index out of bounds!");
        } finally {
            System.out.println("This block always executes.");
        }
    }
}
```
**Key Points:**
- `try` contains the code that might throw an exception.
- `catch` handles the exception.
- `finally` always executes, **whether or not an exception occurs**.

---

## **4. `throw` and `throws` in Java**
### **A. `throw` Keyword**
The `throw` keyword is used **inside a method** to explicitly throw an exception.

✅ **Example: Throwing a Custom Exception**
```java
public class ThrowExample {
    static void checkAge(int age) {
        if (age < 18) {
            throw new IllegalArgumentException("Age must be 18 or above.");
        }
        System.out.println("Eligible to vote.");
    }

    public static void main(String[] args) {
        checkAge(16);  // Throws IllegalArgumentException
    }
}
```

### **B. `throws` Keyword**
The `throws` keyword is used **in the method signature** to indicate that the method might throw an exception.

✅ **Example: Declaring an Exception**
```java
import java.io.*;

public class ThrowsExample {
    static void readFile() throws IOException {
        FileReader file = new FileReader("file.txt");
    }

    public static void main(String[] args) {
        try {
            readFile();
        } catch (IOException e) {
            System.out.println("File not found: " + e.getMessage());
        }
    }
}
```
**Key Differences:**
| `throw`  | `throws`  |
|----------|----------|
| Used to **throw** an exception manually | Used to **declare** exceptions in a method signature |
| Used inside a method body | Used with method signature |
| Only one exception can be thrown at a time | Can declare multiple exceptions (e.g., `throws IOException, SQLException`) |

---

## **5. Custom Exception in Java**
Java allows creating **custom exceptions** by extending `Exception` or `RuntimeException`.

✅ **Example: Creating a Custom Checked Exception**
```java
class AgeException extends Exception {
    public AgeException(String message) {
        super(message);
    }
}

public class CustomExceptionExample {
    static void checkAge(int age) throws AgeException {
        if (age < 18) {
            throw new AgeException("Age must be 18 or above.");
        }
        System.out.println("Welcome!");
    }

    public static void main(String[] args) {
        try {
            checkAge(16);
        } catch (AgeException e) {
            System.out.println("Exception: " + e.getMessage());
        }
    }
}
```

✅ **Example: Creating a Custom Unchecked Exception**
```java
class InvalidDataException extends RuntimeException {
    public InvalidDataException(String message) {
        super(message);
    }
}

public class CustomRuntimeExceptionExample {
    static void processData(int data) {
        if (data < 0) {
            throw new InvalidDataException("Negative data not allowed.");
        }
        System.out.println("Processing data: " + data);
    }

    public static void main(String[] args) {
        processData(-1); // Throws InvalidDataException
    }
}
```

---

## **6. Exception Chaining (Cause & Benefit)**
Exception chaining helps **track the original cause** of an exception using the `Throwable.getCause()` method.

✅ **Example: Chained Exception**
```java
public class ExceptionChainingExample {
    static void method1() throws Exception {
        throw new Exception("Original Exception");
    }

    static void method2() throws Exception {
        try {
            method1();
        } catch (Exception e) {
            throw new Exception("New Exception", e);
        }
    }

    public static void main(String[] args) {
        try {
            method2();
        } catch (Exception e) {
            System.out.println("Exception: " + e.getMessage());
            System.out.println("Caused by: " + e.getCause());
        }
    }
}
```

---

## **7. Best Practices for Exception Handling**
✅ **DOs**
✔ Use **specific exception types** (e.g., `IOException` instead of `Exception`).  
✔ Always clean up resources using `finally` or `try-with-resources`.  
✔ Log exceptions using **SLF4J/Log4J** instead of `printStackTrace()`.  
✔ Create **custom exceptions** for meaningful error messages.  

❌ **DON’Ts**
✘ Don’t use empty catch blocks (`catch (Exception e) {}`) – it hides errors.  
✘ Don’t catch `Throwable` – it includes serious errors like `OutOfMemoryError`.  
✘ Avoid unnecessary use of `throws Exception` at method level.  

---

## **Conclusion**
✔ **Checked Exceptions** = Handled at compile-time (e.g., `IOException`).  
✔ **Unchecked Exceptions** = Occur at runtime (e.g., `NullPointerException`).  
✔ **Use `throw`** to raise exceptions and **`throws`** to declare them.  
✔ **Custom Exceptions** make error handling more meaningful.  
✔ **Exception chaining** helps track root causes.  

This in-depth understanding of **Java Exception Handling** will help you ace interviews and write robust applications! 🚀
