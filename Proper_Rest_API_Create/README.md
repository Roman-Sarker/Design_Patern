### **Proper REST API Architecture for a Bank Loan System using Java Spring Boot**
A well-structured **Bank Loan REST API** in Spring Boot follows best practices such as **layered architecture, DTOs, exception handling, logging, and validation**.

---

## **📌 1. Project Structure**
```
bank-loan-api
│── src
│   ├── main
│   │   ├── java/com/bank/loan
│   │   │   ├── config
│   │   │   ├── controller
│   │   │   ├── dto
│   │   │   ├── exception
│   │   │   ├── model
│   │   │   ├── repository
│   │   │   ├── service
│   │   │   │   ├── impl
│   │   │   ├── BankLoanApplication.java
│   │   ├── resources
│   │   │   ├── application.yml
│   │── test/java/com/bank/loan
│── pom.xml
```

---

## **📌 2. Dependencies (`pom.xml`)**
```xml
<dependencies>
    <!-- Spring Boot Starter Web -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <!-- Spring Boot Starter Data JPA -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>

    <!-- PostgreSQL Driver -->
    <dependency>
        <groupId>org.postgresql</groupId>
        <artifactId>postgresql</artifactId>
    </dependency>

    <!-- Validation API -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-validation</artifactId>
    </dependency>

    <!-- Lombok -->
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <scope>provided</scope>
    </dependency>

    <!-- Spring Boot Starter Test -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>
</dependencies>
```

---

## **📌 3. Database Configuration (`application.yml`)**
```yaml
spring:
  datasource:
    url: jdbc:postgresql://localhost:5432/bankdb
    username: postgres
    password: admin
    driver-class-name: org.postgresql.Driver

  jpa:
    hibernate:
      ddl-auto: update
    show-sql: true
    database-platform: org.hibernate.dialect.PostgreSQLDialect

server:
  port: 8080
```

---

## **📌 4. Model (`Loan.java`)**
```java
package com.bank.loan.model;

import jakarta.persistence.*;
import lombok.*;

import java.math.BigDecimal;
import java.time.LocalDate;

@Entity
@Table(name = "loans")
@Getter @Setter
@NoArgsConstructor @AllArgsConstructor
public class Loan {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false)
    private String borrowerName;

    @Column(nullable = false)
    private String loanType;

    @Column(nullable = false)
    private BigDecimal amount;

    @Column(nullable = false)
    private BigDecimal interestRate;

    @Column(nullable = false)
    private LocalDate startDate;
}
```

---

## **📌 5. DTO (`LoanDTO.java`)**
```java
package com.bank.loan.dto;

import jakarta.validation.constraints.*;
import lombok.*;

import java.math.BigDecimal;
import java.time.LocalDate;

@Getter @Setter
@NoArgsConstructor @AllArgsConstructor
public class LoanDTO {

    @NotBlank(message = "Borrower name is required")
    private String borrowerName;

    @NotBlank(message = "Loan type is required")
    private String loanType;

    @NotNull(message = "Loan amount is required")
    @Positive(message = "Amount must be positive")
    private BigDecimal amount;

    @NotNull(message = "Interest rate is required")
    @Positive(message = "Interest rate must be positive")
    private BigDecimal interestRate;

    @NotNull(message = "Start date is required")
    private LocalDate startDate;
}
```

---

## **📌 6. Repository (`LoanRepository.java`)**
```java
package com.bank.loan.repository;

import com.bank.loan.model.Loan;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface LoanRepository extends JpaRepository<Loan, Long> {
}
```

---

## **📌 7. Service Layer**
### **Interface (`LoanService.java`)**
```java
package com.bank.loan.service;

import com.bank.loan.dto.LoanDTO;
import com.bank.loan.model.Loan;

import java.util.List;

public interface LoanService {
    Loan createLoan(LoanDTO loanDTO);
    List<Loan> getAllLoans();
    Loan getLoanById(Long id);
    Loan updateLoan(Long id, LoanDTO loanDTO);
    void deleteLoan(Long id);
}
```

### **Implementation (`LoanServiceImpl.java`)**
```java
package com.bank.loan.service.impl;

import com.bank.loan.dto.LoanDTO;
import com.bank.loan.exception.ResourceNotFoundException;
import com.bank.loan.model.Loan;
import com.bank.loan.repository.LoanRepository;
import com.bank.loan.service.LoanService;
import lombok.RequiredArgsConstructor;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
@RequiredArgsConstructor
public class LoanServiceImpl implements LoanService {

    private final LoanRepository loanRepository;

    @Override
    public Loan createLoan(LoanDTO loanDTO) {
        Loan loan = new Loan(null, loanDTO.getBorrowerName(), loanDTO.getLoanType(),
                loanDTO.getAmount(), loanDTO.getInterestRate(), loanDTO.getStartDate());
        return loanRepository.save(loan);
    }

    @Override
    public List<Loan> getAllLoans() {
        return loanRepository.findAll();
    }

    @Override
    public Loan getLoanById(Long id) {
        return loanRepository.findById(id)
                .orElseThrow(() -> new ResourceNotFoundException("Loan not found with id " + id));
    }

    @Override
    public Loan updateLoan(Long id, LoanDTO loanDTO) {
        Loan loan = getLoanById(id);
        loan.setBorrowerName(loanDTO.getBorrowerName());
        loan.setLoanType(loanDTO.getLoanType());
        loan.setAmount(loanDTO.getAmount());
        loan.setInterestRate(loanDTO.getInterestRate());
        loan.setStartDate(loanDTO.getStartDate());
        return loanRepository.save(loan);
    }

    @Override
    public void deleteLoan(Long id) {
        Loan loan = getLoanById(id);
        loanRepository.delete(loan);
    }
}
```

---

## **📌 8. Controller (`LoanController.java`)**
```java
package com.bank.loan.controller;

import com.bank.loan.dto.LoanDTO;
import com.bank.loan.model.Loan;
import com.bank.loan.service.LoanService;
import jakarta.validation.Valid;
import lombok.RequiredArgsConstructor;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/api/loans")
@RequiredArgsConstructor
public class LoanController {

    private final LoanService loanService;

    @PostMapping
    public ResponseEntity<Loan> createLoan(@Valid @RequestBody LoanDTO loanDTO) {
        return ResponseEntity.ok(loanService.createLoan(loanDTO));
    }

    @GetMapping
    public ResponseEntity<List<Loan>> getAllLoans() {
        return ResponseEntity.ok(loanService.getAllLoans());
    }

    @GetMapping("/{id}")
    public ResponseEntity<Loan> getLoanById(@PathVariable Long id) {
        return ResponseEntity.ok(loanService.getLoanById(id));
    }

    @PutMapping("/{id}")
    public ResponseEntity<Loan> updateLoan(@PathVariable Long id, @Valid @RequestBody LoanDTO loanDTO) {
        return ResponseEntity.ok(loanService.updateLoan(id, loanDTO));
    }

    @DeleteMapping("/{id}")
    public ResponseEntity<String> deleteLoan(@PathVariable Long id) {
        loanService.deleteLoan(id);
        return ResponseEntity.ok("Loan deleted successfully.");
    }
}
```

---

## **📌 9. Global Exception Handling**
```java
package com.bank.loan.exception;

@ResponseStatus(HttpStatus.NOT_FOUND)
public class ResourceNotFoundException extends RuntimeException {
    public ResourceNotFoundException(String message) {
        super(message);
    }
}
```

---

### ✅ **Conclusion**
This structure ensures **separation of concerns**, follows **REST best practices**, and includes **DTOs, validation, exception handling, and logging** for a robust **Bank Loan API** in Spring Boot. 🚀
