# Complete Spring Boot with Kotlin Tutorial for JavaScript Developers

## Table of Contents

1. [Introduction and Prerequisites](#introduction)
2. [Part 1: Kotlin Fundamentals for JavaScript Developers](#part-1)
3. [Part 2: Setting Up Your First Spring Boot Project](#part-2)
4. [Part 3: Building Your First REST API](#part-3)
5. [Part 4: Data Persistence with Spring Data JPA](#part-4)
6. [Part 5: Advanced REST API Features](#part-5)
7. [Part 6: Security with Spring Security](#part-6)
8. [Part 7: Testing Your Application](#part-7)
9. [Part 8: Reactive Programming with Coroutines](#part-8)
10. [Part 9: Production Features with Actuator](#part-9)
11. [Part 10: Deployment and Best Practices](#part-10)

---

## Introduction and Prerequisites {#introduction}

This tutorial is designed specifically for JavaScript developers transitioning to Spring Boot with Kotlin. We'll build a complete project management system API, introducing concepts progressively.

### Prerequisites

- Java 17 or 21 installed (recommended: Java 21)
- Your favorite IDE (IntelliJ IDEA, VS Code with extensions, or LazyVim)
- Basic understanding of REST APIs
- Familiarity with JavaScript/TypeScript

### What We'll Build

A project management API with:

- User authentication
- Project CRUD operations
- Task management
- Real-time updates
- Production monitoring

---

## Part 1: Kotlin Fundamentals for JavaScript Developers {#part-1}

### 1.1 Kotlin vs JavaScript: Key Similarities

Let's start by comparing familiar JavaScript patterns with Kotlin equivalents:

```kotlin
// JavaScript
const user = {
  name: "John",
  email: "john@example.com",
  age: 30
}

// Kotlin equivalent - Data class
data class User(
    val name: String,
    val email: String,
    val age: Int
)

val user = User("John", "john@example.com", 30)
```

### 1.2 Variables and Types

```kotlin
// JavaScript
let mutableVar = "can change"
const immutableVar = "cannot change"

// Kotlin
var mutableVar = "can change"
val immutableVar = "cannot change"

// Type inference works like TypeScript
val inferredString = "Hello"  // Type: String
val inferredInt = 42          // Type: Int
val inferredDouble = 3.14     // Type: Double

// Explicit types
val explicitString: String = "Hello"
val explicitInt: Int = 42
```

### 1.3 Null Safety (Kotlin's Killer Feature)

```kotlin
// JavaScript - null/undefined issues
function greet(name) {
  return `Hello, ${name.toUpperCase()}`  // Crashes if name is null
}

// Kotlin - Null safety built-in
fun greet(name: String?): String {
    return "Hello, ${name?.uppercase() ?: "Guest"}"
}

// The ? makes types nullable
val nullable: String? = null      // OK
val nonNullable: String = null    // Compilation error!
```

### 1.4 Functions

```kotlin
// JavaScript
function add(a, b) {
  return a + b
}

const multiply = (a, b) => a * b

// Kotlin
fun add(a: Int, b: Int): Int {
    return a + b
}

// Single expression function
fun multiply(a: Int, b: Int) = a * b

// Default parameters
fun greet(name: String = "World") = "Hello, $name!"

// Named arguments (like JavaScript object parameters)
fun createUser(name: String, email: String, age: Int = 0) =
    User(name, email, age)

val user = createUser(
    name = "John",
    email = "john@example.com",
    age = 30
)
```

### 1.5 Collections and Higher-Order Functions

```kotlin
// JavaScript
const numbers = [1, 2, 3, 4, 5]
const doubled = numbers.map(n => n * 2)
const evens = numbers.filter(n => n % 2 === 0)
const sum = numbers.reduce((acc, n) => acc + n, 0)

// Kotlin - Almost identical!
val numbers = listOf(1, 2, 3, 4, 5)
val doubled = numbers.map { it * 2 }
val evens = numbers.filter { it % 2 == 0 }
val sum = numbers.reduce { acc, n -> acc + n }

// Kotlin's 'it' is the implicit parameter in lambdas
// Similar to JS arrow functions with single parameter
```

### 1.6 Classes and Objects

```kotlin
// JavaScript ES6 class
class Animal {
  constructor(name) {
    this.name = name
  }

  speak() {
    console.log(`${this.name} makes a sound`)
  }
}

// Kotlin class
class Animal(val name: String) {
    fun speak() {
        println("$name makes a sound")
    }
}

// Inheritance
open class Dog(name: String) : Animal(name) {
    override fun speak() {
        println("$name barks!")
    }
}
```

### 1.7 Extension Functions (No JS equivalent!)

```kotlin
// Add methods to existing classes
fun String.isPalindrome(): Boolean {
    return this == this.reversed()
}

"racecar".isPalindrome() // true

// Extension functions on collections
fun List<Int>.secondOrNull(): Int? {
    return if (size >= 2) this[1] else null
}
```

---

## Part 2: Setting Up Your First Spring Boot Project {#part-2}

### 2.1 Creating the Project

Visit [Spring Initializr](https://start.spring.io) and configure:

- **Project**: Gradle - Kotlin
- **Language**: Kotlin
- **Spring Boot**: 3.5.3
- **Project Metadata**:
  - Group: `com.example`
  - Artifact: `projectmanager`
  - Name: `projectmanager`
  - Package name: `com.example.projectmanager`
  - Packaging: Jar
  - Java: 21

**Dependencies**:

- Spring Web
- Spring Data JPA
- Spring Security
- Spring Boot DevTools
- H2 Database (for development)
- PostgreSQL Driver (for production)
- Spring Boot Actuator
- Validation

Click **Generate** and extract the ZIP file.

### 2.2 Project Structure

```
projectmanager/
├── build.gradle.kts
├── settings.gradle.kts
├── src/
│   ├── main/
│   │   ├── kotlin/
│   │   │   └── com/example/projectmanager/
│   │   │       └── ProjectmanagerApplication.kt
│   │   └── resources/
│   │       ├── application.properties
│   │       ├── static/
│   │       └── templates/
│   └── test/
```

### 2.3 Understanding build.gradle.kts

```kotlin
import org.jetbrains.kotlin.gradle.tasks.KotlinCompile

plugins {
    id("org.springframework.boot") version "3.5.3"
    id("io.spring.dependency-management") version "1.1.0"
    kotlin("jvm") version "2.2.0"
    kotlin("plugin.spring") version "2.2.0"
    kotlin("plugin.jpa") version "2.2.0"
}

group = "com.example"
version = "0.0.1-SNAPSHOT"
java.sourceCompatibility = JavaVersion.VERSION_21

repositories {
    mavenCentral()
}

dependencies {
    implementation("org.springframework.boot:spring-boot-starter-data-jpa")
    implementation("org.springframework.boot:spring-boot-starter-security")
    implementation("org.springframework.boot:spring-boot-starter-web")
    implementation("org.springframework.boot:spring-boot-starter-validation")
    implementation("org.springframework.boot:spring-boot-starter-actuator")
    implementation("com.fasterxml.jackson.module:jackson-module-kotlin")
    implementation("org.jetbrains.kotlin:kotlin-reflect")
    developmentOnly("org.springframework.boot:spring-boot-devtools")
    runtimeOnly("com.h2database:h2")
    runtimeOnly("org.postgresql:postgresql")
    testImplementation("org.springframework.boot:spring-boot-starter-test")
    testImplementation("org.springframework.security:spring-security-test")
}

tasks.withType<KotlinCompile> {
    kotlinOptions {
        freeCompilerArgs = listOf("-Xjsr305=strict")
        jvmTarget = "21"
    }
}

tasks.withType<Test> {
    useJUnitPlatform()
}
```

### 2.4 Application Configuration

Update `src/main/resources/application.properties`:

```properties
# Server Configuration
server.port=8080

# Database Configuration
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=

# JPA Configuration
spring.jpa.database-platform=org.hibernate.dialect.H2Dialect
spring.jpa.hibernate.ddl-auto=create-drop
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.format_sql=true

# H2 Console (for development)
spring.h2.console.enabled=true
spring.h2.console.path=/h2-console

# Actuator Configuration
management.endpoints.web.exposure.include=health,info,metrics
management.endpoint.health.show-details=always

# Security Configuration (temporary - we'll secure later)
spring.security.user.name=admin
spring.security.user.password=admin
```

### 2.5 Your First Run

Run the application:

```bash
./gradlew bootRun
```

Visit:

- http://localhost:8080 (will show error page - normal!)
- http://localhost:8080/h2-console (database console)
- http://localhost:8080/actuator/health (health check)

---

## Part 3: Building Your First REST API {#part-3}

### 3.1 Creating the Domain Model

Create `src/main/kotlin/com/example/projectmanager/model/Project.kt`:

```kotlin
package com.example.projectmanager.model

import jakarta.persistence.*
import java.time.LocalDateTime

@Entity
@Table(name = "projects")
data class Project(
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    val id: Long = 0,

    @Column(nullable = false)
    var name: String,

    @Column(length = 1000)
    var description: String? = null,

    @Column(nullable = false)
    var status: ProjectStatus = ProjectStatus.PLANNING,

    @Column(nullable = false)
    val createdAt: LocalDateTime = LocalDateTime.now(),

    var updatedAt: LocalDateTime = LocalDateTime.now()
)

enum class ProjectStatus {
    PLANNING,
    IN_PROGRESS,
    COMPLETED,
    ON_HOLD,
    CANCELLED
}
```

### 3.2 Creating the Repository

Create `src/main/kotlin/com/example/projectmanager/repository/ProjectRepository.kt`:

```kotlin
package com.example.projectmanager.repository

import com.example.projectmanager.model.Project
import com.example.projectmanager.model.ProjectStatus
import org.springframework.data.jpa.repository.JpaRepository
import org.springframework.stereotype.Repository

@Repository
interface ProjectRepository : JpaRepository<Project, Long> {
    fun findByStatus(status: ProjectStatus): List<Project>
    fun findByNameContainingIgnoreCase(name: String): List<Project>
}
```

### 3.3 Creating DTOs (Data Transfer Objects)

Create `src/main/kotlin/com/example/projectmanager/dto/ProjectDtos.kt`:

```kotlin
package com.example.projectmanager.dto

import com.example.projectmanager.model.ProjectStatus
import jakarta.validation.constraints.NotBlank
import jakarta.validation.constraints.Size

data class CreateProjectRequest(
    @field:NotBlank(message = "Project name is required")
    @field:Size(min = 3, max = 100, message = "Project name must be between 3 and 100 characters")
    val name: String,

    @field:Size(max = 1000, message = "Description cannot exceed 1000 characters")
    val description: String? = null
)

data class UpdateProjectRequest(
    val name: String?,
    val description: String?,
    val status: ProjectStatus?
)

data class ProjectResponse(
    val id: Long,
    val name: String,
    val description: String?,
    val status: ProjectStatus,
    val createdAt: String,
    val updatedAt: String
)
```

### 3.4 Creating the Service Layer

Create `src/main/kotlin/com/example/projectmanager/service/ProjectService.kt`:

```kotlin
package com.example.projectmanager.service

import com.example.projectmanager.dto.CreateProjectRequest
import com.example.projectmanager.dto.ProjectResponse
import com.example.projectmanager.dto.UpdateProjectRequest
import com.example.projectmanager.model.Project
import com.example.projectmanager.model.ProjectStatus
import com.example.projectmanager.repository.ProjectRepository
import org.springframework.data.repository.findByIdOrNull
import org.springframework.stereotype.Service
import org.springframework.transaction.annotation.Transactional
import java.time.LocalDateTime

@Service
@Transactional
class ProjectService(
    private val projectRepository: ProjectRepository
) {

    fun getAllProjects(): List<ProjectResponse> {
        return projectRepository.findAll().map { it.toResponse() }
    }

    fun getProjectById(id: Long): ProjectResponse {
        val project = projectRepository.findByIdOrNull(id)
            ?: throw NoSuchElementException("Project not found with id: $id")
        return project.toResponse()
    }

    fun createProject(request: CreateProjectRequest): ProjectResponse {
        val project = Project(
            name = request.name,
            description = request.description
        )
        val savedProject = projectRepository.save(project)
        return savedProject.toResponse()
    }

    fun updateProject(id: Long, request: UpdateProjectRequest): ProjectResponse {
        val project = projectRepository.findByIdOrNull(id)
            ?: throw NoSuchElementException("Project not found with id: $id")

        request.name?.let { project.name = it }
        request.description?.let { project.description = it }
        request.status?.let { project.status = it }
        project.updatedAt = LocalDateTime.now()

        val updatedProject = projectRepository.save(project)
        return updatedProject.toResponse()
    }

    fun deleteProject(id: Long) {
        if (!projectRepository.existsById(id)) {
            throw NoSuchElementException("Project not found with id: $id")
        }
        projectRepository.deleteById(id)
    }

    fun getProjectsByStatus(status: ProjectStatus): List<ProjectResponse> {
        return projectRepository.findByStatus(status).map { it.toResponse() }
    }

    fun searchProjects(name: String): List<ProjectResponse> {
        return projectRepository.findByNameContainingIgnoreCase(name).map { it.toResponse() }
    }

    private fun Project.toResponse(): ProjectResponse {
        return ProjectResponse(
            id = this.id,
            name = this.name,
            description = this.description,
            status = this.status,
            createdAt = this.createdAt.toString(),
            updatedAt = this.updatedAt.toString()
        )
    }
}
```

### 3.5 Creating the REST Controller

Create `src/main/kotlin/com/example/projectmanager/controller/ProjectController.kt`:

```kotlin
package com.example.projectmanager.controller

import com.example.projectmanager.dto.CreateProjectRequest
import com.example.projectmanager.dto.ProjectResponse
import com.example.projectmanager.dto.UpdateProjectRequest
import com.example.projectmanager.model.ProjectStatus
import com.example.projectmanager.service.ProjectService
import jakarta.validation.Valid
import org.springframework.http.HttpStatus
import org.springframework.http.ResponseEntity
import org.springframework.web.bind.annotation.*

@RestController
@RequestMapping("/api/projects")
class ProjectController(
    private val projectService: ProjectService
) {

    @GetMapping
    fun getAllProjects(): List<ProjectResponse> {
        return projectService.getAllProjects()
    }

    @GetMapping("/{id}")
    fun getProject(@PathVariable id: Long): ResponseEntity<ProjectResponse> {
        return try {
            ResponseEntity.ok(projectService.getProjectById(id))
        } catch (e: NoSuchElementException) {
            ResponseEntity.notFound().build()
        }
    }

    @PostMapping
    @ResponseStatus(HttpStatus.CREATED)
    fun createProject(@Valid @RequestBody request: CreateProjectRequest): ProjectResponse {
        return projectService.createProject(request)
    }

    @PutMapping("/{id}")
    fun updateProject(
        @PathVariable id: Long,
        @RequestBody request: UpdateProjectRequest
    ): ResponseEntity<ProjectResponse> {
        return try {
            ResponseEntity.ok(projectService.updateProject(id, request))
        } catch (e: NoSuchElementException) {
            ResponseEntity.notFound().build()
        }
    }

    @DeleteMapping("/{id}")
    @ResponseStatus(HttpStatus.NO_CONTENT)
    fun deleteProject(@PathVariable id: Long) {
        projectService.deleteProject(id)
    }

    @GetMapping("/status/{status}")
    fun getProjectsByStatus(@PathVariable status: ProjectStatus): List<ProjectResponse> {
        return projectService.getProjectsByStatus(status)
    }

    @GetMapping("/search")
    fun searchProjects(@RequestParam name: String): List<ProjectResponse> {
        return projectService.searchProjects(name)
    }
}
```

### 3.6 Global Exception Handling

Create `src/main/kotlin/com/example/projectmanager/exception/GlobalExceptionHandler.kt`:

```kotlin
package com.example.projectmanager.exception

import org.springframework.http.HttpStatus
import org.springframework.http.ResponseEntity
import org.springframework.validation.FieldError
import org.springframework.web.bind.MethodArgumentNotValidException
import org.springframework.web.bind.annotation.ExceptionHandler
import org.springframework.web.bind.annotation.RestControllerAdvice
import java.time.LocalDateTime

@RestControllerAdvice
class GlobalExceptionHandler {

    @ExceptionHandler(NoSuchElementException::class)
    fun handleNotFound(ex: NoSuchElementException): ResponseEntity<ErrorResponse> {
        val error = ErrorResponse(
            timestamp = LocalDateTime.now(),
            status = HttpStatus.NOT_FOUND.value(),
            error = "Not Found",
            message = ex.message ?: "Resource not found",
            path = null
        )
        return ResponseEntity.status(HttpStatus.NOT_FOUND).body(error)
    }

    @ExceptionHandler(MethodArgumentNotValidException::class)
    fun handleValidationExceptions(ex: MethodArgumentNotValidException): ResponseEntity<ErrorResponse> {
        val errors = ex.bindingResult.allErrors.map { error ->
            val fieldName = (error as? FieldError)?.field ?: "unknown"
            val errorMessage = error.defaultMessage ?: "Validation failed"
            "$fieldName: $errorMessage"
        }

        val error = ErrorResponse(
            timestamp = LocalDateTime.now(),
            status = HttpStatus.BAD_REQUEST.value(),
            error = "Validation Failed",
            message = errors.joinToString(", "),
            path = null
        )
        return ResponseEntity.status(HttpStatus.BAD_REQUEST).body(error)
    }

    @ExceptionHandler(Exception::class)
    fun handleGenericException(ex: Exception): ResponseEntity<ErrorResponse> {
        val error = ErrorResponse(
            timestamp = LocalDateTime.now(),
            status = HttpStatus.INTERNAL_SERVER_ERROR.value(),
            error = "Internal Server Error",
            message = ex.message ?: "An unexpected error occurred",
            path = null
        )
        return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).body(error)
    }
}

data class ErrorResponse(
    val timestamp: LocalDateTime,
    val status: Int,
    val error: String,
    val message: String,
    val path: String?
)
```

### 3.7 Testing Your API

Temporarily disable security by creating `src/main/kotlin/com/example/projectmanager/config/SecurityConfig.kt`:

```kotlin
package com.example.projectmanager.config

import org.springframework.context.annotation.Bean
import org.springframework.context.annotation.Configuration
import org.springframework.security.config.annotation.web.builders.HttpSecurity
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity
import org.springframework.security.web.SecurityFilterChain

@Configuration
@EnableWebSecurity
class SecurityConfig {

    @Bean
    fun filterChain(http: HttpSecurity): SecurityFilterChain {
        http
            .csrf { it.disable() }
            .authorizeHttpRequests { auth ->
                auth.anyRequest().permitAll()
            }

        return http.build()
    }
}
```

Now test your API:

```bash
# Create a project
curl -X POST http://localhost:8080/api/projects \
  -H "Content-Type: application/json" \
  -d '{
    "name": "My First Project",
    "description": "This is a test project"
  }'

# Get all projects
curl http://localhost:8080/api/projects

# Get specific project
curl http://localhost:8080/api/projects/1

# Update project
curl -X PUT http://localhost:8080/api/projects/1 \
  -H "Content-Type: application/json" \
  -d '{
    "status": "IN_PROGRESS"
  }'

# Search projects
curl http://localhost:8080/api/projects/search?name=First

# Delete project
curl -X DELETE http://localhost:8080/api/projects/1
```

---

## Part 4: Data Persistence with Spring Data JPA {#part-4}

### 4.1 Adding More Entities

Create `src/main/kotlin/com/example/projectmanager/model/User.kt`:

```kotlin
package com.example.projectmanager.model

import jakarta.persistence.*
import java.time.LocalDateTime

@Entity
@Table(name = "users")
data class User(
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    val id: Long = 0,

    @Column(unique = true, nullable = false)
    var username: String,

    @Column(unique = true, nullable = false)
    var email: String,

    @Column(nullable = false)
    var password: String,

    @Column(nullable = false)
    var firstName: String,

    @Column(nullable = false)
    var lastName: String,

    @Column(nullable = false)
    var role: UserRole = UserRole.USER,

    @Column(nullable = false)
    var enabled: Boolean = true,

    @OneToMany(mappedBy = "owner", cascade = [CascadeType.ALL])
    val ownedProjects: MutableList<Project> = mutableListOf(),

    @ManyToMany(mappedBy = "members")
    val memberProjects: MutableSet<Project> = mutableSetOf(),

    @Column(nullable = false)
    val createdAt: LocalDateTime = LocalDateTime.now(),

    var lastLoginAt: LocalDateTime? = null
)

enum class UserRole {
    USER,
    ADMIN,
    PROJECT_MANAGER
}
```

Update `Project.kt` to include relationships:

```kotlin
package com.example.projectmanager.model

import jakarta.persistence.*
import java.time.LocalDateTime

@Entity
@Table(name = "projects")
data class Project(
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    val id: Long = 0,

    @Column(nullable = false)
    var name: String,

    @Column(length = 1000)
    var description: String? = null,

    @Column(nullable = false)
    var status: ProjectStatus = ProjectStatus.PLANNING,

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "owner_id", nullable = false)
    var owner: User,

    @ManyToMany
    @JoinTable(
        name = "project_members",
        joinColumns = [JoinColumn(name = "project_id")],
        inverseJoinColumns = [JoinColumn(name = "user_id")]
    )
    val members: MutableSet<User> = mutableSetOf(),

    @OneToMany(mappedBy = "project", cascade = [CascadeType.ALL], orphanRemoval = true)
    val tasks: MutableList<Task> = mutableListOf(),

    @Column(nullable = false)
    val createdAt: LocalDateTime = LocalDateTime.now(),

    var updatedAt: LocalDateTime = LocalDateTime.now()
) {
    fun addMember(user: User) {
        members.add(user)
        user.memberProjects.add(this)
    }

    fun removeMember(user: User) {
        members.remove(user)
        user.memberProjects.remove(this)
    }
}
```

Create `src/main/kotlin/com/example/projectmanager/model/Task.kt`:

```kotlin
package com.example.projectmanager.model

import jakarta.persistence.*
import java.time.LocalDateTime

@Entity
@Table(name = "tasks")
data class Task(
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    val id: Long = 0,

    @Column(nullable = false)
    var title: String,

    @Column(length = 2000)
    var description: String? = null,

    @Column(nullable = false)
    var status: TaskStatus = TaskStatus.TODO,

    @Column(nullable = false)
    var priority: TaskPriority = TaskPriority.MEDIUM,

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "project_id", nullable = false)
    var project: Project,

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "assignee_id")
    var assignee: User? = null,

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "created_by_id", nullable = false)
    val createdBy: User,

    var dueDate: LocalDateTime? = null,

    @Column(nullable = false)
    val createdAt: LocalDateTime = LocalDateTime.now(),

    var completedAt: LocalDateTime? = null
)

enum class TaskStatus {
    TODO,
    IN_PROGRESS,
    IN_REVIEW,
    DONE,
    CANCELLED
}

enum class TaskPriority {
    LOW,
    MEDIUM,
    HIGH,
    CRITICAL
}
```

### 4.2 Creating Repositories

Create `src/main/kotlin/com/example/projectmanager/repository/UserRepository.kt`:

```kotlin
package com.example.projectmanager.repository

import com.example.projectmanager.model.User
import org.springframework.data.jpa.repository.JpaRepository
import org.springframework.stereotype.Repository
import java.util.Optional

@Repository
interface UserRepository : JpaRepository<User, Long> {
    fun findByUsername(username: String): Optional<User>
    fun findByEmail(email: String): Optional<User>
    fun existsByUsername(username: String): Boolean
    fun existsByEmail(email: String): Boolean
}
```

Create `src/main/kotlin/com/example/projectmanager/repository/TaskRepository.kt`:

```kotlin
package com.example.projectmanager.repository

import com.example.projectmanager.model.Task
import com.example.projectmanager.model.TaskStatus
import com.example.projectmanager.model.User
import com.example.projectmanager.model.Project
import org.springframework.data.domain.Page
import org.springframework.data.domain.Pageable
import org.springframework.data.jpa.repository.JpaRepository
import org.springframework.data.jpa.repository.Query
import org.springframework.stereotype.Repository
import java.time.LocalDateTime

@Repository
interface TaskRepository : JpaRepository<Task, Long> {
    fun findByProject(project: Project, pageable: Pageable): Page<Task>
    fun findByAssignee(assignee: User): List<Task>
    fun findByProjectAndStatus(project: Project, status: TaskStatus): List<Task>
    fun findByDueDateBetween(start: LocalDateTime, end: LocalDateTime): List<Task>

    @Query("SELECT t FROM Task t WHERE t.project.id = :projectId AND t.status != 'DONE' AND t.status != 'CANCELLED'")
    fun findActiveTasksByProjectId(projectId: Long): List<Task>

    fun countByProjectAndStatus(project: Project, status: TaskStatus): Long
}
```

### 4.3 Database Configuration for Production

Create `src/main/resources/application-dev.properties`:

```properties
# Development Profile - H2 Database
spring.datasource.url=jdbc:h2:mem:devdb
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=

spring.jpa.database-platform=org.hibernate.dialect.H2Dialect
spring.jpa.hibernate.ddl-auto=create-drop
spring.jpa.show-sql=true
spring.h2.console.enabled=true
```

Create `src/main/resources/application-prod.properties`:

```properties
# Production Profile - PostgreSQL
spring.datasource.url=${DATABASE_URL:jdbc:postgresql://localhost:5432/projectmanager}
spring.datasource.username=${DATABASE_USERNAME:postgres}
spring.datasource.password=${DATABASE_PASSWORD:password}

spring.jpa.database-platform=org.hibernate.dialect.PostgreSQLDialect
spring.jpa.hibernate.ddl-auto=validate
spring.jpa.show-sql=false
spring.h2.console.enabled=false

# Connection Pool Settings
spring.datasource.hikari.maximum-pool-size=10
spring.datasource.hikari.minimum-idle=5
spring.datasource.hikari.connection-timeout=30000
```

### 4.4 Database Initialization

Create `src/main/kotlin/com/example/projectmanager/config/DataInitializer.kt`:

```kotlin
package com.example.projectmanager.config

import com.example.projectmanager.model.*
import com.example.projectmanager.repository.ProjectRepository
import com.example.projectmanager.repository.TaskRepository
import com.example.projectmanager.repository.UserRepository
import org.springframework.boot.CommandLineRunner
import org.springframework.context.annotation.Bean
import org.springframework.context.annotation.Configuration
import org.springframework.context.annotation.Profile

@Configuration
@Profile("dev")
class DataInitializer {

    @Bean
    fun init(
        userRepository: UserRepository,
        projectRepository: ProjectRepository,
        taskRepository: TaskRepository
    ) = CommandLineRunner {
        // Create sample users
        val john = userRepository.save(User(
            username = "john_doe",
            email = "john@example.com",
            password = "password123", // Will be encrypted later
            firstName = "John",
            lastName = "Doe",
            role = UserRole.PROJECT_MANAGER
        ))

        val jane = userRepository.save(User(
            username = "jane_smith",
            email = "jane@example.com",
            password = "password123",
            firstName = "Jane",
            lastName = "Smith",
            role = UserRole.USER
        ))

        // Create sample project
        val project = Project(
            name = "Spring Boot Tutorial",
            description = "Building a comprehensive Spring Boot application",
            status = ProjectStatus.IN_PROGRESS,
            owner = john
        )
        project.addMember(jane)
        val savedProject = projectRepository.save(project)

        // Create sample tasks
        val tasks = listOf(
            Task(
                title = "Setup project structure",
                description = "Initialize Spring Boot project with required dependencies",
                status = TaskStatus.DONE,
                priority = TaskPriority.HIGH,
                project = savedProject,
                assignee = john,
                createdBy = john
            ),
            Task(
                title = "Implement REST APIs",
                description = "Create CRUD endpoints for projects",
                status = TaskStatus.IN_PROGRESS,
                priority = TaskPriority.HIGH,
                project = savedProject,
                assignee = jane,
                createdBy = john
            ),
            Task(
                title = "Add authentication",
                description = "Implement JWT-based authentication",
                status = TaskStatus.TODO,
                priority = TaskPriority.CRITICAL,
                project = savedProject,
                createdBy = john
            )
        )

        taskRepository.saveAll(tasks)

        println("Sample data initialized successfully!")
    }
}
```

---

## Part 5: Advanced REST API Features {#part-5}

### 5.1 Pagination and Sorting

Update `ProjectController.kt` to add pagination:

```kotlin
@GetMapping
fun getAllProjects(
    @RequestParam(defaultValue = "0") page: Int,
    @RequestParam(defaultValue = "10") size: Int,
    @RequestParam(defaultValue = "createdAt") sortBy: String,
    @RequestParam(defaultValue = "DESC") sortDirection: String
): Page<ProjectResponse> {
    val pageable = PageRequest.of(
        page,
        size,
        Sort.Direction.valueOf(sortDirection),
        sortBy
    )
    return projectService.getAllProjects(pageable)
}
```

Update `ProjectService.kt`:

```kotlin
fun getAllProjects(pageable: Pageable): Page<ProjectResponse> {
    return projectRepository.findAll(pageable).map { it.toResponse() }
}
```

### 5.2 Advanced Filtering

Create `src/main/kotlin/com/example/projectmanager/specification/ProjectSpecification.kt`:

```kotlin
package com.example.projectmanager.specification

import com.example.projectmanager.model.Project
import com.example.projectmanager.model.ProjectStatus
import org.springframework.data.jpa.domain.Specification
import java.time.LocalDateTime

object ProjectSpecification {

    fun hasStatus(status: ProjectStatus?): Specification<Project> {
        return Specification { root, _, criteriaBuilder ->
            status?.let {
                criteriaBuilder.equal(root.get<ProjectStatus>("status"), it)
            }
        }
    }

    fun nameContains(name: String?): Specification<Project> {
        return Specification { root, _, criteriaBuilder ->
            name?.let {
                criteriaBuilder.like(
                    criteriaBuilder.lower(root.get("name")),
                    "%${it.lowercase()}%"
                )
            }
        }
    }

    fun createdAfter(date: LocalDateTime?): Specification<Project> {
        return Specification { root, _, criteriaBuilder ->
            date?.let {
                criteriaBuilder.greaterThanOrEqualTo(root.get("createdAt"), it)
            }
        }
    }

    fun belongsToOwner(ownerId: Long?): Specification<Project> {
        return Specification { root, _, criteriaBuilder ->
            ownerId?.let {
                criteriaBuilder.equal(root.get<User>("owner").get<Long>("id"), it)
            }
        }
    }
}
```

### 5.3 Task Management

Create `src/main/kotlin/com/example/projectmanager/controller/TaskController.kt`:

```kotlin
package com.example.projectmanager.controller

import com.example.projectmanager.dto.*
import com.example.projectmanager.service.TaskService
import jakarta.validation.Valid
import org.springframework.data.domain.Page
import org.springframework.data.domain.PageRequest
import org.springframework.data.domain.Sort
import org.springframework.http.HttpStatus
import org.springframework.http.ResponseEntity
import org.springframework.web.bind.annotation.*

@RestController
@RequestMapping("/api/projects/{projectId}/tasks")
class TaskController(
    private val taskService: TaskService
) {

    @GetMapping
    fun getProjectTasks(
        @PathVariable projectId: Long,
        @RequestParam(defaultValue = "0") page: Int,
        @RequestParam(defaultValue = "10") size: Int
    ): Page<TaskResponse> {
        val pageable = PageRequest.of(page, size, Sort.by("createdAt").descending())
        return taskService.getProjectTasks(projectId, pageable)
    }

    @PostMapping
    @ResponseStatus(HttpStatus.CREATED)
    fun createTask(
        @PathVariable projectId: Long,
        @Valid @RequestBody request: CreateTaskRequest
    ): TaskResponse {
        return taskService.createTask(projectId, request)
    }

    @PutMapping("/{taskId}")
    fun updateTask(
        @PathVariable projectId: Long,
        @PathVariable taskId: Long,
        @RequestBody request: UpdateTaskRequest
    ): ResponseEntity<TaskResponse> {
        return try {
            ResponseEntity.ok(taskService.updateTask(taskId, request))
        } catch (e: NoSuchElementException) {
            ResponseEntity.notFound().build()
        }
    }

    @PatchMapping("/{taskId}/status")
    fun updateTaskStatus(
        @PathVariable projectId: Long,
        @PathVariable taskId: Long,
        @RequestBody request: UpdateTaskStatusRequest
    ): ResponseEntity<TaskResponse> {
        return try {
            ResponseEntity.ok(taskService.updateTaskStatus(taskId, request.status))
        } catch (e: NoSuchElementException) {
            ResponseEntity.notFound().build()
        }
    }

    @PatchMapping("/{taskId}/assign")
    fun assignTask(
        @PathVariable projectId: Long,
        @PathVariable taskId: Long,
        @RequestBody request: AssignTaskRequest
    ): ResponseEntity<TaskResponse> {
        return try {
            ResponseEntity.ok(taskService.assignTask(taskId, request.userId))
        } catch (e: NoSuchElementException) {
            ResponseEntity.notFound().build()
        }
    }

    @DeleteMapping("/{taskId}")
    @ResponseStatus(HttpStatus.NO_CONTENT)
    fun deleteTask(
        @PathVariable projectId: Long,
        @PathVariable taskId: Long
    ) {
        taskService.deleteTask(taskId)
    }

    @GetMapping("/my-tasks")
    fun getMyTasks(): List<TaskResponse> {
        // Will implement after authentication
        return emptyList()
    }
}
```

### 5.4 Task DTOs and Service

Create task-related DTOs in `src/main/kotlin/com/example/projectmanager/dto/TaskDtos.kt`:

```kotlin
package com.example.projectmanager.dto

import com.example.projectmanager.model.TaskPriority
import com.example.projectmanager.model.TaskStatus
import jakarta.validation.constraints.NotBlank
import jakarta.validation.constraints.Size
import java.time.LocalDateTime

data class CreateTaskRequest(
    @field:NotBlank(message = "Task title is required")
    @field:Size(min = 3, max = 200, message = "Title must be between 3 and 200 characters")
    val title: String,

    @field:Size(max = 2000, message = "Description cannot exceed 2000 characters")
    val description: String? = null,

    val priority: TaskPriority = TaskPriority.MEDIUM,
    val assigneeId: Long? = null,
    val dueDate: LocalDateTime? = null
)

data class UpdateTaskRequest(
    val title: String?,
    val description: String?,
    val status: TaskStatus?,
    val priority: TaskPriority?,
    val assigneeId: Long?,
    val dueDate: LocalDateTime?
)

data class UpdateTaskStatusRequest(
    val status: TaskStatus
)

data class AssignTaskRequest(
    val userId: Long?
)

data class TaskResponse(
    val id: Long,
    val title: String,
    val description: String?,
    val status: TaskStatus,
    val priority: TaskPriority,
    val projectId: Long,
    val projectName: String,
    val assignee: UserSummary?,
    val createdBy: UserSummary,
    val dueDate: String?,
    val createdAt: String,
    val completedAt: String?
)

data class UserSummary(
    val id: Long,
    val username: String,
    val fullName: String
)
```

---

## Part 6: Security with Spring Security {#part-6}

### 6.1 JWT Configuration

Add dependencies to `build.gradle.kts`:

```kotlin
dependencies {
    // ... existing dependencies ...
    implementation("io.jsonwebtoken:jjwt-api:0.12.3")
    runtimeOnly("io.jsonwebtoken:jjwt-impl:0.12.3")
    runtimeOnly("io.jsonwebtoken:jjwt-jackson:0.12.3")
}
```

### 6.2 Security Configuration

Update `SecurityConfig.kt`:

```kotlin
package com.example.projectmanager.config

import com.example.projectmanager.security.JwtAuthenticationEntryPoint
import com.example.projectmanager.security.JwtAuthenticationFilter
import org.springframework.context.annotation.Bean
import org.springframework.context.annotation.Configuration
import org.springframework.security.authentication.AuthenticationManager
import org.springframework.security.config.annotation.authentication.configuration.AuthenticationConfiguration
import org.springframework.security.config.annotation.method.configuration.EnableMethodSecurity
import org.springframework.security.config.annotation.web.builders.HttpSecurity
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity
import org.springframework.security.config.http.SessionCreationPolicy
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder
import org.springframework.security.crypto.password.PasswordEncoder
import org.springframework.security.web.SecurityFilterChain
import org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter

@Configuration
@EnableWebSecurity
@EnableMethodSecurity
class SecurityConfig(
    private val jwtAuthenticationEntryPoint: JwtAuthenticationEntryPoint,
    private val jwtAuthenticationFilter: JwtAuthenticationFilter
) {

    @Bean
    fun passwordEncoder(): PasswordEncoder = BCryptPasswordEncoder()

    @Bean
    fun authenticationManager(config: AuthenticationConfiguration): AuthenticationManager =
        config.authenticationManager

    @Bean
    fun filterChain(http: HttpSecurity): SecurityFilterChain {
        http
            .csrf { it.disable() }
            .exceptionHandling { it.authenticationEntryPoint(jwtAuthenticationEntryPoint) }
            .sessionManagement { it.sessionCreationPolicy(SessionCreationPolicy.STATELESS) }
            .authorizeHttpRequests { auth ->
                auth
                    .requestMatchers("/api/auth/**").permitAll()
                    .requestMatchers("/h2-console/**").permitAll()
                    .requestMatchers("/actuator/health").permitAll()
                    .anyRequest().authenticated()
            }
            .headers { headers ->
                headers.frameOptions { it.sameOrigin() }
            }

        http.addFilterBefore(jwtAuthenticationFilter, UsernamePasswordAuthenticationFilter::class.java)

        return http.build()
    }
}
```

### 6.3 JWT Token Provider

Create `src/main/kotlin/com/example/projectmanager/security/JwtTokenProvider.kt`:

```kotlin
package com.example.projectmanager.security

import io.jsonwebtoken.*
import io.jsonwebtoken.io.Decoders
import io.jsonwebtoken.security.Keys
import org.springframework.beans.factory.annotation.Value
import org.springframework.security.core.Authentication
import org.springframework.stereotype.Component
import java.security.Key
import java.util.*

@Component
class JwtTokenProvider {

    @Value("\${app.jwt.secret}")
    private lateinit var jwtSecret: String

    @Value("\${app.jwt.expiration}")
    private lateinit var jwtExpiration: String

    private fun key(): Key = Keys.hmacShaKeyFor(Decoders.BASE64.decode(jwtSecret))

    fun generateToken(authentication: Authentication): String {
        val userPrincipal = authentication.principal as UserPrincipal
        val expiryDate = Date(System.currentTimeMillis() + jwtExpiration.toLong())

        return Jwts.builder()
            .setSubject(userPrincipal.id.toString())
            .setIssuedAt(Date())
            .setExpiration(expiryDate)
            .signWith(key(), SignatureAlgorithm.HS512)
            .compact()
    }

    fun getUserIdFromToken(token: String): Long {
        val claims = Jwts.parserBuilder()
            .setSigningKey(key())
            .build()
            .parseClaimsJws(token)
            .body

        return claims.subject.toLong()
    }

    fun validateToken(token: String): Boolean {
        try {
            Jwts.parserBuilder()
                .setSigningKey(key())
                .build()
                .parseClaimsJws(token)
            return true
        } catch (ex: MalformedJwtException) {
            // Log: Invalid JWT token
        } catch (ex: ExpiredJwtException) {
            // Log: Expired JWT token
        } catch (ex: UnsupportedJwtException) {
            // Log: Unsupported JWT token
        } catch (ex: IllegalArgumentException) {
            // Log: JWT claims string is empty
        }
        return false
    }
}
```

### 6.4 Custom User Details Service

Create `src/main/kotlin/com/example/projectmanager/security/CustomUserDetailsService.kt`:

```kotlin
package com.example.projectmanager.security

import com.example.projectmanager.repository.UserRepository
import org.springframework.security.core.userdetails.UserDetails
import org.springframework.security.core.userdetails.UserDetailsService
import org.springframework.security.core.userdetails.UsernameNotFoundException
import org.springframework.stereotype.Service
import org.springframework.transaction.annotation.Transactional

@Service
class CustomUserDetailsService(
    private val userRepository: UserRepository
) : UserDetailsService {

    @Transactional
    override fun loadUserByUsername(username: String): UserDetails {
        val user = userRepository.findByUsername(username)
            .orElseThrow { UsernameNotFoundException("User not found with username: $username") }

        return UserPrincipal.create(user)
    }

    @Transactional
    fun loadUserById(id: Long): UserDetails {
        val user = userRepository.findById(id)
            .orElseThrow { UsernameNotFoundException("User not found with id: $id") }

        return UserPrincipal.create(user)
    }
}
```

Create `src/main/kotlin/com/example/projectmanager/security/UserPrincipal.kt`:

```kotlin
package com.example.projectmanager.security

import com.example.projectmanager.model.User
import org.springframework.security.core.GrantedAuthority
import org.springframework.security.core.authority.SimpleGrantedAuthority
import org.springframework.security.core.userdetails.UserDetails

data class UserPrincipal(
    val id: Long,
    private val username: String,
    private val email: String,
    private val password: String,
    private val authorities: Collection<GrantedAuthority>
) : UserDetails {

    override fun getAuthorities() = authorities
    override fun getPassword() = password
    override fun getUsername() = username
    override fun isAccountNonExpired() = true
    override fun isAccountNonLocked() = true
    override fun isCredentialsNonExpired() = true
    override fun isEnabled() = true

    companion object {
        fun create(user: User): UserPrincipal {
            val authorities = listOf(SimpleGrantedAuthority("ROLE_${user.role.name}"))

            return UserPrincipal(
                id = user.id,
                username = user.username,
                email = user.email,
                password = user.password,
                authorities = authorities
            )
        }
    }
}
```

### 6.5 JWT Authentication Filter

Create `src/main/kotlin/com/example/projectmanager/security/JwtAuthenticationFilter.kt`:

```kotlin
package com.example.projectmanager.security

import jakarta.servlet.FilterChain
import jakarta.servlet.http.HttpServletRequest
import jakarta.servlet.http.HttpServletResponse
import org.springframework.security.authentication.UsernamePasswordAuthenticationToken
import org.springframework.security.core.context.SecurityContextHolder
import org.springframework.security.web.authentication.WebAuthenticationDetailsSource
import org.springframework.stereotype.Component
import org.springframework.util.StringUtils
import org.springframework.web.filter.OncePerRequestFilter

@Component
class JwtAuthenticationFilter(
    private val tokenProvider: JwtTokenProvider,
    private val userDetailsService: CustomUserDetailsService
) : OncePerRequestFilter() {

    override fun doFilterInternal(
        request: HttpServletRequest,
        response: HttpServletResponse,
        filterChain: FilterChain
    ) {
        try {
            val jwt = getJwtFromRequest(request)

            if (!jwt.isNullOrBlank() && tokenProvider.validateToken(jwt)) {
                val userId = tokenProvider.getUserIdFromToken(jwt)
                val userDetails = userDetailsService.loadUserById(userId)

                val authentication = UsernamePasswordAuthenticationToken(
                    userDetails,
                    null,
                    userDetails.authorities
                )
                authentication.details = WebAuthenticationDetailsSource().buildDetails(request)

                SecurityContextHolder.getContext().authentication = authentication
            }
        } catch (ex: Exception) {
            logger.error("Could not set user authentication in security context", ex)
        }

        filterChain.doFilter(request, response)
    }

    private fun getJwtFromRequest(request: HttpServletRequest): String? {
        val bearerToken = request.getHeader("Authorization")

        return if (StringUtils.hasText(bearerToken) && bearerToken.startsWith("Bearer ")) {
            bearerToken.substring(7)
        } else null
    }
}
```

### 6.6 Authentication Controller

Create `src/main/kotlin/com/example/projectmanager/controller/AuthController.kt`:

```kotlin
package com.example.projectmanager.controller

import com.example.projectmanager.dto.*
import com.example.projectmanager.service.AuthService
import jakarta.validation.Valid
import org.springframework.http.HttpStatus
import org.springframework.http.ResponseEntity
import org.springframework.web.bind.annotation.*

@RestController
@RequestMapping("/api/auth")
class AuthController(
    private val authService: AuthService
) {

    @PostMapping("/signin")
    fun authenticateUser(@Valid @RequestBody request: LoginRequest): ResponseEntity<JwtAuthenticationResponse> {
        val response = authService.authenticateUser(request)
        return ResponseEntity.ok(response)
    }

    @PostMapping("/signup")
    fun registerUser(@Valid @RequestBody request: SignUpRequest): ResponseEntity<ApiResponse> {
        return try {
            authService.registerUser(request)
            ResponseEntity.status(HttpStatus.CREATED)
                .body(ApiResponse(true, "User registered successfully"))
        } catch (e: IllegalArgumentException) {
            ResponseEntity.badRequest()
                .body(ApiResponse(false, e.message ?: "Registration failed"))
        }
    }

    @GetMapping("/me")
    fun getCurrentUser(): ResponseEntity<UserResponse> {
        val user = authService.getCurrentUser()
        return ResponseEntity.ok(user)
    }
}
```

### 6.7 Authentication DTOs

Create `src/main/kotlin/com/example/projectmanager/dto/AuthDtos.kt`:

```kotlin
package com.example.projectmanager.dto

import jakarta.validation.constraints.Email
import jakarta.validation.constraints.NotBlank
import jakarta.validation.constraints.Size

data class LoginRequest(
    @field:NotBlank(message = "Username is required")
    val username: String,

    @field:NotBlank(message = "Password is required")
    val password: String
)

data class SignUpRequest(
    @field:NotBlank(message = "Username is required")
    @field:Size(min = 3, max = 20, message = "Username must be between 3 and 20 characters")
    val username: String,

    @field:NotBlank(message = "Email is required")
    @field:Email(message = "Email must be valid")
    val email: String,

    @field:NotBlank(message = "Password is required")
    @field:Size(min = 6, max = 40, message = "Password must be between 6 and 40 characters")
    val password: String,

    @field:NotBlank(message = "First name is required")
    val firstName: String,

    @field:NotBlank(message = "Last name is required")
    val lastName: String
)

data class JwtAuthenticationResponse(
    val accessToken: String,
    val tokenType: String = "Bearer",
    val user: UserResponse
)

data class UserResponse(
    val id: Long,
    val username: String,
    val email: String,
    val firstName: String,
    val lastName: String,
    val role: String
)

data class ApiResponse(
    val success: Boolean,
    val message: String
)
```

### 6.8 Update application.properties

Add JWT configuration:

```properties
# JWT Properties
app.jwt.secret=404E635266556A586E3272357538782F413F4428472B4B6250645367566B5970
app.jwt.expiration=86400000
```

---

## Part 7: Testing Your Application {#part-7}

### 7.1 Unit Testing Services

Create `src/test/kotlin/com/example/projectmanager/service/ProjectServiceTest.kt`:

```kotlin
package com.example.projectmanager.service

import com.example.projectmanager.dto.CreateProjectRequest
import com.example.projectmanager.model.Project
import com.example.projectmanager.model.ProjectStatus
import com.example.projectmanager.model.User
import com.example.projectmanager.model.UserRole
import com.example.projectmanager.repository.ProjectRepository
import io.mockk.*
import org.junit.jupiter.api.Assertions.*
import org.junit.jupiter.api.BeforeEach
import org.junit.jupiter.api.Test
import org.junit.jupiter.api.assertThrows
import org.springframework.data.repository.findByIdOrNull

class ProjectServiceTest {

    private lateinit var projectRepository: ProjectRepository
    private lateinit var projectService: ProjectService

    @BeforeEach
    fun setup() {
        projectRepository = mockk()
        projectService = ProjectService(projectRepository)
    }

    @Test
    fun `should create project successfully`() {
        // Given
        val request = CreateProjectRequest(
            name = "Test Project",
            description = "Test Description"
        )

        val owner = User(
            id = 1L,
            username = "testuser",
            email = "test@example.com",
            password = "password",
            firstName = "Test",
            lastName = "User",
            role = UserRole.USER
        )

        val savedProject = Project(
            id = 1L,
            name = request.name,
            description = request.description,
            owner = owner
        )

        every { projectRepository.save(any()) } returns savedProject

        // When
        val result = projectService.createProject(request, owner)

        // Then
        assertNotNull(result)
        assertEquals(1L, result.id)
        assertEquals("Test Project", result.name)
        verify { projectRepository.save(any()) }
    }

    @Test
    fun `should throw exception when project not found`() {
        // Given
        every { projectRepository.findByIdOrNull(any()) } returns null

        // When/Then
        assertThrows<NoSuchElementException> {
            projectService.getProjectById(999L)
        }
    }

    @Test
    fun `should update project status successfully`() {
        // Given
        val owner = User(
            id = 1L,
            username = "testuser",
            email = "test@example.com",
            password = "password",
            firstName = "Test",
            lastName = "User",
            role = UserRole.USER
        )

        val existingProject = Project(
            id = 1L,
            name = "Existing Project",
            status = ProjectStatus.PLANNING,
            owner = owner
        )

        every { projectRepository.findByIdOrNull(1L) } returns existingProject
        every { projectRepository.save(any()) } answers { firstArg() }

        // When
        val result = projectService.updateProjectStatus(1L, ProjectStatus.IN_PROGRESS)

        // Then
        assertEquals(ProjectStatus.IN_PROGRESS, result.status)
        verify { projectRepository.save(any()) }
    }
}
```

### 7.2 Integration Testing Controllers

Create `src/test/kotlin/com/example/projectmanager/controller/ProjectControllerIntegrationTest.kt`:

```kotlin
package com.example.projectmanager.controller

import com.example.projectmanager.dto.CreateProjectRequest
import com.example.projectmanager.model.User
import com.example.projectmanager.model.UserRole
import com.example.projectmanager.repository.UserRepository
import com.fasterxml.jackson.databind.ObjectMapper
import org.junit.jupiter.api.BeforeEach
import org.junit.jupiter.api.Test
import org.springframework.beans.factory.annotation.Autowired
import org.springframework.boot.test.autoconfigure.web.servlet.AutoConfigureMockMvc
import org.springframework.boot.test.context.SpringBootTest
import org.springframework.http.MediaType
import org.springframework.security.crypto.password.PasswordEncoder
import org.springframework.security.test.context.support.WithMockUser
import org.springframework.test.context.ActiveProfiles
import org.springframework.test.web.servlet.MockMvc
import org.springframework.test.web.servlet.request.MockMvcRequestBuilders.*
import org.springframework.test.web.servlet.result.MockMvcResultMatchers.*
import org.springframework.transaction.annotation.Transactional

@SpringBootTest
@AutoConfigureMockMvc
@ActiveProfiles("test")
@Transactional
class ProjectControllerIntegrationTest {

    @Autowired
    private lateinit var mockMvc: MockMvc

    @Autowired
    private lateinit var objectMapper: ObjectMapper

    @Autowired
    private lateinit var userRepository: UserRepository

    @Autowired
    private lateinit var passwordEncoder: PasswordEncoder

    private lateinit var testUser: User

    @BeforeEach
    fun setup() {
        testUser = userRepository.save(User(
            username = "testuser",
            email = "test@example.com",
            password = passwordEncoder.encode("password"),
            firstName = "Test",
            lastName = "User",
            role = UserRole.USER
        ))
    }

    @Test
    @WithMockUser(username = "testuser", roles = ["USER"])
    fun `should create project successfully`() {
        val request = CreateProjectRequest(
            name = "Integration Test Project",
            description = "Testing with MockMvc"
        )

        mockMvc.perform(
            post("/api/projects")
                .contentType(MediaType.APPLICATION_JSON)
                .content(objectMapper.writeValueAsString(request))
        )
            .andExpect(status().isCreated)
            .andExpect(jsonPath("$.name").value("Integration Test Project"))
            .andExpect(jsonPath("$.description").value("Testing with MockMvc"))
            .andExpect(jsonPath("$.status").value("PLANNING"))
    }

    @Test
    @WithMockUser(username = "testuser", roles = ["USER"])
    fun `should return 400 for invalid project name`() {
        val request = CreateProjectRequest(
            name = "AB",  // Too short
            description = "Testing validation"
        )

        mockMvc.perform(
            post("/api/projects")
                .contentType(MediaType.APPLICATION_JSON)
                .content(objectMapper.writeValueAsString(request))
        )
            .andExpect(status().isBadRequest)
            .andExpect(jsonPath("$.message").exists())
    }

    @Test
    fun `should return 401 for unauthenticated request`() {
        mockMvc.perform(get("/api/projects"))
            .andExpect(status().isUnauthorized)
    }

    @Test
    @WithMockUser(username = "testuser", roles = ["USER"])
    fun `should get all projects with pagination`() {
        mockMvc.perform(
            get("/api/projects")
                .param("page", "0")
                .param("size", "5")
                .param("sortBy", "createdAt")
                .param("sortDirection", "DESC")
        )
            .andExpect(status().isOk)
            .andExpect(jsonPath("$.content").isArray)
            .andExpect(jsonPath("$.pageable.pageSize").value(5))
    }
}
```

### 7.3 Testing with TestContainers

Add TestContainers dependency:

```kotlin
dependencies {
    // ... existing dependencies ...
    testImplementation("org.testcontainers:testcontainers:1.19.3")
    testImplementation("org.testcontainers:postgresql:1.19.3")
    testImplementation("org.testcontainers:junit-jupiter:1.19.3")
}
```

Create `src/test/kotlin/com/example/projectmanager/repository/ProjectRepositoryTest.kt`:

```kotlin
package com.example.projectmanager.repository

import com.example.projectmanager.model.Project
import com.example.projectmanager.model.ProjectStatus
import com.example.projectmanager.model.User
import com.example.projectmanager.model.UserRole
import org.junit.jupiter.api.Assertions.*
import org.junit.jupiter.api.BeforeEach
import org.junit.jupiter.api.Test
import org.springframework.beans.factory.annotation.Autowired
import org.springframework.boot.test.autoconfigure.orm.jpa.DataJpaTest
import org.springframework.boot.test.autoconfigure.orm.jpa.TestEntityManager
import org.springframework.test.context.DynamicPropertyRegistry
import org.springframework.test.context.DynamicPropertySource
import org.testcontainers.containers.PostgreSQLContainer
import org.testcontainers.junit.jupiter.Container
import org.testcontainers.junit.jupiter.Testcontainers

@DataJpaTest
@Testcontainers
class ProjectRepositoryTest {

    companion object {
        @Container
        val postgresContainer = PostgreSQLContainer<Nothing>("postgres:15-alpine").apply {
            withDatabaseName("testdb")
            withUsername("test")
            withPassword("test")
        }

        @JvmStatic
        @DynamicPropertySource
        fun properties(registry: DynamicPropertyRegistry) {
            registry.add("spring.datasource.url", postgresContainer::getJdbcUrl)
            registry.add("spring.datasource.username", postgresContainer::getUsername)
            registry.add("spring.datasource.password", postgresContainer::getPassword)
        }
    }

    @Autowired
    private lateinit var testEntityManager: TestEntityManager

    @Autowired
    private lateinit var projectRepository: ProjectRepository

    private lateinit var testUser: User

    @BeforeEach
    fun setup() {
        testUser = testEntityManager.persist(User(
            username = "testuser",
            email = "test@example.com",
            password = "password",
            firstName = "Test",
            lastName = "User",
            role = UserRole.USER
        ))
        testEntityManager.flush()
    }

    @Test
    fun `should find projects by status`() {
        // Given
        val activeProject = Project(
            name = "Active Project",
            status = ProjectStatus.IN_PROGRESS,
            owner = testUser
        )
        val completedProject = Project(
            name = "Completed Project",
            status = ProjectStatus.COMPLETED,
            owner = testUser
        )

        testEntityManager.persist(activeProject)
        testEntityManager.persist(completedProject)
        testEntityManager.flush()

        // When
        val inProgressProjects = projectRepository.findByStatus(ProjectStatus.IN_PROGRESS)

        // Then
        assertEquals(1, inProgressProjects.size)
        assertEquals("Active Project", inProgressProjects[0].name)
    }

    @Test
    fun `should find projects by name containing ignore case`() {
        // Given
        val project1 = Project(name = "Spring Boot Project", owner = testUser)
        val project2 = Project(name = "spring tutorial", owner = testUser)
        val project3 = Project(name = "Django Project", owner = testUser)

        testEntityManager.persist(project1)
        testEntityManager.persist(project2)
        testEntityManager.persist(project3)
        testEntityManager.flush()

        // When
        val results = projectRepository.findByNameContainingIgnoreCase("spring")

        // Then
        assertEquals(2, results.size)
        assertTrue(results.any { it.name == "Spring Boot Project" })
        assertTrue(results.any { it.name == "spring tutorial" })
    }
}
```
