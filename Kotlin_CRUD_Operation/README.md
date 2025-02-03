Here’s a **full CRUD (Create, Read, Update, Delete) REST API in Kotlin** using **Spring Boot and JPA**, along with a **global exception handler**.

---

### **Tech Stack:**
- **Spring Boot** (with Kotlin)
- **Spring Data JPA**
- **H2 Database** (for in-memory testing)
- **Spring Boot Validation**
- **Global Exception Handling using `@ControllerAdvice`**

---

### **1. Set Up Dependencies in `build.gradle.kts`**
```kotlin
plugins {
    kotlin("jvm") version "2.1.0"
    kotlin("plugin.spring") version "2.1.0"
    kotlin("plugin.jpa") version "2.1.0"
    id("org.springframework.boot") version "3.3.3"
    id("io.spring.dependency-management") version "1.1.3"
}

group = "com.example"
version = "1.0"

repositories {
    mavenCentral()
}

dependencies {
    implementation("org.springframework.boot:spring-boot-starter-web")
    implementation("org.springframework.boot:spring-boot-starter-data-jpa")
    implementation("com.h2database:h2") // In-memory database
    implementation("org.springframework.boot:spring-boot-starter-validation")
}
```

---

### **2. Create the Entity (`User.kt`)**
```kotlin
package com.example.model

import jakarta.persistence.*
import jakarta.validation.constraints.Email
import jakarta.validation.constraints.NotBlank

@Entity
@Table(name = "users")
data class User(
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    val id: Long = 0,

    @field:NotBlank(message = "Name cannot be empty")
    val name: String,

    @field:Email(message = "Invalid email format")
    @field:NotBlank(message = "Email cannot be empty")
    val email: String
)
```

---

### **3. Create Repository (`UserRepository.kt`)**
```kotlin
package com.example.repository

import com.example.model.User
import org.springframework.data.jpa.repository.JpaRepository
import org.springframework.stereotype.Repository

@Repository
interface UserRepository : JpaRepository<User, Long>
```

---

### **4. Create Service Layer (`UserService.kt`)**
```kotlin
package com.example.service

import com.example.exception.UserNotFoundException
import com.example.model.User
import com.example.repository.UserRepository
import org.springframework.stereotype.Service

@Service
class UserService(private val userRepository: UserRepository) {

    fun getAllUsers(): List<User> = userRepository.findAll()

    fun getUserById(id: Long): User = userRepository.findById(id)
        .orElseThrow { UserNotFoundException("User with ID $id not found") }

    fun createUser(user: User): User = userRepository.save(user)

    fun updateUser(id: Long, updatedUser: User): User {
        val existingUser = getUserById(id)
        val newUser = existingUser.copy(name = updatedUser.name, email = updatedUser.email)
        return userRepository.save(newUser)
    }

    fun deleteUser(id: Long) {
        val user = getUserById(id)
        userRepository.delete(user)
    }
}
```

---

### **5. Create Controller (`UserController.kt`)**
```kotlin
package com.example.controller

import com.example.model.User
import com.example.service.UserService
import jakarta.validation.Valid
import org.springframework.http.HttpStatus
import org.springframework.http.ResponseEntity
import org.springframework.web.bind.annotation.*

@RestController
@RequestMapping("/api/users")
class UserController(private val userService: UserService) {

    @GetMapping
    fun getAllUsers(): ResponseEntity<List<User>> = ResponseEntity.ok(userService.getAllUsers())

    @GetMapping("/{id}")
    fun getUserById(@PathVariable id: Long): ResponseEntity<User> = ResponseEntity.ok(userService.getUserById(id))

    @PostMapping
    fun createUser(@Valid @RequestBody user: User): ResponseEntity<User> =
        ResponseEntity.status(HttpStatus.CREATED).body(userService.createUser(user))

    @PutMapping("/{id}")
    fun updateUser(@PathVariable id: Long, @Valid @RequestBody updatedUser: User): ResponseEntity<User> =
        ResponseEntity.ok(userService.updateUser(id, updatedUser))

    @DeleteMapping("/{id}")
    fun deleteUser(@PathVariable id: Long): ResponseEntity<Void> {
        userService.deleteUser(id)
        return ResponseEntity.noContent().build()
    }
}
```

---

### **6. Global Exception Handler (`GlobalExceptionHandler.kt`)**
```kotlin
package com.example.exception

import org.springframework.http.HttpStatus
import org.springframework.http.ResponseEntity
import org.springframework.web.bind.MethodArgumentNotValidException
import org.springframework.web.bind.annotation.ControllerAdvice
import org.springframework.web.bind.annotation.ExceptionHandler
import org.springframework.web.bind.annotation.ResponseStatus

@ControllerAdvice
class GlobalExceptionHandler {

    @ExceptionHandler(UserNotFoundException::class)
    fun handleUserNotFound(ex: UserNotFoundException): ResponseEntity<String> {
        return ResponseEntity.status(HttpStatus.NOT_FOUND).body(ex.message)
    }

    @ExceptionHandler(MethodArgumentNotValidException::class)
    @ResponseStatus(HttpStatus.BAD_REQUEST)
    fun handleValidationException(ex: MethodArgumentNotValidException): ResponseEntity<Map<String, String?>> {
        val errors = ex.bindingResult.fieldErrors.associate { it.field to it.defaultMessage }
        return ResponseEntity.badRequest().body(errors)
    }
}
```

---

### **7. Custom Exception (`UserNotFoundException.kt`)**
```kotlin
package com.example.exception

class UserNotFoundException(message: String) : RuntimeException(message)
```

---

### **8. Configure `application.yml` for H2 Database**
```yaml
spring:
  datasource:
    url: jdbc:h2:mem:testdb
    driver-class-name: org.h2.Driver
    username: sa
    password: 
  jpa:
    database-platform: org.hibernate.dialect.H2Dialect
    hibernate:
      ddl-auto: update
  h2:
    console:
      enabled: true
```

---

### **Testing the API**
#### **1. Create a User (POST)**
```json
POST /api/users
{
  "name": "John Doe",
  "email": "john.doe@example.com"
}
```
_Response:_
```json
{
  "id": 1,
  "name": "John Doe",
  "email": "john.doe@example.com"
}
```

#### **2. Get All Users (GET)**
```json
GET /api/users
```
_Response:_
```json
[
  {
    "id": 1,
    "name": "John Doe",
    "email": "john.doe@example.com"
  }
]
```

#### **3. Get User by ID (GET)**
```json
GET /api/users/1
```

#### **4. Update User (PUT)**
```json
PUT /api/users/1
{
  "name": "Jane Doe",
  "email": "jane.doe@example.com"
}
```

#### **5. Delete User (DELETE)**
```json
DELETE /api/users/1
```

---

### **Conclusion**
✅ **Fully functional CRUD API in Kotlin using Spring Boot**  
✅ **Global Exception Handling**  
✅ **Validation Handling (`@Valid`)**  
✅ **Uses H2 in-memory database (can be replaced with MySQL, PostgreSQL, etc.)**  
🚀 **Ready for Production with minor modifications!**
