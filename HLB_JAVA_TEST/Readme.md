
Here is a **line-by-line explanation** of the given Java code, breaking it down by method and explaining each step:

---

### **Imports and Class Declaration**

```java
import java.text.SimpleDateFormat;
import java.util.*;
```
- `SimpleDateFormat` is used to parse and format date strings.
- `java.util.*` imports classes like `Date`, `List`, `Map`, `HashMap`, `ArrayList`, etc.

```java
class JavaTest {
```
- Defines a public class named `JavaTest`.

---

### **1. Reverse a String Without Using Built-in `reverse()`**

```java
public static String reverseStringWithoutUsingStringMethod(String s) {
    StringBuilder reversed = new StringBuilder();
    for (int i = s.length() - 1; i >= 0; i--) {
        reversed.append(s.charAt(i));
    }
    return reversed.toString();
}
```
- This method reverses the input string `s` manually.
- A new `StringBuilder` is used to build the reversed string.
- It loops from the last character of `s` to the first and appends each character to `reversed`.
- Returns the reversed string.

---

### **2. Sort an Integer Array in Ascending Order (Without `Arrays.sort`)**

```java
public static Integer[] sortIntegers(Integer[] array) {
    for (int i = 0; i < array.length - 1; i++) {
        for (int j = i + 1; j < array.length; j++) {
            if (array[i] > array[j]) {
                int temp = array[i];
                array[i] = array[j];
                array[j] = temp;
            }
        }
    }
    return array;
}
```
- Implements a simple **bubble sort**.
- Compares each element `array[i]` with all elements after it and swaps if it's greater.
- Ensures smallest elements move toward the front.
- Returns the sorted array.

---

### **3. Check If a Date Falls Within a Date Range**

```java
public static boolean isInDateRange(Date givenDate, Date startDate, Date endDate) {
    return givenDate.compareTo(startDate) >= 0 && givenDate.compareTo(endDate) <= 0;
}
```
- Uses `compareTo()` to check:
  - If `givenDate` is **on or after** `startDate`.
  - And **on or before** `endDate`.
- Returns `true` if it's within the range.

---

### **4. Sort Characters in a String in Ascending Order (Without `Arrays.sort`)**

```java
public static char[] sortStringInAscOrder(String input) {
    char[] chars = input.toCharArray();
    for (int i = 0; i < chars.length - 1; i++) {
        for (int j = i + 1; j < chars.length; j++) {
            if (chars[i] > chars[j]) {
                char temp = chars[i];
                chars[i] = chars[j];
                chars[j] = temp;
            }
        }
    }
    return chars;
}
```
- Converts the input string to a char array.
- Sorts characters using **bubble sort** logic.
- Returns sorted `char[]`.

---

### **5. Find the Character with the Lowest Occurrence**

```java
public static char lowestOccurrence(String input) {
    Map<Character, Integer> map = new HashMap<>();
    for (char c : input.toCharArray()) {
        map.put(c, map.getOrDefault(c, 0) + 1);
    }
```
- Counts occurrences of each character using a HashMap.

```java
    int min = Integer.MAX_VALUE;
    List<Character> minChars = new ArrayList<>();
```
- `min` stores the minimum count found.
- `minChars` holds characters with the same lowest count.

```java
    for (Map.Entry<Character, Integer> entry : map.entrySet()) {
        if (entry.getValue() < min) {
            min = entry.getValue();
            minChars.clear();
            minChars.add(entry.getKey());
        } else if (entry.getValue() == min) {
            minChars.add(entry.getKey());
        }
    }
```
- Loop to find characters with the lowest frequency.

```java
    Collections.sort(minChars); // Get smallest in ASC order
    return minChars.get(0);
}
```
- Sorts characters with minimum frequency.
- Returns the lexicographically smallest one.

---

### **6. Evaluate Expressions Using 'x'**

```java
public static double solveEquations(double input, String[] equations) {
    double x = input;
    for (String eq : equations) {
        String expr = eq.replace("x", Double.toString(x));
        try {
            x = eval(expr);
        } catch (Exception e) {
            System.out.println("Invalid expression: " + expr);
            return Double.NaN;
        }
    }
    return x;
}
```
- Takes an input value and an array of string equations.
- Replaces `'x'` in the equation with the current value of `x`.
- Evaluates each expression using `eval()`.
- Returns the final computed value.

---

### **7. Evaluate Simple Arithmetic Expression (`+`, `-`, `*`, `/`)**

This uses an **anonymous inner class** with methods for parsing and evaluating expressions manually.

#### a. Move to next character
```java
void nextChar() {
    ch = (++pos < expr.length()) ? expr.charAt(pos) : -1;
}
```

#### b. Consume specific character
```java
boolean eat(int charToEat) {
    while (ch == ' ') nextChar();
    if (ch == charToEat) {
        nextChar();
        return true;
    }
    return false;
}
```

#### c. Parse full expression
```java
double parse() {
    nextChar();
    double x = parseExpression();
    if (pos < expr.length()) throw new RuntimeException("Unexpected: " + (char) ch);
    return x;
}
```

#### d. Parse expression with '+' or '-'
```java
double parseExpression() {
    double x = parseTerm();
    while (true) {
        if (eat('+')) x += parseTerm();
        else if (eat('-')) x -= parseTerm();
        else return x;
    }
}
```

#### e. Parse term with '*' or '/'
```java
double parseTerm() {
    double x = parseFactor();
    while (true) {
        if (eat('*')) x *= parseFactor();
        else if (eat('/')) x /= parseFactor();
        else return x;
    }
}
```

#### f. Parse single number or negative
```java
double parseFactor() {
    if (eat('+')) return parseFactor();
    if (eat('-')) return -parseFactor();

    double x;
    int startPos = this.pos;
    if ((ch >= '0' && ch <= '9') || ch == '.') {
        while ((ch >= '0' && ch <= '9') || ch == '.') nextChar();
        x = Double.parseDouble(expr.substring(startPos, this.pos));
    } else {
        throw new RuntimeException("Unexpected: " + (char) ch);
    }

    return x;
}
```

---

### **8. `main()` Method to Test All Above Methods**

```java
public static void main(String[] args) {
    System.out.println("Test 1: " + reverseStringWithoutUsingStringMethod("Automation"));
```
- Calls the reverse function.

```java
    Integer[] intArray = new Integer[]{10, 12, 54, 1, 2, -9, 8};
    System.out.print("Test 2: ");
    Integer[] sortedIntArray = sortIntegers(intArray);
    for (Integer i : sortedIntArray) {
        System.out.print(i + ", ");
    }
```
- Sorts and prints an integer array.

```java
    SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
    Date startDate = sdf.parse("2024-12-01 13:09:22");
    Date endDate = sdf.parse("2025-01-09 20:10:12");
    Date givenDate = sdf.parse("2025-02-02 00:11:22");
    System.out.println("Test 3: " + isInDateRange(givenDate, startDate, endDate));
```
- Parses date strings and checks if the given date is in range.

```java
    char[] sorted = sortStringInAscOrder("testingNG311");
    System.out.print("Test 4 :");
    for (char c : sorted) {
        System.out.print(c + " ");
    }
```
- Sorts string characters and prints them.

> The rest of `main()` likely continues testing other methods similarly.

---

Would you like me to continue with the rest of the `main()` method or provide a diagram or flowchart for one of the functions?
