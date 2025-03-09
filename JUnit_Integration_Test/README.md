### **📌 JUnit Test vs. Integration Test in Spring Boot**
When developing a Spring Boot application, **testing** ensures the reliability and correctness of the code. The two most common types of tests are:

| **Test Type**      | **Description** |
|--------------------|---------------|
| **JUnit Test (Unit Test)** | Tests a single method or class in isolation. It does not involve the database or Spring context. |
| **Integration Test** | Tests the interaction between multiple components (e.g., Controller ↔ Service ↔ Repository) and usually involves the Spring context and database. |

---

## **1️⃣ CRUD Operations in Spring Boot**
Let's implement a **Spring Boot CRUD API** for managing `Product` entities.

### **📂 Project Structure**
```
📦 com.example.product
 ┣ 📂 controller
 ┃ ┗ 📜 ProductController.java
 ┣ 📂 service
 ┃ ┗ 📜 ProductService.java
 ┣ 📂 repository
 ┃ ┗ 📜 ProductRepository.java
 ┣ 📂 model
 ┃ ┗ 📜 Product.java
 ┣ 📜 ProductApplication.java
 ┣ 📜 application.properties
 ┗ 📂 test
   ┣ 📜 ProductServiceTest.java (JUnit Test)
   ┗ 📜 ProductIntegrationTest.java (Integration Test)
```

---

## **2️⃣ Implementing CRUD Operations**
### **🔹 Product Model**
```java
package com.example.product.model;

import jakarta.persistence.*;
import lombok.*;

@Entity
@Data
@NoArgsConstructor
@AllArgsConstructor
public class Product {
    
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String name;
    private double price;
}
```

---

### **🔹 Product Repository**
```java
package com.example.product.repository;

import com.example.product.model.Product;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface ProductRepository extends JpaRepository<Product, Long> {
}
```

---

### **🔹 Product Service**
```java
package com.example.product.service;

import com.example.product.model.Product;
import com.example.product.repository.ProductRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import java.util.List;
import java.util.Optional;

@Service
public class ProductService {

    @Autowired
    private ProductRepository productRepository;

    public List<Product> getAllProducts() {
        return productRepository.findAll();
    }

    public Product getProductById(Long id) {
        return productRepository.findById(id).orElse(null);
    }

    public Product createProduct(Product product) {
        return productRepository.save(product);
    }

    public Product updateProduct(Long id, Product product) {
        Optional<Product> existingProduct = productRepository.findById(id);
        if (existingProduct.isPresent()) {
            product.setId(id);
            return productRepository.save(product);
        }
        return null;
    }

    public void deleteProduct(Long id) {
        productRepository.deleteById(id);
    }
}
```

---

### **🔹 Product Controller**
```java
package com.example.product.controller;

import com.example.product.model.Product;
import com.example.product.service.ProductService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/products")
public class ProductController {

    @Autowired
    private ProductService productService;

    @GetMapping
    public List<Product> getAllProducts() {
        return productService.getAllProducts();
    }

    @GetMapping("/{id}")
    public Product getProductById(@PathVariable Long id) {
        return productService.getProductById(id);
    }

    @PostMapping
    public Product createProduct(@RequestBody Product product) {
        return productService.createProduct(product);
    }

    @PutMapping("/{id}")
    public Product updateProduct(@PathVariable Long id, @RequestBody Product product) {
        return productService.updateProduct(id, product);
    }

    @DeleteMapping("/{id}")
    public void deleteProduct(@PathVariable Long id) {
        productService.deleteProduct(id);
    }
}
```

---

## **3️⃣ JUnit Test (Unit Testing for Service Layer)**
📌 **Unit Test:** Tests a single unit (method) in isolation without database interaction.

📌 **Mockito:** Used for mocking dependencies (repository).

📌 **@ExtendWith(MockitoExtension.class):** Enables Mockito annotations.

📌 **@Mock:** Mocks `ProductRepository`.

📌 **@InjectMocks:** Injects mocked dependencies into `ProductService`.

```java
package com.example.product;

import com.example.product.model.Product;
import com.example.product.repository.ProductRepository;
import com.example.product.service.ProductService;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.junit.jupiter.MockitoExtension;

import java.util.Arrays;
import java.util.List;
import java.util.Optional;

import static org.junit.jupiter.api.Assertions.*;
import static org.mockito.Mockito.*;

@ExtendWith(MockitoExtension.class)
class ProductServiceTest {

    @Mock
    private ProductRepository productRepository;

    @InjectMocks
    private ProductService productService;

    private Product product;

    @BeforeEach
    void setUp() {
        product = new Product(1L, "Laptop", 1200.0);
    }

    @Test
    void testGetAllProducts() {
        when(productRepository.findAll()).thenReturn(Arrays.asList(product));
        List<Product> products = productService.getAllProducts();
        assertEquals(1, products.size());
    }

    @Test
    void testGetProductById() {
        when(productRepository.findById(1L)).thenReturn(Optional.of(product));
        Product foundProduct = productService.getProductById(1L);
        assertNotNull(foundProduct);
        assertEquals("Laptop", foundProduct.getName());
    }

    @Test
    void testCreateProduct() {
        when(productRepository.save(product)).thenReturn(product);
        Product savedProduct = productService.createProduct(product);
        assertNotNull(savedProduct);
        assertEquals("Laptop", savedProduct.getName());
    }

    @Test
    void testUpdateProduct() {
        when(productRepository.findById(1L)).thenReturn(Optional.of(product));
        when(productRepository.save(product)).thenReturn(product);
        Product updatedProduct = productService.updateProduct(1L, product);
        assertNotNull(updatedProduct);
        assertEquals("Laptop", updatedProduct.getName());
    }

    @Test
    void testDeleteProduct() {
        doNothing().when(productRepository).deleteById(1L);
        productService.deleteProduct(1L);
        verify(productRepository, times(1)).deleteById(1L);
    }
}
```

---

## **4️⃣ Integration Test (Testing API Endpoints)**
📌 **Integration Test:** Tests multiple components together (Controller ↔ Service ↔ Repository).  
📌 **@SpringBootTest:** Loads the full application context.  
📌 **@Testcontainers:** Uses a real PostgreSQL/MySQL database in a container.  
📌 **MockMvc:** Performs HTTP requests and verifies responses.  

```java
package com.example.product;

import com.example.product.model.Product;
import com.fasterxml.jackson.databind.ObjectMapper;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.AutoConfigureMockMvc;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.http.MediaType;
import org.springframework.test.web.servlet.MockMvc;

import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.*;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.*;

@SpringBootTest
@AutoConfigureMockMvc
public class ProductIntegrationTest {

    @Autowired
    private MockMvc mockMvc;

    @Autowired
    private ObjectMapper objectMapper;

    @Test
    void testCreateProduct() throws Exception {
        Product product = new Product(null, "Smartphone", 800.0);
        mockMvc.perform(post("/products")
                .contentType(MediaType.APPLICATION_JSON)
                .content(objectMapper.writeValueAsString(product)))
                .andExpect(status().isOk())
                .andExpect(jsonPath("$.name").value("Smartphone"));
    }

    @Test
    void testGetAllProducts() throws Exception {
        mockMvc.perform(get("/products"))
                .andExpect(status().isOk());
    }
}
```

---

## **5️⃣ Summary**
- ✅ **JUnit Test** – Tests `ProductService` using **Mockito**.
- ✅ **Integration Test** – Tests API endpoints using **MockMvc**.
- ✅ **Spring Boot ensures high test reliability** with **@SpringBootTest**.

🚀 **Now, you have a fully tested CRUD Spring Boot application!**
