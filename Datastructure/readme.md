Here’s the table you can copy directly:  

| **Data Structure**  | **Time Complexity (Search)** | **Time Complexity (Insert/Delete)** | **Use Case** |
|--------------------|--------------------------|-----------------------------|-------------|
| **Array**         | O(1)                      | O(n)                         | Fast access, fixed size |
| **ArrayList**     | O(1)                      | O(n)                         | Dynamic array, fast access |
| **LinkedList**    | O(n)                      | O(1)                         | Frequent insertions/deletions |
| **Stack**        | O(n)                      | O(1)                         | Undo, recursion |
| **Queue**        | O(n)                      | O(1)                         | Task scheduling, BFS |
| **PriorityQueue** | O(log n)                  | O(log n)                     | Sorting, scheduling |
| **HashMap**       | O(1)                      | O(1)                         | Caching, fast lookups |
| **TreeMap**       | O(log n)                  | O(log n)                     | Ordered key-value storage |
| **HashSet**       | O(1)                      | O(1)                         | Unique elements |
| **Graph**        | O(n)                      | O(1)                         | Networks, routing |

Let me know if you need any modifications! 🚀

---
### **Understanding Big O Notation: O(1), O(n), O(log n)**  

Big O notation describes **how the runtime (execution time) of an algorithm grows** as the size of the input (**n**) increases. It helps evaluate **algorithm efficiency**.

---

## **1️⃣ O(1) - Constant Time Complexity**
- The runtime **does not depend on input size (n)**.
- The operation takes the **same amount of time** regardless of `n`.

🔹 **Example:**
```java
int getFirstElement(int[] arr) {
    return arr[0];  // Always takes constant time
}
```
🔹 **Time Complexity:** **O(1)** (Only one operation, no loops)  

📌 **Real-World Use Cases:**  
✅ Accessing an array element `arr[index]`  
✅ HashMap lookup (`map.get(key)`)  

---

## **2️⃣ O(n) - Linear Time Complexity**
- The runtime **grows directly proportional to the input size**.
- If `n` doubles, the execution time **also doubles**.

🔹 **Example:**
```java
void printArray(int[] arr) {
    for (int num : arr) {
        System.out.println(num);
    }
}
```
🔹 **Time Complexity:** **O(n)** (Executes `n` times)

📌 **Real-World Use Cases:**  
✅ Looping through an array or list  
✅ Searching in an **unsorted** array  

---

## **3️⃣ O(log n) - Logarithmic Time Complexity**
- The runtime **grows much slower** than `n`.
- The problem size is **divided by 2 in each step**, reducing the number of operations significantly.

🔹 **Example (Binary Search):**
```java
int binarySearch(int[] arr, int target) {
    int left = 0, right = arr.length - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (arr[mid] == target)
            return mid;
        else if (arr[mid] < target)
            left = mid + 1;
        else
            right = mid - 1;
    }
    return -1; // Element not found
}
```
🔹 **Time Complexity:** **O(log n)** (Each step halves the search space)  

📌 **Real-World Use Cases:**  
✅ Binary search in a sorted array  
✅ Balanced search trees (e.g., BST, AVL Tree)  
✅ Searching in a phone book  

---

### **Comparison Table**
| **Big O** | **Example Algorithm** | **Performance** |
|----------|-----------------|--------------|
| **O(1)** | Array access, HashMap lookup | Fastest |
| **O(log n)** | Binary search | Very fast |
| **O(n)** | Looping through an array | Slower as `n` grows |

Would you like examples for other complexities like **O(n²), O(2ⁿ), O(n log n)**? 🚀
