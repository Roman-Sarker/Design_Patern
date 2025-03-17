# **Generics in Java – Complete In-Depth Guide**

**Generics** in Java is a powerful feature that allows developers to write reusable, type-safe code. It enables **parametric polymorphism**, meaning that the same class, interface, or method can operate on different types while ensuring compile-time type safety.

---

## **1. Why Use Generics?**
Before generics, Java used raw types, which caused **type safety issues** and required **explicit casting**.

### **Problem Without Generics (Type Safety Issue)**
```java
import java.util.*;

public class WithoutGenerics {
    public static void main(String[] args) {
        List list = new ArrayList();  // Raw type
        list.add("Hello");
        list.add(10);  // No compile-time error

        String s = (String) list.get(1);  // Causes runtime error: ClassCastException
        System.out.println(s);
    }
}
```
🔴 **Issues Without Generics:**
1. **Type safety issue** – Any type can be added to the collection.
2. **Explicit casting** – Prone to `ClassCastException` at runtime.

### **Solution With Generics**
```java
import java.util.*;

public class WithGenerics {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();  // Type-safe
        list.add("Hello");
        // list.add(10);  // Compile-time error

        String s = list.get(0);  // No casting required
        System.out.println(s);
    }
}
```
✅ **Benefits of Generics:**
- **Compile-time type safety** – Prevents adding incompatible data.
- **No need for explicit casting** – Reduces runtime errors.
- **Code reusability** – Generic classes and methods work with any type.

---

## **2. Generic Classes in Java**
A generic class **allows defining a class with a type parameter**.

✅ **Syntax of a Generic Class**
```java
class Box<T> {  // 'T' is a type parameter
    private T value;

    public void setValue(T value) {
        this.value = value;
    }

    public T getValue() {
        return value;
    }
}
```

✅ **Using the Generic Class**
```java
public class GenericClassExample {
    public static void main(String[] args) {
        Box<Integer> intBox = new Box<>();
        intBox.setValue(100);
        System.out.println(intBox.getValue());

        Box<String> strBox = new Box<>();
        strBox.setValue("Hello Generics");
        System.out.println(strBox.getValue());
    }
}
```
**📌 How It Works:**
- `Box<T>` is a generic class with type parameter `T`.
- `T` is replaced with `Integer` and `String` when creating objects.
- Ensures type safety at compile time.

---

## **3. Generic Methods**
A **generic method** allows defining methods with type parameters independent of the class.

✅ **Example: Generic Method**
```java
class Util {
    // Generic method with type parameter T
    public static <T> void printArray(T[] array) {
        for (T element : array) {
            System.out.print(element + " ");
        }
        System.out.println();
    }
}

public class GenericMethodExample {
    public static void main(String[] args) {
        Integer[] intArray = {1, 2, 3, 4};
        String[] strArray = {"Hello", "Generics"};

        Util.printArray(intArray);
        Util.printArray(strArray);
    }
}
```
**📌 Explanation:**
- `<T>` declares a generic method.
- `T[] array` is a generic parameter.
- Works for both `Integer[]` and `String[]`.

---

## **4. Bounded Type Parameters**
**Bounding** restricts the type parameter to a specific range (e.g., only subclasses of `Number`).

✅ **Example: Upper Bounded Type (`extends`)**
```java
class MathUtil {
    // T must be a subclass of Number
    public static <T extends Number> double square(T num) {
        return num.doubleValue() * num.doubleValue();
    }
}

public class BoundedTypeExample {
    public static void main(String[] args) {
        System.out.println(MathUtil.square(4));        // Works with Integer
        System.out.println(MathUtil.square(3.14));     // Works with Double
        // System.out.println(MathUtil.square("Text")); // Compile-time error
    }
}
```
**📌 Explanation:**
- `<T extends Number>` restricts `T` to **Number and its subclasses** (`Integer`, `Double`, `Float`, etc.).
- Ensures **only numeric types** can be used.

---

## **5. Wildcards in Generics**
Wildcards (`?`) allow **flexibility** when using generics.

### **A. Unbounded Wildcard (`?`)**
Used when the type is unknown or irrelevant.
```java
public static void printList(List<?> list) {  // ? can be any type
    for (Object obj : list) {
        System.out.println(obj);
    }
}
```
✅ **Usage Example**
```java
List<Integer> intList = List.of(1, 2, 3);
List<String> strList = List.of("A", "B", "C");

printList(intList);
printList(strList);
```

---

### **B. Upper Bounded Wildcard (`? extends T`)**
Allows a collection of **T or its subclasses**.

✅ **Example: Accepting Numbers Only**
```java
public static double sum(List<? extends Number> list) {
    double total = 0;
    for (Number num : list) {
        total += num.doubleValue();
    }
    return total;
}
```
✅ **Usage**
```java
List<Integer> intList = List.of(1, 2, 3);
List<Double> doubleList = List.of(1.5, 2.5, 3.5);

System.out.println(sum(intList));   // Output: 6.0
System.out.println(sum(doubleList)); // Output: 7.5
```

---

### **C. Lower Bounded Wildcard (`? super T`)**
Allows a collection of **T or its superclasses**.

✅ **Example: Lower Bound**
```java
public static void addNumbers(List<? super Integer> list) {
    list.add(100);
}
```
✅ **Usage**
```java
List<Number> numList = new ArrayList<>();
addNumbers(numList);  // Works, because Number is a superclass of Integer
```

---

## **6. Generic Interfaces**
✅ **Example: Generic Interface**
```java
interface Data<T> {
    void add(T data);
}

class StringData implements Data<String> {
    public void add(String data) {
        System.out.println("Data: " + data);
    }
}

public class GenericInterfaceExample {
    public static void main(String[] args) {
        Data<String> obj = new StringData();
        obj.add("Hello Interface Generics");
    }
}
```

---

## **7. Type Erasure in Generics**
At runtime, Java **removes type parameters** due to **Type Erasure**.

### **Example**
```java
class GenericClass<T> {
    T value;
}
```
At runtime, this becomes:
```java
class GenericClass {
    Object value;
}
```
🔹 **Why?**
- To maintain **backward compatibility** with older Java versions.
- Generics exist **only at compile-time**.

---

## **8. Key Differences Between Raw Types and Generics**
| Feature | Raw Type | Generics |
|---------|---------|---------|
| Type Safety | No type checking | Ensures type safety |
| Casting | Requires explicit casting | No casting needed |
| Flexibility | Can hold any type | Type-specific |
| Performance | Potential runtime errors | Faster & safer |

---

## **9. Conclusion**
✅ Generics provide **type safety, reusability, and flexibility**.  
✅ They work with **classes, methods, interfaces, and collections**.  
✅ **Wildcards (`?`)** allow for **bounded** and **unbounded** flexibility.  
✅ **Type erasure** removes generic information at runtime.  

Understanding **Generics in Java** is crucial for writing **efficient, scalable, and maintainable** applications! 🚀
